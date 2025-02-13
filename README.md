<h1>SOAR-EDR-Playbook</h1>
<h2>Description</h2>
The SOAR-EDR-Playbook is a Security Orchestration, Automation, and Response (SOAR) project designed to automate incident response by integrating LimaCharlie EDR (Endpoint Detection & Response) with Tines (SOAR). The playbook enables real-time detection of security threats, automated alerting, and machine isolation based on SOC analyst decisions. This project simulates an enterprise cybersecurity workflow, where a malicious tool (e.g. LaZagne) is executed on a Windows system, detected by LimaCharlie, and escalated to Tines for further automation. Analysts are notified via Slack & Email and can choose to isolate the compromised machine in one click.
<br />

<h2>Objectives</h2>

- <b>Automate Threat Detection:</b>  Use LimaCharlie to detect hack tools and malware. 
- <b>Implement SOAR Playbooks:</b> Use Tines for workflow automation.
- <b>Real-time Notifications:</b> Send alerts via Slack & Email.
- <b>Automated Incident Response:</b> Enable one-click machine isolation.
- <b>Enhance SOC Efficiency:</b> Reduce manual investigation time. 

<h2>Workflow Overview</h2>

- <b>Infection Event:</b>  A user runs a hack tool (e.g., LaZagne) on their computer.
- <b>Detection:</b> LimaCharlie detects the hack tool and generates an alert.
- <b>SOAR Automation:</b> Tines receives the alert from LimaCharlie. Tines forwards the alert to Slack (#alerts channel) and Email (SOC Team).
- <b>Incident Response:</b> SOC Analyst is prompted via Tines to decide whether to isolate the infected machine. If the analyst selects yes LimaCharlie isolates the machine, and a Slack notification is sent. If no is Slack notification is sent instructing analysts to investigate further.

### SOAR Workflow Diagram
![SOAR Workflow Diagram](https://github.com/Prajwal-Manjunath/SOAR-EDR-Playbook-Project/blob/main/tine_flow.png)

##  LimaCharlie Detection and Response Rule

```bash
events:
  - NEW_PROCESS
  - EXISTING_PROCESS
op: and
rules:
  - op: is windows
  - op: or
    rules:
      - case sensitive: false
        op: ends with
        path: event/FILE_PATH
        value: LaZagne.exe
      - case sensitive: false
        op: contains
        path: event/COMMAND_LINE
        value: lazagne
      - case sensitive: false
        op: is
        path: event/HASH
        value: 3cc5ee93a9ba1fc57389705283b760c8bd61f35e9398bbfa3210e2becf6d4b05

- action: report
  metadata:
    author: KnightZeus
    description: Detect LaZagne Execution
    falsepositives:
      - FalsePositive
    level: high
    tags:
      - attack.credential_access
  name: "KnightZeus - HackTool - LaZagne"
```
### Message Sent to slack upon detection of event
![Slack Message](https://github.com/Prajwal-Manjunath/SOAR-EDR-Playbook-Project/blob/main/slack%20message%201.png)

### Message Sent to designated Email upon detection of event
![Email Message](https://github.com/Prajwal-Manjunath/SOAR-EDR-Playbook-Project/blob/main/email_received.png)

### SOC Analyst decision page
![Decision page](https://github.com/Prajwal-Manjunath/SOAR-EDR-Playbook-Project/blob/main/user_input.png)

### Message received in Slack when Analyst clicks on NO 
![Slack Message](https://github.com/Prajwal-Manjunath/SOAR-EDR-Playbook-Project/blob/main/devices_not_isolated.png)

### Message received in Slack when Analyst clicks on Yes
![Slack Message](https://github.com/Prajwal-Manjunath/SOAR-EDR-Playbook-Project/blob/main/device_isolated.png)

### Result
This playbook serves as a hands-on learning tool for SOC teams helping them understand how to build, test, and deploy automated security responses using SOAR and EDR technologies.

