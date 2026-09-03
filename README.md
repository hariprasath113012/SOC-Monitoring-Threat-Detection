Yes bro. The previous README was too polished/template-like. For a fresher's GitHub project, it should sound like you actually built the lab, tested things, saw the alerts, and documented what happened—not like a generated corporate report.

I would also remove unnecessary emojis, excessive headings, marketing-style phrases, and claims like “end-to-end” where they don't add much.

Here is a more natural, technically strong version. Delete your current README completely and copy-paste this.

# SOC Monitoring & Threat Detection Home Lab

A hands-on SOC Analyst L1 home lab built to practice how security events are collected, detected, investigated, and documented using Wazuh.

I used a Windows 10 machine as the monitored endpoint and Kali Linux to generate controlled security activity. Wazuh was used to collect and investigate the resulting Windows security logs and Sysmon events.

## What I Built

The main goal of this project was to get practical experience with the basic SOC workflow:

**Detect → Triage → Investigate → Document**

The lab includes:

- Wazuh SIEM
- Windows 10 endpoint
- Kali Linux test machine
- Sysmon
- Windows Security Event Logs
- Custom Wazuh detection rules
- MITRE ATT&CK mapping

## Lab Setup

```text
                    Wazuh Server
                   SIEM / Analysis
                         |
                         |
                  Security Telemetry
                         |
                         v
                  Windows 10
                 Wazuh Agent 003
                   10.0.2.15
                         ^
                         |
                  Controlled Tests
                         |
                         |
                   Kali Linux
                    10.0.2.4

Detection Scenario 1: SMB Brute Force

I used Kali Linux to generate repeated failed SMB authentication attempts against the Windows machine.

The failed authentication activity produced Windows Event ID 4625, which was collected by the Wazuh agent.

I then created a custom Wazuh rule to identify repeated failed logins.

Custom Rule

The rule triggers when 5 failed logins are observed within 60 seconds.

<rule id="100200" level="10" frequency="5" timeframe="60">
    <if_matched_sid>60122</if_matched_sid>
    <description>Possible brute-force attack - 5 failed logins within 60 seconds</description>
    <group>authentication_failed,brute_force,</group>
</rule>

The resulting activity was mapped to:

MITRE ATT&CK T1110 — Brute Force

Detection Scenario 2: Windows Process Investigation

I enabled Windows process creation auditing and later added Sysmon to get more detailed process telemetry.

The investigation included:

Windows Event ID 4688

Sysmon Event ID 1

Process names

Command-line information

Parent and child processes


One of the events I investigated showed:

Parent: C:\Windows\System32\net.exe
Child:  C:\Windows\SysWOW64\net1.exe
Command: net1 user

This activity was mapped to:

MITRE ATT&CK T1087 — Account Discovery

The activity was generated and investigated inside my controlled lab environment.

Lab Results

During the investigation, the following telemetry was visible in Wazuh Threat Hunting:

Event / Telemetry	Results Observed

Windows Event ID 4625	51 hits
Windows Event ID 4688	541 hits
Sysmon telemetry	24 hits


These are Wazuh search results from the lab and should not be interpreted as 51, 541, or 24 separate security incidents.

Investigation Evidence

The screenshots below were captured while working through the lab.

1. Wazuh Agent

Shows the Windows endpoint connected to Wazuh.



2. Successful Logon — Event ID 4624

Windows successful authentication telemetry collected by Wazuh.



3. Failed SMB Authentication — Event ID 4625

Shows the failed authentication activity generated from the Kali test machine.



4. Custom Brute-Force Rule

Shows the custom Wazuh rule used to detect repeated failed logins.



5. Windows Process Creation — Event ID 4688

Process creation telemetry investigated through Wazuh.



6. Sysmon Event ID 1

Process execution details collected through Sysmon.





7. Event ID 4625 Search Results

Wazuh Threat Hunting showing 51 Event ID 4625 results.



8. Event ID 4688 Search Results

Wazuh Threat Hunting showing 541 Event ID 4688 results.



9. Sysmon Search Results

Wazuh Threat Hunting showing 24 Sysmon results.



Incident Reports

I also documented the investigations separately instead of keeping everything only in the README.

Incident 001 — Simulated SMB Brute Force

The report covers:

Alert details

Detection rule

Source and target

Authentication evidence

Timeline

MITRE ATT&CK technique

Classification

Recommended remediation


Incident 002 — Windows Process Investigation

The report covers:

Event details

Process information

Parent-child relationship

Command-line activity

MITRE ATT&CK mapping

Investigation notes

Evidence

Recommended remediation


The reports can be found in:

investigations/

Custom Detection Rule

The custom Wazuh rule used in this project is available here:

rules/local_rules.xml

Repository Structure

SOC-Monitoring-Threat-Detection/
│
├── investigations/
│   ├── incident-001-brute-force.md
│   └── incident-002-process-investigation.md
│
├── screenshots/
│   ├── 01-wazuh-agent-overview.png
│   ├── 02-event-4624-successful-logon.png
│   ├── 03-kali-to-windows-4625.png
│   ├── 04-custom-bruteforce-rule.png
│   ├── 05-event-4688-process-creation.png
│   ├── 06a-sysmon-event-1-process-details.png
│   ├── 06b-sysmon-event-1-event-details.png
│   ├── 07-event-4625-51-hits.png
│   ├── 08-event-4688-541-hits.png
│   └── 09-sysmon-process-events-count.png
│
├── rules/
│   └── local_rules.xml
│
└── README.md

Skills Practiced

Wazuh SIEM monitoring

Windows Event Log analysis

Sysmon analysis

Authentication event investigation

Process investigation

Parent-child process analysis

Custom Wazuh detection rules

Brute-force detection

MITRE ATT&CK mapping

IOC identification

SOC L1 incident documentation


Notes

This project was built as a personal home lab to practice SOC Analyst L1 tasks. The attack activity was generated only against my own lab systems.

The numbers shown in the results section are the actual search-result counts observed during the lab and are included for transparency.

### Why I prefer this version

This one sounds more like **a person documenting something they actually built**:

- No fake corporate language.
- No unnecessary “industry-leading” type wording.
- No exaggerated claims.
- Your **actual 51 / 541 / 24** results are shown.
- Your actual screenshots are connected to the investigation.
- Your actual custom rule is visible.
- The README explains **what you did**, not just what Wazuh is.
- A recruiter can quickly understand the project without reading a huge wall of text.

**Use this version as the final README.** After saving it, we'll upload the project to GitHub and check that the screenshots actually render correctly.