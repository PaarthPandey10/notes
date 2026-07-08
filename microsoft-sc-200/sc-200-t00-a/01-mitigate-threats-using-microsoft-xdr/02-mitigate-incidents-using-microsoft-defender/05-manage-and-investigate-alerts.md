## Manage and investigate alerts

You can manage alerts either from the **Alerts queue** or the **Alerts tab** of a specific device. Clicking an alert opens the management pane where you can view and update details.

![](images/Pasted%20image%2020260708235133.png)

## Alert management

You can view and set metadata (information about the alert) on the Alert preview or the Alert details page.

![](images/Pasted%20image%2020260708235153.png)

The metadata fields and actions include:

### Severity

- **High (Red):** Severe threats like Ransomware or Credential theft. High risk to the organization.
    
- **Medium (Orange):** Suspicious behaviors (like odd registry changes or strange file execution). Needs investigation to see if it’s an attack or a test.
    
- **Low (Yellow):** Common malware, hack-tools, or potentially just a user running exploration commands/clearing logs.
    
- **Informational (Grey):** Not necessarily harmful, but useful for security awareness.
    

**Important:** Defender Antivirus (AV) severity vs. Defender for Endpoint severity:

- **Defender AV:** Measures absolute risk to the **individual device** (e.g., malware detected).
    
- **Defender for Endpoint:** Measures the risk to the **entire organization**.
    
    - _Example:_ If a threat was blocked by AV, the Endpoint severity is "Informational" because the organization wasn't harmed. If the malware actually executed, it gets ranked Medium or High.
        

### Categories

These align with the **MITRE ATT&CK** framework (which categorizes attacker tactics):

- **Collection:** Gathering data.
    
- **Command and control:** Attacker communicating with their own server.
    
- **Credential access:** Stealing login info.
    
- **Defense evasion:** Hiding their tracks (e.g., turning off security).
    
- **Discovery:** Reconnaissance (looking for admin computers, file servers, etc.).
    
- **Execution:** Running malicious code.
    
- **Exfiltration:** Moving stolen data out.
    
- **Exploit:** Using a vulnerability.
    
- **Initial access:** Getting into the network.
    
- **Lateral movement:** Moving between devices.
    
- **Malware:** Trojans, backdoors, etc.
    
- **Persistence:** Setting things up to stay active after a reboot.
    
- **Privilege escalation:** Getting higher admin permissions.
    
- **Ransomware:** Encrypting files for money.
    
- **Suspicious activity:** Odd behavior.
    
- **Unwanted software:** Low-reputation apps (PUAs).
    

### Link to another incident

You can either create a new incident from the alert or link it to an existing one.

### Assign alerts

If it isn't assigned, click **Assign to me** to take ownership.

### Suppress alerts

If you have a known "safe" tool that keeps triggering alerts, you can create a **suppression rule**.

- **Scope:** You can suppress the alert for one device or for the whole organization.
    
- **Timing:** The rule only applies to _future_ alerts, not ones already in the queue.
    

### Change the status of an alert

Update the workflow: **New → In Progress → Resolved**. This helps your team keep track of who is working on what.

### Alert classification

Mark if an alert is a **True alert** or **False alert**. This is critical—it teaches the system to be more accurate and filters out "noise" in the future. You can also provide a "determination" for true alerts to add more detail.

### Add comments and view the history of an alert

Everything you write or change is logged in the **Comments and history** section, creating an audit trail.

## Alert investigation

When you click an alert in the queue, you get the **Alert page**. This includes the title, assets involved, a side pane, and the **Alert story**.

### Investigate using the alert story

The alert story is the core of your investigation. It explains **why** the alert triggered and shows related events before and after the alert.

- **Entities:** These are clickable. You can click the expand icon on an entity card to see more data.
    
- **Focus:** A blue stripe indicates the entity you are currently looking at.
    
- **Actions:** Clicking the "..." next to an entity lets you take action (like isolating a machine or disabling a user) right from the alert page.
    

### Take action from the details pane

Once you select an entity, the details pane on the side updates. It shows you the history and the controls available to fix the issue.

**Once finished:**

1. Go back to the main alert.
    
2. Mark the status as **Resolved**.
    
3. Classify as **True alert** or **False alert**.
    
4. If it was a false alert caused by a company app, create a **suppression rule** so it doesn't happen again.