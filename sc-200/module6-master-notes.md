# Mitigating Threats with Microsoft Sentinel

## The Big Picture

Imagine managing global security for an enterprise whose footprint covers thousands of office buildings across every continent. Millions of employees swipe badges, open doors, access cloud servers, and send messages every second of the day. If a break-in occurs at a remote facility or a rogue insider slowly exfiltrates confidential files over several months, local guards watching individual camera screens will never spot the overall pattern. To protect this global enterprise, you construct a **Global Intelligence Agency**—known in our digital estate as **Microsoft Sentinel** (a cloud-native security platform that aggregates, analyzes, and responds to threat data across the entire enterprise network).

This central intelligence agency acts as both a **SIEM** (security information and event management, a centralized system that ingests and analyzes security logs from across the entire network) and a **SOAR** (security orchestration, automation, and response, an automated computing framework that executes immediate response workflows against active threats). The agency operates a central command floor that ingests massive streams of raw surveillance data from every facility worldwide, translates foreign firewall dialects into a single universal language using a robotic librarian, uses automated analytical tripwires to catch threat actors, and dispatches automated robotic task forces to neutralize high-speed attacks before damage spreads.

---

## The Core Mechanics

### Agency Headquarters & Architecture: Microsoft Sentinel SIEM, Data Lakes, and Multi-Tenant Management

Before running digital cables or deploying automated task forces, architects must design the physical and logical layout of the central intelligence agency.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SENTINEL DUAL-STORAGE ARCHITECTURE                              │
├──────────────────────────────────────────┬─────────────────────────────────────────────┤
│ SIEM OPERATIONS FLOOR                    │ PLATFORM DATA LAKE                          │
│ (Log Analytics Workspace)                │ (Massive Storage Pool)                      │
├──────────────────────────────────────────┼─────────────────────────────────────────────┤
│ • Fast, high-frequency security searches │ • Low-cost long-term retention (up to 12 yrs)│
│ • Operational retention window (~30 days)│ • Graph-based analysis for slow attacks     │
│ • Powers real-time analytics & hunting   │ • Supports asynchronous search jobs         │
└──────────────────────────────────────────┴─────────────────────────────────────────────┘
```

The agency operates across two distinct data management divisions:

- **The Microsoft Sentinel SIEM Operations Floor**: Built directly on top of a **Log Analytics Workspace** (an Azure database environment where log data is collected, stored, and queried), this active floor handles day-to-day threat hunting, incident investigation, and real-time alert correlation. Data resides in high-performance storage tiers (such as the Analytics tier) for an operational window of **30 days** by default (configurable up to 730 days).
- **The Microsoft Sentinel Platform Data Lake**: Serves as a massive **Data Lake** (a scalable, low-cost storage repository that holds raw data in its native format until needed for long-term analysis) that retains historical logs for up to **12 years**. It uses graph-based correlation to uncover slow-moving, multi-year attack campaigns while offering cost-optimized **Basic** and **Data Lake** storage tiers.

Architects choose from three deployment topologies based on organizational structure and compliance needs:

- **Single Tenant with Single Workspace**: A centralized command room where all security logs flow into one workspace managed by a single SOC team.
- **Regional Workspaces**: Dedicated workspaces deployed in specific countries or geographical regions to strictly comply with local **Data Residency** (legal regulations specifying that data collected in a region must remain stored within that region) rules.
- **Multi-Tenant Architecture**: Independent workspaces serving distinct subsidiary companies or managed customers.

When managing multiple regional or customer workspaces, agency leaders deploy **Workspace Manager** (a centralized tool that publishes analytics rules, saved searches, and automation policies across multiple workspaces at once). For managed security providers overseeing multiple enterprise tenants, administrators deploy **Azure Lighthouse** (a cross-tenant management service that lets analysts view and operate customer Sentinel environments without constantly logging in and out of separate corporate accounts).

To streamline operations, Microsoft provides the **Unified Security Operations Platform** (an enterprise integration that merges Microsoft Sentinel and **Microsoft Defender XDR** [extended detection and response, an integrated suite that protects endpoints, identities, email, and cloud apps] into a single web portal at `https://security.microsoft.com/`). When unified, incidents synchronize bi-directionally between platforms. Because Defender XDR assumes primary responsibility for correlating raw alerts into unified incidents, Sentinel's legacy **Fusion Analytics Rule** (a machine-learning engine that stitches multi-stage attack signals together) is automatically disabled to prevent duplicate incident creation.

With agency headquarters established, architects must plug in digital cables to ingest surveillance feeds from every corner of the globe.

---

### Wiring the Global Network: Solutions, Data Connectors, Azure Arc, AMA, and Syslog/CEF Translation

For our global intelligence agency to function, raw telemetry must flow continuously from every facility, server, and firewall into the central command room.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SENTINEL DATA INGESTION PIPELINE                                │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. CONTENT HUB -> Deploy packaged Solution (Connectors + Rules + Workbooks)             │
│ 2. SOURCE CONTROL -> Sync custom KQL & rules from GitHub / Azure DevOps                │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ DATA CONNECTOR TYPES:                                                                  │
│ • Native M365 / Azure  ──► Entra ID, Azure Activity, Defender XDR Telemetry Tables     │
│ • Windows Endpoints    ──► Azure Arc + AMA (Azure Monitor Agent) + DCR (Filters)       │
│ • Linux / Firewalls    ──► Syslog Forwarder (Port 514) ──► AMA (Port 28330) ──► Port 443 │
│                        ──► CEF (Common Event Format) Forwarder (Port 25226)            │
│ • Custom / Third-Party ──► CCP (Codeless Connector Platform) / Ingestion API / TAXII   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

Security teams start at the **Content Hub** (`Microsoft Sentinel > Content management > Content hub`), a digital hardware store where administrators download pre-packaged **Solutions** (bundled integration kits containing data connectors, parsing rules, visual workbooks, and analytic tripwires). Custom rules and infrastructure configurations are managed using **Source Control** repositories (such as GitHub or Azure DevOps) to version-control detection code and automate deployments.

Data connectors pull logs into native database **Tables** (structured storage units like `SecurityAlert`, `SecurityIncident`, `SigninLogs`, and `AzureActivity`) across five core methods:

#### 1. Native Service-to-Service Connectors

Connects Microsoft cloud services with a single toggle:

- **Microsoft Entra ID**: Ingests audit logs, interactive user sign-ins, non-interactive sign-ins, and service principal logs.
- **Azure Activity**: Captures management-plane ARM operations performed across Azure subscriptions.
- **Microsoft Defender for Cloud**: Offers two cabling options: the legacy **Subscription-based Connector** (dumps raw security alerts into the `SecurityAlert` table) or the modern **Tenant-based Connector** (integrates defender alerts directly into unified incident queues).
- **Microsoft Defender XDR Connector**: Streams high-fidelity alerts alongside raw **Advanced Hunting** telemetry tables—such as `DeviceEvents`, `DeviceFileEvents`, `DeviceProcessEvents`, and `DeviceNetworkEvents`—into Sentinel for long-term correlation.

#### 2. Windows Desk Surveillance (AMA + DCR + Azure Arc)

To collect security logs from Windows servers and workstations, administrators deploy the **AMA** (Azure Monitor Agent, lightweight tracking software that collects security logs and operating system events). If a computer lives outside Azure's boundary, it is onboarded using **Azure Arc** (a software bridge that projects non-Azure and on-premises servers into ARM as managed resources).

To prevent network congestion, administrators attach a **DCR** (Data Collection Rule, a cloud-managed policy object that specifies exactly which event IDs to collect and which to ignore). DCRs configure ingestion tiers: **All Events**, **Common** (collects standard audit trails like logon `4624`, logoff `4634`, and group modifications), **Minimal** (collects critical breach indicators), or **Custom** (using explicit KQL filtering statements).

#### 3. Foreign Firewall Translation (Syslog vs. CEF)

Third-party networking appliances and Linux firewalls do not speak native Microsoft formats; they communicate using **Syslog** (a standard event-logging protocol used by Unix systems and network devices).

To ingest foreign logs, administrators build a **Syslog Forwarder** (a dedicated Linux virtual machine that acts as a translation booth between third-party appliances and Sentinel).

```
[ Foreign Appliance ] ──(Syslog: TCP/UDP 514)──► [ Linux Forwarder (AMA) ]
                                                        │
                                          (Internal Route: TCP 28330)
                                                        │
                                                        ▼
 [ Sentinel Cloud ] ◄──────(HTTPS: TCP 443)─────────────┘
```

- **Raw Syslog Pipeline**: The foreign appliance transmits raw text logs over **TCP/UDP port 514** to the Linux forwarder. Internal listening software routes the text to the local AMA agent on **TCP port 28330**, which encrypts and transmits the data to Sentinel over **TCP port 443** (HTTPS). Raw Syslog arrives as an unparsed block of text in the `Syslog` table, requiring custom KQL functions to parse.
- **CEF (Common Event Format) Pipeline**: Preferred for enterprise firewalls (e.g., Palo Alto, Check Point, Fortinet). CEF structures data into key-value pairs before transmission. CEF traffic flows through the Linux forwarder on **TCP port 25226** and arrives in the `CommonSecurityLog` table with pre-parsed columns (e.g., `DeviceAction`, `SourceIP`, `DestinationIP`, `RequestURL`).

#### 4. Custom and API Ingestion

For proprietary web apps or unmonitored cloud tools, developers build custom connectors using the **CCP** (Codeless Connector Platform, a configuration framework that builds REST API data connectors without writing custom code) or send JSON payloads directly to the **Log Analytics Ingestion API**.

#### 5. Threat Intelligence Live Feeds

To track active threat actors, administrators wire automated threat feeds into the `ThreatIntelligenceIndicator` table using **TAXII** (Trusted Automated eXchange of Intelligence Information, an automated protocol used to exchange cyber threat intelligence) connectors or the **Upload Indicators API**.

Once raw telemetry is flowing into database tables, our agency relies on a robotic librarian to search, transform, and organize the incoming data stack.

---

### The Robotic Librarian's Conveyor Belt: Fundamental KQL Query Syntax and Row Filtering

In our global intelligence agency, millions of new data records arrive every minute. Human analysts cannot read these logs line by line; they rely on a robotic librarian powered by **KQL** (Kusto Query Language, a read-only query language designed to search, filter, and analyze massive volumes of structured and unstructured log data).

```
[ Database Table: SecurityEvent ]
               │
               ▼
   | where EventID == 4624  ──► (Filter: Keep only logon events)
               │
               ▼
   | where AccountType =~ "user" ──► (Filter: Case-insensitive user check)
               │
               ▼
   | project TimeGenerated, Account, Computer ──► (Select specific columns)
```

KQL statements operate on a read-only data-flow model where data passes through sequential query operators bound together by the **Pipe (`|`)** symbol (which acts like a physical conveyor belt passing transformed results from left to right).

Key foundational KQL operators and functions include:

- **`search`**: A broad, multi-table and multi-column search operator. While simple to run, `search` is computationally inefficient compared to targeted filtering. Supports wildcards (e.g., `search in (SecurityEvent, SecurityAlert, A*) "error"`).
- **`where`**: A strict filtering operator that evaluates rows against predicate conditions, discarding non-matching records immediately to optimize query performance:
    - Time filtering using **`ago()`** (e.g., `| where TimeGenerated > ago(7d)`).
    - Exact matching using **`==`** and case-insensitive matching using **`=~`** (e.g., `| where AccountType =~ "user"`).
    - Membership filtering using **`in`** or **`!in`** (e.g., `| where EventID in (4624, 4625)`).
- **`let`**: Binds a user-defined name to a scalar value, array, or tabular expression, improving code modularity and readability:

```
// Declare variables and dynamic datatables
let timeOffset = 7d;
let DiscardID = 4688;
let SuspiciousUsers = datatable(Account: string) [
    @"CONTOSO\Administrator",
    @"NT AUTHORITY\SYSTEM"
];
SecurityEvent
| where TimeGenerated > ago(timeOffset * 2) and TimeGenerated < ago(timeOffset)
| where EventID != DiscardID
| where Account in (SuspiciousUsers)
```

- **`project` Family**: Controls column output formatting:
    - **`project`**: Selects specific columns to include, renames columns, or inserts new computed fields while dropping unlisted columns.
    - **`project-away`**: Specifies columns to exclude from the output table.
    - **`project-keep`**: Explicitly defines columns to preserve.
    - **`project-rename`**: Renames specific column headers without altering the rest of the table.
    - **`project-reorder`**: Reorders output columns sequentially.
- **`order by` / `sort by`**: Sorts output rows by one or more columns in **`asc`** (ascending) or **`desc`** (descending) order (default is descending).

Now that the robotic librarian can sift through raw files and drop unwanted columns, we must teach it to calculate statistics, summarize findings, and draw visual graphs.

---

### Data Transformation and Aggregation: Extending Columns, Summarizing Metrics, and Visualizing Trends

To convert raw log lines into actionable intelligence, analysts create calculated metrics and aggregate scattered events into statistical summaries.

```
[ Raw Event Logs ] ──► [ extend: Calculate New Fields ] ──► [ summarize: Group & Aggregate ] ──► [ render: Draw Visual Graph ]
```

#### 1. Calculating New Fields with `extend`

The **`extend`** operator creates calculated or transformed columns and appends them to the tabular result set:

```
SecurityEvent
| where ProcessName != "" and Process != ""
// Calculate starting directory by extracting a substring based on string length
| extend StartDir = substring(ProcessName, 0, string_size(ProcessName) - string_size(Process))
| extend SeverityRank = case(EventID == 4624, "Low", EventID == 4625, "Medium", "High")
```

#### 2. Aggregating Telemetry with `summarize`

The **`summarize`** operator groups rows sharing matching values in the `by` clause and calculates mathematical aggregations across those groups.

Common aggregation functions include:

- **`count()` / `countif()`**: Returns total row count per group, or row count matching a specific condition (e.g., `summarize FailedLogons = countif(EventID == 4625) by Account`).
- **`dcount()` / `dcountif()`**: Calculates an estimated count of **distinct** (unique) values within a group, ignoring duplicates (e.g., `summarize UniqueIPs = dcount(IpAddress) by UserPrincipalName`).
- **`avg()`**, **`min()`**, **`max()`**, **`sum()`**, **`percentile()`**, **`stdev()`**, **`variance()`**: Standard mathematical and statistical metrics.
- **`arg_max()` / `arg_min()`**: Returns the full record containing the maximum or minimum value for a specified column within each group (e.g., `| summarize arg_max(TimeGenerated, *) by Account` returns the most recent event row for every account).
- **`make_list()`**: Bundles all values of a scalar expression in the group into a dynamic JSON array, **preserving duplicates**.
- **`make_set()`**: Bundles all values of a scalar expression in the group into a dynamic JSON array containing **distinct (unique) values only**.

```
// Real-World Detection Query: Detect invalid password brute-force across multiple apps
let timeframe = 30d;
let threshold = 3;
SigninLogs
| where TimeGenerated >= ago(timeframe)
| where ResultDescription has "Invalid password"
| summarize AppCount = dcount(AppDisplayName), TargetedApps = make_set(AppDisplayName) by UserPrincipalName, IPAddress
| where AppCount >= threshold
```

#### 3. Time Series and Visualizations

To chart activity over time, analysts group timestamps into discrete time buckets using the **`bin()`** function, passing the aggregated time series directly into the **`render`** operator to generate visual graphs:

```
SecurityEvent
| where TimeGenerated > ago(7d)
| summarize EventCount = count() by bin(TimeGenerated, 1h), Activity
| render timechart
```

Supported **`render`** visualizations include: **`timechart`**, **`barchart`**, **`columnchart`**, **`piechart`**, **`areachart`**, and **`scatterchart`**.

Once an analyst learns to transform and visualize a single data table, they must learn to combine clues spread across entirely different filing drawers.

---

### Connecting Multiple Intelligence Drawers: Table Unions and Join Flavors

Threat actors rarely confine their footprint to a single service. An attack might start with a phishing click in email logs, transition to an account sign-in failure, and culminate in a database execution. The robotic librarian uses unions and joins to combine data across multiple tables.

#### 1. Combining Stacks with `union`

The **`union`** operator takes two or more tables and appends their rows into a single combined dataset.

- Supports wildcards (e.g., `union Security*` merges all tables whose names start with "Security").
- Includes the **`withsource=ColumnName`** parameter to create a calculated column identifying which original table each row came from:

```
union withsource = SourceTable SecurityEvent, SigninLogs, AuditLogs
| summarize EventCount = count() by SourceTable
```

#### 2. Stapling Tables Together with `join`

The **`join`** operator merges rows from two tables (**LeftTable** and **RightTable**) into a single, wide output table by matching key column values (`on $left.Attribute == $right.Attribute`).

```
Syntax: LeftTable | join kind = [JoinFlavor] ( RightTable ) on KeyAttribute
```

```
┌────────────────────────────────────────────────────────────────────────┐
│                          KQL JOIN FLAVORS MATRIX                       │
├───────────────────┬────────────────────────────────────────────────────┤
│ JOIN KIND / FLAVOR│ OPERATIONAL OUTPUT BEHAVIOR                        │
├───────────────────┼────────────────────────────────────────────────────┤
│ innerunique       │ (Default) Matches one row from left table for each │
│                   │ key value, joining matching right-table rows.      │
├───────────────────┼────────────────────────────────────────────────────┤
│ inner             │ Outputs a row for EVERY combination of matching    │
│                   │ records between left and right tables.             │
├───────────────────┼────────────────────────────────────────────────────┤
│ leftouter         │ Retains ALL rows from left table. Unmatched cells  │
│                   │ from the right table populate with null values.    │
├───────────────────┼────────────────────────────────────────────────────┤
│ rightouter        │ Retains ALL rows from right table. Unmatched cells │
│                   │ from the left table populate with null values.     │
├───────────────────┼────────────────────────────────────────────────────┤
│ fullouter         │ Retains ALL rows from both tables. Unmatched cells │
│                   │ on either side populate with null values.          │
├───────────────────┼────────────────────────────────────────────────────┤
│ leftsemi          │ Returns all rows from the left table that have a   │
│                   │ matching record in the right table (right columns  │
│                   │ are NOT appended to output).                       │
├───────────────────┼────────────────────────────────────────────────────┤
│ rightsemi         │ Returns all rows from the right table that have a  │
│                   │ matching record in the left table.                 │
├───────────────────┼────────────────────────────────────────────────────┤
│ leftanti /        │ Returns all rows from the left table that DO NOT   │
│ leftantisemi      │ have a matching record in the right table.         │
├───────────────────┼────────────────────────────────────────────────────┤
│ rightanti /       │ Returns all rows from the right table that DO NOT  │
│ rightantisemi     │ have a matching record in the left table.          │
└───────────────────┴────────────────────────────────────────────────────┘
```

```
// Example: Correlate user logon events (4624) with logoff events (4634)
SecurityEvent
| where EventID == 4624
| summarize LogOnCount = count() by Account
| join kind = inner (
    SecurityEvent
    | where EventID == 4634
    | summarize LogOffCount = count() by Account
) on Account
| project Account, LogOnCount, LogOffCount
```

With structured tables connected, our librarian must now deploy specialized magnifying glasses to extract data trapped inside unstructured text and JSON strings.

---

### Unlocking Unstructured Files and External Intelligence: Extraction, JSON Parsing, Watchlists, and Threat Intelligence

Security telemetry frequently arrives as unstructured text strings or nested JSON objects. To extract key-value pairs, the robotic librarian uses targeted parsing functions.

#### 1. Parsing Unstructured Text Strings

- **`extract()`**: Uses **Regex** (regular expressions) to capture a specific substring from a text field and optionally convert its data type:

```
// Extract account name from domain\username string using regex capture group 2
SecurityEvent
| where EventID == 4672 and AccountType == 'User'
| extend AccountName = extract(@"^(.*\\)?([^@]*)(@.*)?$", 2, tolower(Account))
```

- **`parse`**: Evaluates a string expression and parses it into calculated columns using a template pattern. Operates in three modes: **`simple`** (default strict string match), **`regex`** (uses regex delimiters), or **`relaxed`** (non-strict parsing that inserts nulls if types mismatch):

```
Traces
| parse EventText with * "resourceName=" ResourceName ", totalSlices=" TotalSlices:long * "lockTime=" LockTime:date ")" *
| project ResourceName, TotalSlices, LockTime
```

#### 2. Parsing Structured JSON and Dynamic Fields

Many log sources store nested key-value pairs inside fields of type **Dynamic**.

- **Dot Notation**: Accesses nested keys directly (e.g., `SigninLogs | extend OS = DeviceDetail.operatingSystem`).
- **`parse_json()` / `todynamic()`**: Converts a raw JSON string into a queryable dynamic object bag.
- **`mv-expand`**: Expands a dynamic JSON array so that **each array element becomes a separate output row**, duplicating all non-array row values.
- **`mv-apply`**: Applies a subquery to each element of an expanded array individually before unioning the results.

```
// Unpack authentication methods from dynamic JSON array
SigninLogs
| extend AuthDetails = parse_json(AuthenticationDetails)
| mv-expand AuthDetails
| extend AuthMethod = tostring(AuthDetails.authenticationMethod)
| extend AuthResult = tostring(AuthDetails.authenticationStepResultDetail)
| project TimeGenerated, UserPrincipalName, AuthMethod, AuthResult
```

#### 3. Integrating External Reference Data (`externaldata`, Watchlists, and Threat Intel)

- **`externaldata`**: Reads external files directly from Azure Blob Storage or Azure Data Lake Storage using a SAS token URL:

```
Users
| where UserID in (
    (externaldata (UserID: string) [@"https://storageaccount.blob.core.windows.net/container/users.txt?sp=r&st=...SAS..."])
)
```

- **Watchlists**: Curated CSV reference lists uploaded directly into Sentinel (`Microsoft Sentinel > Configuration > Watchlists`). Analysts query watchlists using the **`_GetWatchlist('WatchlistAlias')`** function to suppress false positives (e.g., skipping trusted IP ranges) or catch high-risk accounts (e.g., alerting when a user on the `TerminatedEmployees` watchlist logs in):

```
let TerminatedList = _GetWatchlist('TerminatedEmployees') | project UserPrincipalName;
SigninLogs
| where UserPrincipalName in (TerminatedList)
```

- **Threat Intelligence**: Threat indicators (IPs, domains, file hashes) feed into the **`ThreatIntelligenceIndicator`** table, allowing analytics rules to correlate incoming logs against known adversary infrastructure automatically.

Now that data is ingested, parsed, and enriched, we must set automated tripwires across our surveillance grid to detect active intruders.

---

### Automated Alarm Tripwires: Analytics Rules and Anomaly Detection

To catch threat actors automatically, administrators deploy **Analytics Rules** (`Microsoft Sentinel > Configuration > Analytics`). Analytics rules act as automated tripwires that continuously query ingested logs and raise security alerts when specific threat patterns are detected.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SENTINEL ANALYTICS RULE TYPES                   │
├───────────────────┬────────────────────────────────────────────────────┤
│ RULE TYPE         │ OPERATIONAL MECHANISM & PURPOSE                    │
├───────────────────┼────────────────────────────────────────────────────┤
│ Scheduled Rules   │ Custom or template-based KQL queries running on a  │
│                   │ strict timer (e.g., every 5 minutes looking back   │
│                   │ 5 minutes). Generates alerts & incident tickets.   │
├───────────────────┼────────────────────────────────────────────────────┤
│ NRT Rules         │ Near-Real-Time rules running continuously with sub- │
│                   │ minute latency to catch high-speed attacks.        │
├───────────────────┼────────────────────────────────────────────────────┤
│ Anomaly Rules     │ Machine-learning algorithms that flag behavioral   │
│                   │ deviations. Saves clues to the Anomalies table;    │
│                   │ does NOT create noisy incident tickets directly.   │
├───────────────────┼────────────────────────────────────────────────────┤
│ Microsoft Security│ Automatically creates Sentinel incidents from      │
│ Rules             │ alerts raised by connected Defender solutions.     │
└───────────────────┴────────────────────────────────────────────────────┘
```

Configuring a **Scheduled Analytics Rule** involves a 5-step wizard:

1. **General**: Define rule Name, Description, Tactics and Techniques (mapped to MITRE ATT&CK), and Severity (**High**, **Medium**, **Low**, **Informational**).
2. **Set Rule Logic**:
    - **Rule Query**: Enter the KQL detection query.
    - **Entity Mapping**: Maps KQL output columns to standard Sentinel **Entities** (e.g., mapping `UserPrincipalName` to _Account_, `IPAddress` to _IP_, `HostName` to _Host_). Entity mapping is vital for visual investigation graphs and UEBA profiling.
    - **Custom Details**: Extracts query columns into alert metadata properties.
    - **Alert Details**: Dynamically overrides alert name or description using query outputs (e.g., `High volume logon failures for {{UserPrincipalName}}`).
    - **Query Scheduling**: Defines how frequently the query runs (e.g., every 1 hour) and the historical lookback window (e.g., past 1 hour).
    - **Alert Threshold**: Generates an alert when query row output count is _Greater than 0_ (or a custom threshold).
    - **Event Grouping**: Selects whether to group all matched rows into a **Single alert** or raise **An alert for each event**.
    - **Suppression**: Enables the "Stop running query after alert is generated" toggle to pause rule execution for a defined window after an alert fires, preventing duplicate alerts.
3. **Incident Settings**: Controls whether alerts automatically create official **Incidents** in the queue. Configures **Alert Grouping** to group related alerts triggered within a configurable time window (up to 7 days) into a single incident based on matching entities, matching rule names, or all alerts.
4. **Automated Response**: Attaches **Automation Rules** or **Playbooks** to run automatically when the rule triggers.
5. **Review and Create**: Validates rule logic and deploys the tripwire.

When an automated tripwire snaps and raises an alarm, the global agency dispatches automated task forces to contain the threat immediately.

---

### Automated Robotic Task Forces: SOAR, Automation Rules, and Logic App Playbooks

To handle high-volume security incidents without exhausting human guards, Sentinel implements a two-tier **SOAR** automation engine: **Automation Rules** (the dispatch desk) and **Playbooks** (the robotic task force).

```
[ Incident / Alert Triggered ]
               │
               ▼
┌────────────────────────────────────────────────────────┐
│         THE DISPATCH DESK: AUTOMATION RULES            │
├────────────────────────────────────────────────────────┤
│ • Triage metadata (Assign owner, set tag, change severity)│
│ • Evaluate conditions & execution order (Priority 1..N) │
│ • Dispatch Robotic Task Force (Execute Playbook)       │
└──────────────┬─────────────────────────────────────────┘
               │
               ▼
┌────────────────────────────────────────────────────────┐
│        THE ROBOTIC TASK FORCE: LOGIC APP PLAYBOOKS     │
├────────────────────────────────────────────────────────┤
│ • Built on Azure Logic Apps drag-and-drop designer     │
│ • Starts with Trigger: Incident, Alert, or Entity      │
│ • Executes Actions: Disable account in Entra, isolate  │
│   device in MDE, block IP on firewall, post Teams msg  │
└────────────────────────────────────────────────────────┘
```

#### 1. The Dispatch Desk: Automation Rules

**Automation Rules** (`Microsoft Defender portal > Automation` or `Sentinel > Automation`) manage incident metadata and orchestrate automated workflows. They act as a central triage desk that evaluates newly created or updated incidents.

Key capabilities of Automation Rules:

- Automatically changing incident status (**Active**, **Closed**), severity, or classification (**True Positive**, **False Positive**, **Benign Positive**).
- Assigning incident ownership to specific human analysts or security groups.
- Adding operational tags (e.g., `Ransomware`, `FIN7`).
- Running automated **Playbooks** in a specified sequential order.
- Evaluation order is controlled by a numeric **Priority** field (lower numbers execute first).

#### 2. The Robotic Task Forces: Logic App Playbooks

**Playbooks** are automated response workflows built on **Azure Logic Apps** (Microsoft's cloud workflow automation service). Playbooks execute step-by-step remediation tasks across cloud and on-premises systems.

Playbooks are structured around two components:

- **Triggers**: The event that initiates playbook execution. Supported Sentinel triggers include:
    - `When a Microsoft Sentinel Incident creation rule was triggered`: Fires when an incident is generated; provides full access to incident entities.
    - `When a Microsoft Sentinel Alert was triggered`: Fires when an individual alert is raised.
    - `When a Microsoft Sentinel Entity was triggered`: Allows analysts to trigger a playbook manually against a specific entity (e.g., a user account or IP) directly from the investigation screen.
- **Actions**: Automated operational steps executed by API connectors. Examples include disabling a compromised user account in Microsoft Entra ID, isolating an endpoint in Defender for Endpoint, adding a malicious IP to a Palo Alto firewall blocklist, or sending an executive summary to a Microsoft Teams channel.

Playbooks require explicit **Managed Identity** permissions or service principal connections to execute administrative actions across connected Azure and Entra resources.

When automated task forces finish their initial containment steps, human detectives step in to conduct deep case file investigations.

---

### Incident Investigation and Entity Profiling: Hierarchy, UEBA, and Normalized ASIM Parsers

Human detectives review escalated cases using a structured incident hierarchy while leveraging behavioral profiling and standardized data templates.

#### 1. The Incident Hierarchy

Detectives break down security investigations into four nested levels:

- **Events**: Individual, raw log lines ingested from data sources (e.g., Event ID `4625` in `SecurityEvent`).
- **Alerts**: Specific moments where an analytics rule identified suspicious events and triggered a warning.
- **Entities**: The real-world actors and assets involved in the crime (Accounts, Hosts, IPs, URLs, Azure Resources, Mailboxes, File Hashes).
- **Incidents**: The central case file that aggregates related alerts, events, and entities into a single, manageable investigation workspace.

```
[ Events (Raw Logs) ] ──► [ Alerts (Tripwires) ] ──► [ Entities (Actors/Assets) ] ──► [ Incident Case File ]
```

#### 2. User and Entity Behavior Analytics (UEBA)

To detect anomalous behavior that bypasses static rules, Sentinel incorporates **UEBA** (`Microsoft Sentinel > Entity behavior`). UEBA builds dynamic behavioral baselines for users and hosts over time (e.g., tracking typical sign-in locations, working hours, accessed devices, and peer group associations).

When an entity strays from its baseline (e.g., a user logging in at 3:00 AM from a new country and accessing a sensitive database for the first time), UEBA assigns an elevated **Investigation Priority Score** and logs anomalies into the `Anomalies` table. Detectives review entity context on the **Entity Page**, which features an interactive timeline of user alerts, anomalies, sign-in locations, and organizational relationships.

#### 3. Universal Translation with ASIM Data Normalization

When querying security logs across multiple vendors (e.g., comparing Cisco, Palo Alto, and Windows firewall logs), different vendors use different column names for the same data (e.g., `TargetUserName` vs. `user_name` vs. `dst_user`).

To solve this, Sentinel uses **ASIM** (Advanced Security Information Model, an enterprise data normalization framework that standardizes varying vendor schemas into unified fields).

ASIM provides pre-built, workspace-level **Unifying Parsers** that start with the `im` prefix:

- **`imAuthentication`**: Normalizes all authentication logs (sign-in attempts, logons, logoffs) across all vendors into standard fields like `TargetUserPrincipalName` and `SrcIpAddr`.
- **`imProcessCreate`**: Normalizes process creation events across Windows, Linux, and endpoint security agents.
- **`imNetworkSession`**: Normalizes network connections across cloud and on-premises firewalls.
- **`imDns`**, **`imWeb`**, **`imFileEvent`**: Additional standardized schemas.

Detectives query the unifying parser directly instead of querying raw vendor tables:

```
// Query unified authentication attempts across all onboarded vendor logs
imAuthentication
| where TimeGenerated > ago(24h)
| where EventResult == "Failure"
| summarize FailedCount = count() by TargetUserPrincipalName, SrcIpAddr, EventVendor
```

#### 4. Fine-Tuning Ingestion with Workspace Transformation DCRs

To clean up or filter messy logs _before_ they are written to disk, architects configure **Workspace Transformation DCRs** (Data Collection Rules that execute KQL transformations on incoming telemetry right at the ingestion pipeline). Transformations allow dropping unnecessary columns, masking sensitive PII data, or filtering out high-volume benign events before storage, significantly reducing Log Analytics ingestion costs.

While reactive investigations resolve known incidents, elite field detectives must also venture out into the dark hallways to hunt for undiscovered threats.

---

### Proactive Field Operations: Threat Hunting, Bookmarks, Search Jobs, and Python Notebooks

Not all adversaries trigger automated alarms. Sophisticated threat actors use stealthy techniques to blend into normal network traffic. To catch these quiet intruders, security teams engage in **Threat Hunting** (`Microsoft Sentinel > Threat management > Hunting`).

```
┌────────────────────────────────────────────────────────────────────────┐
│                        THE THREAT HUNTING CYCLE                        │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Formulate Hypothesis (Achievable, Narrow, Time-bound, Threat-aligned)│
│ 2. Run Hunting Queries (KQL searches across raw telemetry)            │
│ 3. Preserve Clues via Bookmarks (Tag entities & build Entity Graph)   │
│ 4. Monitor Live Stream (Test queries in real time against active traffic)│
│ 5. Deep Historical Search (Search Jobs, Data Restoration, Data Lake)  │
│ 6. Forensic Crime Lab (Jupyter Notebooks + Python + MSTICPy)           │
└────────────────────────────────────────────────────────────────────────┘
```

Threat hunting is the proactive practice of searching for undetected adversaries, operating in contrast to reactive incident response.

#### 1. Formulating a Hunting Hypothesis

Every threat hunt begins with a **Hypothesis**—a testable hunch about how an adversary might be operating inside the network. A valid hypothesis must satisfy five core criteria:

- **Achievable**: Scope is realistic and manageable.
- **Narrow in Scope**: Targeted at specific dataset variables rather than generic queries.
- **Time-Bound**: Focused on a defined historical window (e.g., past 24 to 72 hours).
- **Useful and Efficient**: Delivers actionable security value without generating excessive noise.
- **Aligned to Threat Model**: Targets techniques known to be used by adversaries threatening the organization's specific industry.

#### 2. Hunting Tools and Clue Preservation

- **Hunting Queries**: Pre-built KQL queries mapped to the **MITRE ATT&CK** framework (`Tactics` and `Techniques`).
- **Bookmarks**: When a hunting query uncovers a suspicious log entry, the analyst saves it as a **Bookmark**. Bookmarks preserve the specific log record, query syntax, and analyst notes. Tagging entities within a bookmark allows Sentinel to automatically map clues onto the visual **Entity Graph** and escalate the bookmark into a formal security incident if malicious activity is confirmed.
- **Live Stream**: Allows analysts to run a KQL hunting query continuously against incoming telemetry in real time, displaying a desktop notification the moment a matching event occurs.

#### 3. Deep Historical Archives (Search Jobs and Data Restoration)

When a hunt requires searching terabytes of cold or archived data across long time horizons:

- **Search Jobs**: Asynchronous background search queries that search massive datasets without timing out or locking up the user interface. Results populate a dedicated search table (ending in `_SRCH`).
- **Data Restoration**: Restores archived logs back into active, queryable Log Analytics tables for temporary deep analysis.
- **Data Lake KQL Jobs**: Runs background analytical tasks across long-term Data Lake storage tiers.

#### 4. The Forensic Crime Lab: Jupyter Notebooks and MSTICPy

For advanced threat hunting, statistical analysis, and machine learning, senior threat hunters leave the web UI and open **Notebooks** (`Microsoft Sentinel > Threat management > Notebooks`).

Notebooks provide an interactive coding environment running **Python** (a popular programming language for data science and security analysis) hosted in an **Azure Machine Learning Workspace** backed by cloud **Compute Clusters** (scalable clusters of virtual machines equipped with machine learning libraries like PyTorch and TensorFlow).

Key components of the forensic notebook lab:

- **`KQLmagic`**: A Python library that allows threat hunters to execute KQL queries directly inside Jupyter Notebook cells and pass the resulting data frames into Python data analysis libraries (such as `pandas` and `matplotlib`).
- **`MSTICPy`** (Microsoft Threat Intelligence Python Security Tool): A specialized Python package developed by Microsoft for threat hunting. Provides built-in functions for IP reputation lookups, threat intelligence enrichment, log parsing, cyber-visualization (e.g., timeline charts and process trees), and malware analysis.
- **Desktop & AI Integration**: Threat hunters can execute notebooks locally using **Jupyter Notebooks** or **Visual Studio Code**, leveraging **GitHub Copilot** (an AI coding assistant) and the **MCP** (Model Context Protocol, an open standard protocol that connects external IDE code editors directly to secure Sentinel backend databases).

---

## Connecting the Dots

To protect a global enterprise from modern cyber threats, every operational component of Microsoft Sentinel integrates into a unified intelligence agency.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE GLOBAL SURVEILLANCE GRID                              │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Agency Architecture    │   │ Global Telemetry Wiring│   │ Robotic Librarian (KQL) │  │
│  │ (Sentinel SIEM, Data   │──►│ (Content Hub, Arc, AMA,│──►│ (Tabular Expressions,   │  │
│  │  Lakes, Unified SecOps)│   │  Syslog/CEF Forwarders)│   │  Pipe '|', where, let) │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Advanced Data Parsing │──►│ Automated Tripwires    │◄──│ Robotic Task Forces     │  │
│  │ (parse_json, Watchlists│   │ (Analytics Rules, NRT, │   │ (SOAR, Automation Rules,│  │
│  │  & Threat Intelligence)│   │  Anomaly Detection)    │   │  Logic App Playbooks)   │  │
│  └───────────────────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ PROACTIVE FIELD LABS   │                                │
│                              │ (Threat Hunting, UEBA, │                                │
│                              │  ASIM & Python / MSTIC) │                                │
│                              └────────────────────────┘                                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Agency Setup and Ingestion**: Architects build the agency on a **Log Analytics Workspace** for fast SIEM operations alongside a long-term **Data Lake**. Ingestion cables are deployed via the **Content Hub**—connecting native cloud services, tracking Windows desktops via **Azure Arc**, **AMA**, and **DCRs**, translating foreign firewall traffic through **Syslog/CEF Forwarders**, and streaming threat feeds into the `ThreatIntelligenceIndicator` table.
2. **Data Filtering and Transformation**: Raw log streams pass to our robotic librarian on the **KQL Pipe (`|`)** conveyor belt. Analysts use `search` and `where` to filter rows, `let` to store variables, `extend` to calculate fields, `summarize` (`count`, `dcount`, `arg_max`) to calculate statistics, `render` to draw timecharts, and `union` and `join` (inner, outer, semi, anti) to connect multiple tables.
3. **Data Parsing and Intelligence Enrichment**: Unstructured strings and JSON objects are unpacked using `extract()`, `parse`, and `parse_json()`. Reference data is cross-referenced against CSV **Watchlists** (`_GetWatchlist`), enriched with Threat Intelligence, and normalized across vendors using **ASIM Unifying Parsers** (`imAuthentication`, `imProcessCreate`).
4. **Tripwires and Automated Response**: Automated tripwires fire via **Scheduled**, **NRT**, and **Anomaly Analytics Rules**. When an alarm trips, **SOAR** dispatches automated task forces: **Automation Rules** triage incident metadata, while **Logic App Playbooks** execute high-speed remediation actions (disabling accounts, isolating endpoints, blocking malicious IPs).
5. **Entity Profiling and Threat Hunting**: Human detectives investigate consolidated **Incidents**, using **UEBA** profiling scores to spot behavioral anomalies. Field hunters conduct proactive searches using structured **Hypotheses**, preserving evidence with **Bookmarks**, monitoring live streams, searching historical archives with **Search Jobs**, and executing advanced machine-learning models inside **Python Jupyter Notebooks** using `KQLmagic` and `MSTICPy`, delivering complete threat detection, investigation, and automated response across the globe.
