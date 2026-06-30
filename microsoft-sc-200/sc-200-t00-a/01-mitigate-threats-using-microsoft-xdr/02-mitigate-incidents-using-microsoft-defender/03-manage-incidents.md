Microsoft Defender XDR acts as a "central command" for security teams. Instead of looking at random security alerts one by one—which is confusing and slow—it groups related events into **Incidents**.

## 1. What is an Incident?

An incident is the **"story of an attack."**

- **Correlation:** Defender XDR automatically gathers suspicious events from devices, users, and mailboxes across your network.
    
- **The Big Picture:** Instead of seeing one isolated alert, you see the full scope:
    
    - Where the attack started.
        
    - What tactics the attacker used.
        
    - How far they got into your network.
        
    - Which devices, users, and mailboxes were hit.
        
- **Automation:** The system uses Artificial Intelligence (AI) to investigate and resolve individual alerts automatically. Security analysts can then perform further remediation steps directly within the incident view.
    

## 2. The Incident Queue

The Incident Queue is your workspace to see, prioritize, and manage ongoing security threats.

- **Timeframe:** By default, it shows incidents from the **last 30 days**.
    
- **Ordering:** The most recent incident appears at the top by default.
    
- **Automatic Naming:** Defender XDR gives incidents names based on what is affected (e.g., number of endpoints or users involved, detection sources, or categories). This helps you quickly understand the scale of the threat.
    
- **Customization:** You can customize the columns to show the specific data you need to make fast decisions.
    

### Available Queue Filters

You can use these filters to sort and prioritize your work:

|**Filter**|**Purpose**|
|---|---|
|**Status**|View Active vs. Resolved incidents.|
|**Severity**|Shows the impact. High severity = immediate attention needed.|
|**Incident Assignment**|Show only incidents assigned to you or those handled by automation.|
|**Multiple Service Source**|Choose to see incidents containing alerts from different sources (Yes/No).|
|**Service Sources**|Filter by origin (e.g., Defender for Endpoint, Identity, Office 365, Cloud Apps).|
|**Tags**|Filter by custom tags you've assigned to incidents.|
|**Multiple Category**|View incidents mapped to multiple attack categories (potentially more dangerous).|
|**Categories**|Focus on specific attack tactics or techniques.|
|**Entities**|Search by specific user/device name or ID.|
|**Data Sensitivity**|Identify if sensitive data is involved (requires Microsoft Purview Information Protection).|
|**Device Group**|Filter by pre-defined device groups.|
|**OS Platform**|Limit by operating system (Windows, Linux, macOS, etc.).|
|**Classification**|Filter by True Alert, False Alert, or Not Set.|
|**Automated Investigation**|Filter by the status of the automated investigation process.|
|**Associated Threat**|Search by specific threat information or previous search criteria.|

## 3. Preview Features

You don't always have to leave the main queue to get information. You can use the "Preview" features:

- **Circle:** Clicking the circle icon opens a side pane on the right. This gives you a quick preview of the item without leaving the page.
    
- **Greater than (>) symbol:** If an incident has related records, this expands the view to show those records beneath the main item.
    
- **Link:** Clicking the text link takes you to the full, detailed page for that specific incident.
    

## 4. How to Manage Incidents

Once you open an incident, you can take control of it to ensure the threat is contained.

### Core Management Actions

- **Edit Name:** You can change the incident name to align with your organization's naming conventions.
    
- **Assign:** If it's unassigned, click **"Assign to me."** This claims ownership of the incident and all associated alerts.
    
- **Status & Classification:**
    
    - **Status:** Change to _Active_ or _Resolved_. Setting an incident to _Resolved_ automatically closes all open alerts tied to it.
        
    - **Classification:** Mark it as _True Alert_ or _False Alert_. This helps your team learn from patterns over time.
        
- **Comments & History:** You can add comments to record your notes. All changes and comments are logged in the "Comments and history" section, providing an audit trail.
    
- **Tags:** Add custom tags to categorize incidents with common characteristics, making them easier to filter later.
    

### Moving Alerts Between Incidents

During an investigation, you might find that an alert belongs to a different incident.

- Go to the **Alerts tab** within an incident.
    
- You can move alerts from one incident to another. This allows you to combine or split incidents so you have all the relevant alerts in one place, creating a cleaner, more accurate view of the attack.