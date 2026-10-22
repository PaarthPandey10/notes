```
1. CENTRAL COMMAND ARCHITECTURE: SIEM, DATA LAKES & MULTI-TENANT WORKSPACES
├── The Big Picture
│   ├── Scenario: digital campus has grown so large that there are thousands of guards, millions of cameras, and endless alarms every second
│   │   └── Problem: even with the best frontline defenders, the security team drowns in noise if they cannot connect scattered clues
│   ├── Solution: build a Master Command Center = Microsoft Sentinel (a cloud-native security platform that aggregates, analyzes, and responds to threat data across the entire enterprise network)
│   ├── Dual role
│   │   ├── SIEM (security information and event management, a centralized system that collects, stores, and analyzes security logs from across the entire organization)
│   │   └── SOAR (security orchestration, automation, and response, an automated computing framework that executes immediate response workflows against active threats)
│   └── Unified interface functions
│       ├── Ingests raw surveillance feeds from every facility
│       ├── Stores historical records in deep archives
│       ├── Sets up automated alarm tripwires
│       ├── Cross-references incoming traffic against VIP guest lists (Watchlists) and most-wanted offender boards (Threat Intelligence)
│       └── Merges seamlessly with frontline security guards to stop attacks at machine speed
├── Installing Microsoft Sentinel begins by provisioning an underlying Log Analytics Workspace (an Azure database environment where log data is collected, stored, and queried)
│   └── Architects design the physical and logical layout of the master command center before running digital cables or setting up security desks
├── Sentinel Command Center Divisions (two distinct operational divisions)
│   ├── The Microsoft Sentinel SIEM Division (SIEM Operations Floor / Log Analytics Workspace)
│   │   ├── Fast, active operations floor: daily threat hunting, incident triage, real-time alert correlation
│   │   ├── Fast, high-frequency security searches
│   │   ├── Operational retention window (30-730 days)
│   │   └── Powers real-time analytics & hunting
│   └── The Microsoft Sentinel Platform Division (Platform Data Lake / Massive Storage Pool)
│       ├── Long-term strategic archive
│       ├── Data Lake (a scalable, low-cost storage repository that holds raw data in its native format for extended analysis)
│       ├── Low-cost long-term retention: logs for up to 12 years
│       ├── Graph-based correlation/analysis to uncover slow-moving, multi-year attack campaigns
│       └── Supports asynchronous KQL / Spark jobs
├── Workspace Architecture Options (three deployment options, chosen based on company footprint and legal requirements)
│   ├── Single-Tenant Single Workspace
│   │   ├── Central repository for all logs across the entire tenant
│   │   ├── Single pane of glass
│   │   └── Drawbacks: may incur cross-region bandwidth costs and fail local data sovereignty laws
│   ├── Single-Tenant Regional Workspaces
│   │   ├── Multiple workspaces deployed in specific regions
│   │   ├── Complies with local Data Residency laws and eliminates cross-region fees
│   │   └── Drawback: lacks a single native pane of glass
│   └── Multi-Tenant Workspaces
│       └── Independent workspaces across distinct corporate tenants, managed centrally using Azure Lighthouse
├── Management Tools for Regional / Multi-Tenant Deployments
│   ├── Workspace Manager
│   │   ├── Centrally publishes analytics rules, saved searches, workbooks, and automation policies from a central workspace out to member workspaces across one or more tenants
│   │   └── Path: Microsoft Sentinel > Configuration > Settings > Workspace manager
│   └── Azure Lighthouse (a cross-tenant management service that allows security providers to view and manage customer Sentinel workspaces without constantly signing in and out of separate corporate accounts)
├── Creating a New Workspace (Azure portal)
│   ├── 1. Search for Microsoft Sentinel in the top search bar and select + Add
│   ├── 2. On the Add Microsoft Sentinel to a workspace screen, select + Create a new workspace
│   ├── 3. On the Basics tab, specify Subscription, Resource Group, workspace Name (becomes the Sentinel workspace name), and Region
│   │   └── Region = geographic location where log data resides; cannot be changed after creation
│   ├── 4. Select Review + Create, then Create
│   ├── 5. Once deployed, select the new workspace from the list and select Add to enable Microsoft Sentinel
│   └── Flow: Search 'Microsoft Sentinel' -> + Add -> + Create a new workspace -> Complete 'Basics' & Create -> Select Workspace & 'Add' -> Workspace Active Screen
├── Workspace Navigation Pane (four main operational areas)
│   ├── General (Overview, Logs, News & guides, Search)
│   ├── Threat management (Incidents, Workbooks, Hunting, Notebooks, Entity behavior, Threat intelligence, MITRE ATT&CK)
│   ├── Content management (Content hub, Repositories, Community)
│   └── Configuration (Data connectors, Analytics, Watchlist, Automation, Settings)
└── Defender for Cloud Workspace Coordination
├── Defender for Cloud (Microsoft Defender for Cloud, a tool that monitors cloud security posture and workload protections)
├── Best practice: use the same Log Analytics workspace for both services
└── Limitation: the default workspace automatically provisioned by Defender for Cloud cannot be converted directly into a Sentinel workspace
├── 1. Manually create a custom Log Analytics workspace
├── 2. Update the Defender for Cloud pricing tier settings to point to the new workspace
└── 3. Enable Sentinel on that workspace
2. ROLE-BASED ACCESS CONTROL & WORKSPACE PERMISSIONS
├── Azure RBAC (role-based access control, a permission system that grants access rights based on job roles)
│   └── Prevents unauthorized staff from altering security rules or viewing sensitive evidence (defines who holds the keys to inspect case files and modify security configurations)
├── Sentinel-Specific Azure RBAC Roles (built-in)
│   ├── Microsoft Sentinel Reader (can view data, incidents, workbooks, and other Sentinel resources; cannot edit)
│   ├── Microsoft Sentinel Responder (includes Reader rights + can manage incidents: assign, dismiss, update)
│   ├── Microsoft Sentinel Contributor (includes Responder rights + can create/edit workbooks, analytics rules, etc.)
│   └── Microsoft Sentinel Automation Contributor (special service role that allows Sentinel to add playbooks to automation rules; not intended for human accounts)
├── Scope best practice: assigning roles at the Resource Group scope ensures all supporting resources deployed alongside the workspace automatically inherit matching permissions
├── Additional Role Assignments for Specialized Tasks
│   ├── Working with Playbooks
│   │   └── Logic App Contributor role assigned to human analysts, in addition to Sentinel permissions
│   ├── Granting Sentinel Permission to Run Playbooks
│   │   ├── When automation rules trigger playbooks, Sentinel uses a dedicated service account
│   │   ├── That service account must be granted explicit Logic App Contributor permissions (or rights to execute playbooks) on the resource group containing the playbook
│   │   └── The administrator configuring this automation must hold Owner permissions on the resource group containing the playbooks to delegate these rights
│   ├── Connecting Data Sources
│   │   └── Write permissions on the target Sentinel workspace, plus source-specific permissions detailed on individual connector pages
│   ├── Guest Users Assigning Incidents
│   │   └── Directory Reader role (assigned by default to regular member users) in Microsoft Entra ID, in addition to Microsoft Sentinel Responder rights, to look up and assign incidents to staff
│   └── Creating and Deleting Workbooks
│       └── Microsoft Sentinel Contributor OR a combination of a lower Sentinel role + Workbook Contributor (an Azure Monitor role)
└── Broader Azure / Log Analytics Roles
├── Azure roles: Owner, Contributor, or Reader at the subscription level
├── Log Analytics roles: Log Analytics Contributor, Log Analytics Reader
└── Caution: broader Azure roles grant permissions across all underlying Azure resources, so restrict permissions so analysts cannot inadvertently edit infrastructure outside Sentinel
3. DATA RETENTION, TABLE PLANS & LOG TIERS
├── Purpose: surveillance feeds stream into structured database tables; configure retention periods and data tiers to manage storage costs based on operational needs (balance search performance against budget constraints)
├── 1. Configuring Workspace Retention
│   ├── Default interactive log retention at the workspace level: 30 days
│   ├── Extendable up to 730 days (2 years) for a prorated fee
│   ├── Microsoft Sentinel solution tables can extend interactive retention up to 90 days at no additional charge
│   └── Azure portal steps
│       ├── 1. Navigate to Microsoft Sentinel > Configuration > Settings
│       ├── 2. Select Workspace Settings (redirects to the Log Analytics workspace portal)
│       ├── 3. Select Usage and estimated costs
│       ├── 4. Select Data Retention at the top of the page
│       ├── 5. Drag the slider to the desired retention value (30 to 730 days) and select OK
│       └── Flow: Sentinel Settings -> Workspace Settings -> Usage & Estimated Costs -> Data Retention Slider
├── 2. Log Data States and Table Plans
│   ├── Two data states
│   │   ├── Analytics retention (hot state for real-time alerts, hunting, and high-performance queries)
│   │   └── Long-term retention (cold state for low-cost compliance archiving, accessible via search jobs and data restores)
│   ├── Three Table Plans
│   │   ├── Analytics Plan
│   │   │   ├── Suited for continuous monitoring, real-time alerts, and threat hunting
│   │   │   └── Supports full KQL (Kusto Query Language, a database query language used to search raw telemetry and logs) across interactive retention windows ranging from 30 days to 2 years
│   │   ├── Basic Plan
│   │   │   ├── Designed for high-volume troubleshooting logs (e.g., ContainerLogV2, AppTraces, or custom DCR-based logs)
│   │   │   └── Discounted ingestion and 30 days of single-table search capabilities
│   │   └── Auxiliary Plan
│   │       ├── Designed for high-volume, low-touch compliance and audit logs
│   │       └── Low-cost ingestion and unoptimized single-table query access for 30 days
│   └── Basic Logs KQL Constraints
│       ├── Supported operators: where, extend, project, project-away, project-keep, project-rename, project-reorder, parse, parse-where
│       └── Unsupported operators (will fail on Basic Logs): join, union, summarize (aggregates)
└── 3. Log Storage Tiers in the Microsoft Defender Portal
├── Managed from the centralized table management experience when Sentinel is onboarded to the Microsoft Defender Portal ([https://security.microsoft.com/](https://security.microsoft.com/))
├── Path: Microsoft Sentinel > Configuration > Tables
├── Defender Portal Log Tiers
│   ├── Analytics Tier
│   │   ├── Hot state data (30 days to 2 years interactive)
│   │   ├── Mirrored to total long-term retention in the data lake for up to 12 years
│   │   └── Supports all real-time analytics rules, hunting, and workbooks
│   ├── Data Lake Tier
│   │   ├── Low-cost cold storage (30 days to 12 years)
│   │   ├── Accessible via ad-hoc KQL jobs, Spark jobs, or summary rules
│   │   └── Unavailable for real-time alerts
│   └── XDR Default Tier
│       ├── Default storage for Defender XDR advanced hunting tables (30 days included with XDR licenses)
│       └── Data can be extended beyond 30 days by ingesting it into the Analytics tier
└── Modifying Table Retention Settings
├── 1. Navigate to Microsoft Sentinel > Configuration > Tables
├── 2. Select the target table (e.g., CommonSecurityLog) to open the details side panel
├── 3. Select Manage table
├── 4. Set Analytics retention (30 days to 2 years) and Total retention (up to 12 years in the data lake)
│   └── Setting Total retention higher than Analytics retention automatically activates low-cost long-term storage for the remaining period
├── 5. Select Save
└── Flow: Defender Portal > Configuration > Tables -> Select Table -> Manage Table -> Set Retention & Save
4. QUERYING THE INTELLIGENCE VAULT: LOGS, COMMON TABLES & DEFENDER XDR SCHEMAS
├── Analysts must know which filing cabinet (table) holds specific evidence; connector telemetry populates dedicated tables queryable with KQL
├── Query Entry Points
│   ├── Logs page in the Azure portal (Microsoft Sentinel > General > Logs)
│   ├── Advanced Hunting (Investigation & response > Hunting > Advanced hunting) in the Defender portal
│   └── Data Lake Exploration (Microsoft Sentinel > Data lake exploration > KQL queries) in the Defender portal
├── Sentinel Feature-Specific Tables
│   ├── SecurityAlert (alerts generated by Sentinel analytics rules or ingested from connectors)
│   ├── SecurityIncident (official incident case files created from triggered alerts)
│   ├── ThreatIntelligenceIndicator (threat indicators such as IPs, domains, file hashes imported from threat feeds)
│   └── Watchlist (imported reference data lists)
├── Common Infrastructure Tables (populated by common data connectors)
│   ├── AzureActivity (management-plane subscription and resource group operations performed in Azure)
│   ├── AzureDiagnostics (internal diagnostic logs for Azure resources running in Azure Diagnostics mode)
│   ├── AuditLogs (Microsoft Entra ID audit trails, including user/group changes and application management)
│   ├── CommonSecurityLog (CEF [Common Event Format, a standardized text format used by network appliances] messages sent over Syslog)
│   ├── McasShadowItReporting (cloud app discovery logs streamed from Defender for Cloud Apps)
│   ├── OfficeActivity (audit logs for Office 365 services including Exchange, SharePoint, and Microsoft Teams)
│   ├── SecurityEvent (Windows security event logs collected from endpoints and domain controllers)
│   ├── SigninLogs (Microsoft Entra ID user sign-in activity logs)
│   ├── Syslog (raw Linux operating system event logs)
│   ├── Event (Windows Sysmon [System Monitor] event logs)
│   └── WindowsFirewall (local Windows Firewall network traffic events)
└── Defender XDR Tables (stream directly when the Microsoft Defender XDR data connector is enabled; raw endpoint, identity, and email telemetry)
├── AlertEvidence (files, IP addresses, URLs, users, or devices linked to active alerts)
├── CloudAppEvents (user activities within Microsoft 365 and connected cloud applications)
├── DeviceEvents (endpoint security control events: Defender AV, Exploit Guard)
├── DeviceFileCertificateInfo (digital certificate details for signed executable files)
├── DeviceFileEvents (file creation, modification, and file system operations on endpoints)
├── DeviceImageLoadEvents (executable DLL loading events on hosts)
├── DeviceInfo (endpoint OS builds, machine names, and system properties)
├── DeviceLogonEvents (local and network authentication events on user devices)
├── DeviceNetworkEvents (network socket connections and inbound/outbound web calls)
├── DeviceNetworkInfo (network adapter configurations, IP assignments, and MAC addresses)
├── DeviceProcessEvents (process creation trees, command-line arguments, and parent processes)
├── DeviceRegistryEvents (Windows registry key creation, deletion, and modification)
├── EmailEvents (email delivery, filtering, and blocking events in Microsoft 365)
├── EmailPostDeliveryEvents (post-delivery email actions, e.g., zero-hour auto-purge)
├── EmailUrlInfo (embedded web links and URLs extracted from incoming emails)
├── EmailAttachmentInfo (file details and hashes for attachments arriving via email)
├── IdentityDirectoryEvents (Active Directory domain controller administrative events)
├── IdentityLogonEvents (on-premises AD and cloud identity authentication attempts)
└── IdentityQueryEvents (Active Directory LDAP query lookups for users, groups, and devices)
5. THE VIP GUEST LISTS: PLANNING, CREATING & MANAGING WATCHLISTS
├── Watchlists (path: Microsoft Sentinel > Configuration > Watchlist)
│   ├── Static reference lists (e.g., spreadsheets listing high-value servers, trusted corporate VPN ranges, recently terminated staff) maintained in Sentinel
│   ├── Separate trusted personnel from high-risk targets using imported business reference lists
│   └── Stored as key-value pairs directly inside the workspace database and cached in memory to ensure low-query latency
├── Common Watchlist Use Cases
│   ├── Rapid Incident Investigation (import malicious IPs or file hashes from CSVs to correlate against raw logs in KQL queries)
│   ├── High-Risk User Tracking (import lists of terminated employees or VIP executives to trigger instant alarms upon sign-in)
│   ├── Reducing Alert Fatigue (allowlists of authorized admin IPs to suppress false alarms from routine maintenance)
│   └── Data Enrichment (join event logs with business metadata, e.g., mapping IP addresses to physical building locations)
├── Creating a New Watchlist (Azure portal)
│   ├── 1. Navigate to Microsoft Sentinel > Configuration > Watchlist and select + Add new
│   ├── 2. General page: enter Name, Description, and Alias (the unique identifier used in KQL queries), then Next
│   ├── 3. Source page: select dataset type, upload a local CSV file (maximum 3.8 MB), select the Search key column (the primary unique column), then Next
│   ├── 4. Review the configuration and select Create
│   └── Flow: Watchlist Page -> + Add New -> Name & Alias -> Upload CSV (<= 3.8MB) -> Select Search Key -> Create
├── Querying a Watchlist
│   ├── Call the _GetWatchlist('WatchlistAlias') function
│   └── Example (correlate sign-in logs against a watchlist of high-value servers): let HighValueHosts = _GetWatchlist('HighValueMachines') | project HostName; DeviceLogonEvents | where DeviceName in (HighValueHosts)
└── Watchlist Management Best Practices
├── Editing Watchlist Items: select the watchlist > Update watchlist > Edit watchlist items > select the target row > modify fields > Save
├── Bulk Updating: Update watchlist > Bulk update > upload a new CSV file
│   ├── Appends new records and automatically deduplicates rows where all column values match
│   └── Does NOT delete items removed from the CSV file; items must be deleted individually in the UI, or the watchlist must be completely deleted and recreated
└── Ingestion SLA Warning
├── Log Analytics enforces a 5-minute SLA (Service Level Agreement) for data ingestion
└── If an analyst deletes and immediately recreates a watchlist, both the deleted and new entries may appear simultaneously in KQL results during this 5-minute window
6. THE MOST WANTED BOARD: TACTICAL THREAT INTELLIGENCE & INDICATOR MANAGEMENT
├── Cyber Threat Intelligence (CTI): the command center maintains a "Most Wanted" board to identify active threat campaigns touching the digital campus (external intelligence feeds track known cybercriminals across the globe)
│   └── Tactical threat intelligence (most actionable form of CTI) = IoCs (indicators of compromise, technical clues such as malicious IP addresses, phishing URLs, domain names, and file hashes)
├── Four Integration Pathways
│   ├── 1. Data Connectors (ingest automated feeds via TAXII [Trusted Automated eXchange of Indicator Information, a protocol for sharing threat data] connectors or the Upload Indicators API)
│   ├── 2. Threat Intelligence / Intel Management Page (view, tag, filter, and create threat indicators manually)
│   ├── 3. Analytics Rules (built-in threat intelligence rule templates, e.g., TI map IP entity to AzureActivity, cross-reference incoming logs against known IoCs automatically)
│   └── 4. Workbooks (visualize indicator matches using the built-in Threat Intelligence workbook)
├── Flow: TAXII / API Threat Feeds -> ThreatIntelIndicators Table -> TI Analytics Rules -> Incident Generated
├── Creating a Threat Indicator Manually (Microsoft Defender portal)
│   ├── 1. Navigate to Microsoft Sentinel and select Threat intelligence under Threat management (if prompted, select Open Intel management to transition to the unified Intel management page under the main Defender navigation)
│   ├── 2. Select + Add new from the top menu
│   ├── 3. Select the Indicator type (IP Address, URL, Domain, File Hash), fill in required fields marked with a red asterisk (e.g., value, confidence score, valid from/until dates), and select Apply
│   └── Flow: Defender Portal > Threat Intelligence > Intel Management -> + Add New -> Fill Indicator Details -> Apply
├── Tags (applied to indicators individually or via multi-select to group clues related to specific incidents, threat actors, or attack campaigns)
├── Threat Indicator Tables (accessible via KQL)
│   ├── ThreatIntelligenceIndicator (legacy table storing imported and user-created indicators)
│   └── ThreatIntelIndicators and ThreatIntelObjects (modern STIX-compliant schema tables)
│       └── STIX (Structured Threat Information eXpression, a standardized language for cyber threat intelligence)
├── Threat Intel Table Transition Timeline
│   ├── April 3, 2025: public preview of ThreatIntelIndicators & ThreatIntelObjects tables; data streams to both old and new tables
│   └── July 31, 2025: hard deprecation date; ingestion into the legacy ThreatIntelligenceIndicator table stops completely; all custom KQL queries, analytics rules, and workbooks MUST be migrated to the new tables before this date
└── Example Query (legacy schema)
└── ThreatIntelligenceIndicator | where TimeGenerated > ago(24h) | where Active == true
7. TEARING DOWN THE WALLS: ONBOARDING SENTINEL TO THE MICROSOFT DEFENDER PORTAL
├── Background: historically teams switched between two control rooms, the Azure portal (hosting the Microsoft Sentinel SIEM) and the Microsoft Defender portal (hosting XDR capabilities)
├── Unified Security Operations Platform ([https://security.microsoft.com/](https://security.microsoft.com/))
│   ├── Merges Microsoft Sentinel and Microsoft Defender XDR into a single operational workspace
│   ├── Microsoft Defender XDR (Endpoints, Identities, Email, Apps)
│   ├── Microsoft Sentinel SIEM (Multicloud, Third-Party, Logs)
│   └── Single Unified Defender Portal Web UI capabilities
│       ├── Unified Incident Queue & Correlated Attack Stories
│       ├── Unified Advanced Hunting (querying Defender + Sentinel tables in one window)
│       ├── Unified Entity Pages (Devices, Users, IP Addresses, Azure Resources)
│       └── Automatic Attack Disruption for SAP Applications
├── Onboarding Prerequisites and RBAC Requirements
│   ├── An active Log Analytics workspace with Microsoft Sentinel enabled
│   ├── The Microsoft Defender XDR data connector installed from Content Hub and enabled in Sentinel for incidents and alerts
│   ├── Microsoft Defender XDR onboarded to the Microsoft Entra tenant
│   └── An Azure account assigned the Owner or User Access Administrator role on the subscription PLUS the Microsoft Sentinel Contributor role on the workspace or resource group scope
├── Onboarding Step-by-Step Workflow
│   ├── 1. Sign in to the Microsoft Defender portal ([https://security.microsoft.com/](https://security.microsoft.com/))
│   ├── 2. On the Home (Overview) page, locate the "Get your SIEM and XDR in one place" banner and select Connect a workspace
│   ├── 3. Select the target primary Microsoft Sentinel workspace from the dropdown list and select Next
│   ├── 4. Review the product changes and select Connect
│   └── Flow: Defender Portal Home -> Connect a Workspace Banner -> Select Workspace -> Review Changes -> Connect
├── Automatic Architectural Changes Upon Connection (four structural changes)
│   ├── 1. All Sentinel log tables, functions, and saved KQL queries become instantly accessible inside Advanced Hunting within the Defender portal
│   ├── 2. The Microsoft Sentinel Contributor role is automatically granted to the Microsoft Threat Protection and WindowsDefenderATP system applications within the Azure subscription
│   ├── 3. Active Microsoft security incident creation rules in Sentinel are automatically deactivated to prevent duplicate incidents
│   │   └── Applies strictly to incident creation rules for Microsoft alerts, NOT custom scheduled analytics rules
│   └── 4. The Fusion Analytics Rule is Disabled
│       ├── Fusion (Sentinel's legacy engine that used machine learning to stitch multi-stage alerts into incidents)
│       └── In the unified portal, Defender XDR's native incident correlation engine replaces Fusion entirely
├── Key Capability Differences Between Azure and Defender Portals (most Sentinel features function identically across both portals)
│   ├── Advanced Hunting Bookmarks: NOT supported inside the Advanced Hunting window in Defender; must be accessed via Sentinel > Hunting
│   ├── Attack Disruption for SAP: available EXCLUSIVELY in the Defender portal (unavailable in the Azure portal)
│   ├── Data Connectors Visibility: native Defender connectors (Defender for Endpoint, Identity, Office 365, Cloud Apps, Defender XDR, Defender for Cloud) are hidden from the Sentinel Data Connectors page in Defender, as data flows natively
│   ├── Adding Entities to TI From Incidents: supported in the Azure portal ONLY
│   ├── Incident Alert Management: in Defender, alerts can be removed from an incident ONLY by linking them to another incident
│   ├── Incident Comments: editing existing incident comments is supported in Azure only; comment edits made in Azure do NOT sync back to the Defender portal
│   ├── Programmatic Incidents: incidents created via API, Logic Apps, or manually in Azure do NOT sync to Defender
│   ├── Reopening Closed Incidents: in Defender, analytics rules cannot reopen closed incidents when new alerts match; a new incident is triggered instead
│   ├── Incident Tasks: supported in Azure ONLY
│   └── Multi-Workspace Management: Defender supports ONE primary workspace per tenant; Azure supports managing multiple workspaces via Workspace Manager
└── Offboarding a Workspace (to switch the connected workspace)
├── 1. Navigate to System > Settings > Microsoft Sentinel in the Defender portal
├── 2. On the Workspaces page, select the connected workspace and select Disconnect workspace
└── 3. Provide a reason for disconnecting and select Confirm; disconnecting removes the Microsoft Sentinel navigation section from the Defender portal sidebar

MASTER MAP: CONFIGURING AND OPERATING MICROSOFT SENTINEL
├── 1. CENTRAL COMMAND ARCHITECTURE
│   ├── Microsoft Sentinel (cloud-native security platform) = Master Command Center; SIEM + SOAR; built on a Log Analytics Workspace
│   ├── Divisions: SIEM Operations Floor (30-730 days, real-time analytics & hunting) + Platform Data Lake (up to 12 years, graph-based, asynchronous KQL / Spark jobs)
│   ├── Architecture options: Single-Tenant Single Workspace (single pane, cross-region costs, sovereignty risk), Single-Tenant Regional Workspaces (Data Residency, no single pane), Multi-Tenant Workspaces (Azure Lighthouse)
│   ├── Workspace Manager (Settings > Workspace manager; publishes rules, searches, workbooks, automation) and Azure Lighthouse (cross-tenant)
│   ├── Create workspace: + Add > + Create a new workspace > Basics (Subscription, Resource Group, Name, Region [permanent]) > Review + Create > Add
│   ├── Nav pane: General, Threat management, Content management, Configuration
│   └── Defender for Cloud: share workspace; auto-provisioned default workspace can't be converted; create custom workspace, repoint pricing tier, enable Sentinel
├── 2. RBAC KEYS
│   ├── Azure RBAC roles: Sentinel Reader, Responder, Contributor, Automation Contributor (service role, not for humans)
│   ├── Assign at Resource Group scope for inheritance
│   └── Task roles: Logic App Contributor (playbooks; service account needs it on playbook RG; admin needs Owner), Write on workspace (data sources), Directory Reader + Responder (guest users assigning incidents), Contributor or lower role + Workbook Contributor (workbooks); caution with Owner/Contributor/Reader and Log Analytics roles
├── 3. DATA RETENTION TIERS
│   ├── Workspace retention: 30 days default, up to 730 days (prorated fee); Sentinel solution tables up to 90 days free
│   ├── Data states: Analytics retention (hot) and Long-term retention (cold; search jobs and restores)
│   ├── Table plans: Analytics (full KQL, 30 days to 2 years), Basic (discounted ingestion, 30-day single-table search), Auxiliary (low-cost, unoptimized single-table query, 30 days)
│   ├── Basic Logs KQL: supports where, extend, project family, parse, parse-where; no join, union, summarize
│   └── Defender portal tiers: Analytics (hot, mirrored to data lake up to 12 years), Data Lake (30 days to 12 years; ad-hoc KQL / Spark / summary rules; no real-time alerts), XDR Default (30 days included; extend via Analytics tier); Manage table sets Analytics retention + Total retention
├── 4. INTELLIGENCE VAULT (TABLES)
│   ├── Entry points: Logs, Advanced Hunting, Data Lake Exploration
│   ├── Feature tables: SecurityAlert, SecurityIncident, ThreatIntelligenceIndicator, Watchlist
│   ├── Infrastructure tables: AzureActivity, AzureDiagnostics, AuditLogs, CommonSecurityLog (CEF), McasShadowItReporting, OfficeActivity, SecurityEvent, SigninLogs, Syslog, Event (Sysmon), WindowsFirewall
│   └── XDR tables: AlertEvidence, CloudAppEvents, Device* (Events, FileCertificateInfo, FileEvents, ImageLoadEvents, Info, LogonEvents, NetworkEvents, NetworkInfo, ProcessEvents, RegistryEvents), Email* (Events, PostDeliveryEvents, UrlInfo, AttachmentInfo), Identity* (DirectoryEvents, LogonEvents, QueryEvents)
├── 5. VIP GUEST LISTS (WATCHLISTS)
│   ├── Key-value pairs cached in memory; CSV up to 3.8 MB; Alias + Search key; _GetWatchlist('Alias')
│   ├── Use cases: rapid incident investigation, high-risk user tracking, reducing alert fatigue, data enrichment
│   └── Management: edit items, bulk update (appends + dedupes, never deletes), 5-minute ingestion SLA overlap
├── 6. MOST WANTED BOARD (THREAT INTELLIGENCE)
│   ├── IoCs via TAXII connectors / Upload Indicators API, Intel Management page, TI analytics rule templates, TI workbook
│   ├── Manual indicators (+ Add new > indicator type > required fields > Apply) and Tags
│   └── Tables: legacy ThreatIntelligenceIndicator vs STIX-based ThreatIntelIndicators + ThreatIntelObjects (preview April 3, 2025; legacy ingestion stops July 31, 2025; migrate queries, rules, workbooks)
├── 7. TEARING DOWN WALLS (UNIFIED PLATFORM IN DEFENDER)
│   ├── Prerequisites: Sentinel-enabled workspace, Defender XDR connector from Content Hub, XDR onboarded to Entra tenant, Owner or User Access Administrator + Sentinel Contributor
│   ├── Onboarding: Connect a workspace banner > select workspace > review > Connect
│   ├── Automatic changes: tables in Advanced Hunting, Sentinel Contributor granted to Microsoft Threat Protection + WindowsDefenderATP apps, Microsoft incident creation rules deactivated, Fusion disabled (Defender XDR correlation replaces it)
│   ├── Portal differences: bookmarks, SAP Attack Disruption, hidden native connectors, TI-from-incident, alert removal by linking, comment sync, programmatic incident sync, closed incidents not reopened, incident tasks, one primary workspace per tenant
│   └── Offboarding: System > Settings > Microsoft Sentinel > Disconnect workspace > reason > Confirm
└── END STATE (connecting the dots)
└── Unified Security Operations Platform: single command center to hunt threats, analyze logs, and execute automated response playbooks at machine speed, built on Log Analytics Workspace + Data Lake, Azure RBAC boundaries, tiered retention, Watchlists, Threat Intelligence, and Defender portal integration
```