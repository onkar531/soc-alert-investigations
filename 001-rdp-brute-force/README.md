# SOC Alert Investigation #001 — RDP Brute Force

## Platform
LetsDefend

## Alert Details
- Rule: SOC176 - RDP Brute Force Detected
- Severity: Medium
- Alert Type: Brute Force
- Protocol: RDP
- Port: 3389

## Investigation Objective

Determine whether the RDP brute-force alert was a true positive,
identify the attack scope, verify successful authentication,
and perform endpoint containment.

## 1. Source Analysis

- Source IP: 218.92.0.56
- Source Type: External

## 2. Target

- Hostname: Matthew
- IP Address: 172.16.17.148
- Protocol: RDP
- Port: 3389

## 3. Threat Intelligence

The source IP was checked using threat-intelligence resources
and was found to have suspicious/malicious reputation.

## 4. Log Analysis

Windows authentication events were investigated.

- Event ID 4625 — Failed logon
- Event ID 4624 — Successful logon

Observed pattern:

4625 → 4625 → 4625 → 4624

A successful authentication was observed from the same
source IP against the target host.

## 5. Traffic Analysis

Observed RDP traffic:

218.92.0.56 → 172.16.17.148:3389

The firewall allowed the traffic.

## 6. Scope

The source IP was investigated for connections to multiple
servers/clients.

Result:

Single target identified:
172.16.17.148

No evidence of the source targeting multiple hosts was identified.

## 7. Containment

The affected endpoint was identified in EDR:

- Hostname: Matthew
- IP Address: 172.16.17.148
- OS: Windows 10

The endpoint was isolated through EDR containment.

## 8. Indicators & Evidence

| Type | Value |
|---|---|
| Source IP | 218.92.0.56 |
| Target IP | 172.16.17.148 |
| Hostname | Matthew |
| Protocol | RDP |
| Port | 3389 |
| Failed Logon Event | 4625 |
| Successful Logon Event | 4624 |

## 9. Final Verdict

**TRUE POSITIVE**

The investigation confirmed suspicious RDP brute-force
activity followed by successful authentication.

## 10. Key Learning

A SOC analyst should not close a brute-force alert simply
because failed authentication attempts were detected.

The critical correlation was:

External IP
→ RDP traffic
→ Multiple failed logons
→ Successful logon
→ Scope determination
→ Endpoint containment

## Skills Practiced

- Alert triage
- IOC identification
- Threat intelligence
- Log analysis
- Windows Event ID analysis
- RDP investigation
- Attack scope determination
- EDR containment
- Incident documentation
- True Positive classification
