# Incident 002 — Windows Process Creation Investigation

## Alert
Windows process creation activity observed through Wazuh and Sysmon.

## Detection
- Windows Event ID: 4688
- Sysmon Event ID: 1
- Wazuh Agent: 003

## Process Activity
- Parent process: C:\Windows\System32\net.exe
- Child process: C:\Windows\SysWOW64\net1.exe
- Command line observed: net1 user

## Analysis
The Sysmon telemetry shows a parent-child process relationship between net.exe and net1.exe. The command line indicates that the net utility was used to query user-account information on the Windows endpoint.

## MITRE ATT&CK
- Technique: T1087 — Account Discovery

## Classification
Observed activity requiring investigation. The activity was generated in the controlled home lab for SOC investigation practice.

## Evidence
- Windows Event ID 4688
- Sysmon Event ID 1
- Wazuh Threat Hunting telemetry
- Parent-child process relationship
- Command-line telemetry

## Recommended Remediation
- Verify whether the account-discovery activity was authorized.
- Review the initiating user's activity and surrounding process events.
- Investigate related authentication and account-management events.
- Escalate if the activity is associated with other suspicious behavior.