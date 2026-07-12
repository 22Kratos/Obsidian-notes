# A05: Injection Scanner — Logic & Implementation Guide

## Overview

This module scans websites for OWASP Top 10 A05:2021 — Injection vulnerabilities. It detects three types:

1. **SQL Injection (SQLi)** — Injecting SQL syntax into parameters to manipulate database queries
2. **Cross-Site Scripting (XSS)** — Injecting JavaScript that gets reflected back to the user
3. **Command Injection (CMDi)** — Injecting OS commands that get executed by the server

---

## Libraries You Need

```python
import re
import asyncio
import time
from difflib import SequenceMatcher
from typing import List, Optional
```

### Why Each Library

| Library | Purpose |
|---------|---------|
| `re` | Regular expressions — search response bodies for SQL errors, OS output patterns |
| `asyncio` | Async/await — send multiple HTTP requests without blocking the program |
| `time` | Measure response time — critical for time-based SQLi detection |
| `SequenceMatcher` | Compare two response bodies and get a similarity ratio (0.0 to 1.0) |
| `typing` | Type hints — `List[Vulnerability]`, `Optional[str]`, etc. for clean code |

---

## Core Logic — The Experiment Pattern

Every detection method follows the same pattern:

```
1. Send a NORMAL request (baseline)  →  record the response
2. Send an ATTACK request (payload)  →  record the response
3. COMPARE the two responses
4. If something changed suspiciously  →  possible vulnerability
```

Everything else is just **what payloads you send** and **how you interpret the differences**.

---

## The Main Loop

Your `scan()` method receives a list of endpoints from the crawler. Each endpoint has a URL, method (GET/POST), and parameters.

```
for endpoint in endpoints:
    for param in endpoint.params:
        baseline = send_normal_request(endpoint, param)

        test_sql_injection(endpoint, param, baseline)
        test_xss(endpoint, param, baseline)
        test_cmd_injection(endpoint, param, baseline)

    sleep(delay)  # be polite, don't hammer the server

return findings
```

### What Counts as a Testable Parameter

- GET parameters: `?id=1`, `?q=search`, `?page=2`
- POST body parameters: form fields sent in the request body
- Form input fields: `<input name="username">`, `<input name="email">`

Each of these is an injection point where you can append attack payloads.

---

## SQL Injection — 4 Sub-Techniques

### 1. Error-Based SQLi (Easiest)

**Concept**: You inject broken SQL syntax. If the database processes it, it throws a syntax error. That error appears in the HTTP response.

**How to test**:
- Send: `?id=1'`
- The single quote closes the string literal in the SQL query, breaking the syntax
- If the server is vulnerable, the response contains a database error message

**How to detect**: Scan the response body with regex patterns for known SQL error strings:

```
MySQL:      "You have an error in your SQL syntax"
            r"sql.*syntax"
            r"mysql_"
            r"MySqlException"

PostgreSQL: "ERROR: syntax error at or near"
            r"PostgreSQL"
            r"pg_"

SQLite:     "SQLITE_ERROR"
            r"sqlite3"
            r"SQLite"

MSSQL:      "Unclosed quotation mark"
            r"Microsoft.*ODBC"
            r"Unclosed quotation mark"

Oracle:     "ORA-01756"
            r"ORA-\d{5}"
```

**Key point**: Compare baseline vs attack response. The baseline should have NO error strings. The attack response has them. That difference confirms the database saw your input.

---

### 2. Boolean-Blind SQLi (Subtle)

**Concept**: No error appears, but the page content changes based on whether a condition is true or false.

**How to test**:
- Send TRUE condition: `?id=1 AND 1=1--` → page shows normal content
- Send FALSE condition: `?id=1 AND 1=2--` → page shows different content (empty, error, "not found")

**How to detect**: Compare the two responses using `SequenceMatcher`:

```python
ratio = SequenceMatcher(None, true_response.body, false_response.body).ratio()
```

- Ratio near 1.0 → responses are nearly identical → NOT vulnerable
- Ratio below 0.95 → responses differ significantly → LIKELY vulnerable

**Why it works**: The database evaluates `AND 1=1` (true, keeps original result) vs `AND 1=2` (false, returns nothing). The web page renders differently based on what the database returns.

---

### 3. Time-Based SQLi (Slow but Reliable)

**Concept**: You make the database pause, and you measure how long the response takes.

**How to test**:
- Send: `?id=1 AND SLEEP(5)--`
- Measure the response time

**How to detect**:
```python
start = time.time()
response = await client.get(url_with_payload)
elapsed = time.time() - start

if elapsed > 4.0:  # threshold (baseline usually < 1 second)
    # Database executed SLEEP(5) → vulnerable
```

**Why the threshold is 4.0 and not 5.0**: Network latency varies. The baseline might take 0.2-0.5s. If the attack response takes 4+ seconds longer, that's significant enough to confirm.

**Multiple payloads to try**:
- `' AND SLEEP(5)--` (MySQL)
- `'; SELECT SLEEP(5);--` (MySQL alternative)
- `' AND (SELECT * FROM (SELECT(SLEEP(5)))a)--` (MySQL subquery)
- `'; WAITFOR DELAY '0:0:5'--` (MSSQL)
- `'; SELECT pg_sleep(5);--` (PostgreSQL)

---

### 4. Union-Based SQLi (Column Count Discovery)

**Concept**: You combine your input with the original query using SQL UNION. If it works without error, injection is confirmed.

**How to test**:
```
?id=1' UNION SELECT NULL--
?id=1' UNION SELECT NULL,NULL--
?id=1' UNION SELECT NULL,NULL,NULL--
```

Keep adding NULLs until the error stops. The number of NULLs tells you how many columns the original query has.

**How to detect**:
- If you get a SQL error on `UNION SELECT NULL--` but NO error on `UNION SELECT NULL,NULL,NULL--`
- The transition from error to no-error means UNION injection works

**Why NULL**: You don't know the column types yet. NULL is compatible with every column type.

---

### Double-Confirm for SQLi

You run all 4 techniques. Each one that triggers increments a counter:

```
confirm_count = 0

if error_detected:          confirm_count += 1
if boolean_diff_detected:   confirm_count += 1
if time_delay_detected:     confirm_count += 1
if union_no_error:          confirm_count += 1

if confirm_count >= 2:
    → Report as SQL Injection (CRITICAL)
```

**Why**: A single detection could be a false positive. Time delay could be network lag. An error could be a coincidental 500 page. Two different techniques confirming the same vulnerability is much more reliable.

---

## XSS (Reflected) — Detection

**Concept**: You inject JavaScript into a parameter. If the server reflects it back into the HTML page without encoding, an attacker could execute malicious JS in a victim's browser.

**How to test**:
```
?q=<script>alert(1)</script>
```

**How to detect**: Check if the payload appears verbatim in the response body:

```python
if payload in response.body:
    # XSS confirmed — input reflected without encoding
```

**Try multiple payloads** because some sites filter `<script>` tags but miss other vectors:

| Payload | Why It Works |
|---------|-------------|
| `<script>alert(1)</script>` | Basic script tag |
| `<img src=x onerror=alert(1)>` | Image tag with error handler |
| `"><script>alert(1)</script>` | Breaks out of an attribute |
| `<svg onload=alert(1)>` | SVG with load event |
| `<body onload=alert(1)>` | Body with load event |
| `javascript:alert(1)` | Protocol handler |

**Confirm with 2+ payloads**: If 2 or more different payloads are reflected, it's confirmed XSS (HIGH severity).

---

## Command Injection — Detection

**Concept**: You append OS commands to a parameter. If the server passes your input to a shell, the command output appears in the response.

**How to test**:
```
?file=report.pdf; cat /etc/passwd
?file=report.pdf | whoami
?file=report.pdf && id
?file=report.pdf $(whoami)
?file=report.pdf `id`
```

**How to detect**: Scan the response for patterns that look like command output:

```
root:x:0:0:root:/root:/bin/bash     ← /etc/passwd contents
uid=33(www-data)                     ← output of `id`
www-data                             ← output of `whoami`
Linux server 5.4.0 #1 SMP            ← output of `uname -a`
```

**Regex patterns to match**:
```python
CMD_PATTERNS = [
    r"root:x:\d+:\d+",           # Linux /etc/passwd
    r"uid=\d+\(",                 # output of `id`
    r"Linux \S+ \d+\.\d+",       # uname output
    r"bin/bash|bin/sh",           # shell references
    r"C:\\Windows",               # Windows paths
]
```

**Confirm with 2+ payloads**: Different command types (pipe, semicolon, &&) that all produce output confirms CMDi (CRITICAL severity).

---

## Response Comparison — The Heart of Detection

Every detection method boils down to comparing two responses. Here's what you check:

| Signal | How You Check It | What It Means |
|--------|-----------------|---------------|
| Status code changed | `baseline.status != attack.status` | Server reacted differently (200 to 500) |
| Body content changed | `SequenceMatcher.ratio() < 0.95` | Page content is different |
| Response got longer | `len(attack.body) > len(baseline.body)` | New content appeared (errors, output) |
| Response got slower | `attack_time - baseline_time > 4.0` | Server executed a sleep command |
| Error strings appeared | `re.search(pattern, body)` | Database/OS errors in response |

### Building `_diff_responses()`

This is a helper that computes all these at once:

```python
def _diff_responses(self, baseline, attack):
    return {
        "status_changed": baseline.status != attack.status,
        "length_diff": len(attack.body) - len(baseline.body),
        "length_diff_pct": abs(len(attack.body) - len(baseline.body)) / max(len(baseline.body), 1),
        "body_ratio": SequenceMatcher(None, baseline.body, attack.body).ratio(),
        "time_diff": attack.time - baseline.time,
        "new_errors": self._check_sql_errors(attack.body) - self._check_sql_errors(baseline.body),
    }
```

---

## Error Handling

Wrap each test in try/except. If one request fails (timeout, connection error), log it and move to the next parameter. Never let one failure kill the entire scan.

```
try:
    result = await self._test_sql_injection(endpoint, param, baseline)
except TimeoutError:
    log.warning(f"Timeout testing {endpoint.url} param {param.name}")
    continue
except ConnectionError:
    log.warning(f"Connection failed for {endpoint.url}")
    continue
```

---

## Build Order — Step by Step

Build it piece by piece. Each step is independently testable.

### Phase 1: Foundation
1. Define your payload dictionaries — just data, no logic
2. Write `_send_payload()` — takes endpoint + param + payload, sends request, returns response
3. Write `_baseline_request()` — sends clean request, returns response

### Phase 2: SQL Injection
4. Write `_check_sql_errors()` — regex matching against response body
5. Write `_test_sql_error_based()` — simplest detection, send `'` and check for errors
6. **Test it** — point at DVWA or WebGoat, verify error-based works
7. Add boolean-blind — two requests, compare with SequenceMatcher
8. Add time-based — measure response time
9. Add union-based — increment NULLs, check for no error

### Phase 3: Other Injection Types
10. Write `_test_xss()` — send payloads, check if reflected
11. Write `_test_cmd_injection()` — send payloads, check for OS patterns

### Phase 4: Assembly
12. Wire together in `scan()` — loop through endpoints, collect findings
13. Add double-confirm logic — require 2+ techniques to confirm

---

## Common Pitfalls

### URL Encoding
Your payload `<script>` becomes `%3Cscript%3E` in a URL. Make sure you URL-encode payloads before sending them in GET parameters. POST bodies may or may not need encoding depending on content type.

### Baseline Noise
Sometimes the server returns slightly different content naturally — timestamps, CSRF tokens, random nonce values. Before comparing responses, normalize them by stripping out:
- CSRF tokens (`<input type="hidden" name="csrf_token" value="...">`)
- Timestamps
- Random strings that change per request

### Time-Based False Positives
Network latency varies. A response might take 2 seconds just because the network is slow. Use a generous threshold (4+ seconds for a 5-second sleep) and consider running the test twice to confirm.

### WAFs and Firewalls
Some sites block obvious payloads. If you get 403 or 406 on all payloads, you might be blocked by a Web Application Firewall, not protected by the application. Try encoding payloads or using alternative syntax.

### Double-Encode Payloads
Some servers decode once, so `%27` becomes `'` and gets blocked. Try double-encoding: `%2527` — the server decodes to `%27`, which then gets decoded again to `'`.

---

## Testing Against Known Vulnerable Targets

Use these to verify your scanner works:

| Target | What It Tests |
|--------|--------------|
| **DVWA** (Damn Vulnerable Web App) | SQLi, XSS, CMDi — has difficulty levels |
| **WebGoat** (OWASP) | Structured lessons for each vulnerability type |
| **bWAPP** | 100+ web vulnerabilities including injection |
| **HackTheBox** | Real-world CTF challenges |

Start with DVWA on low difficulty. Your error-based SQLi detector should find it immediately.

---

## Vulnerability Output Format

Each confirmed finding becomes a `Vulnerability` object:

```
SQL Injection:
  id:           A05-001
  name:         SQL Injection
  severity:     CRITICAL
  cvss_score:   9.8
  description:  Boolean-based blind SQL injection detected
  remediation:  Use parameterized queries, input validation, ORM

XSS (Reflected):
  id:           A05-002
  name:         Reflected Cross-Site Scripting
  severity:     HIGH
  cvss_score:   6.1
  description:  Parameter reflects unsanitized user input
  remediation:  Output encoding, CSP headers, DOMPurify

Command Injection:
  id:           A05-003
  name:         OS Command Injection
  severity:     CRITICAL
  cvss_score:   9.8
  description:  User input passed to shell command
  remediation:  Never use shell commands with user input, use native APIs
```
