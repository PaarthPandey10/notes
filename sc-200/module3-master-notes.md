### Mitigating Threats with Microsoft Purview

**The Big Picture**

While front-gate security teams and perimeter guards focus on stopping external intruders from breaking into the digital estate, the most devastating security breaches often originate from inside the organization itself. Protecting our digital corporate campus requires a dedicated **Internal Affairs** department (the compliance and data security team that monitors employee activities for policy violations and data theft) working alongside a secure, immutable **Corporate Archive** (the unified audit logging and evidence preservation system that records every digital action for legal and regulatory review). Operated through the **Microsoft Purview Portal** ([https://purview.microsoft.com/](https://purview.microsoft.com/), the unified web interface for data governance, risk, and compliance management), this internal surveillance grid tracks sensitive information, monitors employee behavior for risk patterns, secures electronic evidence for litigation, and records every file access and administrative action across the entire enterprise campus.

  

**The Core Mechanics**

  

**The Internal Mailroom Checkpoint: Data Loss Prevention (DLP) Mechanics and Alert Lifecycles**

In our campus analogy, **DLP** (data loss prevention, automated policy rules that prevent users from inappropriately sharing sensitive information) acts as an automated inspection checkpoint at every office door, mail chute, and USB port. DLP monitors user actions across Exchange email, SharePoint sites, OneDrive accounts, Teams chat and channel messages, Windows devices, cloud app instances, on-premises repositories, and Microsoft Fabric/Power BI workspaces.

  

DLP policies evaluate content against two primary indicators:

  

- **SIT** (sensitive information type, a pre-defined or custom text pattern like a credit card number, Social Security number, or health record used to spot sensitive data).
    
      
    
- **Sensitivity Labels** (digital classification tags attached to files and emails, such as "Confidential" or "Highly Confidential", that dictate access and encryption rules).
    
      
    

Plaintext

```
[ User Performs Action: Email / Download / Share ]
                        │
                        ▼
┌────────────────────────────────────────────────────────┐
│               DLP POLICY RULE EVALUATION               │
├─────────────────────────┬──────────────────────────────┤
│ Single-Event Match      │ Aggregate-Event Match        │
│ (Low volume, High risk) │ (Threshold: Count or Volume) │
└───────────┬─────────────┴──────────────┬───────────────┘
            │                            │
            └──────────────┬─────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│            DLP ALERT GENERATION LOCATIONS              │
├──────────────────────────┬─────────────────────────────┤
│ Purview Alerts Dashboard │ Defender XDR Incident Queue │
│ (Policy Tuning Focus)    │ (Cross-Domain Correlation)  │
└──────────────────────────┴─────────────────────────────┘
```

When a user's action matches a DLP policy condition, the system generates a DLP Alert. Alerts follow a strict operational configuration based on licensing tiers and thresholds:

  

- **Single-Event Alerts** generate an alert every single time a policy rule matches. These are ideal for high-sensitivity, low-volume violations (e.g., emailing a single document containing multiple credit card numbers). Single-event alerts are available under Microsoft 365 E1, F1, G1, E3, or G3 licenses.
    
      
    
- **Aggregate-Event Alerts** generate an alert only when a specific threshold is met over a defined time window—such as 10 matching events within 60 minutes or more than 1 MB of matching data. This prevents alert fatigue in high-volume environments. Aggregate-event alerts require an E5 or G5 license, or add-ons such as Office 365 Advanced Threat Protection Plan 2, Microsoft Purview Suite (formerly M365 E5 Compliance), or the Microsoft 365 eDiscovery and Audit add-on.
    
      
    

To prevent queue flooding, policy matches on the exact same item in the same location occurring within a 1-minute window (on E5/add-on licenses) or a 15-minute window (on E3/G3 licenses without add-ons) are grouped into a single alert. Administrators can also enable **User-and-Rule-Based Alert Aggregation (Preview)** under Data Loss Prevention > Settings in the Purview portal, which groups single-event alerts triggered by the same user matching the same rule within a configurable window of 15 to 60 minutes.

  

Managing DLP alerts requires explicit **RBAC** (role-based access control, a permission model that grants access rights based on job duties) roles in Microsoft Purview:

  

- **Compliance Administrator or Information Protection Admin**: Full policy configuration rights.
    
      
    
- **Security Operator, Security Reader, or Information Protection Investigator**: Alert triage permissions.
    
      
    
- **Manage Alerts Role** (with membership in DLP Compliance Management or View-Only DLP Compliance Management role groups): Required to open the DLP alert management dashboard.
    
      
    
- **Content Explorer Content Viewer**: Required to inspect the actual raw text or file content that triggered the match.
    
      
    

DLP alerts surface simultaneously in two distinct locations:

  

1. **Microsoft Purview Alerts Dashboard** (Data loss prevention > Alerts): Focused on policy tuning, rule adjustments, and compliance reporting.
    
      
    
2. **Microsoft Defender Portal** (Incidents & alerts > Incidents, filtered by Service source Microsoft Data Loss Prevention): Focused on security investigations, correlating data exfiltration with endpoint malware or compromised accounts. Defender retains incident history for 6 months.
    
      
    

The complete DLP Alert Lifecycle spans six operational phases:

  

1. **Trigger**: User activity matches a policy rule (e.g., uploading protected files to unsanctioned cloud apps or copying data to removable USB media).
    
      
    
2. **Notify**: Alerts publish to Purview and Defender dashboards; automated email notifications dispatch to admins or site owners.
    
      
    
3. **Triage**: Analysts review new alerts, assign ownership, tag cases, and evaluate user risk levels.
    
      
    
4. **Investigate**: Analysts examine evidence using **Activity Explorer** (a timeline tool showing user actions across locations), **Content Explorer** (a deep inspection tool for reviewing file text), or the **User Activity Summary** tab (which displays up to 120 days of user exfiltration behavior if data sharing is enabled between Insider Risk Management and DLP settings).
    
      
    
5. **Remediate**: Analysts execute direct response actions—such as quarantining or removing files, revoking sharing permissions, deleting emails, resetting user passwords, disabling accounts, or isolating devices. In Purview, analysts can generate a read-only event link via Actions > Copy event link to share evidence with authorized staff without granting full admin permissions.
    
      
    
6. **Tune**: Policy conditions are refined to eliminate false positives (e.g., adding exceptions for masked test data or adjusting match sensitivity thresholds).
    
      
    

To accelerate triage, organizations deploy the **DLP Triage Agent (Preview)** (an AI agent embedded in the DLP Alerts dashboard powered by Microsoft Security Copilot).

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                   DLP TRIAGE AGENT OPERATIONAL PIPELINE                │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Evaluates Active Policies (Excludes Simulation Mode)                │
│ 2. Scans Data Items (Analyzes Top 10 Relevant Files per Alert < 2MB)   │
│ 3. Assesses Risk Factors: Content Risk + Exfiltration Risk + Policy Risk│
│ 4. Categorizes Queue: [Needs Attention] [Less Urgent] [Not Categorized]│
└────────────────────────────────────────────────────────────────────────┘
```

The DLP Triage Agent operates under specific technical constraints and requirements:

  

- **Prerequisites**: Provisioned **SCU** (security compute unit, the metric used to measure and bill for AI computing power) capacity, the Purview plugin enabled in Security Copilot, and an assigned agent service identity.
    
      
    
- **Required Roles**: Information Protection Analyst or Investigator, Purview Agent Analysis, and Security Copilot Contributor (plus Data Classification Content Viewer and Content Downloader for device alerts).
    
      
    
- **Execution Modes**: Can run Automatically on a scheduled timeframe (looking back 24 hours up to 30 days) or Manually on individual alerts.
    
      
    
- **Prioritization Factors**:
    
      
    - **Content Risk**: Evaluates sensitive information types, trainable classifiers, and sensitivity labels.
        
          
        
    - **Exfiltration Risk**: Evaluates external sharing, cloud uploads, or bulk downloads.
        
          
        
    - **Policy Risk**: Evaluates rule mode, blocking actions, or label downgrade attempts.
        
          
        
- **Categorization Output**: Places evaluated alerts into three distinct categories: Needs attention (highest risk, reviewed first), Less urgent, and Not categorized (unsupported alert types).
    
      
    
- **Limitations**: Analyzes alerts from Exchange, SharePoint, OneDrive, Teams, and Endpoint devices (requires evidence collection enabled for file activities on devices). Does not analyze files larger than 2 MB, processes only the top 10 most relevant files when an alert contains more than 10 items, excludes policies running in simulation mode, and does not evaluate custom SITs or custom trainable classifiers.
    
      
    

Resolving a DLP alert or incident sets its status to Resolved, indicating the specific instance was handled; it does not suppress future alerts if the user performs the same action again.

  

While DLP catches specific policy tripwires, Internal Affairs must also watch for broader behavioral patterns that signal a rogue employee.

  

Plaintext

```
1. THE INTERNAL MAILROOM CHECKPOINT: DLP & ALERTS
├── Microsoft Purview Portal (the unified web interface for data governance, risk, and compliance management)
├── Data Loss Prevention (DLP, automated policy rules that prevent users from inappropriately sharing sensitive information)
│   └── Primary Indicators
│       ├── SIT (sensitive information type, a pre-defined or custom text pattern like a credit card number used to spot sensitive data)
│       └── Sensitivity Labels (digital classification tags attached to files and emails that dictate access and encryption rules)
├── Alert Generation Mechanics
│   ├── Single-Event Alerts (generate an alert every single time a policy rule matches; low volume/high risk)
│   ├── Aggregate-Event Alerts (generate an alert only when a specific threshold is met over a defined time window)
│   └── User-and-Rule-Based Alert Aggregation (groups single-event alerts triggered by the same user matching the same rule)
├── Role-Based Access Control (RBAC, a permission model that grants access rights based on job duties)
│   ├── Manage Alerts Role (Required to open the DLP alert management dashboard)
│   └── Content Explorer Content Viewer (Required to inspect the actual raw text or file content)
├── DLP Alert Lifecycle (6 Phases)
│   ├── 1. Trigger
│   ├── 2. Notify (Publishes to Purview Alerts Dashboard and Defender XDR Incident Queue)
│   ├── 3. Triage
│   ├── 4. Investigate
│   │   ├── Activity Explorer (a timeline tool showing user actions across locations)
│   │   ├── Content Explorer (a deep inspection tool for reviewing file text)
│   │   └── User Activity Summary (displays up to 120 days of user exfiltration behavior)
│   ├── 5. Remediate
│   └── 6. Tune
└── AI Automation
    └── DLP Triage Agent (Preview) (an AI agent embedded in the DLP Alerts dashboard powered by Microsoft Security Copilot)
        ├── Prerequisites: SCU (security compute unit, the metric used to measure and bill for AI computing power) capacity
        ├── Prioritization Factors: Content Risk, Exfiltration Risk, Policy Risk
        └── Categorization Output: Needs attention, Less urgent, Not categorized
```

**Watching Internal Affairs: Insider Risk Management Mechanics and Case Handling**

In our campus analogy, Insider Risk Management acts as an internal detective unit monitoring employee behavior patterns over time. Rather than evaluating isolated events, it correlates cross-domain signals to identify intentional IP theft, disgruntled employee exfiltration, or severe security policy abuse.

  

To protect employee privacy, the system enables **Pseudonymization** by default, masking user names with anonymous identifiers across dashboards until an investigator explicitly elevates the case.

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│               INSIDER RISK ALERT GENERATION PROCESS                    │
├──────────────────┬──────────────────┬──────────────────┬───────────────┤
│ 1. SETTINGS      │ 2. POLICY        │ 3. TRIGGERING    │ 4. SCORING &  │
│    CONFIGURED    │    CREATED       │    EVENT         │    ALERTS     │
├──────────────────┼──────────────────┼──────────────────┼───────────────┤
│ Select risk      │ Assign users and │ HR resignation   │ Risk score    │
│ indicators,      │ select policy    │ signal, account  │ calculated;   │
│ privacy, and     │ template (e.g.,  │ deletion, or     │ alert fires   │
│ domain lists.    │ Data Theft).     │ DLP violation.   │ if threshold  │
│                  │                  │                  │ exceeded.     │
└──────────────────┴──────────────────┴──────────────────┴───────────────┘
```

Alert generation follows a strict 5-step process:

  

1. **Settings Configured**: Global options are set, including monitored risk indicators, sensitive domain allow/block lists, and privacy preferences.
    
      
    
2. **Policy Created**: Administrators define target user populations and select specialized Policy Templates (e.g., Data theft by departing employees, General data leaks, or Risky browser usage).
    
      
    
3. **Triggering Event Occurs**: An external or system event activates active monitoring for a specific user (e.g., an HR connector sending a resignation notice, an account deletion date, or a high-severity DLP violation).
    
      
    
4. **User Activity Evaluated and Scored**: The engine calculates cumulative risk scores based on activity frequency, severity thresholds, and user history.
    
      
    
5. **Alert Generated**: An alert fires into the queue if the user's aggregated score breaches the policy threshold.
    
      
    

Investigators balance alert volume using specific tuning controls:

  

- **To Increase Alert Volume** (if signals are missed): Enable additional risk indicators in Policy indicators settings, broaden user scope, lower trigger thresholds, lower indicator thresholds, or adjust the Intelligent Detections slider toward "More alerts" (Settings > Intelligent detections).
    
      
    
- **To Decrease Alert Volume** (if overwhelmed): Enable Analytics (Settings > Analytics) to evaluate baseline risk, adjust thresholds using real-time recommendations, narrow user scope to high-risk roles, enable inline alert customization during triage, or perform **Bulk Dismissal** (dismissing up to 400 low-priority alerts at once from the command bar).
    
      
    
- **Global Controls**: Configure global file exclusions, establish Detection Groups (applying distinct policies to different departments), create indicator variants, or adjust policy evaluation timeframes.
    
      
    

The Alerts Dashboard displays active cases, highlighting high-priority items using **Spotlight** (a feature that uses rule-based logic to highlight critical alerts based on activity type, tags, and cross-organizational risk patterns). Untriaged alerts in a "Needs review" state are retained for 120 days, after which they are automatically deleted unless attached to an active case. Organizations can maintain up to 100 active cases simultaneously.

  

Selecting an alert opens three specialized investigation tabs:

  

- **All Risk Factors Tab**: Displays top exfiltration activities, cumulative exfiltration trends, recognized activity sequences, priority content interactions, unallowed domain transfers, and high-impact user status. Includes the Content Detected section linking directly to file metadata.
    
      
    
- **Activity Explorer Tab**: Provides a granular event timeline. Events can be filtered by activity scope, risk factor, and review status. File types normally excluded from risk scoring (e.g., .png files) display a score of 0 but remain visible inside activity sequences if used during obfuscation attempts.
    
      
    
- **User Activity Tab**: Renders an interactive, color-coded **Scatter Plot Timeline** spanning 1, 3, or 6 months. Vertical height represents risk score (0 to 100), horizontal axis represents time, colored bubbles represent distinct risk events, and connecting lines display multi-step Risk Sequences.
    
      
    

Plaintext

```
[ Scatter Plot Timeline: 1 / 3 / 6 Month Window ]
  Y-Axis: Risk Score (0 - 100)
  X-Axis: Event Date
  Bubbles = Scored Events
  Connecting Lines = Multi-Step Risk Sequences
```

Triage can be automated using the **Insider Risk Triage Agent (Preview)**. Powered by Security Copilot, it evaluates user context (adaptive protection levels, HR status, role changes), policy types, and risk indicators to categorize alerts into Needs attention, Less urgent, and Not categorized. Agent configurations run under the security context of the saving user and must be renewed every 90 days.

  

To extend investigations to SOC teams, administrators turn on Share user risk details with other security solutions (Settings > Insider Risk Management > Data sharing). Alerts sync bi-directionally between Purview and Defender XDR within approximately 30 minutes:

  

- Defender New / In progress status maps to Purview Needs review.
    
      
    
- Defender Resolved (True positive) maps to Purview Confirmed.
    
      
    
- Defender Resolved (Informational / False positive) maps to Purview Dismissed.
    
      
    

In Defender XDR, analysts query raw insider risk telemetry using Advanced Hunting across four specialized schema tables: **AlertInfo**, **AlertEvidence**, **DataSecurityBehaviors**, and **DataSecurityEvents**.

  

When an alert warrants formal escalation, an analyst selects Confirm all alerts & create case. A **Case** consolidates all historical alerts for a single user into a permanent investigation workspace containing eight tabs: Case overview, Alerts, User activity, Activity explorer, Forensic evidence (captured screen recordings), Content explorer, Case notes, and Contributors.

  

From the Case toolbar, investigators execute five primary actions:

  

- **Send Email Notice**: Sends an official policy notice from a template to the user (logged in Case Notes; does not close the case).
    
      
    
- **Escalate for Investigation**: Escalates the case directly into an eDiscovery (Premium) case for legal hold placement.
    
      
    
- **Run Power Automate Flows**: Triggers automated workflows (e.g., notifying managers, creating ServiceNow tickets, or requesting HR input).
    
      
    
- **Create/View Teams Team**: Automatically provisions a private Microsoft Team for investigator collaboration (if enabled in Settings > Insider Risk Management > Microsoft Teams; archived automatically when the case is resolved).
    
      
    
- **Resolve Case**: Closes the case with a final classification of Benign (accidental/low-risk) or Confirmed policy violation.
    
      
    

Once an internal investigation shifts toward formal legal proceedings, investigators must turn to the corporate archive to secure digital evidence.

  

Plaintext

```
[ Triage Alert ] ──► [ Confirm & Create Case ] ──► [ Case Toolbar Actions ]
                                                           │
        ┌──────────────────────────────────────────────────┼──────────────────────────────────────────────────┐
        ▼                                                  ▼                                                  ▼
[ Send Email Notice ]                             [ Escalate to eDiscovery ]                         [ Resolve Case ]
(Policy Warning)                                  (Legal Hold & Review Sets)                         (Benign vs Violation)
```

Plaintext

```
2. WATCHING INTERNAL AFFAIRS: INSIDER RISK MANAGEMENT
├── Core Concepts
│   └── Pseudonymization (masking user names with anonymous identifiers across dashboards to protect privacy)
├── Alert Generation Process
│   ├── 1. Settings Configured
│   ├── 2. Policy Created (Data theft, Data leaks, Risky browser usage)
│   ├── 3. Triggering Event Occurs
│   ├── 4. User Activity Evaluated and Scored
│   └── 5. Alert Generated
├── Tuning Controls
│   ├── Increase Volume (Broaden scope, lower thresholds, Intelligent Detections slider)
│   └── Decrease Volume (Analytics, Bulk Dismissal of up to 400 alerts, real-time recommendations)
├── Alerts Dashboard & Investigation
│   ├── Spotlight (a feature that uses rule-based logic to highlight critical alerts based on activity type, tags, and risk patterns)
│   ├── All Risk Factors Tab (Exfiltration activities, recognized sequences)
│   ├── Activity Explorer Tab (Granular event timeline)
│   ├── User Activity Tab
│   │   └── Scatter Plot Timeline (interactive, color-coded map spanning 1, 3, or 6 months where bubbles = events and lines = Risk Sequences)
│   └── Insider Risk Triage Agent (Preview) (AI agent to evaluate user context, policy types, and categorize risk)
├── SOC Integration (Defender XDR)
│   └── Advanced Hunting Schema Tables (AlertInfo, AlertEvidence, DataSecurityBehaviors, DataSecurityEvents)
└── Case Handling
    ├── Case (consolidates all historical alerts for a single user into a permanent investigation workspace)
    └── Case Toolbar Actions
        ├── Send Email Notice (Official policy notice)
        ├── Escalate for Investigation (Escalates directly to eDiscovery)
        ├── Run Power Automate Flows (Triggers automated workflows)
        ├── Create/View Teams Team (Provisions a private collaboration channel)
        └── Resolve Case (Closes case as Benign or Confirmed policy violation)
```

**Searching the Corporate Archive: Microsoft Purview eDiscovery and Legal Holds**

In our campus analogy, **eDiscovery** (electronic discovery, a secure system used to locate, preserve, and export digital evidence across M365 services) serves as the legal vault within the corporate archive. It supports internal HR investigations, legal discovery, regulatory audits, and data subject requests across Exchange Online, SharePoint Online, OneDrive for Business, Teams (1:1 chats, group chats, channel messages, and shared files), M365 Groups, and Viva Engage.

  

Access to eDiscovery is strictly isolated. Global Administrators and Compliance Administrators have zero access to search user content or open cases unless explicitly assigned to eDiscovery role groups under Settings > Roles and Scopes > Role groups:

  

- **eDiscovery Manager**: Can create and manage cases, execute content searches, place legal holds, and export results.
    
      
    
- **eDiscovery Administrator**: Possesses full Manager rights plus the ability to manage role assignments and view all active cases tenant-wide.
    
      
    

Every search must be attached to a **Case** (an auditable workspace that tracks searches, holds, and export logs). Users with the eDiscovery Manager role cannot view or access a case unless they are explicitly added as a case member.

  

Administrators can create searches through two portal methods (Solutions > eDiscovery > Cases):

  

- **Create Search Directly**: Selecting the arrow next to + Create case and choosing Create search creates both a new case and an attached search simultaneously.
    
      
    
- **Create Search Through a Case**: Selecting + Create case provisions the case workspace first, after which analysts navigate to the Searches tab and select Create search.
    
      
    

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                   THE 4 PHASES OF AN eDISCOVERY SEARCH                 │
├───────────────────┬───────────────────┬────────────────┬───────────────┤
│ PHASE 1: CRITERIA │ PHASE 2: SOURCES  │ PHASE 3: QUERY │ PHASE 4: RUN  │
├───────────────────┼───────────────────┼────────────────┼───────────────┤
│ Define search name│ Select specific   │ Build KeyQL via│ Execute query;│
│ and initial scope.│ users, groups,    │ Condition      │ evaluate via  │
│                   │ SharePoint/OneDrive│ Builder, AI    │ Statistics or │
│                   │ URLs, or tenant.  │ Copilot, or    │ Random        │
│                   │                   │ File Upload.   │ Sampling.     │
└───────────────────┴───────────────────┴────────────────┴───────────────┘
```

Conducting an eDiscovery search follows a 4-phase workflow:

  

1. **Define Search Criteria**: Enter a search name, description, and initial case parameters.
    
      
    
2. **Identify Data Sources**: Select targeted people, groups, SharePoint site URLs, or OneDrive account URLs, or select Add tenant-wide sources. Toggle Exclude inactive users to restrict scope.
    
      
    
3. **Build the Query**:
    
      
    

- **Condition Builder**: Combines filters using **KeyQL** (Keyword Query Language, an advanced search syntax), Date ranges, Subject/Title terms, Participants, and Message Kind (Email, Chat, Teams) joined by AND logic.
    
      
    
- **Draft Query with Copilot (Preview)**: Converts natural language prompts (e.g., "Find Teams messages sent by Alex Wilber between March 1 and March 15 containing budget attachments") into validated KeyQL syntax.
    
      
    
- **Search by File (Preview)**: Uploads plain text .txt files (to find similar content) or audit log .csv files (to locate reference content) up to 10 MB per file. Using Search by File disables manual KQL and condition builder options, using uploaded file patterns as the search baseline.
    
      
    

4. **Run and Review Results**: Executes the search and evaluates output using Statistics (item counts, total byte size, location breakdown) or Sample (previewing a random sample of matched items).
    
      
    

Once search results are verified, analysts select Export from the Searches tab. In the export configuration pane, analysts define parameters across workloads:

  

- **Scope**: Choose between Indexed items matching query, Indexed and partially indexed items (unindexed files due to encryption or formatting errors), or Only partially indexed items.
    
      
    
- **SharePoint and OneDrive Items**: Select document version ranges (Latest version only, Recent 10 or 100, or All versions), collect subfolder items, or include list attachments.
    
      
    
- **Mailboxes and Teams Messages**: Select Organize conversations into HTML transcript (threads Teams chat messages into readable HTML transcripts), select Include Teams and Viva Engage conversations (collects up to 12 hours of surrounding contextual conversation), and include cloud attachments (access links).
    
      
    
- **Packaging and Format**: Export mailbox content as PST (Personal Storage Table, a standard Outlook data file) or MSG (individual message file) formats, organize data by source location, and enable Condense paths to fit within 259 characters to prevent long file path errors during download.
    
      
    

Export jobs process in the background and are monitored via the Process Manager. Once complete, completed packages are downloaded from the Exports tab. Packages include raw message files, PSTs, metadata summary reports, and cryptographic **Hashes** (unique mathematical fingerprints) that guarantee a verifiable **Chain of Custody** for courtroom presentation.

  

To ensure historical evidence is not altered or deleted while an investigation is underway, Internal Affairs must review the security camera tapes that record every system event.

  

Plaintext

```
3. SEARCHING THE CORPORATE ARCHIVE: eDISCOVERY & LEGAL HOLDS
├── eDiscovery (electronic discovery, a secure system used to locate, preserve, and export digital evidence across M365 services)
├── Role-Based Isolation (Global Admins do not inherently have access)
│   ├── eDiscovery Manager (Can create/manage cases, content searches, legal holds, and exports)
│   └── eDiscovery Administrator (Full Manager rights plus ability to view all active cases tenant-wide)
├── Core Components
│   └── Case (an auditable workspace that tracks searches, holds, and export logs)
├── The 4 Phases of an eDiscovery Search
│   ├── 1. Define Search Criteria (Name and initial scope)
│   ├── 2. Identify Data Sources (Users, groups, site URLs, tenant-wide)
│   ├── 3. Build the Query
│   │   ├── KeyQL (Keyword Query Language, an advanced search syntax)
│   │   ├── Draft Query with Copilot (Preview) (Converts natural language prompts into validated KeyQL)
│   │   └── Search by File (Preview) (Uploads .txt or .csv files to use as baseline matching patterns)
│   └── 4. Run and Review Results
│       ├── Statistics (item counts, byte sizes)
│       └── Sample (random sample of matched items)
└── Export Configuration & Evidentiary Handling
    ├── Scope Options (Indexed items, partially indexed/unindexed files)
    ├── Output Formats (PST, MSG, HTML transcripts for Teams)
    ├── Condense paths (prevents long file path errors exceeding 259 characters)
    └── Evidence Integrity
        ├── Hashes (unique mathematical fingerprints for downloaded data)
        └── Chain of Custody (guaranteed verifiable evidence presentation for legal settings)
```

**Checking the Security Camera Tapes: Microsoft Purview Audit (Standard vs. Premium)**

In our campus analogy, Microsoft Purview Audit represents the central security camera recording system. It continuously records user and administrator activities across Exchange, SharePoint, OneDrive, Teams, Entra ID, Microsoft Copilot, and third-party AI applications into a single, unified audit log.

  

Auditing is enabled by default for all M365 tenants, retaining records for a default window of 180 days. Administrators verify auditing status in Exchange Online PowerShell using `Get-AdminAuditLogConfig | Format-List UnifiedAuditLogIngestionEnabled`. If set to False, auditing is activated via `Set-AdminAuditLogConfig -UnifiedAuditLogIngestionEnabled $true` (or selecting the banner in the Purview portal under Audit). Ingestion enablement takes up to 60 minutes to propagate, and turning on auditing does not backfill past activity.

  

Searching audit logs requires explicit RBAC permissions:

  

- **Audit Reader**: Allows searching and exporting audit logs.
    
      
    
- **Audit Manager**: Allows searching, exporting, and configuring audit settings.
    
      
    
- **Administrative Units (AUs)**: Scopes audit visibility. Unrestricted Admins (no AU assigned) can search all logs tenant-wide, including system/service principal events and special operations (e.g., AIP Discover, Endpoint DLP file events, Set-Mailbox, Forms ViewRuntimeForm). Restricted Admins (assigned to specific AUs) can search logs only for users inside their designated admin units.
    
      
    

Microsoft Purview Audit operates across two feature tiers:

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                   PURVIEW AUDIT TIER COMPARISON                        │
├──────────────────────────────┬─────────────────────────────────────────┤
│ AUDIT (STANDARD)             │ AUDIT (PREMIUM)                         │
├──────────────────────────────┼─────────────────────────────────────────┤
│• Included in M365 E3/G3      │• Included in M365 E5/G5/E5 Compliance   │
│• 180-Day Retention           │• 1-Year Default Retention for Core Apps │
│• Core Workload Events        │• Custom Retention Policies up to 10 Yrs │
│• Max 50,000 Row Portal Export│• High-Value Events (MailItemsAccessed)  │
│                              │• Max 1,000,000 Row Portal Export        │
│                              │• Higher Management Activity API Limits  │
└──────────────────────────────┴─────────────────────────────────────────┘
```

To access Audit (Premium) high-value events—such as **MailItemsAccessed** (message-level open logs), Send, SearchQueryInitiatedExchange, and SearchQueryInitiatedSharePoint—administrators must explicitly enable the Microsoft 365 Advanced Auditing service plan per user in the M365 Admin Center (Users > Active users > Licenses & apps > Apps). License propagation takes 15 to 30 minutes, and full event logging can take up to 24 hours to initiate. Records exist strictly from the per-user enablement date forward. If mailbox audit configurations were previously customized on legacy mailboxes, search query events must be manually restored in Exchange Online PowerShell using `Set-Mailbox <user> -AuditOwner @{Add="SearchQueryInitiated"}`.

  

**Custom Audit Log Retention Policies (Audit Premium)**

Audit (Premium) allows administrators to build custom retention policies storing specific audit records for up to 10 years (retaining logs beyond 1 year requires the 10-Year Audit Log Retention add-on license assigned per user).

Custom retention policy rules:

  

- **Role Requirement**: Requires the Organization Configuration role in the Purview portal.
    
      
    
- **Policy Limit**: Maximum of 50 custom retention policies per organization.
    
      
    
- **Priority Architecture**: Custom policies evaluate based on numeric priority where lower numbers have higher priority (e.g., Priority 5 overrides Priority 10). Custom policies override the unmodifiable 1-year default policy.
    
      
    
- **Non-User Data Capping**: Audit logs generated by non-user entities (service principals, system events, app permissions) are fixed at a 1-year maximum retention limit regardless of license tier or custom retention policy settings.
    
      
    

Custom retention policies are configured in the portal (Solutions > Audit > Audit retention policies) or via Security & Compliance PowerShell:

`New-UnifiedAuditLogRetentionPolicy -Name "Teams 10-Yr Retention" -Description "Retain Teams logs for 10 years" -RecordTypes MicrosoftTeams -RetentionDuration TenYears -Priority 100`

  

**Executing Audit Searches**

Searches are initiated in the portal (Solutions > Audit). Users can run up to 10 concurrent search jobs simultaneously (limited to a maximum of 1 unfiltered search job). Broad queries in large tenants process in the background and can take up to 48 hours to complete. Completed search job definitions and results remain accessible in the portal for 30 days.

  

Searches are scoped using core parameters: Date Range (interpreted strictly in **UTC** [Coordinated Universal Time]; local times must be converted to UTC to avoid missing events), Keyword, Admin Units, Activity types, Record types, Users, File/Folder/Site URLs, and Workloads. Telemetry latency for core services (Exchange, SharePoint, OneDrive, Teams) averages 60 to 90 minutes.

  

If an audit search returns zero results, analysts follow a 6-step troubleshooting checklist:

  

1. Verify auditing is enabled tenant-wide (`Get-AdminAuditLogConfig`).
    
      
    
2. Confirm the date range falls within the retention window (180 days for Standard vs. 1 year for Premium).
    
      
    
3. Confirm event indexing latency has elapsed (>90 minutes).
    
      
    
4. Verify per-user Advanced Auditing was active during the targeted timeframe for Premium events.
    
      
    
5. Confirm administrative unit role scoping includes the target user.
    
      
    
6. Verify date parameters were entered in UTC.
    
      
    

**Deep Forensic Investigations with MailItemsAccessed**

To determine whether an employee opened a specific email, investigators analyze the MailItemsAccessed event from Audit (Premium). Exchange logs this event using two distinct access types:

  

- **Sync Access**: Logs a single audit event when an email client (e.g., desktop Outlook) downloads a bulk folder or mailbox chunk.
    
      
    
- **Bind Access**: Logs an individual audit event every time a user opens or previews a specific, single email message.
    
      
    

To preserve system performance, Exchange Online enforces **Throttling** on MailItemsAccessed. If a mailbox generates more than 1,000 bind events within 24 hours, logging for MailItemsAccessed bind events pauses on that mailbox (affecting less than 1% of mailboxes globally). Throttling does not affect sync operations or other audit events. Missing bind events during a throttled window do not backfill; investigators must reconstruct gaps using Exchange message traces, Entra sign-in logs, or SearchQueryInitiatedExchange logs.

  

To prevent log clutter, Audit (Premium) enforces **Deduplication**, filtering out duplicate MailItemsAccessed records for the exact same message occurring within a 1-hour window, unless contextual properties change (e.g., ClientIPAddress, SessionId, or MailAccessType).

  

Analysts query MailItemsAccessed events in Exchange Online PowerShell using `Search-UnifiedAuditLog`:

`$log = Search-UnifiedAuditLog -StartDate "2026-01-06" -EndDate "2026-01-20" -UserIds "physician@contoso.com" -Operations MailItemsAccessed -ResultSize 1000`

  

Analysts filter the results object to check throttling state and isolate bind vs. sync operations:

  

PowerShell

```
# Check for Throttling Gaps
$log | Where-Object {$_.AuditData -like '*"IsThrottled","Value":"True"*'} | Format-List

# Isolate Specific Opened Messages (Bind Access)
$log | Where-Object {$_.AuditData -like '*"MailAccessType","Value":"Bind"*'} | Format-List
```

With message-level email access verified, defenders must extend their audit searches to cover employee interactions with generative AI tools.

  

Plaintext

```
[ Mailbox Activity Occurs ]
             │
             ▼
┌────────────────────────────────────────────────────────┐
│            EVALUATE MailItemsAccessed ACCESS           │
├──────────────────────────┬─────────────────────────────┤
│ Sync Access              │ Bind Access                 │
│ (Bulk Folder Downloads)  │ (Individual Message Opened) │
└────────────┬─────────────┴──────────────┬──────────────┘
             │                            │
             │                            ▼
             │              [ Check 24-Hr Bind Limit ]
             │              (> 1,000 Events = Throttled)
             │                            │
             ▼                            ▼
[ 1-Hr Deduplication Check ] ◄─(Unthrottled Logs)
(Logs unique events or context changes)
```

Plaintext

```
4. CHECKING THE SECURITY CAMERA TAPES: MICROSOFT PURVIEW AUDIT
├── Core Concepts & Enablement
│   ├── Administrative Units (AUs) (Scopes audit visibility for Restricted Admins)
│   └── UTC (Coordinated Universal Time, the mandatory format for all audit search date ranges)
├── Audit Tier Comparison
│   ├── Audit (Standard) (M365 E3/G3, 180-Day default retention, Max 50K Row Export)
│   └── Audit (Premium) (M365 E5/G5, 1-Year default retention, Max 1M Row Export)
│       └── High-Value Events (Requires M365 Advanced Auditing service plan per user)
├── Custom Audit Log Retention Policies (Premium Only)
│   ├── Rules: Max 50 policies, requires Organization Configuration role
│   ├── Priority Architecture (Lower numbers have higher priority, e.g., Priority 5 overrides Priority 10)
│   └── Non-User Data Capping (Service principal/system logs are permanently capped at 1 year max retention)
├── Search Troubleshooting Checklist (6 Steps)
│   └── (1) Enablement, (2) Retention window, (3) Indexing latency, (4) Premium license, (5) AU scoping, (6) UTC dates
└── Deep Forensics: MailItemsAccessed (Premium)
    ├── Access Types
    │   ├── Sync Access (Logs a single audit event when an email client downloads a bulk folder/chunk)
    │   └── Bind Access (Logs an individual audit event every time a user opens or previews a specific message)
    ├── Throttling (Pauses bind logging if a mailbox generates >1,000 bind events in 24 hours)
    └── Deduplication (Filters out duplicate MailItemsAccessed records for the exact same message within a 1-hour window)
```

**AI Surveillance: Auditing Copilot and Third-Party AI Applications**

As employees leverage artificial intelligence to summarize reports and write code, Internal Affairs must inspect AI prompts and data interactions. Microsoft Purview Audit captures rich metadata for AI interactions: user identity, timestamps, client app host, referenced files (AccessedResources including SiteUrl, Name, Type, and SensitivityLabelId), and the **JailbreakDetected** safety flag inside Messages. Audit captures interaction metadata; it does not store raw prompt or response text (raw text content is reviewed using Purview Data Security Posture Management, Communication Compliance, or eDiscovery).

  

AI interactions are categorized into three distinct Record Types:

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        AI AUDIT RECORD TYPES                           │
├───────────────────────┬───────────────────────┬────────────────────────┤
│ CopilotInteraction    │ ConnectedAIApp        │ AIAppInteraction       │
│                       │ Interaction           │                        │
├───────────────────────┼───────────────────────┼────────────────────────┤
│ First-party M365      │ Custom Copilot Studio │ Non-Microsoft AI       │
│ Copilot, Copilot Chat,│ agents & Entra-       │ web apps (e.g., public │
│ Security Copilot,     │ registered AI apps.   │ ChatGPT).              │
│ and Fabric Copilot.   │                       │                        │
├───────────────────────┼───────────────────────┼────────────────────────┤
│ Ingests automatically │ Requires agent/app    │ Requires Purview       │
│ when auditing is on.  │ onboarding through    │ Browser Extension +    │
│                       │ DSPM!                 │ DLP AI policy +        │
│                       │                       │ Pay-as-You-Go Billing! │
└───────────────────────┴───────────────────────┴────────────────────────┘
```

1. **CopilotInteraction** (Workload: Copilot): Logs user interactions with first-party Microsoft Copilot experiences (M365 Copilot, Copilot Chat, Security Copilot, Copilot in Fabric). Ingests automatically as soon as standard user auditing is active.
    
      
    
2. **ConnectedAIAppInteraction** (Workload: ConnectedAIApp): Logs interactions with custom Copilots built in Microsoft Copilot Studio and non-Microsoft AI apps registered in Microsoft Entra ID. _Critical Dependency_: Records do not log until the specific agent or app is formally onboarded through Data Security Posture Management (DSPM).
    
      
    
3. **AIAppInteraction** (Workload: AIApp): Logs employee usage of third-party, non-registered AI web apps accessed via web browsers. _Critical Dependencies_: Requires the Microsoft Purview Browser Extension deployed on endpoint devices, an active DLP policy inspecting AI app traffic, and an active Azure Subscription linked to Purview via Pay-as-You-Go Billing.
    
      
    

Administrative actions modifying AI settings are logged under specific operation names: UpdateTenantSettings, CreatePlugin, DeletePlugin, and EnablePromptBook.

  

When reviewing AI audit records, analysts inspect specific properties:

  

- **AppIdentity**: Displays the full workload format, such as Copilot.MicrosoftCopilot.Microsoft365Copilot or Copilot.Security.SecurityCopilot.
    
      
    
- **AppHost**: Identifies the physical client interface used by the employee: BizChat (M365 Copilot Chat / Teams), Bing (Edge browser / mobile), Office (office.com), or specific desktop apps (Word, Excel, PowerPoint, Stream).
    
      
    
- **AccessedResources**: Lists every document or site Copilot read to formulate its response, including sensitivity label IDs.
    
      
    
- **JailbreakDetected**: A Boolean flag inside the Messages JSON structure that flags user prompts attempting to bypass AI safety guardrails.
    
      
    

To transform raw audit logs into actionable reports for executive leadership, analysts parse exported data using data analysis tools.

  

Plaintext

```
5. AI SURVEILLANCE: AUDITING COPILOT & AI APPS
├── AI Audit Capabilities
│   └── Captures metadata (identity, timestamps, accessed files); does NOT capture raw prompt/response text
├── AI Audit Record Types
│   ├── CopilotInteraction (Workload: Copilot)
│   │   └── First-party Microsoft experiences (M365 Copilot, Security Copilot). Ingests automatically.
│   ├── ConnectedAIAppInteraction (Workload: ConnectedAIApp)
│   │   └── Custom Copilot Studio agents & Entra-registered AI apps. Requires formal DSPM onboarding to log!
│   └── AIAppInteraction (Workload: AIApp)
│       └── Third-party non-Microsoft AI web apps (e.g., ChatGPT). Requires Purview Browser Extension, DLP AI policy, and Pay-as-You-Go Billing.
├── Administrative Logging Operations
│   └── UpdateTenantSettings, CreatePlugin, DeletePlugin, EnablePromptBook
└── Key AI Audit Properties
    ├── AppIdentity (e.g., Copilot.Security.SecurityCopilot)
    ├── AppHost (Physical client interface: BizChat, Bing, Office apps)
    ├── AccessedResources (Lists every document/site read to formulate response)
    └── JailbreakDetected (A Boolean flag inside the Messages JSON structure flagging user attempts to bypass AI safety guardrails)
```

**Parsing Evidence: Log Transformations, Power Query, and Analysis**

Exporting search results from the Purview portal generates a CSV file containing an AuditData column. This column contains nested **JSON** (JavaScript Object Notation, a structured text format for storing complex data) text that is difficult to read in raw spreadsheet formats.

  

Plaintext

```
[ Portal / PowerShell Export ] ──► [ CSV File with Nested JSON 'AuditData' ]
                                                      │
                                                      ▼
[ Excel Power Query Editor ] ◄── [ Open File via "Transform Data" ]
            │
            ▼
[ Right-Click 'AuditData' Column ──► Transform ──► JSON ]
            │
            ▼
[ Expand JSON Properties: SiteUrl, AccessedResources, SensitivityLabelId ]
            │
            ▼
[ Close & Load ──► Flattened, Sortable Excel Report ]
```

Analysts transform raw CSV exports into structured reports using **Excel Power Query Editor**:

  

1. Open the exported CSV file in Microsoft Excel and select Transform Data to launch Power Query Editor.
    
      
    
2. Locate and right-click the AuditData column header, select Transform, and choose JSON.
    
      
    
3. Select the Expand icon on the column header to promote JSON keys into individual, structured table columns (e.g., SiteUrl, AccessedResources, SensitivityLabelId, ClientIPAddress).
    
      
    
4. Deselect unneeded properties and click Close & Load to output a flattened, sortable Excel table.
    
      
    

To combine multi-source investigations, analysts execute a **Table Join** in Excel or Power Query:

  

1. Filter the parsed Copilot audit table to isolate rows where AccessedResources contains sensitive document IDs or targeted sensitivity labels.
    
      
    
2. Filter the parsed MailItemsAccessed audit table for the same user accounts and time window.
    
      
    
3. Join the two tables on User Principal Name, resource identifier, and timestamp (allowing a loose time window match). Rows where both tables match confirm that the exact same sensitive file was accessed both directly via mailbox/SharePoint and through an AI Copilot prompt.
    
      
    

For large-scale, automated log extraction, analysts execute `Search-UnifiedAuditLog` in Exchange Online PowerShell, exporting results directly to CSV:

`$auditlog = Search-UnifiedAuditLog -StartDate "2026-06-01" -EndDate "2026-06-30" -RecordType SharePointSharingOperation`

`$auditlog | Select-Object CreationDate, UserIds, RecordType, AuditData | Export-Csv -Path "C:\AuditLogs\SharePointAudit.csv" -NoTypeInformation -Encoding UTF8`

  

To prevent character corruption when exporting user names, file paths, or Copilot prompts containing non-ASCII characters, PowerShell commands must include the `-Encoding UTF8` (or `-Encoding UTF8BOM`) parameter.

  

When querying broad date ranges, `Search-UnifiedAuditLog` caps output at 5,000 rows per call (returning 100 rows by default). To extract datasets exceeding 5,000 rows without silent truncation, analysts implement result pagination using the `-SessionId` and `-SessionCommand ReturnLargeSet` parameters.

  

Plaintext

```
6. PARSING EVIDENCE: LOG TRANSFORMATIONS & POWER QUERY
├── Log Formats
│   └── JSON (JavaScript Object Notation, a structured text format for storing complex data)
├── Export Transformation Workflow
│   └── Excel Power Query Editor (Extracts nested JSON out of the 'AuditData' column)
│       ├── 1. Transform Data
│       ├── 2. Right-click 'AuditData' -> Transform -> JSON
│       ├── 3. Expand Properties (SiteUrl, AccessedResources, etc.)
│       └── 4. Close & Load to flattened table
├── Multi-Source Correlation
│   └── Table Join (Combines Copilot audit table with MailItemsAccessed table to confirm multi-vector file access)
└── PowerShell Extraction Best Practices (Search-UnifiedAuditLog)
    ├── -Encoding UTF8 (Parameter used to prevent character corruption for non-ASCII characters on export)
    └── Pagination (Using -SessionId and -SessionCommand ReturnLargeSet to extract large datasets exceeding 5,000 rows)
```

**Connecting the Dots**

To protect our digital campus from internal threats and data leaks, every security mechanism in Microsoft Purview fits together into a unified compliance and investigative ecosystem.

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE INTERNAL SURVEILLANCE GRID                            │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Mailroom Checkpoint   │   │ Internal Affairs       │   │ AI & App Surveillance   │  │
│  │ (Data Loss Prevention)│──►│ (Insider Risk Mgmt)    │──►│ (Copilot / AI Auditing) │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Formal Legal Vault    │──►│ SECURITY CAMERA TAPES  │◄──│ Evidence Processing     │  │
│  │ (Purview eDiscovery)  │   │ (Purview Audit Log)    │   │ (Power Query & Exports) │  │
│  └───────────────────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ DEFENDER XDR / SIEM    │                                │
│                              │ (Unified Incidents)    │                                │
│                              └────────────────────────┘                                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Internal Checkpoints and Behavioral Monitoring**: Accidental and policy-based data leaks are caught at internal checkpoints using Data Loss Prevention (DLP) policies triggered by Sensitive Information Types (SITs) and Sensitivity Labels. When employees exhibit suspicious behavioral patterns, Insider Risk Management evaluates cumulative risk scores across policy templates, utilizing the DLP Triage Agent and Insider Risk Triage Agent to prioritize high-risk alerts.
    
      
    
2. **Escalation and Legal Evidence Vaults**: When an insider threat warrants formal investigation, analysts convert alerts into an Insider Risk Case and escalate directly into eDiscovery (Premium). Authorized eDiscovery Managers define targeted search scopes across Exchange, SharePoint, OneDrive, and Teams using KeyQL condition builders, natural language Copilot prompts, or Search by File samples, placing legal holds and exporting evidence as PST or MSG files with verifiable cryptographic hashes to preserve the Chain of Custody.
    
      
    
3. **Security Camera Tapes and AI Auditing**: To verify exact file accesses, analysts inspect the Microsoft Purview Audit camera tapes. Audit (Standard) provides 180 days of core event history, while Audit (Premium) unlocks 1-year default retention, custom retention policies up to 10 years, and high-value events like MailItemsAccessed (differentiating bulk Sync downloads from individual message Bind opens). AI usage is monitored across CopilotInteraction, ConnectedAIAppInteraction (requiring DSPM onboarding), and AIAppInteraction (requiring the Purview Browser Extension, DLP AI policies, and pay-as-you-go billing).
    
      
    
4. **Data Transformation and Cross-Domain Sync**: Raw audit CSV exports are transformed using Excel Power Query Editor to parse nested JSON data from the AuditData column, joining Copilot logs with mailbox access logs to prove file access. Alerts and risk scores synchronize bi-directionally with Microsoft Defender XDR and Microsoft Sentinel, providing compliance officers and SOC analysts with full visibility across the enterprise campus.
    

Plaintext

```
MASTER MAP: MITIGATING THREATS WITH MICROSOFT PURVIEW
├── 1. THE INTERNAL MAILROOM CHECKPOINT (DATA LOSS PREVENTION)
│   ├── DLP (automated policy rules that prevent users from inappropriately sharing sensitive information)
│   │   ├── SIT (sensitive information type, a pre-defined/custom text pattern used to spot sensitive data)
│   │   └── Sensitivity Labels (digital classification tags dictating access/encryption rules)
│   ├── Alert Lifecycle (Trigger, Notify, Triage, Investigate, Remediate, Tune)
│   │   ├── Single-Event Alerts vs. Aggregate-Event Alerts
│   │   └── DLP Triage Agent (Preview) (AI categorization into Needs attention, Less urgent, Not categorized)
│   └── Investigation Tools (Activity Explorer, Content Explorer, User Activity Summary)
├── 2. WATCHING INTERNAL AFFAIRS (INSIDER RISK MANAGEMENT)
│   ├── Pseudonymization (masking user names with anonymous identifiers)
│   ├── Alert & Case Workflow
│   │   ├── Alert tuning (Bulk Dismissal, Intelligent Detections)
│   │   ├── Spotlight (Highlights critical alerts based on activity/risk patterns)
│   │   └── Insider Risk Triage Agent (Preview)
│   ├── Visual Investigation
│   │   └── Scatter Plot Timeline (interactive map spanning 1-6 months showing Risk Sequences)
│   ├── Case Escalation Toolbar
│   │   └── (Send Email Notice, Escalate to eDiscovery, Run Power Automate, Create Teams, Resolve Case)
│   └── Defender XDR SOC Sync (AlertInfo, AlertEvidence, DataSecurityBehaviors, DataSecurityEvents)
├── 3. SEARCHING THE CORPORATE ARCHIVE (eDISCOVERY & LEGAL HOLDS)
│   ├── eDiscovery (secure system to locate, preserve, and export digital evidence)
│   ├── RBAC Segregation (eDiscovery Manager, eDiscovery Administrator)
│   ├── Workflow (The 4 Phases)
│   │   ├── 1. Define Criteria / 2. Identify Sources
│   │   ├── 3. Build Query (KeyQL, Draft with Copilot, Search by File)
│   │   └── 4. Run & Review (Statistics, Sample)
│   └── Evidence Integrity & Export
│       ├── Condense paths (fixes 259-character path limit errors)
│       └── Hashes & Chain of Custody (verifiable legal evidence preservation)
├── 4. CHECKING THE SECURITY CAMERA TAPES (MICROSOFT PURVIEW AUDIT)
│   ├── Tier Comparison
│   │   ├── Audit Standard (180 days, core events)
│   │   └── Audit Premium (1-year default, 10-year custom retention, high-value events)
│   ├── Operational Parameters
│   │   ├── UTC (Coordinated Universal Time required for searching)
│   │   └── Administrative Units (AUs, scoping log visibility)
│   └── Forensic High-Value Logs (MailItemsAccessed)
│       ├── Sync Access (bulk downloads) vs. Bind Access (individual message opens)
│       ├── Throttling (> 1,000 bind events/24 hrs)
│       └── Deduplication (filters exact same opens within 1 hour)
├── 5. AI SURVEILLANCE (COPILOT & AI APP AUDITING)
│   ├── Record Types
│   │   ├── CopilotInteraction (First-party M365/Security Copilot)
│   │   ├── ConnectedAIAppInteraction (Custom/Entra apps; requires DSPM onboarding)
│   │   └── AIAppInteraction (Third-party; requires Browser Extension/DLP AI policy)
│   └── Audit Metadata
│       ├── AppIdentity, AppHost, AccessedResources
│       └── JailbreakDetected (Boolean flag for bypassing safety guardrails)
└── 6. PARSING EVIDENCE (LOG TRANSFORMATIONS & POWER QUERY)
    ├── JSON (JavaScript Object Notation format extracted from the AuditData column)
    ├── Excel Power Query Editor (Transform Data -> JSON -> Expand properties)
    ├── Table Join (Cross-referencing multi-source file access)
    └── PowerShell Optimization
        ├── -Encoding UTF8 (prevents character corruption)
        └── Pagination (-SessionId / -SessionCommand ReturnLargeSet for >5,000 rows)
```