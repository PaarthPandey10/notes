## Explore advanced hunting

Advanced hunting is a powerful tool that uses queries to search through 30 days of raw data across your network. It helps you proactively find threats, indicators, and potential breaches. It pulls data from Microsoft Defender for Endpoint, Office 365, Cloud Apps, and Identity.

### Data freshness and update frequency

- **Event/Activity data:** Populates tables like alerts and system events. This is available almost immediately after it is sent to the cloud.
    
- **Entity data:** Information about users and devices. These tables update every 15 minutes and are fully consolidated every 24 hours.
    

### Time zone

All time information is recorded in UTC.

### Data schema

The schema consists of various tables containing event or entity info. You need to understand these tables and their columns to write effective queries.

#### Get schema information

When building queries, you can check the reference to see:

- **Table description:** What data is inside and where it comes from.
    
- **Columns:** The specific data fields available.
    
- **Action types:** The types of events supported by that table.
    
- **Sample query:** Examples of how to use the table.
    

#### Access the schema reference

You can access this by selecting "View reference" next to the table name or searching via "Schema reference".

#### Learn the schema tables

The schema includes many tables, such as:

- **AlertInfo:** Alerts from all Defender services (includes severity and threat info).
    
- **DeviceEvents/DeviceProcessEvents:** Various events triggered on devices.
    
- **EmailEvents/EmailAttachmentInfo:** Data regarding email activity.
    
- **IdentityLogonEvents:** Authentication events.
    

## Custom detections

You can create custom detection rules based on your hunting queries. These rules run automatically to monitor for specific breaches or misconfigurations and can trigger alerts and response actions.

### Create detection rules

1. **Prepare the query:** Run your query first to ensure it works.
    
    - _Important:_ Each rule is limited to 100 alerts per run.
        
    - _Required columns:_ The query must return `Timestamp`, `DeviceId`, and `ReportId`.
        
2. **Create a new rule:** Provide details like the detection name, frequency, alert title, severity, and MITRE ATT&CK techniques.
    
3. **Rule frequency:** You can set the rule to run every hour, 3 hours, 12 hours, 24 hours, or **Continuous (NRT)** for near real-time results.
    
4. **Choose the impacted entities:** Identify which query columns represent the main affected items (like Device or User).
    
5. **Specify actions:** You can have the rule automatically take actions on:
    
    - **Devices:** Isolate, collect investigation package, run AV scan, or initiate investigation.
        
    - **Files:** Allow/Block or Quarantine.
        
6. **Set the rule scope:** Choose whether to apply the rule to all devices or specific groups.
    
7. **Review and turn on:** Save the rule to start monitoring.
    

## Visualize threat paths with the Hunting graph (Preview)

The Hunting graph integrates Microsoft Sentinel with Microsoft Defender XDR to create visual, interactive maps of potential threat scenarios.

- **Why use it:** It helps trace lateral movement, assess exposure to sensitive assets, and identify identities with high-value access without needing to manually write complex KQL joins.
    
- **Prerequisites:** You need proper Entra ID roles, access to the Sentinel data lake, and read-only access to Security Exposure Management.
    
- **How to launch:** Go to **Investigation & response > Hunting > Advanced hunting** and click the hunting graph icon.
    

### Graph basics

- **Nodes:** Represent entities (users, devices, storage, etc.).
    
- **Edges:** Represent relationships (permissions, traffic routing, etc.).
    

### Using predefined scenarios

You can load guided panels (scenarios) that bundle specific queries and logic. Common examples include:

- **Paths between two entities:** Checks if A can reach B.
    
- **Users with access to sensitive data:** Lists who can reach a sensitive asset.
    
- **Data exfiltration by a device:** Shows storage accounts a device can access.
    

### Refining with filters

You can narrow the graph by:

- Looking for only the shortest paths.
    
- Adding constraints like "Is vulnerable" or "Has sensitive data".
    
- Filtering by edge type (e.g., "can authenticate as" or "has role on").
    

### Workflow integration

- **Hypothesize:** Use the graph to scope the investigation visually, then pivot to KQL for evidence validation.
    
- **Privilege review:** Use graph scenarios to export identities for access reviews.
    
- **Attack surface reduction:** Find over-centralized assets (choke points) to apply better security.