## Investigate incidents

The incident page is your central hub for reviewing a security event. It provides navigation links to see the full story of the attack.

### Incident overview

A snapshot that shows you the most important facts immediately.

![](images/Pasted%20image%2020260708233234.png)

- **Attack categories:** Shows you how far the attack has progressed along the "kill chain" (aligned with the MITRE ATT&CK framework).
    
- **Scope:** A list of the most important assets involved, including their risk level and any tags.
    
- **Alerts timeline:** A chronological view of how the alerts happened and why they were linked together.
    
- **Evidence:** A summary of all "artifacts" (files, emails, etc.) involved and whether they have been cleaned up or still need your attention.
    
- **Goal:** This helps you quickly "triage" (assess the priority of) the incident.
    

### Alerts

A list of every alert tied to this incident.

- Includes severity, involved entities, and source (e.g., Defender for Identity/Endpoint/Office 365).
    
- Listed chronologically so you can follow the story of the attack.
    
- You can click any alert to go to its specific page for a deeper look.
    

### Devices

A list of all machines (devices) where alerts were seen.

- Click a machine name to go to its "Device page" to see all events and alerts associated with that specific computer.
    

### Users

A list of all users identified in the incident.

- Clicking a username takes you to their page in Microsoft Defender for Cloud Apps for further investigation.
    

### Mailboxes

Investigate email accounts identified as part of the incident.

### Apps

Investigate software applications identified as part of the incident.

### Investigations

Shows any "Automated Investigations" triggered by the alerts.

- These investigations might perform actions automatically, or they might be waiting for your approval.
    
- If any actions are waiting for you, you can find them in the **Pending actions** tab.
    

### Evidence and Responses

Defender automatically analyzes suspicious entities (files, processes, services, etc.) found in the alerts.

- Each item is marked with a verdict: **Malicious**, **Suspicious**, or **Clean**.
    
- It also shows the "remediation status," telling you if the threat is already blocked or if you need to take the next step.
    

### Incident Graph

A visual map of the cybersecurity attack.

- It tells the story by showing the entry point and how the attack spread across devices.
    
- You can click the circles (entities) to see if a malicious file was found elsewhere in your organization or globally.
    

### Blast radius analysis (Preview)

An interactive map that shows both current damage (post-breach) and potential future damage (pre-breach).

- **Replaces:** This feature replaces "Attack path analysis."
    
- **Requirements:** Requires onboarding to the **Microsoft Sentinel data lake** (otherwise, it won't appear).
    
- **Value:** It shows "attack paths" (routes hackers use to reach critical assets), helping you prioritize what to fix.
    
- **Roles:** Useful for analysts (gauge scope), engineers (fix vulnerabilities), responders (map actions), and CISOs (track posture).
    
- **How to use:** From the incident graph, select a node (entity) and choose **"View blast radius."** You can also take actions directly from here (like isolating a device or disabling a user) to stop the attack.
![](images/Pasted%20image%2020260708234139.png)
    

**Limitations & modeling notes:**

- **Path depth:** Max 7 "hops" (steps) deep.
    
- **Accuracy:** It shows _possible_ attack paths, not guaranteed ones.
    
- **Data lag:** New relationships might not show up instantly.
    
- **Visibility:** You can only see nodes/edges that your permission level allows.
    
	- **Islands:** You might occasionally see disconnected nodes due to calculation timing.