# Incident 001 — Simulated SMB Brute-Force Attack

## Alert
Possible brute-force attack detected by Wazuh.

## Detection
- Wazuh custom rule: 100200
- Detection condition: 5 failed logins within 60 seconds
- Severity: Level 10

## Source
- Attacker/Test IP: 10.0.2.4
- Target Windows host: 10.0.2.15
- Target account: HP
- Protocol: SMB
- Port: 445

## Evidence
- Windows Event ID 4625 — failed authentication
- Wazuh custom rule 100200 — brute-force detection
- Multiple failed SMB authentication attempts from the controlled Kali test machine

## MITRE ATT&CK
- Technique: T1110 — Brute Force

## Timeline
1. Kali generated failed SMB authentication attempts.
2. Windows generated Event ID 4625.
3. Wazuh received the authentication events.
4. Custom rule 100200 detected the repeated failures.
5. Wazuh generated a Level 10 alert for investigation.

## Classification
True positive — controlled brute-force simulation in the home lab.

## Recommended Remediation
- Block or restrict the source IP.
- Investigate the targeted account.
- Review successful and failed authentication events.
- Verify whether any unauthorized access occurred.