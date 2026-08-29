# SOC Alert Investigation – Possible LFI Attack

## Alert Information

- Alert ID: 120
- Alert Type: Web Attack
- Rule: SOC170 - Passwd Found in Requested URL - Possible LFI Attack
- Hostname: WebServer1006
- Source IP: 106.55.45.162
- Destination IP: 172.16.17.13
- Destination Port: 443
- HTTP Method: GET
- Device Action: Permitted
- MITRE ATT&CK: T1190

## Investigation

### 1. Alert Trigger Reason

The alert was triggered because the requested URL contained a reference to `/etc/passwd`.

### 2. Source IP Investigation

Source IP: `106.55.45.162`

VirusTotal:
- 0/91 security vendors flagged the IP as malicious.
- IP belongs to Tencent Cloud.
- Country: China.

AbuseIPDB:
- 3,435 reports.
- Abuse Confidence Score: 0%.

IP reputation alone was not enough to confirm the attack.

### 3. HTTP Traffic Investigation

HTTP Method: `GET`

User-Agent:
`Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)`

Requested URL:
`https://172.16.17.13/?file=../../../../etc/passwd`

The request contains directory traversal (`../../../../`) and attempts to access `/etc/passwd`.

This is consistent with an LFI/directory traversal attack attempt.

### 4. Attack Result

HTTP Response Status: `500`

The HTTP 500 response indicates a server-side error.

There was no evidence that `/etc/passwd` was successfully retrieved.

## Final Classification

- Traffic: Malicious
- Attack Type: LFI / Directory Traversal
- Planned Test: Not Planned
- Direction: Internet → Company Network
- Attack Successful: No
- Tier 2 Escalation: No
- Final Verdict: True Positive

## SOC Analyst Conclusion

The alert is classified as a True Positive.

The source attempted to access the sensitive `/etc/passwd` file using directory traversal.

The request resulted in an HTTP 500 response, and there was no evidence of successful exploitation.

## Key SOC Lessons

- Inspect the complete HTTP request.
- Check suspicious URL parameters.
- Look for directory traversal patterns.
- Investigate the source IP reputation.
- Check HTTP response status and response size.
- Determine whether exploitation was successful.
- Correctly classify True Positive vs False Positive.
