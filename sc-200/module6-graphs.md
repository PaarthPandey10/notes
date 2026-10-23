```
1. THE BIG PICTURE & AGENCY ARCHITECTURE
├── GLOBAL INTELLIGENCE AGENCY ANALOGY (Managing enterprise security is like monitoring a global surveillance grid)
│   ├── The Challenge: Billions of raw logs from physical and cloud assets. Humans can't read it all without "alert fatigue."
│   └── The Solution: Microsoft Sentinel (a cloud-native platform aggregating, analyzing, and responding to enterprise threat data).
│       ├── SIEM (Security Information and Event Management): The central system that ingests and analyzes logs.
│       └── SOAR (Security Orchestration, Automation, and Response): The automated robotic framework that executes immediate response actions.
│
├── THE DUAL-STORAGE ARCHITECTURE (Hot vs. Cold Storage)
│   ├── SIEM Operations Floor (Log Analytics Workspace)
│   │   ├── The active "hot" database environment for day-to-day operations.
│   │   ├── Kept in high-performance storage (Analytics tier) for real-time alerts.
│   │   └── Operational retention window: 30 days by default (up to 730 days).
│   └── Platform Data Lake
│       ├── The "cold" long-term archive for historical logs (up to 12 years).
│       └── Uses basic/Data Lake storage tiers for cost-efficiency; supports asynchronous graph-based analysis for slow-moving attacks.
│
├── DEPLOYMENT TOPOLOGIES & MANAGEMENT
│   ├── Single Tenant with Single Workspace: One command room for a single SOC.
│   ├── Regional Workspaces: Used strictly to comply with Data Residency rules (legal requirements keeping data within specific geographic borders).
│   ├── Multi-Tenant Architecture: Independent workspaces for different subsidiaries/customers.
│   │   ├── Workspace Manager: Centrally publishes rules/searches across multiple workspaces.
│   │   └── Azure Lighthouse: Lets security providers view/operate customer Sentinel environments without constantly logging in and out.
│   └── Unified Security Operations Platform ([https://security.microsoft.com/](https://security.microsoft.com/))
│       ├── Merges Microsoft Sentinel and Microsoft Defender XDR into a single web portal.
│       ├── Correlates XDR's specialized alerts into unified Incident case files automatically.
│       └── Sentinel's legacy Fusion Analytics Rule (machine-learning multi-stage correlator) is automatically disabled to prevent duplicate tickets.
│
2. WIRING THE GLOBAL NETWORK (DATA INGESTION)
├── CONTENT HUB & MANAGEMENT
│   ├── Content Hub: The "digital hardware store" where you download pre-packaged Solutions (kits containing data connectors, parsing rules, and workbooks).
│   └── Source Control: Syncs custom rules and code via GitHub or Azure DevOps.
│
├── NATIVE MICROSOFT CONNECTORS (Service-to-Service friction-less cables)
│   ├── Microsoft Entra ID: Ingests audit, interactive, non-interactive, and service principal sign-in logs.
│   ├── Azure Activity: Captures ARM management operations across Azure.
│   ├── Microsoft Defender for Cloud: Offers legacy Subscription-based (raw SecurityAlerts) or modern Tenant-based (unified incident queues).
│   └── Microsoft Defender XDR Connector: Streams raw Advanced Hunting tables (DeviceEvents, DeviceFileEvents, DeviceProcessEvents, DeviceNetworkEvents).
│
├── WINDOWS DESK SURVEILLANCE (Physical & Cloud Servers)
│   ├── Azure Arc: A software bridge projecting non-Azure and on-premises servers into Azure as managed resources.
│   ├── AMA (Azure Monitor Agent): The lightweight OS tracking bug that actually collects the logs.
│   └── DCR (Data Collection Rule): The cloud-managed "ruthless mailroom sorter" that stops network congestion by filtering data before it leaves the laptop.
│       └── Tiers: All Events, Common (logon 4624, logoff 4634), Minimal (critical breach only), Custom (KQL filtered).
│
├── FOREIGN FIREWALL TRANSLATION (Third-Party Appliances)
│   ├── The Problem: Appliances (Palo Alto, Cisco) speak Syslog, a messy, unstructured event-logging protocol on TCP/UDP 514.
│   ├── The Translation Booth (Syslog Forwarder): A dedicated Linux VM built to process foreign logs.
│   ├── Raw Syslog Pipeline: Firewall -> Linux Forwarder (Port 514) -> AMA (TCP 28330) -> Sentinel Cloud (HTTPS Port 443). Lands unparsed in the `Syslog` table.
│   └── CEF (Common Event Format) Pipeline: Preferred. Structures data into key-value columns before transmission (Port 25226). Lands in `CommonSecurityLog` table.
│
└── CUSTOM & EXTERNAL INGESTION
├── CCP (Codeless Connector Platform): Builds REST API connectors for custom apps without coding.
├── Log Analytics Ingestion API: Receives direct JSON payloads.
└── Threat Intelligence: Wires live feeds of known-bad IPs/domains into the `ThreatIntelligenceIndicator` table via TAXII connectors or Upload Indicators API.
│
3. THE ROBOTIC LIBRARIAN'S CONVEYOR BELT (KQL SYNTAX)
├── KQL (Kusto Query Language) BASICS
│   ├── A read-only language designed for massive column-store database queries.
│   └── The Pipe (`|`): Acts as a physical conveyor belt passing transformed data from left to right.
│
├── FOUNDATIONAL OPERATORS (The Workstations)
│   ├── `search`: The "CPU Killer." Multi-table/column wildcard search. Computationally inefficient.
│   ├── `where`: The strict filter/shredder. Instantly discards non-matching rows to speed up queries (uses `ago()`, `==`, `=~`, `in`, `!in`).
│   ├── `let`: The "sticky note." Declares variables, arrays, or subqueries at the top for modularity.
│   ├── The `project` Family: The "black marker" controlling output columns.
│   │   ├── `project` (keeps listed columns/creates new ones)
│   │   ├── `project-away` (excludes columns)
│   │   ├── `project-keep` (explicitly preserves columns)
│   │   ├── `project-rename` (changes column headers)
│   │   └── `project-reorder` (reorders visually)
│   └── `order by` / `sort by`: Sorts rows (asc/desc).
│
├── TRANSFORMATION & AGGREGATION (The Accountant's Desk)
│   ├── `extend`: Calculates brand-new columns on the fly (e.g., using `substring`, `string_size`) without modifying the database.
│   └── `summarize`: Groups rows by a common denominator (the `by` clause) and performs math.
│       ├── `count()` / `countif()`: Tallies total rows (identifies Brute Force).
│       ├── `dcount()` / `dcountif()`: Estimates distinct/unique values (identifies Password Spray).
│       ├── `avg()`, `min()`, `max()`, `sum()`, `percentile()`, `stdev()`, `variance()`: Standard math.
│       ├── `arg_max()` / `arg_min()`: Grabs the full row for the newest (max) or oldest (min) event.
│       └── `make_list()` / `make_set()`: Bundles values into a dynamic JSON array (list = keeps duplicates, set = distinct unique values only).
│
└── TIME SERIES & VISUALIZATIONS
├── `bin()`: Rounds scattered timestamps into discrete buckets (e.g., 1-hour chunks).
└── `render`: Draws the visual graph (timechart, barchart, columnchart, piechart, areachart, scatterchart).
│
4. CONNECTING INTELLIGENCE DRAWERS (UNIONS & JOINS)
├── VERTICAL STACKING: `union`
│   ├── Dumps tables on top of each other (makes dataset taller). Supports wildcards (`union Security*`).
│   └── `withsource`: Parameter that creates a new column showing which original table each row came from.
│
└── HORIZONTAL STAPLING: `join`
├── Merges a LeftTable and RightTable into a wider table based on a matching key column.
├── Golden Rule: Smallest dataset goes on the Left to prevent cluster memory crashes.
└── KQL Join Flavors (How it handles matches/mismatches)
├── `innerunique` (Default): 1 row from left matches right rows.
├── `inner`: Strict. Outputs only if there is a match on BOTH sides.
├── `leftouter` / `fullouter`: Inclusive. Keeps rows even if unmatched (fills blanks with nulls).
├── `rightouter`: Keeps all right rows.
├── `leftsemi` / `rightsemi`: Keeps rows with a match, but does NOT append the other table's columns.
└── `leftanti` / `rightanti` (leftantisemi / rightantisemi): The Hunter's Negative Space. Keeps rows ONLY if they DO NOT have a match on the other side.
│
5. UNLOCKING UNSTRUCTURED FILES & EXTERNAL INTELLIGENCE
├── PARSING TEXT & JSON
│   ├── Text: `extract()` (uses strict Regex capture groups) and `parse` (uses a simpler string template with modes: simple, regex, relaxed).
│   ├── JSON: Extracted using Dot Notation (e.g., `Device.OS`), or `parse_json()` / `todynamic()` (converts strings into queryable dynamic object bags).
│   └── Arrays: `mv-expand` (explodes a JSON list by duplicating the row for every item) and `mv-apply` (applies subqueries to expanded elements).
│
└── INTEGRATING EXTERNAL DATA
├── `externaldata`: Pulls external files directly from Azure Blob/Data Lake via a SAS token URL.
├── Watchlists: Static CSV reference lists (e.g., VIPs, Terminated Employees) uploaded to Sentinel. Queried via the `_GetWatchlist()` function to suppress false positives.
└── Threat Intelligence: Correlates incoming logs against known adversary infrastructure automatically.
│
6. AUTOMATED ALARM TRIPWIRES (ANALYTICS RULES)
├── Scheduled Rules: KQL queries running on a strict timer (e.g., every 5 minutes). Built via a 5-step wizard.
│   ├── Entity Mapping: Crucial step mapping KQL columns to standard Sentinel Entities (Account, IP, Host) for UEBA and visual graphs.
│   ├── Custom Details / Alert Details: Extracts query metadata to dynamically name the alert.
│   ├── Scheduling & Threshold: Uses overlapping historical lookback windows. Generates alert when row count > threshold.
│   ├── Event Grouping: Groups matching rows into a Single alert or an alert for each event.
│   ├── Suppression: Pauses the rule temporarily after firing to stop duplicate ticket floods.
│   └── Incident Settings / Alert Grouping: Groups alerts within a window (up to 7 days) into a single incident case file.
├── NRT Rules (Near-Real-Time): Runs continuously with sub-minute latency to catch high-speed attacks (ransomware).
├── Anomaly Rules: Machine-learning algorithms establishing behavioral baselines. Writes silently to the `Anomalies` table instead of creating noisy incident tickets.
└── Microsoft Security Rules: Auto-generates Sentinel incidents from connected Defender solutions.
│
7. AUTOMATED ROBOTIC TASK FORCES (SOAR)
├── THE DISPATCH DESK: Automation Rules
│   ├── Natively manages incident metadata (Triage).
│   ├── Actions: Changes status/severity, assigns owner, adds operational tags, and evaluates execution order via the Priority field.
│   └── Dispatches Playbooks.
│
└── THE ROBOTIC TASK FORCE: Logic App Playbooks
├── Built on Azure Logic Apps (drag-and-drop cloud workflow designer).
├── Triggers: Incident triggered, Alert triggered, or Entity triggered (manual execution from investigation page).
├── Actions: API connector tasks (e.g., Disable Entra account, isolate MDE device, block IP, send Teams message).
└── Security: Requires explicit Managed Identity permissions or service principals to execute administrative actions securely.
│
8. INCIDENT INVESTIGATION & ENTITY PROFILING
├── The Incident Hierarchy
│   ├── 1. Events: Individual, raw log lines.
│   ├── 2. Alerts: Moments an analytics rule tripwire identified suspicious events.
│   ├── 3. Entities: The real-world nouns (Accounts, Hosts, IPs, File Hashes).
│   └── 4. Incidents: The central case file grouping all of the above.
│
├── UEBA (User and Entity Behavior Analytics)
│   ├── Builds dynamic behavioral baselines (working hours, locations, peer groups).
│   ├── Assigns an Investigation Priority Score for deviations (logging to the Anomalies table).
│   └── Viewable on the interactive Entity Page.
│
├── ASIM (Advanced Security Information Model)
│   ├── The "Universal Translator" framework. Normalizes varying vendor column schemas (Cisco, Windows, Palo Alto) into standard fields.
│   └── Unifying Parsers: `imAuthentication`, `imProcessCreate`, `imNetworkSession`, `imDns`, `imWeb`, `imFileEvent`. (e.g., standardizing everything to `TargetUserPrincipalName`).
│
└── Workspace Transformation DCRs
└── KQL rules that execute right at the ingestion pipeline to drop/mask unnecessary or PII data before it hits the disk, saving storage costs.
│
9. PROACTIVE FIELD OPERATIONS (THREAT HUNTING)
├── The Threat Hunting Cycle (Assuming Breach)
│   ├── Hypothesis: Must be Achievable, Narrow in Scope, Time-Bound, Useful/Efficient, and Aligned to Threat Model (MITRE ATT&CK).
│   ├── Bookmarks: Digital evidence bags. Preserves logs and query syntax. Escalate to the visual Entity Graph and formal incidents.
│   └── Live Stream: Runs a KQL hunt continuously in real-time with desktop notifications.
│
├── Deep Historical Archives
│   ├── Search Jobs: Asynchronous background queries for massive Data Lakes (writes to `_SRCH` tables).
│   ├── Data Restoration: Restores archived cold logs back into active Log Analytics.
│   └── Data Lake KQL Jobs: Background analytical tasks across cold storage.
│
└── The Forensic Crime Lab (Jupyter Notebooks)
├── Interactive Python coding environments running on Azure Machine Learning / Compute Clusters.
├── KQLmagic: Python library allowing hunters to execute KQL inside Jupyter and pass results to Python data frames.
├── MSTICPy: Microsoft Threat Intelligence Python Security Tool (built for IP lookups, decoding, cyber-visualization, malware analysis).
└── Integration: Uses GitHub Copilot (AI) and MCP (Model Context Protocol) to connect IDEs to Sentinel backends securely.
│
10. CONNECTING THE DOTS (THE CLOSED LOOP)
└── The Loop: Security is not a straight line. Proactive Threat Hunting discovers new adversary techniques, which are extracted into CSV Watchlists, and translated into brand new Analytics Rules. The hunt hardens the baseline, becoming tomorrow's automated tripwire.
```