# Threat Detection, Incident Management, and Automation in Microsoft Sentinel

## The Big Picture

Imagine operating a massive, state-of-the-art security command center for a sprawling corporate campus. Millions of raw security logs, badge swipes, firewall logs, and email transactions pour into your database files every hour. If your human security team is forced to stare at these raw screens 24/7 trying to spot a single thief in a crowd, they will eventually miss the actual break-in.

To turn this overwhelming noise into a clear signal, you establish a **Global Intelligence Agency**—known in our digital estate as **Microsoft Sentinel** (a cloud-native security platform that aggregates, analyzes, and responds to threat data across the entire enterprise network).

This centralized agency operates as an intelligent pipeline. It sets up digital tripwires to catch bad behavior, deploys a dispatch desk to triage alarms, posts robotic guards to respond instantly, builds smart behavioral profiles of everyone on campus, translates foreign firewall dialects into a single universal language using a robotic librarian, and displays the processed results on a giant command center Jumbotron. By mastering these infrastructure engineering and workflow automation solutions, you transform chaotic mountains of telemetry into clear, actionable proof of a security threat.

---

## The Core Mechanics

### 1. Ingestion Governance: Content Hub Solutions and Repository Connections

Before setting up digital tripwires or automating security workflows, engineers must import the proper integrations and instructions into the command center. Security teams begin at the **Content Hub** (`Microsoft Sentinel > Content management > Content hub`), a digital hardware store used to centrally discover, install, and update out-of-the-box solutions.

```
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                               SENTINEL CONTENT GOVERNANCE                             │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  CONTENT HUB (Discover & Deploy Out-of-the-Box Solutions)                              │
│  • Data Connectors ──► Ingest log telemetry into workspace tables                      │
│  • Workbooks       ──► Visual, interactive reporting dashboards                         │
│  • Analytics Rules ──► Real-time security detection queries                            │
│  • Playbooks       ──► Step-by-step automation workflows (Logic Apps)                 │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  REPOSITORIES (CI/CD Automated Integration via GitHub / Azure DevOps)                 │
│  • Version-control custom KQL queries, playbook JSONs, and workbook templates         │
│  • Requires Workspace Owner or User Access Administrator + Sentinel Contributor roles │
│  • Limits: Max 5 active connections per workspace; 800 deployments per resource group │
└───────────────────────────────────────────────────────────────────────────────────────┘
```

- **Solutions**: Packages containing data connectors, visual workbooks, analytic rules, hunting queries, and playbooks that fulfill an end-to-end product or industry vertical scenario. Installing a solution initiates a wizard where administrators configure target subscriptions, resource groups, and workspace destinations.
- **Source Control Repositories**: To manage custom analytics templates, automation rules, and workbooks across multiple environments, engineers utilize **Repositories** (`Microsoft Sentinel > Content management > Repositories`). This feature connects Sentinel to external version-control hosts, specifically **GitHub** or **Azure DevOps**.
- **Repository Deployment Prerequisites**: Creating a repository connection requires holding the **Owner** role in the resource group containing the Sentinel workspace, or a combination of the **User Access Administrator** and **Microsoft Sentinel Contributor** roles.
- **Repository Limits and Constraints**: Each Sentinel workspace is limited to a maximum of **five** active repository connections. Additionally, Azure restricts each resource group to **800 deployments** in its historical log; exceeding this quota triggers a `DeploymentQuotaExceeded` error.
- **The GitHub Connection Flow**: When connecting a GitHub repository, engineers authorize the connection using their credentials, select a repository, and install the **Azure-Sentinel** application on the target repository. Selecting a branch and content type generates an automated deployment pipeline.
- **Deploying Parsers and Hunting Queries**: Both data parsers and hunting queries utilize the **Saved Searches API** (application programming interface, a standard gateway for computer programs to talk to each other). If you select either content type during repository setup, both types will be deployed together to the workspace.
- **The DevOps Connection Flow**: For Azure DevOps, engineers select their organization, project, repository, branch, and target content types. This generates an automated CI/CD (continuous integration and continuous deployment, a pipeline that tests and deploys configuration files) build pipeline that publishes updates to Sentinel whenever files are modified in the repository.

Now that the required integration packages are installed and governed through version-controlled repositories, security engineers can establish digital alarm tripwires across the enterprise.

---

### 2. Digital Alarm Tripwires: Analytics Rules and Alert Generation

To detect threat actors automatically, Sentinel uses **Analytics Rules** (automated search queries that continuously scan raw security logs and raise alarms when malicious patterns are detected).

```
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                               SENTINEL ANALYTICS RULE TYPES                           │
├───────────────────┬───────────────────────────────────────────────────────────────────┤
│ Scheduled Rules   │ Custom KQL queries running on a strict timer (e.g., every 1 hour) │
├───────────────────┼───────────────────────────────────────────────────────────────────┤
│ Near-Real-Time    │ Low-latency rules that run continuously to detect rapid attacks   │
├───────────────────┼───────────────────────────────────────────────────────────────────┤
│ Fusion Rules      │ Multi-stage ML engine correlating low-fidelity alerts into        │
│                   │ high-fidelity incidents (Enabled by default)                      │
├───────────────────┼───────────────────────────────────────────────────────────────────┤
│ Anomaly Rules     │ ML behavior rules that log findings silently to the Anomalies     │
│                   │ table (No noisy incident generation)                              │
├───────────────────┼───────────────────────────────────────────────────────────────────┤
│ Microsoft Security│ Automatically creates Sentinel incidents from alerts raised by    │
│ Rules             │ connected services (e.g., Defender for Office 365)               │
└───────────────────┴───────────────────────────────────────────────────────────────────┘
```

Security engineers configure six distinct styles of analytics rule templates:

- **Scheduled Query Rules**: Pre-configured or custom KQL queries that run on a strict timer. Administrators configure the **Analytics Rule Wizard** across five sections:
    1. **General Tab**: Define the rule's Name, Description, Severity (High, Medium, Low, Informational), Tactics (mapped to 14 MITRE ATT&CK methodology categories), and Status (Enabled/Disabled).
    2. **Set Rule Logic Tab**:
        - **Rule Query**: Enter the KQL query. Select **Test with current data** in the Results Simulation pane to preview the estimated alert volume.
        - **Entity Mapping**: Map up to five target entities (e.g., Account, Host, IP, URL, FileHash) to specific columns in the query output. This enables visual graph investigations.
        - **Custom Details**: Map key-value pairs from query results to display as event parameters.
        - **Alert Details**: Dynamically customize properties (such as severity or name) for each alert using query output variables.
        - **Query Scheduling**: Configure how often the query runs and the historical lookback window. To prevent duplicate alerts, the lookback window should not overlap with the run frequency.
        - **Alert Threshold**: Set the logical expression (e.g., _Is greater than_, _Is fewer than_, _Is equal to_, _Isn't equal to_) for the row count required to trigger an alert.
        - **Event Grouping**: Select **Group all events into a single alert** (default) or **Trigger an alert for each event**.
        - **Suppression**: Toggle **Stop running the Query after the alert is generated** to "On" to temporarily pause rule execution for a specified duration to prevent alert storms.
    3. **Incident Settings Tab**: Turn on **Create incidents from alerts triggered by this analytics rule**. Group related alerts into a single incident based on matching entities, matching rule names, or all alerts within a defined timeframe. Enable or disable reopening closed matching incidents.
    4. **Automated Response Tab**: Attach an **Automation Rule** or direct Logic App playbooks.
    5. **Review and Create**: Validate the query logic and publish the rule.
- **Near-Real-Time (NRT) Rules**: Low-latency, high-speed rules that execute continuously to detect rapid attack techniques.
- **Fusion Rules**: A machine-learning engine that correlates multiple low-fidelity alerts across different security solutions into high-fidelity actionable incidents.
    - Fusion is enabled by default, operates with hidden logic, and is limited to a single active instance.
    - Requires specific connectors to be configured, including out-of-the-box anomalies, Microsoft Entra ID Protection, Microsoft Defender for Cloud, Microsoft Defender for IoT, Microsoft Defender XDR, and custom scheduled analytics rules.
    - Scheduled rules must contain MITRE ATT&CK tactics and entity mapping to be ingested by the Fusion engine.
    - Common Fusion scenarios include identifying compromised accounts involved in data exfiltration, data destruction, denial of service, lateral movement, or ransomware.
- **Anomaly Rules**: Machine-learning rules that flag unusual behavior. Anomaly rules do not trigger noisy incident tickets directly; instead, they log "suspicion markers" into the `Anomalies` table to assist threat hunters during investigations.
    - _Tuning Anomalies_: Since active anomaly rules cannot be edited directly, engineers **Duplicate** the rule to create a copy with the suffix " - Customized". The customized copy runs in **Flighting (testing) mode** while the original runs in **Production mode**. Analysts compare results in the `Anomalies` table and promote the customized rule to Production, which automatically demotes the original to Flighting.
- **Microsoft Security Rules**: Automatically creates Sentinel incidents from alerts generated by connected security solutions (such as Microsoft Defender for Cloud, Defender for Office 365, or Microsoft Entra ID Protection). These can be filtered by severity and specific alert name substrings.
- **ML Behavior Analytics**: Non-customizable, built-in machine learning rules that analyze logs to identify behavioral anomalies like remote desktop protocol (RDP) or secure shell protocol (SSH) sign-in activity.

Once these digital tripwires identify suspicious activity and generate alerts, they must be triaged and remediated at machine speed using the dispatch desk and automated robotic task forces.

---

### 3. Automated Robotic Task Forces: SOAR, Automation Rules, and Logic App Playbooks

When an alert triggers, managing every ticket manually would quickly exhaust your security staff. Microsoft Sentinel integrates **SOAR** (security orchestration, automation, and response, an automated computing framework that executes immediate response workflows against active threats) to remediate threats at machine speed. This automated response framework is divided into two roles: **Automation Rules** (the dispatch desk) and **Playbooks** (the robotic task forces).

```
[ Incident or Alert Triggered ]
               │
               ▼
┌───────────────────────────────────────────────────────────────────────┐
│              THE DISPATCH DESK: AUTOMATION RULES                      │
├───────────────────────────────────────────────────────────────────────┤
│ • Ingests incident/alert metadata and runs automated triage           │
│ • Triggers: When incident is created, updated, or when alert is made │
│ • Conditions: AND / OR / NOT / CONTAINS logic evaluations             │
│ • Actions: Assign owner, set severity, add tags, or close incident    │
│ • Dispatches Robotic Task Forces (Executes Logic App Playbooks)       │
└──────────────────────────────────┬────────────────────────────────────┘
                                   │
                                   ▼
┌───────────────────────────────────────────────────────────────────────┐
│             THE ROBOTIC TASK FORCE: LOGIC APP PLAYBOOKS               │
├───────────────────────────────────────────────────────────────────────┤
│ • Built using Azure Logic Apps drag-and-drop workflow designer        │
│ • Triggers: Sentinel Alert Triggered vs. Sentinel Incident Triggered  │
│ • Connectors: Establish API credentials using Service Principals      │
│ • Actions: Disable Entra ID user, isolate devices, block firewall IPs │
└───────────────────────────────────────────────────────────────────────┘
```

#### The Dispatch Desk: Automation Rules

**Automation Rules** provide centralized, lightweight metadata orchestration. They can be created from the Automation page (`Microsoft Sentinel > Configuration > Automation`), the Analytics Rule Wizard, or directly from an active incident in the queue to handle suppression.

- **Triggers**: Supports three event triggers:
    1. _When an incident is created_: The most common trigger, used for initial triage, classification, owner assignment, and tagging.
    2. _When an incident is updated_: Fires when status, severity, owner, or other properties change.
    3. _When an alert is created_: Fires when a scheduled or NRT rule triggers an alert.
- **Conditions**: Evaluate incident and entity properties using logical operators (`AND`, `OR`, `NOT`, `CONTAINS`).
- **Actions**: Change incident status (e.g., closing false positives with a reason and comment), change severity, assign an owner, add classifying tags (such as `Ransomware`), or trigger a playbook.
- **Order and Expiration**: Rules execute sequentially based on their defined **Order** (Priority). Rules can include an **Expiration date**, which is useful for temporary alert suppression during scheduled activities like penetration testing.

#### The Robotic Task Forces: Logic App Playbooks

**Playbooks** handle complex remediation tasks across external services and are built on **Azure Logic Apps** (an enterprise cloud service used to automate business processes and workflows).

- **The Microsoft Sentinel Logic Apps Connector**: Provides two triggers:
    1. _When a response to a Microsoft Sentinel alert is triggered_: Passes individual alert properties to the workflow.
    2. _When a Microsoft Sentinel incident creation rule was triggered_: Passes full incident context and entities to the workflow.
- **Built-in Actions**: The connector supports actions like _Add comment to incident_, _Add labels to incident_, _Change incident status_, and _Update incident_.
- **API Connections**: The first time a playbook is designed, the engineer signs in using Microsoft Entra ID or a **Service Principal** (an application identity created for automated services). This establishes an encrypted API connection resource in the resource group, prefixed with `azuresentinel`.
- **Dynamic Content and Logic Controls**: Playbook steps use **Dynamic content** outputs from previous steps. Engineers use control groups—such as **Conditions** (if/then statements), switch cases, and **For each** loops (e.g., iterating through a list of accounts returned by _Entities - Get Accounts_)—to build decision-making logic into the workflow.
- **Permissions**: To allow Sentinel to execute playbooks automatically, you must assign permissions. Navigate to `Microsoft Sentinel > Configuration > Settings > Settings tab > Playbook permissions > Configure permissions` and select the resource group containing your playbooks.
- **On-Demand Playbook Execution**: Playbooks can be run manually during investigations. On the Incidents page, select an incident, select **View full details**, go to the Alerts panel, click **Actions**, and choose **Run playbook**.

Once automation rules and playbooks handle the initial response, security analysts must step in to investigate the compiled case files.

---

### 4. Organizing the Evidence: Incident Hierarchy and UEBA Baselines

To conduct thorough investigations, security analysts organize evidence into a structured hierarchy and leverage machine-learning baselines to identify stealthy attackers.

```
[ Events (Raw Database Logs) ] ──► [ Alerts (Analytics Rules) ] ──► [ Entities (Mapped Assets) ] ──► [ Incidents (Case Files) ]
```

#### The Incident Hierarchy

Investigators evaluate security cases across four nested levels:

1. **Events**: Raw log entries stored in Log Analytics database tables (e.g., a single failed logon attempt).
2. **Alerts**: Warnings generated when an analytics rule query matches specific events.
3. **Entities**: Real-world assets or actors involved in an alert, such as Accounts, Hosts, IP addresses, URLs, and FileHashes.
4. **Incidents**: Unified case files that aggregate related alerts, events, and entities into a single, manageable ticket.

#### Analyzing Incidents in the Queue

On the **Incidents Page** (`Microsoft Sentinel > Threat management > Incidents`), analysts review active cases and manage their metadata:

- **Triage Controls**: Update **Status** (New, Active, Closed), change **Severity** (Informational, Low, Medium, High), and assign **Owner** metadata.
- **Directory Reader Role Requirement**: Users investigating incidents in Microsoft Sentinel must be members of the **Directory Reader** role in Microsoft Entra ID to successfully search and resolve entity details.
- **Closing Incident Classifications**: When closing an incident, investigators must select a resolution: _True Positive_, _Benign Positive_, _False Positive - Incorrect alert logic_, _False Positive - Inaccurate data_, or _Undetermined_.
- **The Investigation Graph**: Click **Investigate** on an incident page to open an interactive visualization of the attack. The graph displays entities as nodes and alerts as connections, allowing analysts to explore relationships, timelines, and raw log files in a visual workspace.
- **Microsoft Defender Portal Integration**: In the unified Defender portal (`https://security.microsoft.com/`), analysts triage synchronized incidents, run **Security Copilot** to generate plain-English summaries, and review process execution trees on device pages.

#### User and Entity Behavior Analytics (UEBA)

To detect quiet threats that bypass static rules, Sentinel integrates **UEBA**.

- **Behavioral Profiling**: UEBA analyzes ingested logs to build baseline behavioral profiles for users, hosts, IP addresses, and applications over time and peer horizons.
- **Investigation Priority Score**: Anomalous activities are scored on a scale of **0 to 10** based on how far they deviate from the baseline of the user, their peers, and the broader organization.
- **Entity Pages**: Selecting any mapped user or host opens a detailed datasheet featuring:
    - _Left Panel_: Identifying information pulled from Entra ID, Azure Monitor, and Defender.
    - _Center Panel_: An interactive chronological timeline of alerts, bookmarks, and aggregated activities.
    - _Right Panel_: Behavioral insights derived from ML models, querying datasets such as `Syslog`, `SecurityEvent`, `AuditLogs`, `SigninLogs`, `OfficeActivity`, and `BehaviorAnalytics`.

When analyzing security events across different systems, analysts often find that different tools use different names for the same type of data. To fix this, the command center uses a universal translator.

---

### 5. The Universal Translator: ASIM Data Normalization and Custom Parsers

When data is ingested from multiple vendors (e.g., comparing Cisco, Palo Alto, and Windows logs), they often use different column names for the same attributes (e.g., `user_name` vs. `TargetUserName`). To normalize this data, Sentinel implements the **ASIM** (Advanced Security Information Model, an enterprise data normalization framework that standardizes varying vendor schemas into unified fields).

```
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                             ASIM DATA NORMALIZATION WORKFLOW                          │
├───────────────────────────────────────────────────────────────────────────────────────┤
│ 1. INGESTION  ──► Raw log data flows into tables (e.g., Syslog, CommonSecurityLog)   │
│ 2. TRANSLATE  ──► Source-Specific Parsers map raw columns to standardized fields      │
│ 3. COALESCE   ──► Unifying Parsers (_Im_Schema) combine multiple sources into one view│
│ 4. CONSUME    ──► Analytic rules and workbooks query the Unifying Parser directly     │
├───────────────────────────────────────────────────────────────────────────────────────┤
│                                 OPTIMIZATION STRATEGIES                               │
│ • Ingestion Transformations: Run KQL queries inside a Workspace Transformation DCR    │
│   on the virtual 'source' table to filter or mask data before writing to disk         │
│ • Query Optimization: Pass parameters (starttime, srcipaddr) directly to the parser   │
│   function (e.g., _Im_Dns(starttime=ago(1d))) to filter before parsing               │
└───────────────────────────────────────────────────────────────────────────────────────┘
```

ASIM aligns with the community-led **OSSEM** (Open Source Security Events Metadata) project to enable predictable correlation across normalized tables.

#### ASIM Parser Hierarchy

Data translation occurs at query time using nested KQL user-defined functions:

- **Source-Specific Parsers**: Map a specific product's raw columns to ASIM standardized schemas.
- **Unifying Parsers**: Combine all source-specific parsers for a schema into a single queryable view. Built-in unifying parsers start with the `_Im_` prefix (e.g., `_Im_Authentication`, `_Im_ProcessEvent`, `_Im_Dns`, `_Im_NetworkSession`, `_Im_WebSession`), while manually deployed parsers use the `im` prefix.

#### Optimizing Parser Performance with Parameters

Because parsing every row at query time can impact performance, ASIM schemas utilize optional **filtering parameters** (such as `starttime`, `endtime`, `srcipaddr`, or `domain_has_any`):

- **Pre-Filtering**: Adding parameters to a unifying parser call (e.g., `_Im_Dns(starttime=ago(1d), responsecodename='NXDOMAIN')`) filters the raw tables _before_ the parsing logic runs, which significantly improves query speed.
- **Parameterized KQL Functions**: To build these optimized tools, engineers save queries as workspace functions with defined parameters (such as `string CategoryParam` and `datetime DateParam`).

#### Developing a Custom ASIM Source-Specific Parser

When a connected device does not have a built-in parser, engineers build one using a KQL query configured on the Logs page:

1. **Filter**: Use `where` to isolate target records using physical built-in columns rather than parsed fields to optimize performance. Parsers must not filter by time, as the user's query defines the time range.
    - _Watchlist Filtering_: If raw logs do not contain clear identifying fields, query the `ASimSourceType` watchlist using a `let` helper function to identify source computers (e.g., `| where Computer in (Sources_by_SourceType('InfobloxNIOS'))`).
2. **Parse**: Extract data from unparsed text columns. KQL parsing operators are ranked by efficiency:
    - `split` (best performance for delimited strings).
    - `parse_csv` (parses CSV-formatted strings).
    - `parse` (parses patterns or simplified expressions).
    - `extract_all` and `extract` (use regex patterns for single or multiple values).
    - `parse_json` (best for JSON structures; if you only need a few keys, use `parse` or `extract` instead for better performance).
    - _Type Conversion and Mapping_: Convert types using functions like `todatetime` or `tohex`. Map numeric codes or values using `iff` (for two values) or `lookup` against a `datatable` (for multiple values) to ensure output matches ASIM standards.
3. **Prepare Fields**: Format the final output table:
    - `project-rename`: Renames physical columns to preserve their high indexing performance.
    - `project-away`: Excludes temporary parsing variables or unneeded columns.
    - _Warning_: Never use `project` in a parser, as it discards non-normalized columns, which violates ASIM's robustness principle.
    - `extend`: Creates standard field aliases.
4. **Handle Variants**: If log formats vary across versions, build separate queries for each variant and combine them using a `union` statement, ensuring each branch pre-filters its source logs to prevent duplicate entries.
5. **Deploy**: Save the query as a workspace function, or use the ASIM YAML-to-ARM template converter to deploy the function at scale via PowerShell.

#### Ingestion-Time Transformations using DCRs

If a log source requires extensive parsing or contains sensitive information (such as personally identifiable information), engineers configure **Workspace Transformation DCRs**. This ingestion-time pipeline runs a KQL query against the virtual `source` table, allowing you to filter out noise, mask sensitive columns, or pre-parse data _before_ it is written to disk, which can help reduce Log Analytics storage fees.

Once raw telemetry is normalized and stored, engineers must present these insights to security leaders using interactive visual dashboards.

---

### 6. The Jumbotron Visualizer: Sentinel Workbooks and Data Monitoring

To visualize security trends and monitor active investigations, engineers use **Workbooks** (interactive, visual reporting dashboards that combine text, analytics queries, metrics, and parameters).

```
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                               SENTINEL WORKBOOK ELEMENTS                              │
├─────────────────┬─────────────────────────────────────────────────────────────────────┤
│ Text Blocks     │ Formatted using Markdown for titles, headers, and descriptions      │
├─────────────────┼─────────────────────────────────────────────────────────────────────┤
│ KQL Queries     │ Runs background queries and renders data as tables (grids), area    │
│                 │ charts, bar charts, line charts, pie charts, or timecharts          │
├─────────────────┼─────────────────────────────────────────────────────────────────────┤
│ Parameters      │ Dropdowns, text inputs, or time pickers that filter query results   │
├─────────────────┼─────────────────────────────────────────────────────────────────────┤
│ Links & Tabs    │ Custom navigation buttons that open URLs or set parameters         │
├─────────────────┼─────────────────────────────────────────────────────────────────────┤
│ Metric Steps    │ Ingests performance metrics from connected Azure resources          │
├─────────────────┼─────────────────────────────────────────────────────────────────────┤
│ Advanced Editor │ Raw JSON configuration editor for custom workbook structures        │
└─────────────────┴─────────────────────────────────────────────────────────────────────┘
```

- **The Logs Interface**: Before adding visualizations to a workbook, analysts build and test KQL queries on the **Logs Page** (`Microsoft Sentinel > General > Logs`).
    - _Query Tools_: The interface includes a Queries gallery of predefined templates, a Query Explorer to access saved queries, a Time Range selector, and an **Export** tool that downloads query outputs as a CSV (comma-separated values) file or a Power BI M query.
    - _KQL Operators_: Standard analytical queries utilize operators like `count`, `take` (limit results), `project`, `sort` / `order`, `top`, `extend`, `summarize`, and `render`.
- **Workbook Templates**: Sentinel includes preconfigured workbook templates (such as the _Microsoft Entra sign-in logs_ workbook, which visualizes sign-in locations, failed authentication errors, and Conditional Access MFA status).
- **Creating and Saving Workbooks**: To customize a template, select it, click **Save** (which generates an Azure Resource containing the workbook's JSON configuration), and click **View saved workbook** under the _My workbooks_ tab.
- **Workbook Design Canvas**: Clicking **Edit** opens the design canvas, exposing "Edit" controls for each step. Designers add elements using the **+ Add** menu:
    - **Text**: Written using **Markdown** formatting.
    - **Query**: Enter KQL queries and select a **Visualization** (e.g., Grid, Area chart, Bar chart, Pie chart, Scatter chart, or Time chart).
    - **Grid Column Settings**: Grids (tables) can be customized using Column Settings to define labels, decimal formats, and advanced renderers like **Heatmaps**, **Bars**, and **Spark areas**.
    - **Parameters**: Allow users to filter workbook contents dynamically. Parameter types include Text, Drop downs (populated by KQL queries or JSON arrays), Time range pickers, Resource pickers, and Subscription pickers. Parameter values are referenced in other query steps using **Bindings** or value expansions.
    - **Links/Tabs**: Create tabbed navigation and assign button styles (primary or secondary).
    - **Metric Steps**: Ingest performance metrics from other Azure resources.
- **The Advanced Editor**: For advanced configurations, select **Advanced Editor** to view and modify the raw JSON representation of the workbook.

---

## Connecting the Dots

To eliminate blind spots across an enterprise, every component in Microsoft Sentinel integrates into a unified threat detection and automated response pipeline.

```
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                              THE AUTONOMIC SIEM/SOAR PIPELINE                         │
│                                                                                       │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Governance & Ingestion│   │ Ingestion Translation  │   │ Alarm Tripwires         │  │
│  │ (Content Hub, Solutions│──►│ (Transformation DCRs,  │──►│ (Scheduled, NRT, and    │  │
│  │  & Repositories)      │   │  ASIM Unifying Parsers)│   │  Anomaly Detection)     │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Automated Dispatch    │──►│ Robotic Task Forces    │◄──│ Triage and profiling    │  │
│  │ (Automation Rules,    │   │ (Logic App Playbooks,  │   │ (Incident management,   │  │
│  │  Triage metadata)     │   │  Dismiss-AADRiskyUser) │   │  UEBA, investigation)   │  │
│  └───────────────────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ THE JUMBOTRON GRAPH    │                                │
│                              │ (Interactive KQL and   │                                │
│                              │  Sentinel Workbooks)   │                                │
│                              └────────────────────────┘                                │
└───────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Acquisition and Compliance**: Security teams begin by deploying out-of-the-box integrations from the **Content Hub**. Custom rules, parsers, and playbooks are version-controlled in **Repositories** (GitHub or Azure DevOps) and deployed automatically via CI/CD pipelines.
2. **Universal Data Normalization**: Raw telemetry is normalized using **ASIM unifying parsers** (e.g., `_Im_Authentication`), mapping vendor fields to standard schemas. Ingestion-time KQL queries run within **Workspace Transformation DCRs** to clean, filter, and mask raw events before they are written to disk.
3. **Configuring Alarm Tripwires**: **Analytics Rules** monitor these standardized tables. **Scheduled Rules** run on defined intervals, using Results Simulation to test detection logic and mapping entities to enable graphical investigations. High-speed threats trigger **NRT Rules**. Complex multi-stage attacks are correlated by the **Fusion** engine, while behavioral anomalies are logged to the `Anomalies` table.
4. **Triage and Incident Management**: Triggered alerts are grouped into **Incidents** based on alert grouping rules to minimize noise. Analysts investigate cases on the **Incidents Page** using the **Investigation Graph** to analyze entities and relationships. The team leverages **UEBA** profiling scores and entity timelines to identify activities that deviate from normal user and peer habits.
5. **Automated Dispatch and Response**: **Automation Rules** act as a dispatch desk, triaging newly created incidents by modifying status, assigning owners, applying tags, and enforcing order of execution. For complex response scenarios, automation rules execute **Logic App Playbooks**. These playbooks use the Microsoft Sentinel connector, dynamic content, and loop actions to disable compromised accounts and isolate endpoints at machine speed.
6. **Visualizing Insights**: Finally, security metrics are put up on the command center Jumbotron using **Workbooks**. Designers build interactive dashboards using Markdown text, parameters, tabs, and query steps, transforming raw KQL logs into real-time visual charts to help defenders keep the enterprise secure.

---

📊 **Next Step**: I can now generate a detailed visual slide deck outline or an interactive quiz on Module 9 to help solidify these threat detection, incident management, and automation configurations for your SC-200 preparation!