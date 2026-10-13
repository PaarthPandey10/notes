```
1. ATTACK FRAMEWORKS & THE CAMPUS ARCHITECTURE
├── CAMPUS ANALOGY (managing enterprise security is like managing physical security for a Sprawling Corporate Campus with a Centralized Security Control Room)
│   ├── Sprawling Corporate Campus (your entire digital estate across email, endpoints, identities, and cloud applications)
│   ├── Central Security Control Room (Microsoft Defender XDR; gathers signals from every building entrance, hallway camera, and mailroom scanner into one unified dashboard)
│   ├── Historical camera tapes / long-term logs (the control room connects directly to Microsoft Sentinel to keep them)
│   ├── Human guards (rely on Microsoft Security Copilot to analyze massive amounts of threat data at machine speed)
│   └── Every component works together (mailroom envelope scanners to undercover sensors placed next to server vaults) to stop digital break-ins, shrink attacker dwell time, and protect company assets
│
├── ATTACK TIMELINES (three standard frameworks that describe the timeline of an attack so everyone speaks the same security language; defenders must understand how intruders plan and carry out a break-in across the campus before placing guards or installing cameras)
│   ├── Cyber Kill Chain (an older security model that breaks an attack into sequential phases like a military operation, from scouting the building to stealing data)
│   │   └── Maps out the traditional progression of a breach
│   ├── MITRE ATT&CK (a modern, highly detailed playbook of the exact tactics and techniques hackers use)
│   │   └── Provides a complete matrix of real-world adversary behaviors; used to test whether security cameras are pointed in the right direction
│   └── PETE (Prepare, Enter, Traverse, and Execute, a simplified four-step attack summary)
│       ├── Gives business leaders an easy-to-understand view of an attack without getting lost in technical details
│       ├── Attack behavior (regardless of model): attackers rarely break a window and instantly steal data; they sneak inside and look around quietly
│       └── PRIMARY OBJECTIVE: Reduce Dwell Time (attacker dwell time, the total amount of time a hacker remains undetected inside your network)
│
└── THE DIGITAL CAMPUS ARCHITECTURE
    ├── ENTRANCE CHECKPOINTS
    │   ├── MDO (Microsoft Defender for Office 365, an email security service that filters malicious messages and attachments)
    │   │   └── Attack spans multiple security domains: e.g., a malicious attachment arrives on a personal email account NOT protected by MDO
    │   └── MDE (Microsoft Defender for Endpoint, an endpoint detection and response platform that protects user devices)
    │       ├── Detects the malicious payload, raises an alert, and communicates with Intune to report the device risk level has changed
    │       └── CORRELATED ENFORCEMENT
    │           ├── Microsoft Intune (a cloud-based endpoint management service that enforces device compliance policies)
    │           │   └── Intune policy immediately marks the endpoint as noncompliant
    │           └── Conditional Access (a security feature in Microsoft Entra ID that grants or blocks access based on automated conditions and risk)
    │               ├── Blocks the compromised user account from accessing corporate applications
    │               ├── User can still browse public internet sites (e.g., Wikipedia, YouTube) that do not require corporate credentials; all corporate access remains blocked
    │               └── CAE (continuous access evaluation, a real-time token check that revokes access instantly when user risk changes)
    │                   └── Continuous evaluation is guaranteed through CAE
    │
    ├── CROSS-DOMAIN ATTACK & RESPONSE SCENARIO (Attack Vector: Personal Email / infected USB drive)
    │   ├── Step 1: User opens the file / inserts infected USB; malware infects the computer
    │   ├── Step 2: MDE detects the malicious payload and raises an alert
    │   ├── Step 3: MDE tells Intune the device risk level changed
    │   ├── Step 4: Intune policy marks the endpoint noncompliant
    │   ├── Step 5: Conditional Access blocks the compromised user from corporate apps (CAE revokes access in real time)
    │   ├── Step 6: MDE remediates the threat (automated cleanup routines OR manual analyst intervention)
    │   ├── Step 7: MDE signals Intune to reset the device risk status
    │   ├── Step 8: Conditional Access automatically restores access to corporate resources
    │   └── Step 9: MDE contributes attack signals to Microsoft Threat Intelligence, allowing MDO and Defender for Cloud to automatically detect and remediate similar threat variants across mailboxes and cloud workloads company-wide
    │
    └── THE CENTRAL CONTROL ROOM (coordinates human guards and automated responses; bridges all security systems together)
        ├── Microsoft Defender XDR (extended detection and response, a connected security system that tracks threats across different areas)
        ├── Microsoft Sentinel (a cloud-native security information and event management system that stores long-term logs across all systems)
        ├── Microsoft Security Copilot (an artificial intelligence assistant built specifically to help security teams investigate threats)
        ├── Microsoft Threat Intelligence (a global database of security threat signals maintained by Microsoft)
        │   └── Receives attack signals contributed by MDE
        └── Microsoft Defender for Cloud (a cloud workload protection platform that secures cloud infrastructure and servers)
            └── Automatically detects and remediates similar threat variants across cloud workloads (alongside MDO for mailboxes)
```

```
2. MODERN SOC FUNCTIONS & UNIFIED GOVERNANCE
├── SOC (security operations center, the human team monitoring and defending the network)
│   ├── When an alarm triggers, the SOC must act quickly to reduce MTTR
│   ├── Rather than chasing isolated warnings across separate screens, Defender XDR automatically groups related alerts into an Incident (a correlated collection of alerts that tell the complete end-to-end story of an attack)
│   ├── CDOC (Cyber Defense Operations Center, Microsoft's operational security facility)
│   │   └── High-quality alert sources observed by CDOC analysts
│   │       ├── ~65% originate from integrated XDR tools
│   │       ├── ~10% come from user-reported issues
│   │       └── ~25% come from traditional log queries and external sources
│   ├── PERFORMANCE METRICS
│   │   ├── MTTR (mean time to remediate, the average clock time it takes to fix a security breach)
│   │   └── True Positive Rate (the percentage of generated alerts that represent genuine security threats rather than false alarms)
│   │       └── Strict quality standard: any alert feed assigned to human analysts must maintain a 90% True Positive Rate
│   │
│   ├── Modern SOC organizes personnel into distinct operational functions rather than strict hierarchical tiers (specialized skills match specific threat levels)
│   └── OPERATIONAL FUNCTIONS
│       ├── Automation (Near-real-time resolution of well-defined attack types)
│       │   └── Handles highly repeatable attack types using pre-programmed security playbooks, no human intervention
│       ├── Triage (Tier 1) (Rapid remediation of high-volume incidents)
│       │   └── Well-known incident types that still require quick human judgment (e.g., approving automated remediation workflows, escalating complex anomalies)
│       ├── Investigation (Tier 2) (Complex multi-stage attacks and escalations)
│       │   └── Primary escalation point for Tier 1; handles sophisticated multi-stage attacks, behavioral alerts, business-critical asset threats, and ongoing campaigns across cloud environments, virtual machines, and containers
│       ├── Hunt and Incident Management (Tier 3) (Proactive hunting and forensics)
│       │   └── Hypothesis-driven hunting for undetected adversaries that slipped past automated defenses; advanced digital forensics; coordinates non-technical incident requirements with legal, executive, and communications teams
│       ├── Threat Intelligence (Strategic research on attacker groups)
│       │   └── Also covers emerging attack techniques and technical context supporting all other SOC teams, using a dedicated threat intelligence platform
│       └── Combining all domain tools into a single interface: Tier 1 resolves incidents rapidly; Tier 2 and Tier 3 handle lower volumes of complex human-operated attacks
│
└── WORKSPACE & PERMISSIONS
    ├── Microsoft Defender Portal (https://security.microsoft.com/, single pane of glass for all security operations across the digital campus)
    │   ├── Consolidates capabilities from legacy security portals into a unified workspace built around RBAC
    │   ├── Analysts see cards, dashboards, and data tailored to their specific roles
    │   └── EIGHT CORE SECURITY WORKLOADS (natively integrated signal feeds, detection engines, and management workflows)
    │       ├── Microsoft Defender for Office 365 (threat prevention, detection, investigation, and hunting for email, attachments, links, and collaboration spaces)
    │       ├── Microsoft Defender for Endpoint (preventative protection, post-breach detection, automated investigation, and response for computers, servers, and mobile devices)
    │       ├── Microsoft Defender XDR (automatically correlates cross-domain threat data from endpoints, identities, email, and cloud apps into a unified attack story on a single dashboard)
    │       ├── Microsoft Defender for Cloud Apps (a cloud access security broker providing visibility, data controls, and threat protection for cloud software services)
    │       ├── Microsoft Defender for Identity (monitors on-premises Active Directory signals to detect advanced threats, compromised accounts, and insider risks)
    │       ├── Microsoft Defender Vulnerability Management (continuous asset visibility, risk assessments, and built-in remediation tools to prioritize software weaknesses)
    │       ├── Microsoft Defender for IoT (secures operational technology environments, physical processes, and specialized hardware across manufacturing and critical infrastructure)
    │       └── Microsoft Sentinel (streams Defender XDR incidents and advanced hunting events into a cloud data lake; synchronizes incidents between Azure and Defender workspaces)
    │
    ├── "More Resources" menu (related security management tools accessible to analysts)
    │   ├── Microsoft Purview Portal (the compliance workspace for data governance and classification)
    │   │   └── URL: https://purview.microsoft.com/
    │   ├── Microsoft Entra ID (the cloud identity management portal)
    │   │   └── URL: https://entra.microsoft.com/
    │   ├── Microsoft Entra ID Protection (the identity risk monitoring engine)
    │   ├── Azure Information Protection (the client scanner for document labeling)
    │   └── Microsoft Defender for Cloud (the multi-cloud server security workspace)
    │
    └── ACCESS CONTROL
        ├── RBAC (role-based access control, a permission model that grants access rights based on job duties)
        └── URBAC (unified role-based access control, a single centralized permission framework across Defender tools)
            ├── DEFAULT MODEL DATES
            │   ├── Default for new Defender for Endpoint tenants: February 16, 2025
            │   ├── Default for new Defender for Identity tenants: March 2, 2025
            │   └── Existing tenants maintain imported configurations while mapping legacy roles into unified permissions
            ├── PERMISSIONS (categorized into functional security areas and access levels)
            │   ├── Security Operations \ Security Data \ Security Data Basics (Read) (read-only access to basic alert metadata, incident lists, and asset information across workloads)
            │   ├── Security Operations \ Security Data \ Alerts (Manage) (update alert states, assign ownership, edit classifications, link alerts to incidents)
            │   ├── Security Operations \ Security Data \ Response (Manage) (execute manual response actions such as isolating endpoints or quarantining files)
            │   ├── Security Operations \ Basic Live Response (Manage) (remote command-line access to onboarded devices for basic forensic file and process inspection)
            │   ├── Security Operations \ Advanced Live Response (Manage) (full remote shell capabilities: execute custom scripts, download files, modify endpoints)
            │   ├── Security Posture \ Posture Management \ Vulnerability Management (Read) (displays software weaknesses, exposure scores, and security recommendations)
            │   └── Authorization and Settings \ Security Settings \ Core Security Settings (Manage) (configure global detection parameters, notification rules, and portal settings)
            └── GLOBAL ENTRA ID ROLE MAPPING (roles automatically map into URBAC permissions)
                ├── Global Administrator (full manage permissions across all workloads, including live response and file collection)
                ├── Security Administrator (full manage permissions across all workloads, including live response and file collection)
                ├── Security Operator (alert management and response permissions)
                └── Security Reader / Global Reader (read-only permissions across security data, Secure Score, and vulnerability management)
```

```
3. INCIDENT MANAGEMENT & AUTOMATED REMEDIATION (AIR)
├── INVESTIGATION PACKAGING
│   ├── Alert (an individual warning signal generated when a security sensor detects a suspicious or malicious event)
│   │   ├── ALERT MANAGEMENT PANE (opens when an alert is selected)
│   │   │   ├── Inspect alert details
│   │   │   ├── Operational states: New, In Progress, Resolved
│   │   │   ├── Analyst classifications: True Positive, False Positive
│   │   │   ├── Determination (e.g., Malware, Phishing, Security Testing)
│   │   │   ├── Assign ownership
│   │   │   └── Link the alert to a different incident
│   │   ├── SEVERITY LEVELS (four standardized levels based on operational risk)
│   │   │   ├── High Severity (Red) (advanced persistent threats, active ransomware, credential theft tooling e.g., Mimikatz execution, security sensor tampering, human adversary activity capable of catastrophic organizational damage)
│   │   │   ├── Medium Severity (Orange) (EDR post-breach behaviors typical of advanced attack stages, e.g., anomalous registry modifications, execution of suspicious scripts, unexpected internal connections)
│   │   │   ├── Low Severity (Yellow) (prevalent malware, non-malware hack tools, basic exploration commands, isolated security testing; limited immediate risk to the broader organization)
│   │   │   └── Informational Severity (Grey) (security events that cause no actual damage, e.g., a commercial malware file blocked and cleaned by antivirus before executing)
│   │   ├── CATEGORIES
│   │   │   ├── MITRE ATT&CK Tactics (map to the MITRE ATT&CK Enterprise Matrix)
│   │   │   │   ├── Initial Access
│   │   │   │   ├── Execution
│   │   │   │   ├── Persistence
│   │   │   │   ├── Privilege Escalation
│   │   │   │   ├── Defense Evasion
│   │   │   │   ├── Credential Access
│   │   │   │   ├── Discovery
│   │   │   │   ├── Lateral Movement
│   │   │   │   ├── Collection
│   │   │   │   ├── Command and Control
│   │   │   │   ├── Exfiltration
│   │   │   │   ├── Exploit
│   │   │   │   ├── Ransomware
│   │   │   │   └── Suspicious Activity
│   │   │   └── Unwanted Software (potentially unwanted applications that degrade system performance; non-MITRE category)
│   │   └── SUPPRESSION (Alert Suppression Rules; for legitimate line-of-business apps or administrative scripts that repeatedly trigger benign alerts)
│   │       ├── Hides known innocuous alerts from the portal queue WITHOUT disabling underlying detection engines
│   │       ├── Suppress Alert on This Device (silences the specific alert only when triggered on a selected computer or server)
│   │       ├── Suppress Alert in My Organization (silences the specific alert across all devices tenant-wide)
│   │       └── Applies exclusively to NEW alerts generated after rule creation; does not alter or remove pre-existing alerts in the historical queue
│   │
│   └── Incident (a correlated collection of alerts that tell the complete end-to-end story of an attack)
│       ├── Complete, connected narrative compiled from individual alerts across multiple devices, user accounts, and mailboxes
│       ├── Grouping correlated alerts prevents alert fatigue; shows where the attack started, what tactics were used, how far the attacker progressed, and which assets were impacted
│       ├── INCIDENT QUEUE (default view: all active and resolved cases from the last 30 days, most recent at the top)
│       │   ├── Incident names auto-generated from alert attributes (affected endpoints, impacted users, detection sources, or MITRE tactics) to show scope at a glance
│       │   ├── Analysts can customize queue columns and filter the list
│       │   ├── INCIDENT QUEUE FILTERS (Status, Severity, Assignment, Service Sources, Tags, Classification)
│       │   │   ├── Status (operational state: Active or Resolved)
│       │   │   ├── Severity (potential business impact: High, Medium, Low, or Informational)
│       │   │   ├── Incident Assignment (cases assigned to specific analysts or handled by automated playbooks)
│       │   │   ├── Multiple Service Source (cases containing alerts from more than one Defender product, e.g., MDE and MDO combined)
│       │   │   ├── Service Sources (restricts view to specific detection engines: Defender for Endpoint, Defender for Identity, Defender for Cloud Apps, Defender for Office 365)
│       │   │   ├── Tags (custom labels added by SOC teams to flag specific attack campaigns or priority assets)
│       │   │   ├── Data Sensitivity (whether sensitive files with Microsoft Purview Information Protection labels are involved)
│       │   │   └── Classification (analyst validation: True Positive or False Positive)
│       │   └── QUEUE UI CONTROLS
│       │       ├── Circle (opens preview panel on the right: incident details, impacted assets, quick response options)
│       │       ├── Greater Than Symbol (>) (expands the row in the queue to show all individual alerts nested inside the incident)
│       │       └── Incident Name Link (navigates to the full multi-tab Incident Page for deep investigation)
│       ├── INCIDENT PAGE MANAGEMENT ACTIONS
│       │   ├── Edit incident name
│       │   ├── Update status (Active or Resolved); resolving an incident automatically closes all open alerts within it
│       │   ├── Assign ownership (Assign to Me)
│       │   ├── Add custom tags
│       │   ├── Enter investigator comments (historical audit log)
│       │   └── Reassign alerts between incidents using the Alerts tab
│       ├── INCIDENT PAGE TABS (nine specialized investigation tabs)
│       │   ├── Overview (top characteristics, MITRE ATT&CK-aligned attack categories, affected scope, chronological alert timeline, evidence summary)
│       │   ├── Alerts (all correlated warnings in chronological order: severity, detection source, linking reasons)
│       │   ├── Devices (every onboarded computer or server involved; links to individual device pages)
│       │   ├── Users (compromised or targeted accounts; links to user risk profiles in Defender for Cloud Apps and Entra ID Protection)
│       │   ├── Mailboxes (targeted email accounts involved in phishing or exfiltration attempts)
│       │   ├── Apps (cloud software applications targeted or abused during the attack)
│       │   ├── Investigations (all AIR runs triggered by the incident, including pending manual actions)
│       │   ├── Evidence and Response (all analyzed artifacts: files, processes, IP addresses, URLs, registry keys; with verdict Malicious, Suspicious, or Clean, and remediation status)
│       │   └── Incident Graph (interactive node graph of the attack story: entry points, compromised devices, indicators of compromise, propagation routes)
│       └── Incident Graph 
│           └── Blast Radius Analysis (combines post-breach current impact with pre-breach potential attack paths)
│               ├── Status: Preview
│               ├── Targets critical assets such as domain controllers, key vaults, sensitive databases
│               ├── Powered by Microsoft Sentinel Data Lake (the underlying graph and data infrastructure that powers cross-workspace relationships)
│               │   └── Requires onboarding to the Microsoft Sentinel Data Lake
│               ├── Path depth bounded to a maximum of 7 total hops (typically up to 5 hops for pure cloud/on-premises paths and 3 hops for hybrid paths)
│               └── Renders top-rated attack routes based on the investigating analyst's RBAC scope
│
└── AIR (automated investigation and remediation, automated software playbooks that analyze and clean up threats)
    ├── Uses inspection algorithms modeled after human SOC workflows to evaluate triggered alerts, inspect related artifacts, and execute containment actions automatically
    ├── Purpose: reduce analyst workload
    ├── Investigation Graph (Analyzes artifacts and expands scope)
    │   ├── Launches when an alert fires and constructs the graph
    │   ├── Inspects files, processes, services, registry keys, network connections, user accounts, and mailboxes across the affected device
    │   ├── If the same suspicious artifact is found on other machines, scope automatically expands to include those endpoints
    │   └── If expansion encompasses 10 or more devices, the action automatically pauses and requires explicit human analyst approval
    ├── Verdicts (every analyzed entity): Malicious, Suspicious, No Threats Found
    ├── Remediation Actions (generated from verdicts)
    │   ├── Send files to quarantine
    │   ├── Stop malicious processes
    │   ├── Delete scheduled tasks
    │   ├── Remove registry keys
    │   ├── Disable drivers
    │   ├── Turn off external mail forwarding
    │   └── Isolate endpoints
    ├── Automation Levels (set across defined device groups)
    │   ├── Full Automation (executes all recommended remediation actions automatically on artifacts determined malicious; completed steps logged in audit history)
    │   ├── Semi - Require Approval for Any Remediation (pauses every remediation action in the pending queue awaiting human approval)
    │   ├── Semi - Require Approval for Core Folders (auto-executes on files in non-core directories; requires human approval for operating system directories e.g., \windows*)
    │   ├── Semi - Require Approval for Non-Temp Folders (auto-executes on temporary directories e.g., \users*\appdata\local\temp*, \windows\temp*, \downloads*; requires human approval for files in all other system locations)
    │   └── No Automated Response (disables automated investigation playbooks completely on covered devices; significantly reduces organizational security posture)
    │
    └── Unified Action Center (https://security.microsoft.com/action-center)
        ├── Collects all pending and completed actions across endpoints, email, and identities
        ├── Pending Tab (Awaiting human review)
        │   └── Analysts inspect file details, view triggering alerts, open the full investigation page, and approve or reject remediation actions individually or in bulk
        ├── History Tab (Audit log of completed actions, supports Undo)
        │   ├── Tracks completed remediation actions, approved playbooks, Live Response commands (remote shell actions executed by analysts), and Microsoft Defender Antivirus cleanups
        │   ├── Undo (for false positives): restore files from quarantine, un-isolate devices, restore registry keys, restart stopped services
        │   └── Apply to X More Instances (select a file entry on the History tab to undo file quarantines across multiple devices simultaneously)
        └── Action Source column (records the origin of every remediation step)
            ├── Manual Device Action
            ├── Manual Email Action
            ├── Automated Device Action
            ├── Automated Email Action
            ├── Advanced Hunting Action
            ├── Explorer Action
            ├── Manual Live Response Action
            └── Live Response API Action
```

```
4. DOMAIN SENSORS (EMAIL, IDENTITY, & CLOUD APPS)
├── MAILROOM: Microsoft Defender for Office 365
│   ├── Acts as an aggressive mailroom inspection station; analyzes over 6.5 trillion daily signals across global messaging traffic
│   ├── Cloud-based filtering against zero-day malware, business email compromise, and credential phishing
│   ├── DEPLOYMENT MODELS
│   │   ├── Filtering-Only Deployment (cloud email security for on-premises Exchange Server or third-party SMTP email infrastructure)
│   │   ├── Cloud-Hosted Deployment (native protection for Exchange Online cloud mailboxes)
│   │   └── Hybrid Deployment (controls mail routing and protection across mixed on-premises and cloud mailboxes, using Exchange Online Protection for inbound filtering)
│   ├── CORE POLICIES: Safe Attachments, Safe Links, Anti-Phishing (Impersonation Protection, Anti-Spoofing)
│   │   ├── 1. Safe Attachments (zero-day malware protection; routes attachments without known virus signatures into a virtual hypervisor environment where machine learning and detonation techniques analyze file behavior)
│   │   │   ├── Unknown malware actions (five options)
│   │   │   │   ├── Off (disables Safe Attachments scanning)
│   │   │   │   ├── Monitor (continues delivery while tracking detonation results in background logs)
│   │   │   │   ├── Block (blocks current email and all future attachments with the same malware signature)
│   │   │   │   ├── Replace (strips the malicious attachment, inserts a text warning file, delivers original email body)
│   │   │   │   └── Dynamic Delivery (delivers email body immediately with a temporary placeholder attachment during background detonation; seamlessly reattaches once verified safe)
│   │   │   ├── Redirect Attachment on Detection (check Enable Redirect and enter a security administrator email address to receive blocked or timed-out attachments)
│   │   │   └── Bypass for trusted internal senders (e.g., multifunction scanners): mail flow rule in Exchange Admin Center (https://admin.exchange.microsoft.com/) setting header X-MS-Exchange-Organization-SkipSafeAttachmentProcessing
│   │   ├── 2. Safe Links (protects against malicious URLs in emails, Teams chats, and Office documents; rewrites URLs to route requests through Microsoft inspection servers at the moment of click = time-of-click protection)
│   │   │   ├── Safe Links Policy settings (global URL handling across desktop, web, and mobile Office apps)
│   │   │   │   ├── Action for Unknown Potentially Malicious URLs (enables dynamic URL rewriting and real-time checking)
│   │   │   │   ├── Use Safe Attachments to Scan Downloadable Content (routes linked web files through hypervisor detonation chambers before user downloads)
│   │   │   │   ├── Apply Safe Links to Messages Sent Within the Organization (time-of-click checking on internal emails)
│   │   │   │   ├── Do Not Track When Users Click Safe Links (controls whether click metrics are stored; Microsoft recommends leaving tracking enabled)
│   │   │   │   ├── Do Not Allow Users to Click Through to Original URL (prevents bypassing warning screens when a link is confirmed malicious)
│   │   │   │   └── Do Not Rewrite the Following URLs (allowlist of trusted URLs, e.g., partner portals, that bypass link wrapping)
│   │   │   └── Bypass for internal routes: mail flow transport rule setting header X-MS-Exchange-Organization-SkipSafeLinksProcessing
│   │   └── 3. Anti-Phishing Policies (advanced machine learning models detect credential harvesting and spoofing)
│   │       ├── Impersonation Protection (detects senders/domains designed to look like company executives or corporate domains)
│   │       │   ├── Domain impersonation (e.g., ćóntoso.com spoofing contoso.com)
│   │       │   └── User impersonation (e.g., michele@contoso.com spoofing michelle@contoso.com)
│   │       ├── Anti-Spoofing Controls (verify email authentication records: SPF, DKIM, DMARC)
│   │       └── Safety Tips (visual tips in Outlook when unusual characters or unfamiliar senders are detected)
│   │
│   ├── ADMIN EVALUATION TOOLS
│   │   ├── Threat Trackers (dashboards monitoring active malware campaigns)
│   │   ├── Threat Explorer (real-time reporting console analyzing delivered, blocked, or quarantined email across custom timeframes)
│   │   ├── Submissions Portal (flyout to submit suspect emails, files, or URLs to Microsoft human graders; throttled at 150 submissions per 15 minutes)
│   │   └── Attack Simulation Training (realistic phishing simulations, credential harvesting tests, and password spray drills to train employee security awareness)
│   │
│   └── AI INTEGRATION
│       └── Phishing Triage Agent (an autonomous AI agent embedded in Defender for Office 365)
│           ├── Triggered automatically when a user reports a suspicious email
│           ├── Powered by LLMs (large language models, artificial intelligence systems trained on text to analyze natural language)
│           │   └── Evaluates email headers, message bodies, file/URL detonations, and screenshot artifacts
│           ├── Billed via SCU (security compute unit, the metric used to measure and bill AI computing capacity)
│           ├── Prerequisites
│           │   ├── Security Copilot SCU capacity
│           │   ├── MDO Plan 2
│           │   ├── URBAC enabled
│           │   ├── MDO workload activated in XDR settings
│           │   ├── Monitor Reported Messages in Outlook enabled (under User Reported settings)
│           │   └── Alert policy "Email reported by user as malware or phish" active (built-in auto-resolve rule disabled)
│           ├── Permissions (agent identity, scoped to the MDO data source)
│           │   ├── Security Data Basics (Read)
│           │   ├── Alerts (Manage)
│           │   ├── Security Copilot (Read)
│           │   ├── Email Metadata (Read)
│           │   └── Email Content (Read)
│           ├── Identity Options
│           │   ├── New Entra Agent ID (recommended; dedicated AI service identity)
│           │   └── Connect an existing user account
│           ├── Workflow
│           │   ├── Assigns reported alerts to itself
│           │   ├── Tags the incident with an Agent tag
│           │   ├── Resolves false positives automatically
│           │   └── Leaves true positive phishing alerts open for human review with transparent rationale and visual workflow diagrams
│           └── Feedback Memory (analysts teach the agent)
│               ├── Select Change Classification
│               ├── Enter plain-text reasoning
│               ├── Select Use this feedback to teach the agent
│               ├── Click Evaluate Feedback
│               └── Save the lesson into agent memory
│
├── BADGE SCANNERS: Microsoft Entra ID Protection
│   ├── Functions as a network of smart facial-recognition badge scanners at every entrance
│   ├── Requires a Microsoft Entra ID Premium P2 license
│   ├── Analyzes 6.5 trillion daily signals across Microsoft consumer, gaming, and commercial identity platforms to calculate real-time sign-in risk and user compromise probability
│   ├── Risk Detections (Anonymous IP, Leaked Credentials, Password Spray, etc.) (fourteen specific risk detection types)
│   │   ├── Anonymous IP Address (sign-ins from anonymizing proxies, Tor networks, or VPNs)
│   │   ├── Atypical Travel (sign-ins from geographically distant locations within a time shorter than expected travel time)
│   │   ├── Malicious IP Address (sign-ins from IPs actively flagged on Microsoft threat intelligence lists)
│   │   ├── Unfamiliar Sign-in Properties (characteristics such as user agent, browser, OS not recently observed for the user)
│   │   ├── Leaked Credentials (valid credentials exposed on public paste sites or the dark web)
│   │   ├── Password Spray (brute-force attacks targeting multiple accounts with common passwords)
│   │   ├── Microsoft Entra Threat Intelligence (sign-in patterns matching specialized threat actor tactics)
│   │   ├── Anomalous Token (abnormal token lifetimes, token replay, or tokens from unfamiliar locations)
│   │   ├── Token Issuer Anomaly (potential compromise of SAML token issuing infrastructure)
│   │   ├── Suspicious Browser (anomalous sign-in behavior across multiple tenants from a single browser session)
│   │   ├── Verified Threat Actor IP (authentication attempts from IPs owned by confirmed malicious groups)
│   │   ├── New Country (discovered by MDCA; sign-ins from countries never previously visited by the user)
│   │   ├── Activity from Anonymous IP Address (discovered by MDCA; cloud app sessions initiated through anonymizing services)
│   │   └── Suspicious Inbox Forwarding (discovered by MDCA; automated forwarding rules created immediately after a suspicious sign-in)
│   ├── Directory Risk Policies (three, automate risk response)
│   │   ├── User Risk Policy (Triggers SSPR: self-service password reset, a tool allowing users to reset their own passwords without helpdesk intervention)
│   │   │   ├── Evaluates the probability that an identity account is fully compromised
│   │   │   ├── Microsoft recommended threshold: High
│   │   │   └── Access control: Allow Access only when requiring a secure Password Change via SSPR and MFA
│   │   ├── Sign-in Risk Policy & MFA Registration Policy
│   │   │   ├── Sign-in Risk Policy (evaluates the probability that a specific authentication request is fraudulent)
│   │   │   │   ├── Microsoft recommended threshold: Medium and Higher
│   │   │   │   └── Access control: require MFA challenge completion
│   │   │   └── MFA Registration Policy (forces targeted users to complete combined security information registration upon sign-in so they can self-remediate when risk triggers)
│   │   └── Policy exclusions: emergency access break-glass administrator accounts and trusted corporate network IP ranges (prevent accidental lockout during incidents)
│   ├── Portal blades (Entra admin center: Identity > Protection > Identity Protection)
│   │   ├── Risky Users Report
│   │   │   ├── Tracks accounts with elevated risk scores; shows risk history
│   │   │   ├── Risk states: At Risk, Remediated, Dismissed
│   │   │   ├── Admin actions: Reset Password, Confirm User Compromised, Dismiss User Risk, Block User
│   │   │   └── Export to CSV/JSON up to 2,500 entries
│   │   ├── Risky Sign-ins Report
│   │   │   ├── Retains 30 days of filterable authentication logs
│   │   │   ├── Details: real-time risk levels, applied Conditional Access policies, device metadata, application targets, location coordinates
│   │   │   └── Actions: Confirm Sign-in Compromised, Confirm Sign-in Safe
│   │   └── Risk Detections Report
│   │       ├── Retains 90 days of granular risk event telemetry
│   │       └── Links directly to deep log views in MDCA
│   ├── Programmatic access via Microsoft Graph APIs
│   │   ├── Entra App Registration with Client Credentials grant type
│   │   ├── Application permissions: IdentityRiskEvent.Read.All, IdentityRiskyUser.Read.All
│   │   └── Endpoints: /v1.0/identityProtection/riskDetections, /v1.0/identityProtection/riskyUsers, /v1.0/identityProtection/signIn
│   ├── Workload Identities Protection (non-human identities: service principals, app registrations, managed identities)
│   │   ├── Why: workload identities store static secrets, lack MFA capabilities, and have non-standard lifecycles
│   │   ├── Monitored risks: Suspicious Sign-ins (learned over a 2 to 60-day baseline), Unusual Addition of Credentials to an OAuth App, Leaked Credentials, Entra Threat Intelligence
│   │   └── Conditional Access policies can block access for single-tenant service principals marked at risk
│   └── Identity Risk Management Agent (embedded AI agent in Entra ID Protection, uses LLMs)
│       ├── Triggers: continuous 5-minute intervals, daily, or manual runs
│       ├── Scope: scans up to 100 recent risky users across 7 to 90-day timeframes
│       ├── Generates risk summaries and verdicts (Compromised or Not Compromised)
│       └── Surfaces bulk remediation actions (Dismiss Risk or Reset Password)
│
├── VAULT SENSORS: Microsoft Defender for Identity
│   ├── Undercover security sensors installed beside on-premises Active Directory domain controllers
│   ├── Monitors signals from on-premises Active Directory, AD FS, and AD CS to detect advanced identity attacks across hybrid environments
│   ├── Evaluates identity configurations via Microsoft Secure Score; flags accounts authenticating with unencrypted clear-text passwords
│   ├── DEPLOYMENT
│   │   ├── AD FS (Active Directory Federation Services, a software feature that provides single sign-on authentication)
│   │   ├── AD CS (Active Directory Certificate Services, a server role that issues digital security certificates)
│   │   ├── Requires FQDN (fully qualified domain name, the complete domain name specifying an exact host location)
│   │   ├── Sensor Architecture
│   │   │   ├── MDI Sensor installs directly on domain controllers, AD FS, or AD CS servers; captures local network traffic and Windows event logs
│   │   │   ├── Parses telemetry on the local host; transmits only lightweight, parsed metadata to the MDI Cloud Service
│   │   │   └── Standalone deployments: sensors on separate servers receiving mirrored domain controller traffic via network port mirroring
│   │   ├── Sensor management path: Defender portal (Settings > Identities > Deployment > On-premises > Sensors)
│   │   │   ├── FQDN List: must include FQDN of all monitored domain controllers; at least one must be a Global Catalog server (resolves user and computer objects across forest boundaries)
│   │   │   └── Network Adapters: select the capture adapters used for domain traffic or destination mirror ports
│   │   └── Deployment validation (three steps)
│   │       ├── 1. Confirm service "Azure Advanced Threat Protection sensor service" is running (if it fails, check %programfiles%\Azure Advanced Threat Protection sensor\<version>\Logs\Microsoft.Tri.sensor-Errors.log)
│   │       ├── 2. Generate a test alert from a domain-joined machine: command prompt > nslookup > server contosodc.contoso.azure > ls -d contoso.azure (domain zone transfer query)
│   │       └── 3. Verify the activity appears on the device timeline in the Defender portal as an MdiDnsQuery action type
│   │
│   ├── LMPs (lateral movement paths, visual diagrams showing how an attacker can hop between accounts to reach sensitive domain admin accounts)
│   └── KILL CHAIN DETECTIONS (four stages)
│       ├── Reconnaissance: LDAP (Lightweight Directory Access Protocol, an application protocol used to query directory services) & SMB (Server Message Block, a network file sharing protocol used to access shared resources)
│       │   └── Detects attackers mapping domain structure via LDAP enumeration or SMB session queries targeting sensitive groups
│       ├── Compromised Credentials: NTLM (NT LAN Manager, a suite of legacy Microsoft security protocols)
│       │   └── Detects brute-force attempts and large-scale Password Spraying using Kerberos or NTLM
│       ├── Lateral Movement: Pass-the-Ticket, Overpass-the-Hash
│       │   ├── Pass-the-Ticket (reusing stolen Kerberos tickets on secondary machines)
│       │   ├── Overpass-the-Hash (using a stolen NTLM hash to request a Kerberos ticket-granting ticket)
│       │   └── Identifies LMPs (lateral movement paths, visual diagrams showing how an attacker can hop between accounts to reach sensitive domain admin accounts)
│       └── Domain Dominance: DCShadow, Skeleton Key
│           ├── DCShadow (registering a rogue domain controller to inject malicious directory objects via replication)
│           └── Skeleton Key (injecting a master password into domain controller memory)
│   └── INTEGRATION
│       ├── Alerts flow into the unified incident queue (Incidents & alerts > Alerts), correlating on-premises identity events with MDE endpoint alerts and MDCA cloud app alerts
│       └── Native integration with PAM (privileged access management, specialized software used to secure administrative accounts) platforms: CyberArk, Delinea, BeyondTrust; plus Okta sign-in feeds and Microsoft Security Copilot
│
└── OFF-CAMPUS PATROLS: Microsoft Defender for Cloud Apps
    ├── Off-campus security patrol and mobile checkpoint system for third-party software services outside the main gates
    ├── CASB (cloud access security broker, an intermediary security enforcement point between users and cloud services)
    │   ├── Multi-cloud; enforces corporate security rules across Microsoft and third-party SaaS services
    │   ├── FOUR FRAMEWORK PILLARS
    │   │   ├── 1. Discover and Control Shadow IT (identify unapproved cloud apps and infrastructure services used without IT authorization)
    │   │   ├── 2. Protect Sensitive Information Anywhere (scan, classify, and enforce data protection policies on documents in cloud repositories)
    │   │   ├── 3. Protect Against Cyberthreats and Anomalies (ML analysis of user session behavior to catch compromised accounts, malicious insider actions, ransomware)
    │   │   └── 4. Assess Cloud App Compliance (evaluate apps against industry regulations, legal standards, corporate risk benchmarks)
    │   ├── Cloud Discovery (Finds Shadow IT)
    │   │   ├── Analyzes network traffic logs from firewalls, proxies, and onboarded endpoints against a master cloud app catalog of over 16,000 applications
    │   │   ├── Each app scored 1 to 10 based on 80+ risk factors (security, compliance, legal frameworks)
    │   │   └── Cloud Discovery Dashboard (six investigation steps)
    │   │       ├── 1. Review High-Level Usage Overview (top data consumers and source IP addresses)
    │   │       ├── 2. Inspect App Categories (sanctioned vs. unsanctioned usage volumes)
    │   │       ├── 3. Navigate the Discovered Apps Tab
    │   │       ├── 4. Evaluate app safety in the App Risk Overview
    │   │       ├── 5. Locate hosting locations using the App Headquarters Map
    │   │       └── 6. Mark high-risk apps as Unsanctioned (automatically triggers MDE to block user device access to those sites)
    │   └── CAAC (Conditional Access App Control, a feature that proxies user app sessions to enforce real-time security rules)
    │       ├── Stops breaches in real time (Cloud Discovery analyzes traffic after the fact)
    │       ├── Integrates Entra ID Conditional Access with MDCA: set Session control to "Use Conditional Access App Control"
    │       ├── User session traffic routed through a reverse proxy; real-time access and session policies based on user identity, device state, and location
    │       └── SIX REAL-TIME SESSION POLICY CONTROLS
    │           ├── Prevent Data Exfiltration (block download, cut, copy, or print of sensitive files on unmanaged personal devices)
    │           ├── Protect on Download (auto-apply AIP sensitivity labels and encryption on download)
    │           ├── Prevent Upload of Unlabeled Files (block uploads lacking mandatory sensitivity labels)
    │           ├── Monitor User Sessions for Compliance (log all actions in high-risk sessions to establish behavioral baselines)
    │           ├── Block Access (terminate sessions on untrusted device certificates or elevated risk factors)
    │           └── Block Custom Activities (real-time content inspection of chat messages in apps like Microsoft Teams or Slack; blocks messages containing sensitive data types)
    │
    └── DATA & ANOMALY PROTECTION
        ├── AIP (Azure Information Protection, a cloud service that classifies and encrypts sensitive documents)
        │   └── Native MDCA integration enforces a 4-phase data protection lifecycle
        │       ├── 1. Discover Data (via app connectors or CAAC proxy)
        │       ├── 2. Classify Sensitive Information (100+ built-in SITs or default AIP labels: Personal, Public, General, Confidential, Highly Confidential)
        │       ├── 3. Protect Data (File Policies with DLP governance actions: quarantine, permission removal, file trashing)
        │       └── 4. Monitor & Report
        ├── SIT (sensitive information type, a pre-defined text pattern like a credit card number used to spot sensitive data)
        └── UEBA (user and entity behavioral analytics, machine learning that learns normal user habits to detect abnormal behavior)
            ├── Out-of-the-box Anomaly Detection Policies
            │   ├── Baseline established over a 7-day initial learning period
            │   ├── Evaluates user sessions against 30+ risk indicators (e.g., login failures, activity rates, inactive accounts, suspicious locations)
            │   └── Built-in policies: Impossible Travel, Activity from Infrequent Country, Malware Detection, Ransomware Activity, Activity from Suspicious IP Addresses, Suspicious Inbox Forwarding, Unusual Multiple File Downloads, Unusual Administrative Activities
            ├── System Suppression (built-in platform suppressions that are always active)
            ├── Tenant Suppression (suppresses alerts for tenant-wide baseline activities, e.g., known corporate VPN gateways)
            ├── User Suppression (suppresses alerts for individual user habits, e.g., routine travel locations)
            └── Policy Sensitivity Levels (tune suppression types)
                ├── Low (System + Tenant + User suppressions; strictest filtering, fewest alerts)
                ├── Medium (System + User suppressions)
                └── High (System suppressions only; highest alert volume, maximum coverage)
```

```
5. HUNTING, POSTURE, & APIS
├── ADVANCED HUNTING (campus hallway patrols; proactively search for quiet attackers who trigger no standard alarms)
│   ├── Searches up to 30 days of raw telemetry across endpoints, email, cloud apps, and identities
│   ├── KQL (Kusto Query Language, a query tool used to search raw security log databases)
│   │   └── Executes read-only queries against structured tables
│   ├── Data Schema: Event Data (NRT) & Entity Data (Timestamps in UTC: Coordinated Universal Time)
│   │   ├── Event or Activity Data (populates tables immediately after sensors transmit events; near-real-time ingestion)
│   │   ├── Entity Data (user and device metadata; dynamic fields update every 15 minutes; comprehensive record consolidated every 24 hours)
│   │   └── All timestamps stored in UTC
│   ├── Schema Reference Tables
│   │   ├── Alert & Identity Tables: AlertInfo, AlertEvidence, IdentityInfo, IdentityLogonEvents, IdentityDirectoryEvents, IdentityQueryEvents
│   │   ├── Endpoint Telemetry Tables: DeviceInfo, DeviceLogonEvents, DeviceProcessEvents, DeviceNetworkEvents, DeviceFileEvents, DeviceRegistryEvents, DeviceEvents, DeviceImageLoadEvents, DeviceFileCertificateInfo, DeviceNetworkInfo
│   │   ├── Vulnerability Management Tables: DeviceTvmSoftwareInventory, DeviceTvmSoftwareVulnerabilities, DeviceTvmSoftwareVulnerabilitiesKB, DeviceTvmSecureConfigurationAssessment, DeviceTvmSecureConfigurationAssessmentKB
│   │   └── Email & Cloud Application Tables: EmailEvents, EmailUrlInfo, EmailAttachmentInfo, EmailPostDeliveryEvents, CloudAppEvents
│   ├── Custom Detection Rules (Require Timestamp, DeviceId, ReportId. Max 100 alerts/run)
│   │   ├── Built from advanced hunting queries to automate ongoing threat monitoring
│   │   ├── Three mandatory management columns: Timestamp, DeviceId, ReportId
│   │   ├── In aggregation queries, obtain the columns with summarize + arg_max()
│   │   │   └── Example: DeviceEvents | where Timestamp > ago(7d) | where ActionType == "AntivirusDetection" | summarize (Timestamp, ReportId)=arg_max(Timestamp, ReportId), count() by DeviceId | where count_ > 5
│   │   ├── Max 100 alerts per execution run (prevents portal flooding)
│   │   ├── Rule evaluation frequency (query schedule and lookback window)
│   │   │   ├── Continuous (NRT): runs continuously, evaluating new events in near-real-time
│   │   │   ├── Every 1 Hour: runs hourly, evaluates past 4 hours
│   │   │   ├── Every 3 Hours: runs every 3 hours, evaluates past 12 hours
│   │   │   ├── Every 12 Hours: runs twice daily, evaluates past 48 hours
│   │   │   └── Every 24 Hours: runs daily, evaluates past 30 days
│   │   ├── Automated actions on devices: Isolate Device, Collect Investigation Package, Run Antivirus Scan, Initiate Investigation
│   │   ├── Automated actions on files: Allow/Block indicator addition, Quarantine File
│   │   └── Scope: All Devices or specific Device Groups
│   └── Hunting Graph (Preview)
│       ├── Interactive visualization tool integrated with the Sentinel graph engine
│       ├── Replaces manual KQL join queries; analysts select predefined scenarios to visually traverse entity nodes and relationship edges and identify attack choke points
│       └── Predefined scenarios
│           ├── Paths Between Two Entities
│           ├── Users with Access to Sensitive Data
│           ├── Data Exfiltration by a Device
│           ├── Paths to Critical Kubernetes Cluster
│           ├── Identities with Access to Azure DevOps Repositories
│           ├── Nodes in Highest Number of Paths to SQL Stores
│           ├── Critical Users with Access to Sensitive Storage
│           └── Entities with Access to Key Vault
│
└── POSTURE & OVERSIGHT (campus structural inspections, APIs, and threat intelligence briefings; security is not purely reactive)
    ├── Microsoft Secure Score (Calculates enterprise security posture rating)
    │   ├── Composed of Microsoft Secure Score (365) for workplace identities and apps, and Cloud Secure Score for cloud workloads
    │   ├── Evaluates settings across: Defender for Office 365, Exchange Online, Entra ID, Defender for Endpoint, Defender for Identity, Defender for Cloud Apps, Purview, Teams, and third-party SaaS apps (e.g., Salesforce, ServiceNow, Okta, Zoom)
    │   └── Recommended Actions tab (prioritized fixes) with action states: To Address, Planned, Risk Accepted, Resolved Through Third Party, Resolved Through Alternate Mitigation, Completed
    ├── Microsoft Graph Security API (Intermediary broker endpoint for REST APIs)
    │   ├── Endpoint: https://graph.microsoft.com (v1.0 and Beta)
    │   ├── Federates REST API requests across Microsoft 365 core services, Enterprise Mobility + Security (Entra ID, MDI, Intune), Windows, and partner solutions
    │   ├── Purpose: automated software integration and custom dashboard development
    │   └── Programmatic hunting: POST method to the runHuntingQuery endpoint
    │       ├── POST https://graph.microsoft.com/v1.0/security/runHuntingQuery
    │       ├── Content-Type: application/json
    │       └── Body: {"Query": "DeviceProcessEvents | where InitiatingProcessFileName =~ \"powershell.exe\" | project Timestamp, FileName, InitiatingProcessFileName | order by Timestamp desc | limit 2"}
    ├── Security Copilot Threat Intelligence Briefing Agent (Dynamically analyzes campaigns against vulnerabilities)
    │   ├── Embedded on the Threat Analytics Dashboard (Threat intelligence > Threat analytics)
    │   ├── Delivers executive summaries to CISOs and security managers; prioritized intelligence reports
    │   ├── Powered by SCU capacity
    │   ├── Prerequisites: Security Copilot enabled; plugins active (Microsoft Threat Intelligence, Microsoft Threat Intelligence Agents, optional Microsoft Defender External Attack Surface Management)
    │   ├── Permissions: access to Defender Vulnerability Management data; Security Reader access to Threat Analytics; Security Admin access for settings configuration
    │   ├── Management path: System > Settings > Microsoft Defender XDR > Threat Intelligence Briefing Agent (schedule behaviors and communication parameters)
    │   └── Audit Transparency: reports saved in Security Copilot under Activity; select View Activity to inspect run history, evaluate reasoning chains, or give feedback with thumbs up/down icons
    └── Portal administrative monitoring
        ├── Reports Blade
        │   ├── Endpoint reports: Threat Protection, Device Health and Compliance, Vulnerable Devices, Web Protection, Firewall, Device Control, ASR Rules
        │   └── Email reports: Exchange Mail Flow, Schedules, Downloadable Reports
        └── Settings Menu (custom email notification rules targeting specific incident severities and Threat Analytics updates)
```

```
6. THE MASTER MAP: THREAT PROTECTION & INCIDENT RESPONSE (END-TO-END CORRELATION)
THE DIGITAL CAMPUS (Modern Enterprise Security Architecture)
│
├── 1. THREAT FRAMEWORKS & OBJECTIVES
│   ├── Attack methods: social engineering, weaponized links, or stolen credentials
│   ├── Frameworks: Cyber Kill Chain, MITRE ATT&CK, PETE (Prepare, Enter, Traverse, Execute)
│   └── Goal: Reduce Dwell Time (Total time hacker is undetected)
│
├── 2. THE PERIMETER & DOMAIN SENSORS (Detecting the Attack)
│   │
│   ├── Email Checkpoints (MDO - Microsoft Defender for Office 365)
│   │   ├── Polices: Safe Attachments, Safe Links, Anti-Phishing
│   │   └── AI Assistant: Phishing Triage Agent (Powered by LLMs, billed in SCU)
│   │
│   ├── Device Sensors (MDE - Microsoft Defender for Endpoint)
│   │   ├── Integration: Signals Intune for compliance changes -> Triggers Conditional Access
│   │   ├── Conditional Access (Microsoft Entra ID feature) + CAE (continuous access evaluation) revoke/restore access based on device and user risk
│   │   └── Contributes attack signals to Microsoft Threat Intelligence (MDO and Defender for Cloud then remediate variants)
│   │
│   ├── Identity Scanners (Entra ID Protection & MDI)
│   │   ├── Cloud Identity: Entra ID Protection (Risk detections, triggers SSPR)
│   │   │   └── Calculates real-time User Risk and Sign-in Risk to enforce risk-based Conditional Access (badge scanners)
│   │   └── On-Premises AD: MDI (Defender for Identity)
│   │       ├── Deployed on: Domain Controllers, AD FS, AD CS (using FQDN)
│   │       ├── Detections: LDAP/SMB Recon, NTLM attacks, LMPs, DCShadow
│   │       └── Sensors track Lateral Movement Paths (vault sensors)
│   │
│   └── Cloud Software Guardrails (MDCA - Microsoft Defender for Cloud Apps)
│       ├── Architecture: CASB (Cloud Access Security Broker)
│       ├── Cloud Discovery (finds Shadow IT)
│       ├── Real-Time Proxy: CAAC (Conditional Access App Control)
│       └── Protections: AIP labels, SIT detection, UEBA anomaly models
│
├── 3. THE CENTRAL CONTROL ROOM (Defender Portal & SOC Operations)
│   │
│   ├── Unified Governance
│   │   ├── Core Platform: Microsoft Defender XDR & Microsoft Purview Portal
│   │   ├── Defender Portal: https://security.microsoft.com/
│   │   └── Permissions: URBAC (Unified RBAC superseding legacy RBAC)
│   │
│   ├── SOC Operations & Triage
│   │   ├── Team structure: Automation, Tier 1 Triage, Tier 2 Investigation, Tier 3 Hunt and Incident Management, Threat Intelligence
│   │   ├── Metrics: MTTR & 90% True Positive Rate
│   │   └── Incident Creation: Grouping Alerts (inc. Unwanted Software) into unified Incidents (correlation engines group scattered alerts)
│   │
│   ├── Investigation & Automation
│   │   ├── Visuals: Incident Graph & Blast Radius Analysis
│   │   └── Automation: AIR (Automated Investigation and Remediation)
│   │       ├── Builds investigation graph, analyzes evidence, generates remediation actions (per assigned Automation Levels)
│   │       └── Management: Handled via the Unified Action Center (https://security.microsoft.com/action-center); guards approve pending tasks or undo past cleanups
│   │
│   └── Advanced Proactive Hunting
│       ├── Querying: KQL (Kusto Query Language) scanning UTC time-stamped Event/Entity data
│       └── Custom Detection Rules (hallway patrols)
│
└── 4. LONG-TERM ARCHIVE & EXECUTIVE INTELLIGENCE
    ├── Data Lake: Microsoft Sentinel (Stores long-term logs & powers Blast Radius graphs)
    ├── Automation Hooks: Microsoft Graph Security API (REST API federated access)
    ├── Structural Reviews: Microsoft Secure Score (continuous inspection of structural building safety)
    ├── AI Insights: Security Copilot Threat Intelligence Briefing Agent (executive briefings)
    └── End state: end-to-end security architecture capable of detecting, containing, and remediating threats at machine speed
```