```
NOTE: Module 6's notes contained no existing Plaintext tree graphs (only box/flow diagrams), so the tree graphs below are built from scratch in the same numbered-section + Master Map format, covering 100% of the raw text.

1. AGENCY HEADQUARTERS & ARCHITECTURE: SENTINEL SIEM, DATA LAKES & MULTI-TENANT MANAGEMENT
├── The Big Picture
│   ├── Scenario: managing global security for an enterprise with thousands of office buildings across every continent
│   │   ├── Millions of employees swipe badges, open doors, access cloud servers, and send messages every second
│   │   └── Problem: a break-in at a remote facility or a rogue insider slowly exfiltrating confidential files over several months is never spotted by local guards watching individual camera screens
│   ├── Solution: construct a Global Intelligence Agency = Microsoft Sentinel (a cloud-native security platform that aggregates, analyzes, and responds to threat data across the entire enterprise network)
│   ├── Dual role
│   │   ├── SIEM (security information and event management, a centralized system that ingests and analyzes security logs from across the entire network)
│   │   └── SOAR (security orchestration, automation, and response, an automated computing framework that executes immediate response workflows against active threats)
│   └── Agency operations
│       ├── Central command floor ingests massive streams of raw surveillance data from every facility worldwide
│       ├── Robotic librarian translates foreign firewall dialects into a single universal language
│       ├── Automated analytical tripwires catch threat actors
│       └── Automated robotic task forces neutralize high-speed attacks before damage spreads
├── Sentinel Dual-Storage Architecture (two distinct data management divisions)
│   ├── The Microsoft Sentinel SIEM Operations Floor (Log Analytics Workspace)
│   │   ├── Built directly on top of a Log Analytics Workspace (an Azure database environment where log data is collected, stored, and queried)
│   │   ├── Handles day-to-day threat hunting, incident investigation, and real-time alert correlation
│   │   ├── Fast, high-frequency security searches; powers real-time analytics & hunting
│   │   ├── Data resides in high-performance storage tiers (such as the Analytics tier)
│   │   └── Operational retention window: 30 days by default (configurable up to 730 days)
│   └── The Microsoft Sentinel Platform Data Lake (Massive Storage Pool)
│       ├── Data Lake (a scalable, low-cost storage repository that holds raw data in its native format until needed for long-term analysis)
│       ├── Low-cost long-term retention: historical logs for up to 12 years
│       ├── Graph-based correlation / analysis to uncover slow-moving, multi-year attack campaigns
│       ├── Supports asynchronous search jobs
│       └── Cost-optimized Basic and Data Lake storage tiers
├── Deployment Topologies (chosen based on organizational structure and compliance needs)
│   ├── Single Tenant with Single Workspace (centralized command room; all security logs flow into one workspace managed by a single SOC team)
│   ├── Regional Workspaces (dedicated workspaces in specific countries or regions to comply with local Data Residency rules)
│   │   └── Data Residency (legal regulations specifying that data collected in a region must remain stored within that region)
│   └── Multi-Tenant Architecture (independent workspaces serving distinct subsidiary companies or managed customers)
├── Multi-Workspace / Multi-Tenant Management
│   ├── Workspace Manager (a centralized tool that publishes analytics rules, saved searches, and automation policies across multiple workspaces at once)
│   └── Azure Lighthouse (a cross-tenant management service that lets analysts view and operate customer Sentinel environments without constantly logging in and out of separate corporate accounts)
│       └── Used by managed security providers overseeing multiple enterprise tenants
└── Unified Security Operations Platform
    ├── Enterprise integration merging Microsoft Sentinel and Microsoft Defender XDR into a single web portal: https://security.microsoft.com/
    │   └── Microsoft Defender XDR (extended detection and response, an integrated suite that protects endpoints, identities, email, and cloud apps)
    ├── When unified, incidents synchronize bi-directionally between platforms
    └── Defender XDR assumes primary responsibility for correlating raw alerts into unified incidents, so Sentinel's legacy Fusion Analytics Rule (a machine-learning engine that stitches multi-stage attack signals together) is automatically disabled to prevent duplicate incident creation

2. WIRING THE GLOBAL NETWORK: SOLUTIONS, DATA CONNECTORS, ARC, AMA & SYSLOG/CEF
├── Data Ingestion Pipeline (raw telemetry flows continuously from every facility, server, and firewall into the central command room)
│   ├── 1. CONTENT HUB (path: Microsoft Sentinel > Content management > Content hub)
│   │   ├── Digital hardware store for downloading pre-packaged Solutions
│   │   └── Solutions (bundled integration kits containing data connectors, parsing rules, visual workbooks, and analytic tripwires)
│   │       └── Deploy packaged Solution (Connectors + Rules + Workbooks)
│   └── 2. SOURCE CONTROL
│       └── Repositories (such as GitHub or Azure DevOps) version-control detection code and infrastructure configurations; sync custom KQL & rules and automate deployments
├── Data connectors pull logs into native database Tables
│   └── Tables (structured storage units like SecurityAlert, SecurityIncident, SigninLogs, and AzureActivity)
├── Five Core Connector Methods
│   ├── 1. Native Service-to-Service Connectors (connect Microsoft cloud services with a single toggle)
│   │   ├── Microsoft Entra ID (ingests audit logs, interactive user sign-ins, non-interactive sign-ins, and service principal logs)
│   │   ├── Azure Activity (captures management-plane ARM operations performed across Azure subscriptions)
│   │   ├── Microsoft Defender for Cloud (two cabling options)
│   │   │   ├── Legacy Subscription-based Connector (dumps raw security alerts into the SecurityAlert table)
│   │   │   └── Modern Tenant-based Connector (integrates defender alerts directly into unified incident queues)
│   │   └── Microsoft Defender XDR Connector
│   │       ├── Streams high-fidelity alerts into Sentinel for long-term correlation
│   │       └── Also streams raw Advanced Hunting telemetry tables: DeviceEvents, DeviceFileEvents, DeviceProcessEvents, DeviceNetworkEvents
│   ├── 2. Windows Desk Surveillance (AMA + DCR + Azure Arc)
│   │   ├── AMA (Azure Monitor Agent, lightweight tracking software that collects security logs and operating system events)
│   │   ├── Azure Arc (a software bridge that projects non-Azure and on-premises servers into ARM as managed resources)
│   │   │   └── Used to onboard computers living outside Azure's boundary
│   │   └── DCR (Data Collection Rule, a cloud-managed policy object that specifies exactly which event IDs to collect and which to ignore)
│   │       ├── Prevents network congestion
│   │       └── Ingestion tiers
│   │           ├── All Events
│   │           ├── Common (standard audit trails like logon 4624, logoff 4634, and group modifications)
│   │           ├── Minimal (critical breach indicators)
│   │           └── Custom (explicit KQL filtering statements)
│   ├── 3. Foreign Firewall Translation (Syslog vs. CEF)
│   │   ├── Syslog (a standard event-logging protocol used by Unix systems and network devices)
│   │   │   └── Third-party networking appliances and Linux firewalls don't speak native Microsoft formats; they communicate using Syslog
│   │   ├── Syslog Forwarder (a dedicated Linux virtual machine that acts as a translation booth between third-party appliances and Sentinel)
│   │   ├── Raw Syslog Pipeline
│   │   │   ├── Foreign appliance -> raw text logs over TCP/UDP port 514 -> Linux forwarder (AMA)
│   │   │   ├── Internal listening software routes text to the local AMA agent on TCP port 28330
│   │   │   ├── AMA encrypts and transmits data to Sentinel over TCP port 443 (HTTPS)
│   │   │   └── Arrives as an unparsed block of text in the Syslog table, requiring custom KQL functions to parse
│   │   └── CEF (Common Event Format) Pipeline
│   │       ├── Preferred for enterprise firewalls (e.g., Palo Alto, Check Point, Fortinet)
│   │       ├── Structures data into key-value pairs before transmission
│   │       ├── Flows through the Linux forwarder on TCP port 25226
│   │       └── Arrives in the CommonSecurityLog table with pre-parsed columns (e.g., DeviceAction, SourceIP, DestinationIP, RequestURL)
│   ├── 4. Custom and API Ingestion (proprietary web apps or unmonitored cloud tools)
│   │   ├── CCP (Codeless Connector Platform, a configuration framework that builds REST API data connectors without writing custom code)
│   │   └── Log Analytics Ingestion API (send JSON payloads directly)
│   └── 5. Threat Intelligence Live Feeds
│       ├── Automated threat feeds wired into the ThreatIntelligenceIndicator table
│       ├── TAXII (Trusted Automated eXchange of Intelligence Information, an automated protocol used to exchange cyber threat intelligence) connectors
│       └── Upload Indicators API
└── Connector Summary Map
    ├── Native M365 / Azure -> Entra ID, Azure Activity, Defender XDR Telemetry Tables
    ├── Windows Endpoints -> Azure Arc + AMA + DCR (Filters)
    ├── Linux / Firewalls -> Syslog Forwarder (Port 514) -> AMA (Port 28330) -> Port 443; CEF Forwarder (Port 25226)
    └── Custom / Third-Party -> CCP / Ingestion API / TAXII

3. THE ROBOTIC LIBRARIAN'S CONVEYOR BELT: FUNDAMENTAL KQL SYNTAX & ROW FILTERING
├── KQL (Kusto Query Language, a read-only query language designed to search, filter, and analyze massive volumes of structured and unstructured log data)
│   ├── Millions of new data records arrive every minute; analysts rely on the robotic librarian rather than reading logs line by line
│   └── Read-only data-flow model: data passes through sequential query operators bound together by the Pipe (|) symbol (a physical conveyor belt passing transformed results from left to right)
├── Example flow: Database Table SecurityEvent -> | where EventID == 4624 (keep only logon events) -> | where AccountType =~ "user" (case-insensitive user check) -> | project TimeGenerated, Account, Computer (select specific columns)
├── search
│   ├── Broad, multi-table and multi-column search operator
│   ├── Simple to run but computationally inefficient compared to targeted filtering
│   └── Supports wildcards (e.g., search in (SecurityEvent, SecurityAlert, A*) "error")
├── where (strict filtering operator; evaluates rows against predicate conditions, discarding non-matching records immediately to optimize query performance)
│   ├── Time filtering using ago() (e.g., | where TimeGenerated > ago(7d))
│   ├── Exact matching using == and case-insensitive matching using =~ (e.g., | where AccountType =~ "user")
│   └── Membership filtering using in or !in (e.g., | where EventID in (4624, 4625))
├── let (binds a user-defined name to a scalar value, array, or tabular expression; improves code modularity and readability)
│   └── Example: let timeOffset = 7d; let DiscardID = 4688; let SuspiciousUsers = datatable(Account: string) [@"CONTOSO\Administrator", @"NT AUTHORITY\SYSTEM"]; SecurityEvent | where TimeGenerated > ago(timeOffset * 2) and TimeGenerated < ago(timeOffset) | where EventID != DiscardID | where Account in (SuspiciousUsers)
├── project Family (controls column output formatting)
│   ├── project (selects specific columns to include, renames columns, or inserts new computed fields while dropping unlisted columns)
│   ├── project-away (specifies columns to exclude from the output table)
│   ├── project-keep (explicitly defines columns to preserve)
│   ├── project-rename (renames specific column headers without altering the rest of the table)
│   └── project-reorder (reorders output columns sequentially)
└── order by / sort by
    ├── Sorts output rows by one or more columns
    ├── asc (ascending) or desc (descending)
    └── Default is descending

4. DATA TRANSFORMATION & AGGREGATION: EXTEND, SUMMARIZE & RENDER
├── Flow: Raw Event Logs -> extend (calculate new fields) -> summarize (group & aggregate) -> render (draw visual graph)
├── 1. extend (creates calculated or transformed columns and appends them to the tabular result set)
│   ├── Example 1: SecurityEvent | where ProcessName != "" and Process != "" | extend StartDir = substring(ProcessName, 0, string_size(ProcessName) - string_size(Process))
│   │   └── Calculates the starting directory by extracting a substring based on string length
│   └── Example 2: | extend SeverityRank = case(EventID == 4624, "Low", EventID == 4625, "Medium", "High")
├── 2. summarize (groups rows sharing matching values in the by clause and calculates mathematical aggregations across those groups)
│   ├── count() / countif() (total row count per group, or row count matching a specific condition; e.g., summarize FailedLogons = countif(EventID == 4625) by Account)
│   ├── dcount() / dcountif() (estimated count of distinct/unique values within a group, ignoring duplicates; e.g., summarize UniqueIPs = dcount(IpAddress) by UserPrincipalName)
│   ├── avg(), min(), max(), sum(), percentile(), stdev(), variance() (standard mathematical and statistical metrics)
│   ├── arg_max() / arg_min() (returns the full record containing the maximum or minimum value for a specified column within each group)
│   │   └── Example: | summarize arg_max(TimeGenerated, *) by Account returns the most recent event row for every account
│   ├── make_list() (bundles all values of a scalar expression in the group into a dynamic JSON array, preserving duplicates)
│   ├── make_set() (bundles all values of a scalar expression in the group into a dynamic JSON array containing distinct/unique values only)
│   └── Real-World Detection Query: brute-force invalid passwords across multiple apps
│       └── let timeframe = 30d; let threshold = 3; SigninLogs | where TimeGenerated >= ago(timeframe) | where ResultDescription has "Invalid password" | summarize AppCount = dcount(AppDisplayName), TargetedApps = make_set(AppDisplayName) by UserPrincipalName, IPAddress | where AppCount >= threshold
└── 3. Time Series and Visualizations
    ├── bin() (groups timestamps into discrete time buckets)
    ├── render (operator that turns aggregated time series into visual graphs)
    ├── Example: SecurityEvent | where TimeGenerated > ago(7d) | summarize EventCount = count() by bin(TimeGenerated, 1h), Activity | render timechart
    └── Supported render visualizations: timechart, barchart, columnchart, piechart, areachart, scatterchart

5. CONNECTING MULTIPLE INTELLIGENCE DRAWERS: TABLE UNIONS & JOIN FLAVORS
├── Rationale: threat actors rarely confine their footprint to a single service (phishing click in email logs -> account sign-in failure -> database execution); the librarian uses unions and joins to combine data across multiple tables
├── 1. union (takes two or more tables and appends their rows into a single combined dataset)
│   ├── Supports wildcards (e.g., union Security* merges all tables whose names start with "Security")
│   ├── withsource=ColumnName (creates a calculated column identifying which original table each row came from)
│   └── Example: union withsource = SourceTable SecurityEvent, SigninLogs, AuditLogs | summarize EventCount = count() by SourceTable
└── 2. join (merges rows from two tables [LeftTable and RightTable] into a single wide output table by matching key column values)
    ├── Matching syntax: on $left.Attribute == $right.Attribute
    ├── Syntax: LeftTable | join kind = [JoinFlavor] ( RightTable ) on KeyAttribute
    ├── KQL Join Flavors
    │   ├── innerunique (Default; matches one row from the left table for each key value, joining matching right-table rows)
    │   ├── inner (outputs a row for EVERY combination of matching records between left and right tables)
    │   ├── leftouter (retains ALL rows from the left table; unmatched cells from the right table populate with null values)
    │   ├── rightouter (retains ALL rows from the right table; unmatched cells from the left table populate with null values)
    │   ├── fullouter (retains ALL rows from both tables; unmatched cells on either side populate with null values)
    │   ├── leftsemi (returns all rows from the left table that have a matching record in the right table; right columns NOT appended)
    │   ├── rightsemi (returns all rows from the right table that have a matching record in the left table)
    │   ├── leftanti / leftantisemi (returns all rows from the left table that DO NOT have a matching record in the right table)
    │   └── rightanti / rightantisemi (returns all rows from the right table that DO NOT have a matching record in the left table)
    └── Example: correlate user logon events (4624) with logoff events (4634)
        └── SecurityEvent | where EventID == 4624 | summarize LogOnCount = count() by Account | join kind = inner ( SecurityEvent | where EventID == 4634 | summarize LogOffCount = count() by Account ) on Account | project Account, LogOnCount, LogOffCount

6. UNLOCKING UNSTRUCTURED FILES & EXTERNAL INTELLIGENCE: EXTRACTION, JSON, WATCHLISTS & THREAT INTEL
├── Problem: security telemetry frequently arrives as unstructured text strings or nested JSON objects; targeted parsing functions extract key-value pairs
├── 1. Parsing Unstructured Text Strings
│   ├── extract() (uses Regex [regular expressions] to capture a specific substring from a text field and optionally convert its data type)
│   │   └── Example: SecurityEvent | where EventID == 4672 and AccountType == 'User' | extend AccountName = extract(@"^(.*\\)?([^@]*)(@.*)?$", 2, tolower(Account)) (extracts account name from domain\username using regex capture group 2)
│   └── parse (evaluates a string expression and parses it into calculated columns using a template pattern)
│       ├── simple (default; strict string match)
│       ├── regex (uses regex delimiters)
│       ├── relaxed (non-strict parsing that inserts nulls if types mismatch)
│       └── Example: Traces | parse EventText with * "resourceName=" ResourceName ", totalSlices=" TotalSlices:long * "lockTime=" LockTime:date ")" * | project ResourceName, TotalSlices, LockTime
├── 2. Parsing Structured JSON and Dynamic Fields (many log sources store nested key-value pairs inside fields of type Dynamic)
│   ├── Dot Notation (accesses nested keys directly, e.g., SigninLogs | extend OS = DeviceDetail.operatingSystem)
│   ├── parse_json() / todynamic() (converts a raw JSON string into a queryable dynamic object bag)
│   ├── mv-expand (expands a dynamic JSON array so each array element becomes a separate output row, duplicating all non-array row values)
│   ├── mv-apply (applies a subquery to each element of an expanded array individually before unioning the results)
│   └── Example: SigninLogs | extend AuthDetails = parse_json(AuthenticationDetails) | mv-expand AuthDetails | extend AuthMethod = tostring(AuthDetails.authenticationMethod) | extend AuthResult = tostring(AuthDetails.authenticationStepResultDetail) | project TimeGenerated, UserPrincipalName, AuthMethod, AuthResult
└── 3. Integrating External Reference Data
    ├── externaldata
    │   ├── Reads external files directly from Azure Blob Storage or Azure Data Lake Storage using a SAS token URL
    │   └── Example: Users | where UserID in ((externaldata (UserID: string) [@"https://storageaccount.blob.core.windows.net/container/users.txt?sp=r&st=...SAS..."]))
    ├── Watchlists (curated CSV reference lists uploaded directly into Sentinel)
    │   ├── Path: Microsoft Sentinel > Configuration > Watchlists
    │   ├── Queried using the _GetWatchlist('WatchlistAlias') function
    │   ├── Uses: suppress false positives (e.g., skipping trusted IP ranges) or catch high-risk accounts (e.g., alert when a user on the TerminatedEmployees watchlist logs in)
    │   └── Example: let TerminatedList = _GetWatchlist('TerminatedEmployees') | project UserPrincipalName; SigninLogs | where UserPrincipalName in (TerminatedList)
    └── Threat Intelligence
        ├── Threat indicators (IPs, domains, file hashes) feed into the ThreatIntelligenceIndicator table
        └── Analytics rules automatically correlate incoming logs against known adversary infrastructure

7. AUTOMATED ALARM TRIPWIRES: ANALYTICS RULES & ANOMALY DETECTION
├── Analytics Rules (path: Microsoft Sentinel > Configuration > Analytics)
│   └── Automated tripwires that continuously query ingested logs and raise security alerts when specific threat patterns are detected
├── Sentinel Analytics Rule Types
│   ├── Scheduled Rules (custom or template-based KQL queries running on a strict timer, e.g., every 5 minutes looking back 5 minutes; generates alerts & incident tickets)
│   ├── NRT Rules (Near-Real-Time rules running continuously with sub-minute latency to catch high-speed attacks)
│   ├── Anomaly Rules (machine-learning algorithms that flag behavioral deviations)
│   │   ├── Saves clues to the Anomalies table
│   │   └── Does NOT create noisy incident tickets directly
│   └── Microsoft Security Rules (automatically create Sentinel incidents from alerts raised by connected Defender solutions)
└── Scheduled Analytics Rule: 5-Step Wizard
    ├── 1. General
    │   ├── Rule Name, Description
    │   ├── Tactics and Techniques (mapped to MITRE ATT&CK)
    │   └── Severity (High, Medium, Low, Informational)
    ├── 2. Set Rule Logic
    │   ├── Rule Query (enter the KQL detection query)
    │   ├── Entity Mapping (maps KQL output columns to standard Sentinel Entities)
    │   │   ├── e.g., UserPrincipalName -> Account, IPAddress -> IP, HostName -> Host
    │   │   └── Vital for visual investigation graphs and UEBA profiling
    │   ├── Custom Details (extracts query columns into alert metadata properties)
    │   ├── Alert Details (dynamically overrides alert name or description using query outputs, e.g., High volume logon failures for {{UserPrincipalName}})
    │   ├── Query Scheduling (how frequently the query runs, e.g., every 1 hour, and the historical lookback window, e.g., past 1 hour)
    │   ├── Alert Threshold (generate an alert when query row output count is Greater than 0, or a custom threshold)
    │   ├── Event Grouping (group all matched rows into a Single alert OR raise An alert for each event)
    │   └── Suppression ("Stop running query after alert is generated" toggle pauses rule execution for a defined window after an alert fires, preventing duplicate alerts)
    ├── 3. Incident Settings
    │   ├── Controls whether alerts automatically create official Incidents in the queue
    │   └── Alert Grouping (groups related alerts triggered within a configurable time window, up to 7 days, into a single incident based on matching entities, matching rule names, or all alerts)
    ├── 4. Automated Response (attach Automation Rules or Playbooks to run automatically when the rule triggers)
    └── 5. Review and Create (validates rule logic and deploys the tripwire)

8. AUTOMATED ROBOTIC TASK FORCES: SOAR, AUTOMATION RULES & LOGIC APP PLAYBOOKS
├── Two-tier SOAR automation engine: Automation Rules (the dispatch desk) and Playbooks (the robotic task force)
├── Flow: Incident / Alert Triggered -> Dispatch Desk (Automation Rules) -> Robotic Task Force (Logic App Playbooks)
├── 1. The Dispatch Desk: Automation Rules
│   ├── Path: Microsoft Defender portal > Automation OR Sentinel > Automation
│   ├── Manage incident metadata and orchestrate automated workflows; central triage desk evaluating newly created or updated incidents
│   ├── Key capabilities
│   │   ├── Change incident status (Active, Closed), severity, or classification (True Positive, False Positive, Benign Positive)
│   │   ├── Assign incident ownership to specific human analysts or security groups
│   │   ├── Add operational tags (e.g., Ransomware, FIN7)
│   │   ├── Run automated Playbooks in a specified sequential order
│   │   └── Dispatch Robotic Task Force (execute playbook)
│   └── Evaluation order controlled by a numeric Priority field (lower numbers execute first; Priority 1..N)
└── 2. The Robotic Task Forces: Logic App Playbooks
    ├── Automated response workflows built on Azure Logic Apps (Microsoft's cloud workflow automation service; drag-and-drop designer)
    ├── Execute step-by-step remediation tasks across cloud and on-premises systems
    ├── Triggers (the event that initiates playbook execution)
    │   ├── "When a Microsoft Sentinel Incident creation rule was triggered" (fires when an incident is generated; full access to incident entities)
    │   ├── "When a Microsoft Sentinel Alert was triggered" (fires when an individual alert is raised)
    │   └── "When a Microsoft Sentinel Entity was triggered" (analysts manually trigger a playbook against a specific entity, e.g., a user account or IP, from the investigation screen)
    ├── Actions (automated operational steps executed by API connectors)
    │   └── Examples: disable a compromised user account in Microsoft Entra ID, isolate an endpoint in Defender for Endpoint, add a malicious IP to a Palo Alto firewall blocklist, send an executive summary to a Microsoft Teams channel
    └── Permissions: Managed Identity permissions or service principal connections required to execute administrative actions across connected Azure and Entra resources

9. INCIDENT INVESTIGATION & ENTITY PROFILING: HIERARCHY, UEBA & ASIM
├── 1. The Incident Hierarchy (four nested levels)
│   ├── Events (individual, raw log lines ingested from data sources, e.g., Event ID 4625 in SecurityEvent)
│   ├── Alerts (specific moments where an analytics rule identified suspicious events and triggered a warning)
│   ├── Entities (real-world actors and assets: Accounts, Hosts, IPs, URLs, Azure Resources, Mailboxes, File Hashes)
│   ├── Incidents (central case file aggregating related alerts, events, and entities into a single investigation workspace)
│   └── Flow: Events (Raw Logs) -> Alerts (Tripwires) -> Entities (Actors/Assets) -> Incident Case File
├── 2. UEBA (User and Entity Behavior Analytics; path: Microsoft Sentinel > Entity behavior)
│   ├── Builds dynamic behavioral baselines for users and hosts over time (typical sign-in locations, working hours, accessed devices, peer group associations)
│   ├── Detects anomalous behavior that bypasses static rules (e.g., login at 3:00 AM from a new country and first-time access to a sensitive database)
│   ├── Assigns an elevated Investigation Priority Score and logs anomalies into the Anomalies table
│   └── Entity Page (interactive timeline of user alerts, anomalies, sign-in locations, and organizational relationships)
├── 3. ASIM (Advanced Security Information Model, an enterprise data normalization framework that standardizes varying vendor schemas into unified fields)
│   ├── Problem: different vendors use different column names for the same data (e.g., TargetUserName vs. user_name vs. dst_user)
│   ├── Pre-built workspace-level Unifying Parsers starting with the im prefix
│   │   ├── imAuthentication (normalizes all authentication logs across vendors into standard fields like TargetUserPrincipalName and SrcIpAddr)
│   │   ├── imProcessCreate (normalizes process creation events across Windows, Linux, and endpoint security agents)
│   │   ├── imNetworkSession (normalizes network connections across cloud and on-premises firewalls)
│   │   └── imDns, imWeb, imFileEvent (additional standardized schemas)
│   └── Example: imAuthentication | where TimeGenerated > ago(24h) | where EventResult == "Failure" | summarize FailedCount = count() by TargetUserPrincipalName, SrcIpAddr, EventVendor
└── 4. Workspace Transformation DCRs (Data Collection Rules that execute KQL transformations on incoming telemetry right at the ingestion pipeline)
    ├── Clean up or filter messy logs BEFORE they are written to disk
    ├── Drop unnecessary columns, mask sensitive PII data, or filter out high-volume benign events
    └── Significantly reduces Log Analytics ingestion costs

10. PROACTIVE FIELD OPERATIONS: THREAT HUNTING, BOOKMARKS, SEARCH JOBS & PYTHON NOTEBOOKS
├── Threat Hunting (path: Microsoft Sentinel > Threat management > Hunting)
│   ├── Proactive practice of searching for undetected adversaries (contrasts with reactive incident response)
│   └── Rationale: sophisticated actors use stealthy techniques to blend into normal network traffic and don't trigger automated alarms
├── The Threat Hunting Cycle
│   ├── 1. Formulate Hypothesis
│   ├── 2. Run Hunting Queries (KQL searches across raw telemetry)
│   ├── 3. Preserve Clues via Bookmarks (tag entities & build Entity Graph)
│   ├── 4. Monitor Live Stream (test queries in real time against active traffic)
│   ├── 5. Deep Historical Search (Search Jobs, Data Restoration, Data Lake)
│   └── 6. Forensic Crime Lab (Jupyter Notebooks + Python + MSTICPy)
├── 1. Formulating a Hunting Hypothesis (a testable hunch about how an adversary might be operating inside the network)
│   ├── Achievable (scope is realistic and manageable)
│   ├── Narrow in Scope (targeted at specific dataset variables rather than generic queries)
│   ├── Time-Bound (focused on a defined historical window, e.g., past 24 to 72 hours)
│   ├── Useful and Efficient (actionable security value without excessive noise)
│   └── Aligned to Threat Model (targets techniques known to be used by adversaries threatening the organization's specific industry)
├── 2. Hunting Tools and Clue Preservation
│   ├── Hunting Queries (pre-built KQL queries mapped to MITRE ATT&CK Tactics and Techniques)
│   ├── Bookmarks
│   │   ├── Saved when a hunting query uncovers a suspicious log entry
│   │   ├── Preserve the specific log record, query syntax, and analyst notes
│   │   ├── Tagging entities within a bookmark maps clues onto the visual Entity Graph
│   │   └── Can be escalated into a formal security incident if malicious activity is confirmed
│   └── Live Stream (runs a KQL hunting query continuously against incoming telemetry in real time; desktop notification the moment a matching event occurs)
├── 3. Deep Historical Archives (terabytes of cold or archived data across long time horizons)
│   ├── Search Jobs (asynchronous background search queries that don't time out or lock up the UI; results populate a dedicated search table ending in _SRCH)
│   ├── Data Restoration (restores archived logs back into active, queryable Log Analytics tables for temporary deep analysis)
│   └── Data Lake KQL Jobs (background analytical tasks across long-term Data Lake storage tiers)
└── 4. The Forensic Crime Lab: Jupyter Notebooks and MSTICPy
    ├── Notebooks (path: Microsoft Sentinel > Threat management > Notebooks)
    │   ├── Interactive coding environment running Python (a popular programming language for data science and security analysis)
    │   ├── Hosted in an Azure Machine Learning Workspace
    │   └── Backed by cloud Compute Clusters (scalable clusters of virtual machines equipped with machine learning libraries like PyTorch and TensorFlow)
    ├── KQLmagic (Python library that executes KQL queries directly inside Jupyter Notebook cells and passes resulting data frames into libraries such as pandas and matplotlib)
    ├── MSTICPy (Microsoft Threat Intelligence Python Security Tool)
    │   └── Microsoft-developed Python package for threat hunting: IP reputation lookups, threat intelligence enrichment, log parsing, cyber-visualization (timeline charts and process trees), malware analysis
    └── Desktop & AI Integration
        ├── Execute notebooks locally using Jupyter Notebooks or Visual Studio Code
        ├── GitHub Copilot (an AI coding assistant)
        └── MCP (Model Context Protocol, an open standard protocol that connects external IDE code editors directly to secure Sentinel backend databases)

MASTER MAP: MITIGATING THREATS WITH MICROSOFT SENTINEL
├── 1. AGENCY ARCHITECTURE (SIEM + SOAR)
│   ├── Microsoft Sentinel (cloud-native security platform) = Global Intelligence Agency; SIEM + SOAR
│   ├── Dual storage: SIEM Operations Floor (Log Analytics Workspace, 30 days default up to 730) + Platform Data Lake (up to 12 years, graph-based, Basic and Data Lake tiers, search jobs)
│   ├── Topologies: Single Tenant/Single Workspace, Regional Workspaces (Data Residency), Multi-Tenant
│   ├── Workspace Manager (publish rules/searches/automation across workspaces) + Azure Lighthouse (cross-tenant for managed providers)
│   └── Unified Security Operations Platform (Sentinel + Defender XDR at https://security.microsoft.com/; bi-directional incident sync; Fusion Analytics Rule auto-disabled)
├── 2. GLOBAL TELEMETRY WIRING
│   ├── Content Hub (Solutions) and Source Control (GitHub / Azure DevOps)
│   ├── Native connectors (Entra ID, Azure Activity, Defender for Cloud [Subscription-based vs Tenant-based], Defender XDR [Advanced Hunting tables])
│   ├── Windows: Azure Arc + AMA + DCR (All Events, Common 4624/4634, Minimal, Custom KQL)
│   ├── Linux/firewalls: Syslog Forwarder (514 -> AMA 28330 -> 443; Syslog table unparsed) and CEF (25226; CommonSecurityLog pre-parsed)
│   └── Custom/API: CCP, Log Analytics Ingestion API; Threat Intel via TAXII / Upload Indicators API into ThreatIntelligenceIndicator
├── 3. THE ROBOTIC LIBRARIAN (KQL)
│   ├── Core syntax: Pipe (|), search (inefficient, wildcards), where (ago, ==, =~, in, !in), let, project family (project, project-away, project-keep, project-rename, project-reorder), order by / sort by
│   ├── Transformation: extend, summarize (count, countif, dcount, dcountif, avg, min, max, sum, percentile, stdev, variance, arg_max, arg_min, make_list [keeps duplicates], make_set [distinct only]), bin(), render (timechart, barchart, columnchart, piechart, areachart, scatterchart)
│   ├── Combining: union (wildcards, withsource) and join flavors (innerunique default, inner, leftouter, rightouter, fullouter, leftsemi, rightsemi, leftanti, rightanti)
│   └── Parsing & enrichment: extract (regex), parse (simple/regex/relaxed), dot notation, parse_json / todynamic, mv-expand, mv-apply, externaldata (SAS token), Watchlists (_GetWatchlist), Threat Intelligence
├── 4. AUTOMATED TRIPWIRES & TASK FORCES
│   ├── Analytics Rules: Scheduled, NRT, Anomaly (Anomalies table, no direct incidents), Microsoft Security Rules
│   ├── Scheduled rule wizard (General, Set Rule Logic [Entity Mapping, Custom Details, Alert Details, Scheduling, Threshold, Event Grouping, Suppression], Incident Settings [Alert Grouping up to 7 days], Automated Response, Review and Create)
│   ├── Automation Rules (dispatch desk: status, severity, classification, owner, tags, Priority order where lower runs first, run playbooks)
│   └── Playbooks (Azure Logic Apps; triggers Incident / Alert / Entity; actions: Entra disable account, MDE isolate, firewall blocklist, Teams; Managed Identity required)
├── 5. INVESTIGATION & PROFILING
│   ├── Hierarchy: Events -> Alerts -> Entities -> Incidents
│   ├── UEBA (behavioral baselines, Investigation Priority Score, Anomalies table, Entity Page)
│   ├── ASIM (im parsers: imAuthentication, imProcessCreate, imNetworkSession, imDns, imWeb, imFileEvent)
│   └── Workspace Transformation DCRs (filter, drop columns, mask PII before storage; lower ingestion cost)
├── 6. PROACTIVE FIELD LABS
│   ├── Threat Hunting (hypothesis criteria: Achievable, Narrow, Time-Bound, Useful and Efficient, Aligned to Threat Model)
│   ├── Bookmarks (entity tagging, Entity Graph, escalate to incident), Live Stream, Hunting Queries (MITRE ATT&CK)
│   ├── Deep archives: Search Jobs (_SRCH tables), Data Restoration, Data Lake KQL Jobs
│   └── Notebooks (Python in Azure Machine Learning Workspace + Compute Clusters; KQLmagic; MSTICPy; Jupyter / VS Code; GitHub Copilot; MCP)
└── END STATE (connecting the dots)
    └── Complete threat detection, investigation, and automated response across the globe: Log Analytics Workspace + Data Lake ingestion -> KQL filtering/transformation/parsing/enrichment -> Analytics Rule tripwires -> SOAR (Automation Rules + Playbooks) -> UEBA/ASIM investigation -> hypothesis-driven hunting with Bookmarks, Search Jobs, and Python notebooks
```