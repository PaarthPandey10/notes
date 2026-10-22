```
1. CENTRAL COMMAND ARCHITECTURE: SIEM, DATA LAKES & MULTI-TENANT WORKSPACES
├── The Big Picture & The Ticking Clock
│   ├── The Hook: On July 31, 2025, many command centers risk going silently blind to global threats if architectural deprecations aren't handled properly.
│   ├── Scenario: A digital campus has grown so large (thousands of guards, millions of cameras) that the security team drowns in noise if they cannot connect scattered clues.
│   ├── Solution: Build a Master Command Center = Microsoft Sentinel (a cloud-native security platform that aggregates, analyzes, and responds to threat data).
│   ├── Cloud-Native Advantage: Sentinel completely decouples compute from storage; it scales elastically to absorb massive telemetry spikes (like a DDoS attack) without dropping logs on the floor like legacy on-prem hardware would.
│   ├── Dual Role (Brains & Brawn)
│   │   ├── SIEM (Brains / Security Information and Event Management): The analysis floor that collects feeds, sorts noise, and triggers alarms.
│   │   └── SOAR (Brawn / Security Orchestration, Automation, and Response): The automated robotic guards that execute immediate response workflows (e.g., instantly locking doors).
│   └── Unified interface functions
│       ├── Ingests raw surveillance feeds from every facility
│       ├── Stores historical records in deep archives
│       ├── Sets up automated alarm tripwires
│       ├── Cross-references traffic against VIP guest lists (Watchlists) and most-wanted boards (Threat Intelligence)
│       └── Merges seamlessly with frontline security guards to stop attacks at machine speed
├── Sentinel Command Center Divisions (built on an Azure Log Analytics Workspace)
│   ├── The Microsoft Sentinel SIEM Division (SIEM Operations Floor)
│   │   ├── Fast, active operations floor: daily threat hunting, incident triage, real-time alert correlation
│   │   ├── Fast, high-frequency security searches
│   │   ├── Operational retention window (30-730 days)
│   │   └── Powers real-time analytics & hunting
│   └── The Microsoft Sentinel Platform Division (Platform Data Lake)
│       ├── Long-term strategic archive (Massive Storage Pool)
│       ├── Data Lake: Scalable, low-cost storage repository holding raw data in its native format
│       ├── Low-cost long-term retention: logs for up to 12 years
│       ├── Graph-based correlation/analysis to uncover slow-moving, multi-year attack campaigns
│       └── Supports asynchronous KQL / Spark jobs
├── Workspace Architecture Options (navigating the "twin buzzsaws" of economics and compliance)
│   ├── Single-Tenant Single Workspace
│   │   ├── Central repository for all logs across the entire tenant (true uninterrupted single pane of glass)
│   │   └── Drawbacks: Runs into the twin buzzsaws of cloud economics (massive cross-region bandwidth egress fees) and data sovereignty laws (failing GDPR/local privacy laws).
│   ├── Single-Tenant Regional Workspaces
│   │   ├── Multiple localized control rooms deployed in specific regions
│   │   ├── Complies with Data Residency laws and eliminates cross-region fees
│   │   └── Drawback: Introduces operational friction; lacks a single native pane of glass (threat hunters are blind to the big picture).
│   └── Multi-Tenant Workspaces
│       └── Independent workspaces across distinct corporate tenants, managed centrally using Azure Lighthouse
├── Management Tools for Regional / Multi-Tenant Deployments
│   ├── Workspace Manager (solves configuration drift)
│   │   ├── A master broadcast system that centrally publishes analytics rules, saved searches, and playbooks out to all member workspaces at once.
│   │   └── Path: Microsoft Sentinel > Configuration > Settings > Workspace manager
│   └── Azure Lighthouse (solves the visibility problem)
│       └── A cross-tenant management service (like a master key ring for a property manager) allowing managed service providers (MSPs) to view and manage customer workspaces seamlessly without constantly signing in and out.
├── Creating a New Workspace (Azure portal)
│   ├── 1. Search for Microsoft Sentinel in the top search bar and select + Add
│   ├── 2. On the Add Microsoft Sentinel to a workspace screen, select + Create a new workspace
│   ├── 3. On the Basics tab, specify Subscription, Resource Group, workspace Name, and Region
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
├── Best practice: use the same Log Analytics workspace for both services
└── Limitation: the default workspace automatically provisioned by Defender for Cloud cannot be converted directly into a Sentinel workspace
├── 1. Manually create a custom Log Analytics workspace
├── 2. Update the Defender for Cloud pricing tier settings to point to the new workspace
└── 3. Enable Sentinel on that workspace
2. ROLE-BASED ACCESS CONTROL & WORKSPACE PERMISSIONS (THE ID BADGES)
├── Azure RBAC (role-based access control)
│   └── The system of printing specific "ID badges" that determine what doors staff can open and what buttons they can press in the command center.
├── Sentinel-Specific Azure RBAC Roles (built-in badges)
│   ├── Microsoft Sentinel Reader (Read-only badge)
│   │   └── Can look at camera feeds, data, incidents, workbooks; strictly look, don't touch (cannot clear an alarm).
│   ├── Microsoft Sentinel Responder (Tier 1 Triage badge)
│   │   └── Includes Reader rights + authority to manage the actual alarms (assign incidents, dismiss as false positives); manages the lifecycle of the threat but cannot build rules.
│   ├── Microsoft Sentinel Contributor (Architect badge)
│   │   └── Includes Responder rights + can completely build new alarm rules and dashboards (they own the SIEM).
│   └── Microsoft Sentinel Automation Contributor
│       └── Special service role allowing Sentinel itself to add playbooks to automation rules; not intended for human accounts.
├── Scope best practice: Assign roles at the Resource Group scope to ensure all supporting resources inherit matching permissions.
├── Additional Role Assignments for Specialized Tasks (The Missing Badges)
│   ├── Working with Playbooks (Separation of Duties)
│   │   ├── The Issue: A Sentinel Contributor cannot build automated robots by default. Why? Because playbooks run on a separate service (Logic Apps) and have destructive power (e.g., deleting users, shutting down firewalls).
│   │   ├── Logic App Contributor role is strictly required for human analysts to assign permissions to run automated scripts.
│   │   └── The administrator configuring this automation must hold Owner permissions on the resource group to delegate these rights.
│   ├── Connecting Data Sources
│   │   └── Write permissions on the target Sentinel workspace + source-specific permissions.
│   ├── Guest Users Assigning Incidents
│   │   └── Directory Reader role in Microsoft Entra ID + Microsoft Sentinel Responder rights.
│   └── Creating and Deleting Workbooks
│       └── Microsoft Sentinel Contributor OR a lower Sentinel role + Workbook Contributor (Azure Monitor role).
└── Broader Azure / Log Analytics Roles
├── Azure roles (Owner/Contributor/Reader at subscription level) and Log Analytics roles.
└── Caution: These grant permissions across all underlying Azure resources. Restrict them so analysts don't inadvertently edit external infrastructure.
3. DATA RETENTION, TABLE PLANS & LOG TIERS (STOPPING THE FINANCIAL BLEEDING)
├── Purpose: Surveillance feeds stream in constantly. Configure retention periods and data tiers to balance search performance against budget constraints.
├── 1. Configuring Workspace Retention
│   ├── Default interactive log retention at the workspace level: 30 days
│   ├── Extendable up to 730 days (2 years) for a prorated fee
│   ├── Microsoft Sentinel solution tables can extend interactive retention up to 90 days for free
│   └── Azure portal steps
│       ├── 1. Navigate to Microsoft Sentinel > Configuration > Settings
│       ├── 2. Select Workspace Settings (redirects to Log Analytics portal)
│       ├── 3. Select Usage and estimated costs > Data Retention > Drag slider (30-730 days) > OK
│       └── Flow: Sentinel Settings -> Workspace Settings -> Usage & Estimated Costs -> Data Retention Slider
├── 2. Log Data States and Table Plans (Paying for the right real estate)
│   ├── Three Table Plans
│   │   ├── Analytics Plan (The Hot State)
│   │   │   ├── Like paying premium real estate prices in downtown Manhattan.
│   │   │   ├── Suited for continuous monitoring, real-time alerts, and threat hunting.
│   │   │   └── Supports full KQL (Kusto Query Language, the language used to talk to the "robotic librarian") across interactive retention windows (30 days to 2 years).
│   │   ├── Basic Plan (The Warm State)
│   │   │   ├── Designed for bulk troubleshooting logs (e.g., ContainerLogV2).
│   │   │   └── Offers incredibly cheap ingestion, but data is kept only for 30 days and the robotic librarian is heavily restricted.
│   │   └── Auxiliary Plan (The Cold State)
│   │       ├── Very low touch; strictly for long-term auditing/compliance.
│   │       └── Low-cost ingestion, unoptimized single-table queries for 30 days.
│   └── Basic Logs KQL Constraints
│       ├── Supported operators: where, extend, project family, parse.
│       └── Unsupported operators (will fail on Basic Logs): join, union, summarize (cannot do complex multi-table correlation).
└── 3. Log Storage Tiers in the Microsoft Defender Portal
├── Managed from Microsoft Sentinel > Configuration > Tables (when onboarded to unified portal).
├── Defender Portal Log Tiers
│   ├── Analytics Tier (Hot state; 30 days to 2 years; supports all real-time rules).
│   ├── Data Lake Tier (Cold storage)
│   │   ├── Mirrors data for up to 12 years at a fraction of the cost.
│   │   └── Accessible via ad-hoc KQL/Spark jobs, but unavailable for real-time alerts.
│   └── XDR Default Tier (Default for Defender XDR hunting tables; 30 days included with license).
└── Modifying Table Retention Settings
├── 1. Microsoft Sentinel > Configuration > Tables > Select target table > Manage table
├── 2. Set Analytics retention (30 days to 2 years) and Total retention (up to 12 years).
│   └── Setting Total retention higher than Analytics retention automatically activates low-cost Data Lake cold storage.
└── Flow: Defender Portal > Configuration > Tables -> Select Table -> Manage Table -> Set Retention & Save
4. QUERYING THE INTELLIGENCE VAULT: LOGS, COMMON TABLES & DEFENDER XDR SCHEMAS
├── Analysts must know which "filing cabinet" (database table) holds specific evidence.
├── Query Entry Points
│   ├── Logs page in the Azure portal
│   ├── Advanced Hunting in the Defender portal
│   └── Data Lake Exploration in the Defender portal
├── Sentinel Feature-Specific Tables
│   ├── SecurityAlert (alerts generated by Sentinel's own analytic rules)
│   ├── SecurityIncident (official incident case files)
│   ├── ThreatIntelligenceIndicator (imported digital fingerprints)
│   └── Watchlist (imported reference VIP lists)
├── Common Infrastructure Tables (The translated logs)
│   ├── AzureActivity, AzureDiagnostics, AuditLogs, OfficeActivity, SecurityEvent, SigninLogs
│   └── CommonSecurityLog
│       └── Massive cabinet holding CEF (Common Event Format) messages sent over Syslog. It's how third-party non-Microsoft appliances (Cisco, Palo Alto) get translated into structured data.
└── Defender XDR Tables (The Raw Telemetry)
├── Streams directly when the Defender XDR connector is enabled; these are the "hyper-advanced guards" patrolling the actual endpoints and emails.
├── DeviceEvents / DeviceFileEvents / DeviceProcessEvents / DeviceNetworkEvents (Endpoint security control events, file creations, process trees)
├── EmailEvents / EmailUrlInfo / EmailAttachmentInfo (Microsoft 365 email delivery, blocked payloads, URLs)
├── IdentityLogonEvents / IdentityDirectoryEvents (AD authentication attempts)
└── The Power of the Robotic Librarian (KQL): You don't have to search one cabinet at a time. A single KQL query can track an intruder from an EmailEvent to a DeviceEvent to a CommonSecurityLog firewall block in milliseconds.
5. THE VIP GUEST LISTS: PLANNING, CREATING & MANAGING WATCHLISTS
├── Watchlists (path: Microsoft Sentinel > Configuration > Watchlist)
│   ├── External business data imported into the command center acting as a "VIP guest list" or "Do Not Enter blocklist."
│   └── Stored as key-value pairs inside the workspace database and cached in memory for subsecond query latency.
├── Common Watchlist Use Cases
│   ├── Rapid Incident Investigation (import malicious IPs/hashes to correlate against raw logs)
│   ├── High-Risk User Tracking (lists of recently fired employees or VIPs to trigger instant alarms)
│   ├── Reducing Alert Fatigue (allowlists of authorized admin IPs to suppress false alarms)
│   └── Data Enrichment (mapping IP addresses to physical building locations)
├── Creating a New Watchlist (Azure portal)
│   ├── General page: enter Name, Description, and Alias (the unique identifier used in KQL).
│   ├── Source page: select dataset type, upload CSV file, select Search key column.
│   │   └── THE STRICT TECHNICAL LIMIT: CSVs are strictly limited to a maximum 3.8 MB file size. Why? Because the entire list is loaded into the ultra-fast RAM cache. If it were larger, the cache would choke, and subsecond queries would take minutes.
│   └── Flow: Watchlist Page -> + Add New -> Name & Alias -> Upload CSV (<= 3.8MB) -> Select Search Key -> Create
├── Querying a Watchlist
│   ├── Call the _GetWatchlist('WatchlistAlias') function
│   └── Example: let HighValueHosts = _GetWatchlist('HighValueMachines') | project HostName; DeviceLogonEvents | where DeviceName in (HighValueHosts)
└── Watchlist Management Best Practices (Avoiding Ghost Records)
├── Editing Watchlist Items: Use the UI to edit individual rows directly.
├── Bulk Updating: Update watchlist > Bulk update > upload a new CSV file (appends new records and deduplicates).
└── The Ingestion SLA Warning: Log Analytics has a 5-minute data ingestion window. If you rapidly delete an old list and recreate a new one with the same name, the cache gets confused. Alarms will process both the old deleted entries and the new ones simultaneously, creating "duplicate ghosts." Always use Bulk Update instead.
6. THE MOST WANTED BOARD: TACTICAL THREAT INTELLIGENCE & INDICATOR MANAGEMENT
├── Cyber Threat Intelligence (CTI): The global "Most Wanted Board" identifying active threat campaigns.
│   └── Tactical TI = IOCs (Indicators of Compromise). Digital fingerprints like malicious URLs, hacker IPs, or malware file hashes.
├── Four Integration Pathways
│   ├── 1. Data Connectors (automated feeds via TAXII connectors or Upload Indicators API)
│   ├── 2. Threat Intelligence / Intel Management Page (view, tag, filter, create manually)
│   ├── 3. Analytics Rules (built-in templates that cross-reference incoming logs against IOCs automatically)
│   └── 4. Workbooks (visualize indicator matches)
├── Creating a Threat Indicator Manually
│   ├── Defender Portal > Threat Intelligence > Intel Management -> + Add New -> Select type (IP, URL, Hash) -> Fill Details -> Apply
│   └── Analysts apply Tags to group clues related to specific incidents or threat actors.
├── Threat Indicator Tables & The Deprecation Time Bomb
│   ├── The issue: Threat intelligence is no longer just flat lists of IPs; it's complex relational data.
│   ├── Legacy Table: ThreatIntelligenceIndicator
│   ├── Modern Tables: ThreatIntelIndicators and ThreatIntelObjects (using STIX—Structured Threat Information eXpression—a globally standardized grammar for cyber threats).
│   └── The Ticking Clock:
├── April 3, 2025: Public preview of modern tables (data streams to both).
├── July 31, 2025: HARD DEPRECATION DATE. Microsoft fully severs the feed to the legacy table.
└── Consequence: Any custom KQL queries, analytic rules, or playbooks hardcoded to the old table will query a dead table and go completely, silently blind. They MUST be manually migrated to the new STIX tables before the deadline.
7. TEARING DOWN THE WALLS: ONBOARDING SENTINEL TO THE MICROSOFT DEFENDER PORTAL
├── Unified Security Operations Platform ([https://security.microsoft.com/](https://security.microsoft.com/))
│   ├── Historically, teams constantly switched browser tabs between Azure (SIEM) and Defender (XDR).
│   ├── The Unified Portal fuses Sentinel (multicloud/third-party logs) and Defender XDR (endpoints/identities) into a single screen.
│   └── Capabilities: Unified Incident Queue, Unified Advanced Hunting (querying both natively in one window), Unified Entity Pages.
├── Onboarding Prerequisites
│   ├── Active Log Analytics workspace with Sentinel enabled
│   ├── Defender XDR data connector enabled in Sentinel
│   ├── Defender XDR onboarded to the Microsoft Entra tenant
│   └── RBAC: Subscription Owner/User Access Administrator PLUS Sentinel Contributor on the workspace.
├── Onboarding Workflow
│   └── Defender Portal Home -> Connect a Workspace Banner -> Select Workspace -> Review Changes -> Connect
├── Automatic Architectural Changes Upon Connection (Under the Hood)
│   ├── 1. Sentinel log tables/functions become accessible inside Defender's Advanced Hunting.
│   ├── 2. Microsoft Sentinel Contributor role is auto-granted to Microsoft Threat Protection system apps.
│   ├── 3. Active Microsoft security incident creation rules in Sentinel are deactivated (prevents duplicate tickets in both systems).
│   └── 4. The Legacy Fusion Rule is Disabled: Sentinel's native machine-learning correlation detective is instantly turned off. Defender XDR's correlation engine (which has better endpoint context) entirely takes over the job so the two don't fight over clues.
└── Portal Capability Differences (The Load-Bearing Pillars)
├── The Multi-Tenant Limit (Critical for MSPs): Defender portal currently only supports ONE primary Sentinel workspace per tenant. You lose the Azure Lighthouse single-pane-of-glass visibility for 20+ clients in the new unified portal.
├── Azure Portal ONLY: Advanced Hunting Bookmarks, editing incident comments, incident tasks, adding entities to TI from incidents.
├── Defender Portal ONLY: Automatic Attack Disruption for SAP Applications, alert removal (only by linking to another incident).
├── Hidden Connectors: Native Defender connectors are hidden from the Sentinel Data Connectors page in Defender (data flows natively).
└── Programmatic Sync limits: Incidents created via API/Logic Apps in Azure do NOT sync over to Defender.

MASTER MAP: CONFIGURING AND OPERATING MICROSOFT SENTINEL
├── 1. CENTRAL COMMAND ARCHITECTURE
│   ├── Microsoft Sentinel = Master Command Center; SIEM (Brains/Analysis) + SOAR (Brawn/Robotic Guards); built on a Log Analytics Workspace.
│   ├── Divisions: SIEM Operations Floor (Hot, 30-730 days) + Platform Data Lake (Cold, up to 12 years).
│   ├── Topologies: Single Workspace (Twin buzzsaws: egress costs & data residency limits), Regional Workspaces (no single pane), Multi-Tenant Workspaces.
│   └── Tools: Workspace Manager (broadcasts rules) and Azure Lighthouse (master key ring for MSPs).
├── 2. RBAC KEYS (THE ID BADGES)
│   ├── Built-in Roles: Sentinel Reader (Look), Responder (Manage lifecycle), Contributor (Build rules/Own the SIEM).
│   └── The Missing Badge: Logic App Contributor is strictly required to build/run SOAR Playbooks due to separation of duties (preventing destructive automation abuse).
├── 3. DATA RETENTION TIERS (STOPPING FINANCIAL BLEEDS)
│   ├── Workspace settings: 30 days default, up to 730 days.
│   ├── Table plans: Analytics (Premium real estate/Full KQL), Basic (Discounted/No joins/Warm state), Auxiliary (Cold state/Compliance).
│   └── Defender portal tiers: Analytics Tier (mirrored to data lake), Data Lake Tier (cold storage), XDR Default Tier (30 days included).
├── 4. INTELLIGENCE VAULT (THE FILING CABINETS)
│   ├── Feature Tables: SecurityAlert, SecurityIncident, ThreatIntelligenceIndicator, Watchlist.
│   ├── Translated Logs: CommonSecurityLog (CEF/Syslog translations).
│   ├── Raw Telemetry: Defender XDR tables (DeviceEvents, EmailEvents).
│   └── The Robotic Librarian: KQL joins data across all cabinets instantly.
├── 5. VIP GUEST LISTS (WATCHLISTS)
│   ├── CSV imports up to strictly 3.8 MB (to prevent RAM cache choking). Queried via _GetWatchlist('Alias').
│   └── Management: Use Bulk Update. Avoid deleting/recreating rapidly to prevent "Duplicate Ghosts" due to the 5-minute ingestion SLA.
├── 6. MOST WANTED BOARD (THREAT INTELLIGENCE)
│   ├── Integrating IOCs (digital fingerprints) via TAXII/APIs.
│   └── The July 2025 Time Bomb: Hard deprecation of legacy `ThreatIntelligenceIndicator` table. Must migrate custom queries and playbooks to STIX-compliant `ThreatIntelIndicators` & `ThreatIntelObjects` or go silently blind.
└── 7. TEARING DOWN WALLS (UNIFIED PLATFORM IN DEFENDER)
├── Single screen fusion of Sentinel & Defender XDR ([https://security.microsoft.com/](https://security.microsoft.com/)).
├── Automatic shutdowns: Legacy Fusion rule disabled (XDR correlation takes over); MS incident rules deactivated to stop duplicates.
└── Operational Limits: Azure ONLY (Bookmarks); Defender ONLY (SAP Disruption); MSP limit (Only one primary workspace supported per tenant in Defender, breaking Lighthouse multi-tenant views).
```