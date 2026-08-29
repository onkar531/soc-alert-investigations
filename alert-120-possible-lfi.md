# SOC Alert Investigation – Possible LFI Attack

## Alert Information

- Alert ID: 120
- Alert Type: Web Attack
- Rule: SOC170 - Passwd Found in Requested URL - Possible LFI Attack
- MITRE ATT&CK: T1190
- Source IP: 106.55.45.162
- Destination IP: 172.16.17.13
- Destination Port: 443
- Request Method: GET
- Device Action: Allowed

## Investigation

### 1. Source IP Analysis

The source IP 106.55.45.162 was checked using VirusTotal and AbuseIPDB.

- VirusTotal: 0/91 security vendors flagged the IP as malicious.
- AbuseIPDB: Abuse confidence score was 0%.
- ISP/ASN: Tencent Cloud / AS45090
- Usage Type: Data Center / Web Hosting / Transit
- Country: China

The IP reputation alone did not confirm malicious activity.

### 2. HTTP Traffic Analysis

The HTTP request was examined.

- Method: GET
- User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)
- Requested URL:
  `https://172.16.17.13/?file=../../../../etc/passwd`

The URL contains directory traversal (`../`) and attempts to access `/etc/passwd`.

This is consistent with a Local File Inclusion (LFI) attack attempt.

### 3. Attack Evidence

The strongest evidence is the requested file:

`/etc/passwd`

The attacker attempted to move outside the intended web directory using directory traversal.

### 4. Attack Result

The request was allowed by the device.

However, the available evidence does not confirm that the attacker successfully retrieved the `/etc/passwd` file.

Therefore, the attack is considered an unsuccessful LFI attempt based on the available evidence.

## Final Verdict

**True Positive – Malicious**

The traffic is malicious because the request contains a clear LFI/directory traversal pattern targeting `/etc/passwd`.

**Attack Successful: No**

## Tier 2 Escalation

No Tier 2 escalation required because the attack originated from the Internet and there is no evidence that the attack was successful.

## Key IOCs

- Source IP: 106.55.45.162
- Destination IP: 172.16.17.13
- Target: `/etc/passwd`
- Attack Type: Local File Inclusion (LFI)
- Technique: Directory Traversal

## Analyst Conclusion

The alert was a True Positive. The source made a suspicious web request attempting to access `/etc/passwd` through directory traversal. Although the request was allowed, there is no evidence confirming successful exploitation. The incident should be documented and the source/request pattern should be monitored.
