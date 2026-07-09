## Manage automated investigations

Microsoft Defender for Endpoint uses **Automated Investigation and Remediation (AIR)**. Instead of your team manually handling every single alert, this technology acts like a digital security analyst. It uses algorithms to examine alerts, investigate breaches, and take immediate action.

This reduces the "noise" (alert volume) so your security team can focus on complex, high-value threats. The **Action center** is your central hub for tracking these investigations, including their status, source, and any actions taken (or waiting for your approval).

## How the automated investigation starts

When an alert triggers, a security "playbook" kicks in.

- **Example:** If a malicious file is detected on a device, the system automatically starts investigating.
    
- **Expansion:** It doesn't just look at one spot; it checks if that file exists on _other_ devices in your organization.
    
- **Results:** During and after the investigation, you get clear verdicts (Malicious, Suspicious, or No threats found).
    

## Details of an automated investigation

When you click a triggering alert, you can view the investigation’s progress through several tabs:

- **Alerts:** The events that started the process.
    
- **Devices:** Machines where the threat was spotted.
    
- **Evidence:** Specific items found to be malicious.
    
- **Entities:** A breakdown of analyzed items (with their specific verdicts).
    
- **Log:** A chronological history of every action taken.
    
- **Pending actions:** If the system is waiting for you to approve or reject a fix, you’ll find it here.
    

## How an automated investigation expands its scope

While an investigation is running, it is "smart":

- If new alerts pop up on that same device, they are added to the active investigation.
    
- If the threat is found on _other_ devices, those devices are automatically added to the scope.
    
- **Safety Check:** If the system tries to expand the investigation to 10 or more devices, it requires your manual approval before proceeding. You will see this request in the **Pending actions** tab.
    

## How threats are remediated

As the investigation runs, it assigns a verdict (Malicious, Suspicious, or No threats found) to everything it finds. Based on those verdicts, it takes "remediation actions."

- **Actions:** Quarantining files, stopping services, removing malicious tasks, etc.
    
- **Tracking:** All actions (pending or completed) appear in the **Action Center**. You can manually undo an action if needed.
    

## Automation levels in automated investigation and remediation capabilities

You can configure how much "trust" you give the system. This determines whether it fixes things automatically or asks for your permission first.

### Levels of automation

- **Full (Recommended):** The system remediates malicious threats automatically. You can view the history in the Action Center. This is safe, reliable, and frees up your team.
    
- **Semi (Require approval for any remediation):** The system investigates, but it cannot fix _anything_ without you clicking "Approve" in the Action Center.
    
- **Semi (Require approval for core folders):** It auto-remediates files in general folders but stops and asks for permission before touching "Core folders" (like Windows system directories).
    
- **Semi (Require approval for non-temp folders):** It auto-remediates files in temporary folders (like `\temp` or `\downloads`) but asks for permission before touching files in standard locations.
    
- **No automated response:** The system does nothing. **(Not recommended)**.
    

## Important points about automation levels

Full automation is the industry-standard recommendation. It is proven to be efficient and safe. By using Full automation, you allow your security team to stop "fighting fires" and start working on strategic security improvements. If you have defined specific "device groups" with different settings, those will stay as they are.