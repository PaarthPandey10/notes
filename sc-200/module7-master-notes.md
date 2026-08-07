# Configuring and Operating Microsoft Sentinel

## The Big Picture

Imagine your digital campus has grown so large that you have thousands of guards, millions of cameras, and endless alarms going off every single second. Even with the best frontline defenders, your security team will drown in the noise if they cannot connect the scattered clues. To manage this massive flow of intelligence, you build a **Master Command Center** known as **Microsoft Sentinel** (a cloud-native security platform that aggregates, analyzes, and responds to threat data across the entire enterprise network).

This central command post functions simultaneously as a **SIEM** (security information and event management, a centralized system that collects, stores, and analyzes security logs from across the entire organization) and a **SOAR** (security orchestration, automation, and response, an automated computing framework that executes immediate response workflows against active threats). Operating from a unified interface, this master command center ingests raw surveillance feeds from every facility, stores historical records in deep archives, sets up automated alarm tripwires, cross-references incoming traffic against VIP guest lists and most-wanted offender boards, and merges seamlessly with frontline security guards to stop attacks at machine speed.

---

## The Core Mechanics

### Central Command Architecture: SIEM, Data Lakes, and Multi-Tenant Workspaces

Before running digital cables or setting up security desks, architects must design the physical and logical layout of the master command center. Installing Microsoft Sentinel begins by provisioning an underlying **Log Analytics Workspace** (an Azure database environment where log data is collected, stored, and queried).

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SENTINEL COMMAND CENTER DIVISIONS                               │
├──────────────────────────────────────────┬─────────────────────────────────────────────┤
│ SIEM OPERATIONS FLOOR                    │ PLATFORM DATA LAKE                          │
│ (Log Analytics Workspace)                │ (Massive Storage Pool)                      │
├──────────────────────────────────────────┼─────────────────────────────────────────────┤
│ • Fast, high-frequency security searches │ • Low-cost long-term retention (up to 12 yrs)│
│ • Operational retention window (30-730d) │ • Graph-based analysis for slow attacks     │
│ • Powers real-time analytics & hunting   │ • Supports asynchronous KQL / Spark jobs    │
└──────────────────────────────────────────┴─────────────────────────────────────────────┘
```

The master command center operates across two distinct operational divisions:

- **The Microsoft Sentinel SIEM Division**: The fast, active operations floor where security analysts perform daily threat hunting, incident triage, and real-time alert correlation.
- **The Microsoft Sentinel Platform Division**: A long-term strategic archive functioning as a **Data Lake** (a scalable, low-cost storage repository that holds raw data in its native format for extended analysis) that can retain logs for up to **12 years** while using graph-based correlation to uncover slow-moving, multi-year attack campaigns.

Architects choose from three deployment options based on company footprint and legal requirements:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      WORKSPACE ARCHITECTURE OPTIONS                    │
├──────────────────────────┬─────────────────────────────────────────────┤
│ SINGLE-TENANT SINGLE     │ Central repository for all logs across the  │
│ WORKSPACE                │ entire tenant. Single pane of glass, but may│
│                          │ incur cross-region bandwidth costs and fail │
│                          │ local data sovereignty laws.                │
├──────────────────────────┼─────────────────────────────────────────────┤
│ SINGLE-TENANT REGIONAL   │ Multiple workspaces deployed in specific    │
│ WORKSPACES               │ regions. Complies with local Data Residency │
│                          │ laws and eliminates cross-region fees, but  │
│                          │ lacks a single native pane of glass.        │
├──────────────────────────┼─────────────────────────────────────────────┤
│ MULTI-TENANT WORKSPACES  │ Independent workspaces across distinct      │
│                          │ corporate tenants, managed centrally using  │
│                          │ Azure Lighthouse.                           │
└──────────────────────────┴─────────────────────────────────────────────┘
```

When managing regional or multi-tenant deployments, administrators deploy specialized management tools:

- **Workspace Manager**: Centrally publishes analytics rules, saved searches, workbooks, and automation policies from a central workspace out to member workspaces across one or more tenants (`Microsoft Sentinel > Configuration > Settings > Workspace manager`).
- **Azure Lighthouse**: A cross-tenant management service that allows security providers to view and manage customer Sentinel workspaces without constantly signing in and out of separate corporate accounts.

To create a new workspace in the Azure portal:

1. Search for **Microsoft Sentinel** in the top search bar and select **+ Add**.
2. On the **Add Microsoft Sentinel to a workspace** screen, select **+ Create a new workspace**.
3. On the **Basics** tab, specify the **Subscription**, **Resource Group**, workspace **Name** (which becomes the Sentinel workspace name), and **Region** (the geographic location where log data resides; cannot be changed after creation).
4. Select **Review + Create**, then **Create**.
5. Once deployed, select the new workspace from the list and select **Add** to enable Microsoft Sentinel.

```
[ Search 'Microsoft Sentinel' ] ──► [ Select '+ Add' ] ──► [ Select '+ Create a new workspace' ]
                                                                      │
                                                                      ▼
 [ Workspace Active Screen ] ◄── [ Select Workspace & 'Add' ] ◄── [ Complete 'Basics' & Create ]
```

The workspace navigation pane is organized into four main operational areas: **General** (_Overview_, _Logs_, _News & guides_, _Search_), **Threat management** (_Incidents_, _Workbooks_, _Hunting_, _Notebooks_, _Entity behavior_, _Threat intelligence_, _MITRE ATT&CK_), **Content management** (_Content hub_, _Repositories_, _Community_), and **Configuration** (_Data connectors_, _Analytics_, _Watchlist_, _Automation_, _Settings_).

If an organization already uses **Defender for Cloud** (Microsoft Defender for Cloud, a tool that monitors cloud security posture and workload protections), security teams should use the same Log Analytics workspace for both services. However, the default workspace automatically provisioned by Defender for Cloud cannot be converted directly into a Sentinel workspace; administrators must manually create a custom Log Analytics workspace, update the Defender for Cloud pricing tier settings to point to the new workspace, and then enable Sentinel on that workspace.

Now that the command center's physical layout is established, we must define who holds the keys to inspect case files and modify security configurations.

---

### Role-Based Access Control and Workspace Permissions

To prevent unauthorized staff from altering security rules or viewing sensitive evidence, Microsoft Sentinel enforces access boundaries using **Azure RBAC** (role-based access control, a permission system that grants access rights based on job roles).

```
┌────────────────────────────────────────────────────────────────────────┐
│                   SENTINEL SPECIFIC Azure RBAC ROLES                   │
├──────────────────────────────┬─────────────────────────────────────────┤
│ BUILT-IN ROLE                │ ALLOWED ACTIONS & PERMISSIONS           │
├──────────────────────────────┼─────────────────────────────────────────┤
│ Microsoft Sentinel Reader    │ Can view data, incidents, workbooks, and│
│                              │ other Sentinel resources. Cannot edit.  │
├──────────────────────────────┼─────────────────────────────────────────┤
│ Microsoft Sentinel Responder │ Includes Reader rights + can manage     │
│                              │ incidents (assign, dismiss, update).    │
├──────────────────────────────┼─────────────────────────────────────────┤
│ Microsoft Sentinel Contributor│ Includes Responder rights + can create/ │
│                              │ edit workbooks, analytics rules, etc.   │
├──────────────────────────────┼─────────────────────────────────────────┤
│ Microsoft Sentinel Automation│ Special service role that allows        │
│ Contributor                  │ Sentinel to add playbooks to automation │
│                              │ rules. Not intended for human accounts. │
└──────────────────────────────┴─────────────────────────────────────────┘
```

Assigning roles at the **Resource Group** scope ensures that all supporting resources deployed alongside the workspace automatically inherit matching permissions.

Executing specialized operational tasks requires additional role assignments:

- **Working with Playbooks**: Managing automated workflows requires the **Logic App Contributor** role assigned to human analysts, in addition to Sentinel permissions.
- **Granting Sentinel Permission to Run Playbooks**: When automation rules trigger playbooks, Sentinel uses a dedicated service account. For an automation rule to execute a playbook, this service account must be granted explicit **Logic App Contributor** permissions (or rights to execute playbooks) on the resource group containing the playbook. The administrator configuring this automation must hold **Owner** permissions on the resource group containing the playbooks to delegate these rights.
- **Connecting Data Sources**: Ingesting new log feeds requires **Write** permissions on the target Sentinel workspace, plus source-specific permissions detailed on individual connector pages.
- **Guest Users Assigning Incidents**: Guest users in Microsoft Entra ID require the **Directory Reader** role (assigned by default to regular member users) in addition to **Microsoft Sentinel Responder** rights to look up and assign incidents to staff.
- **Creating and Deleting Workbooks**: Creating or deleting visual dashboards requires **Microsoft Sentinel Contributor** or a combination of a lower Sentinel role plus **Workbook Contributor** (an Azure Monitor role).

Analysts can also leverage broader Azure roles—such as **Owner**, **Contributor**, or **Reader** at the subscription level—or Log Analytics roles (**Log Analytics Contributor**, **Log Analytics Reader**). However, because broader Azure roles grant permissions across all underlying Azure resources, administrators must carefully restrict permissions to ensure analysts cannot inadvertently edit infrastructure outside Sentinel.

Once permission boundaries are locked down, administrators must configure data retention limits and storage tiers to balance search performance against budget constraints.

---

### Managing Data Retention, Table Plans, and Log Tiers

In our master command center, surveillance feeds stream into structured database tables. To manage storage costs, administrators configure retention periods and data tiers based on operational needs.

#### 1. Configuring Workspace Retention

By default, interactive log retention at the workspace level is set to **30 days** (which can be extended up to **730 days** [2 years] for a prorated fee). Microsoft Sentinel solution tables can extend interactive retention up to **90 days** at no additional charge.

To adjust workspace retention in the Azure portal:

1. Navigate to `Microsoft Sentinel > Configuration > Settings`.
2. Select **Workspace Settings** (which redirects to the Log Analytics workspace portal).
3. Select **Usage and estimated costs**.
4. Select **Data Retention** at the top of the page.
5. Drag the slider to the desired retention value (30 to 730 days) and select **OK**.

```
[ Sentinel Settings ] ──► [ Workspace Settings ] ──► [ Usage & Estimated Costs ] ──► [ Data Retention Slider ]
```

#### 2. Log Data States and Table Plans

Log Analytics workspace tables operate across two data states: **Analytics retention** (hot state for real-time alerts, hunting, and high-performance queries) and **Long-term retention** (cold state for low-cost compliance archiving, accessible via search jobs and data restores).

Tables are assigned to one of three **Table Plans**:

- **Analytics Plan**: Suited for continuous monitoring, real-time alerts, and threat hunting. Supports full **KQL** (Kusto Query Language, a database query language used to search raw telemetry and logs) queries across interactive retention windows ranging from 30 days to 2 years.
- **Basic Plan**: Designed for high-volume troubleshooting logs (e.g., `ContainerLogV2`, `AppTraces`, or custom DCR-based logs). Offers discounted ingestion and 30 days of single-table search capabilities.
- **Auxiliary Plan**: Designed for high-volume, low-touch compliance and audit logs. Offers low-cost ingestion and unoptimized single-table query access for 30 days.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        BASIC LOGS KQL CONSTRAINTS                      │
├────────────────────────────────────────────────────────────────────────┤
│ SUPPORTED KQL OPERATORS:                                               │
│ • where, extend, project, project-away, project-keep, project-rename,  │
│   project-reorder, parse, parse-where                                  │
├────────────────────────────────────────────────────────────────────────┤
│ UNSUPPORTED KQL OPERATORS (WILL FAIL ON BASIC LOGS):                   │
│ • join, union, summarize (aggregates)                                  │
└────────────────────────────────────────────────────────────────────────┘
```

#### 3. Log Storage Tiers in the Microsoft Defender Portal

When Sentinel is onboarded to the **Microsoft Defender Portal** (`https://security.microsoft.com/`), log retention is managed from the centralized table management experience (`Microsoft Sentinel > Configuration > Tables`).

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DEFENDER PORTAL LOG TIERS                       │
├─────────────────┬──────────────────────────────────────────────────────┤
│ STORAGE TIER    │ RETENTION CAPABILITIES & FUNCTIONALITY               │
├─────────────────┼──────────────────────────────────────────────────────┤
│ Analytics Tier  │ Hot state data (30 days to 2 years interactive)      │
│                 │ mirrored to total long-term retention in the data    │
│                 │ lake for up to 12 years. Supports all real-time     │
│                 │ analytics rules, hunting, and workbooks.             │
├─────────────────┼──────────────────────────────────────────────────────┤
│ Data Lake Tier  │ Low-cost cold storage (30 days to 12 years). Data is │
│                 │ accessible via ad-hoc KQL jobs, Spark jobs, or       │
│                 │ summary rules, but unavailable for real-time alerts. │
├─────────────────┼──────────────────────────────────────────────────────┤
│ XDR Default Tier│ Default storage for Defender XDR advanced hunting    │
│                 │ tables (30 days included with XDR licenses). Data    │
│                 │ can be extended beyond 30 days by ingesting it into │
│                 │ the Analytics tier.                                  │
└─────────────────┴──────────────────────────────────────────────────────┘
```

To modify table retention settings in the Defender portal:

1. Navigate to `Microsoft Sentinel > Configuration > Tables`.
2. Select the target table (e.g., `CommonSecurityLog`) to open the details side panel.
3. Select **Manage table**.
4. Set **Analytics retention** (30 days to 2 years) and **Total retention** (up to 12 years in the data lake). Setting Total retention higher than Analytics retention automatically activates low-cost long-term storage for the remaining period.
5. Select **Save**.

```
[ Defender Portal > Configuration > Tables ] ──► [ Select Table ] ──► [ Manage Table ] ──► [ Set Retention & Save ]
```

Now that storage structures and retention rules are configured, we must examine the specific database tables where incoming security events are stored.

---

### Querying the Intelligence Vault: Logs, Common Tables, and Defender XDR Schemas

To investigate threats effectively, security analysts must know which filing cabinet holds specific evidence. When data connectors stream telemetry into Sentinel, records populate dedicated tables that can be queried using KQL from the **Logs** page in the Azure portal (`Microsoft Sentinel > General > Logs`) or from **Advanced Hunting** (`Investigation & response > Hunting > Advanced hunting`) and **Data Lake Exploration** (`Microsoft Sentinel > Data lake exploration > KQL queries`) in the Defender portal.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   SENTINEL FEATURE-SPECIFIC TABLES                     │
├───────────────────────────┬────────────────────────────────────────────┤
│ TABLE NAME                │ DESCRIPTION & PURPOSE                      │
├───────────────────────────┼────────────────────────────────────────────┤
│ SecurityAlert             │ Stores alerts generated by Sentinel        │
│                           │ analytics rules or ingested from connectors.│
├───────────────────────────┼────────────────────────────────────────────┤
│ SecurityIncident          │ Stores official incident case files        │
│                           │ created from triggered alerts.             │
├───────────────────────────┼────────────────────────────────────────────┤
│ ThreatIntelligenceIndicator│ Stores threat indicators (IPs, domains,    │
│                           │ file hashes) imported from threat feeds.   │
├───────────────────────────┼────────────────────────────────────────────┤
│ Watchlist                 │ Stores imported reference data lists.      │
└───────────────────────────┴────────────────────────────────────────────┘
```

In addition to feature-specific tables, common data connectors populate standard infrastructure tables:

- **`AzureActivity`**: Records management-plane subscription and resource group operations performed in Azure.
- **`AzureDiagnostics`**: Stores internal diagnostic logs for Azure resources running in Azure Diagnostics mode.
- **`AuditLogs`**: Records Microsoft Entra ID audit trails, including user/group changes and application management.
- **`CommonSecurityLog`**: Stores **CEF** (Common Event Format, a standardized text format used by network appliances) messages sent over Syslog.
- **`McasShadowItReporting`**: Stores cloud app discovery logs streamed from Defender for Cloud Apps.
- **`OfficeActivity`**: Audit logs for Office 365 services, including Exchange, SharePoint, and Microsoft Teams.
- **`SecurityEvent`**: Windows security event logs collected from endpoints and domain controllers.
- **`SigninLogs`**: Microsoft Entra ID user sign-in activity logs.
- **`Syslog`**: Raw Linux operating system event logs.
- **`Event`**: Windows Sysmon (System Monitor) event logs.
- **`WindowsFirewall`**: Local Windows Firewall network traffic events.

When the Microsoft Defender XDR data connector is enabled, raw endpoint, identity, and email telemetry streams directly into specialized XDR tables:

- **`AlertEvidence`**: Files, IP addresses, URLs, users, or devices linked to active alerts.
- **`CloudAppEvents`**: User activities within Microsoft 365 and connected cloud applications.
- **`DeviceEvents`**: Endpoint security control events (Defender AV, Exploit Guard).
- **`DeviceFileCertificateInfo`**: Digital certificate details for signed executable files.
- **`DeviceFileEvents`**: File creation, modification, and file system operations on endpoints.
- **`DeviceImageLoadEvents`**: Executable DLL loading events on hosts.
- **`DeviceInfo`**: Endpoint OS builds, machine names, and system properties.
- **`DeviceLogonEvents`**: Local and network authentication events on user devices.
- **`DeviceNetworkEvents`**: Network socket connections and inbound/outbound web calls.
- **`DeviceNetworkInfo`**: Network adapter configurations, IP assignments, and MAC addresses.
- **`DeviceProcessEvents`**: Process creation trees, command-line arguments, and parent processes.
- **`DeviceRegistryEvents`**: Windows registry key creation, deletion, and modification.
- **`EmailEvents`**: Email delivery, filtering, and blocking events in Microsoft 365.
- **`EmailPostDeliveryEvents`**: Post-delivery email actions (e.g., zero-hour auto-purge).
- **`EmailUrlInfo`**: Embedded web links and URLs extracted from incoming emails.
- **`EmailAttachmentInfo`**: File details and hashes for attachments arriving via email.
- **`IdentityDirectoryEvents`**: Active Directory domain controller administrative events.
- **`IdentityLogonEvents`**: On-premises AD and cloud identity authentication attempts.
- **`IdentityQueryEvents`**: Active Directory LDAP query lookups for users, groups, and devices.

With our intelligence tables identified and receiving data, we must now import business reference lists to separate trusted personnel from high-risk targets.

---

### The VIP Guest Lists: Planning, Creating, and Managing Watchlists

In our master command center, security guards frequently refer to static reference lists—such as spreadsheets listing high-value servers, trusted corporate VPN ranges, or recently terminated staff. In Microsoft Sentinel, these reference sheets are maintained as **Watchlists** (`Microsoft Sentinel > Configuration > Watchlist`).

Watchlists are stored as key-value pairs directly inside the workspace database and are cached in memory to ensure low-query latency.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        COMMON WATCHLIST USE CASES                      │
├───────────────────┬────────────────────────────────────────────────────┤
│ USE CASE          │ OPERATIONAL MECHANISM & DESCRIPTION                │
├───────────────────┼────────────────────────────────────────────────────┤
│ Rapid Incident    │ Importing malicious IPs or file hashes from CSVs   │
│ Investigation     │ to correlate against raw logs in KQL queries.      │
├───────────────────┼────────────────────────────────────────────────────┤
│ High-Risk User    │ Importing lists of terminated employees or VIP     │
│ Tracking          │ executives to trigger instant alarms upon sign-in. │
├───────────────────┼────────────────────────────────────────────────────┤
│ Reducing Alert    │ Creating allowlists of authorized admin IPs to     │
│ Fatigue           │ suppress false alarms from routine maintenance.    │
├───────────────────┼────────────────────────────────────────────────────┤
│ Data Enrichment   │ Joining event logs with business metadata (e.g.,   │
│                   │ mapping IP addresses to physical building locations)│
└───────────────────┴────────────────────────────────────────────────────┘
```

To create a new watchlist in the Azure portal:

1. Navigate to `Microsoft Sentinel > Configuration > Watchlist` and select **+ Add new**.
2. On the **General** page, enter a **Name**, **Description**, and **Alias** (the unique identifier used in KQL queries), then select **Next**.
3. On the **Source** page, select the dataset type, upload a local CSV file (file size is limited to a maximum of **3.8 MB**), select the **Search key** column (the primary unique column), and select **Next**.
4. Review the configuration and select **Create**.

```
[ Watchlist Page ] ──► [ + Add New ] ──► [ Name & Alias ] ──► [ Upload CSV (<= 3.8MB) ] ──► [ Select Search Key ] ──► [ Create ]
```

To query a watchlist inside KQL statements, analysts call the **`_GetWatchlist('WatchlistAlias')`** function:

```
// Correlate user sign-in logs against a watchlist of high-value servers
let HighValueHosts = _GetWatchlist('HighValueMachines') | project HostName;
DeviceLogonEvents
| where DeviceName in (HighValueHosts)
```

Managing watchlists requires adhering to operational best practices:

- **Editing Watchlist Items**: To edit or add individual rows, select the watchlist, select **Update watchlist > Edit watchlist items**, select the target row, modify fields, and select **Save**.
- **Bulk Updating**: To append multiple items at scale, select **Update watchlist > Bulk update** and upload a new CSV file. The bulk update appends new records and automatically deduplicates rows where all column values match. Note: Uploading a new CSV file during a bulk update will **not** delete items that were removed from the CSV file; items must be deleted individually in the UI or the watchlist must be completely deleted and recreated.
- **Ingestion SLA Warning**: Log Analytics enforces a **5-minute SLA** (Service Level Agreement) for data ingestion. If an analyst deletes and immediately recreates a watchlist, both the deleted and new entries may appear simultaneously in KQL query results during this 5-minute window.

While watchlists track internal business data, defenders must also import external intelligence feeds to track known cybercriminals across the globe.

---

### The Most Wanted Board: Tactical Threat Intelligence and Indicator Management

To identify active threat campaigns touching the digital campus, our command center maintains a "Most Wanted" board using **Cyber Threat Intelligence (CTI)**. The most actionable form of CTI is tactical threat intelligence, commonly called **IoCs** (indicators of compromise, technical clues such as malicious IP addresses, phishing URLs, domain names, and file hashes).

Threat intelligence is integrated into Sentinel through four primary pathways:

1. **Data Connectors**: Ingesting automated feeds via **TAXII** (Trusted Automated eXchange of Indicator Information, a protocol for sharing threat data) connectors or the **Upload Indicators API**.
2. **Threat Intelligence / Intel Management Page**: Viewing, tagging, filtering, and creating threat indicators manually.
3. **Analytics Rules**: Utilizing built-in threat intelligence rule templates (e.g., _TI map IP entity to AzureActivity_) to cross-reference incoming logs against known IoCs automatically.
4. **Workbooks**: Visualizing indicator matches using the built-in Threat Intelligence workbook.

```
[ TAXII / API Threat Feeds ] ──► [ ThreatIntelIndicators Table ] ──► [ TI Analytics Rules ] ──► [ Incident Generated ]
```

To create a threat indicator manually in the Microsoft Defender portal:

1. Navigate to **Microsoft Sentinel** and select **Threat intelligence** under _Threat management_ (if prompted, select **Open Intel management** to transition to the unified **Intel management** page under the main Defender navigation).
2. Select **+ Add new** from the top menu.
3. Select the **Indicator type** (e.g., _IP Address_, _URL_, _Domain_, _File Hash_), fill in the required fields marked with a red asterisk (e.g., value, confidence score, valid from/until dates), and select **Apply**.

```
[ Defender Portal > Threat Intelligence > Intel Management ] ──► [ + Add New ] ──► [ Fill Indicator Details ] ──► [ Apply ]
```

Analysts apply **Tags** to indicators (individually or via multi-select) to group clues related to specific incidents, threat actors, or attack campaigns.

Threat indicators are stored in database tables accessible via KQL:

- **`ThreatIntelligenceIndicator`**: The legacy table storing imported and user-created indicators.
- **`ThreatIntelIndicators` and `ThreatIntelObjects`**: Modern STIX-compliant (Structured Threat Information eXpression, a standardized language for cyber threat intelligence) schema tables introduced in public preview on April 3, 2025.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   THREAT INTEL TABLE TRANSITION TIMELINE               │
├────────────────────────────────────────────────────────────────────────┤
│ • April 3, 2025: Public preview of ThreatIntelIndicators &             │
│   ThreatIntelObjects tables. Data streams to both old and new tables.  │
├────────────────────────────────────────────────────────────────────────┤
│ • July 31, 2025: Hard deprecation date. Ingestion into the legacy     │
│   ThreatIntelligenceIndicator table stops completely. All custom KQL   │
│   queries, analytics rules, and workbooks MUST be migrated to the new  │
│   tables before this date.                                             │
└────────────────────────────────────────────────────────────────────────┘
```

Querying threat indicators in KQL:

```
// Query active threat indicators using the legacy schema
ThreatIntelligenceIndicator
| where TimeGenerated > ago(24h)
| where Active == true
```

Now that our command center contains threat feeds, watchlists, and database logs, we must examine how Microsoft is merging these SIEM capabilities directly into the frontline security operations portal.

---

### Tearing Down the Walls: Onboarding Sentinel to the Microsoft Defender Portal

Historically, security operations teams were forced to switch back and forth between two separate control rooms: the Azure portal (hosting the Microsoft Sentinel SIEM) and the Microsoft Defender portal (hosting XDR capabilities). Today, Microsoft provides the **Unified Security Operations Platform** (`https://security.microsoft.com/`), which merges Microsoft Sentinel and Microsoft Defender XDR into a single operational workspace.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE UNIFIED OPERATIONS PLATFORM                           │
│                                                                                        │
│  ┌─────────────────────────────────────┐      ┌─────────────────────────────────────┐  │
│  │     MICROSOFT DEFENDER XDR          │      │      MICROSOFT SENTINEL SIEM        │  │
│  │ (Endpoints, Identities, Email, Apps)│      │  (Multicloud, Third-Party, Logs)    │  │
│  └──────────────────┬──────────────────┘      └──────────────────┬──────────────────┘  │
│                     │                                            │                     │
│                     └──────────────────────┬─────────────────────┘                     │
│                                            ▼                                           │
│  ┌──────────────────────────────────────────────────────────────────────────────────┐  │
│  │                      SINGLE UNIFIED DEFENDER PORTAL WEB UI                       │  │
│  │                                                                                  │  │
│  │ • Unified Incident Queue & Correlated Attack Stories                             │  │
│  │ • Unified Advanced Hunting (Querying Defender + Sentinel Tables in One Window)   │  │
│  │ • Unified Entity Pages (Devices, Users, IP Addresses, Azure Resources)           │  │
│  │ • Automatic Attack Disruption for SAP Applications                               │  │
│  └──────────────────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

#### Onboarding Prerequisites and RBAC Requirements

To connect a Microsoft Sentinel workspace to the Microsoft Defender portal, administrators must satisfy specific prerequisites:

- An active Log Analytics workspace with Microsoft Sentinel enabled.
- The **Microsoft Defender XDR data connector** installed from Content Hub and enabled in Sentinel for incidents and alerts.
- Microsoft Defender XDR onboarded to the Microsoft Entra tenant.
- An Azure account assigned the **Owner** or **User Access Administrator** role on the subscription _plus_ the **Microsoft Sentinel Contributor** role on the workspace or resource group scope.

#### Onboarding Step-by-Step Workflow

1. Sign in to the Microsoft Defender portal (`https://security.microsoft.com/`).
2. On the **Home (Overview)** page, locate the **Get your SIEM and XDR in one place** banner and select **Connect a workspace**.
3. Select the target primary Microsoft Sentinel workspace from the dropdown list and select **Next**.
4. Review the product changes and select **Connect**.

```
[ Defender Portal Home ] ──► [ Connect a Workspace Banner ] ──► [ Select Workspace ] ──► [ Review Changes ] ──► [ Connect ]
```

#### Automatic Architectural Changes Upon Connection

When a workspace is connected to the Defender portal, the system automatically executes four structural changes:

1. All Sentinel log tables, functions, and saved KQL queries become instantly accessible inside **Advanced Hunting** within the Defender portal.
2. The **Microsoft Sentinel Contributor** role is automatically granted to the `Microsoft Threat Protection` and `WindowsDefenderATP` system applications within the Azure subscription.
3. Active Microsoft security incident creation rules in Sentinel are automatically deactivated to prevent duplicate incidents (this applies strictly to incident creation rules for Microsoft alerts, not custom scheduled analytics rules).
4. **The Fusion Analytics Rule is Disabled**: Sentinel's legacy **Fusion** rule (an engine that used machine learning to stitch multi-stage alerts into incidents) is automatically turned off. In the unified portal, Defender XDR's native incident correlation engine replaces Fusion entirely.

#### Key Capability Differences Between Azure and Defender Portals

While most Sentinel features function identically across both portals, specific capabilities differ:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        PORTAL CAPABILITY DIFFERENCES                   │
├──────────────────────────┬─────────────────────────────────────────────┤
│ CAPABILITY               │ PORTAL AVAILABILITY & BEHAVIOR DIFFERENCE   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Advanced Hunting         │ Bookmarks are NOT supported inside the      │
│ Bookmarks                │ Advanced Hunting window in Defender; must be│
│                          │ accessed via Sentinel > Hunting.            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Attack Disruption for SAP│ Available EXCLUSIVELY in the Defender portal│
│                          │ (unavailable in the Azure portal).          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Data Connectors          │ Native Defender connectors (Defender for    │
│ Visibility               │ Endpoint, Identity, Office 365, Cloud Apps, │
│                          │ Defender XDR, Defender for Cloud) are hidden│
│                          │ from the Sentinel Data Connectors page in   │
│                          │ Defender, as data flows natively.           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Adding Entities to TI    │ Adding entities to Threat Intelligence      │
│ From Incidents           │ directly from an incident page is supported │
│                          │ in the Azure portal ONLY.                   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Incident Alert Management│ In Defender, alerts can be removed from an   │
│                          │ incident ONLY by linking them to another    │
│                          │ incident.                                   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Incident Comments        │ Editing existing incident comments is supported│
│                          │ in Azure only; comment edits made in Azure  │
│                          │ do NOT sync back to the Defender portal.    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Programmatic Incidents   │ Incidents created via API, Logic Apps, or   │
│                          │ manually in Azure do NOT sync to Defender.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Reopening Closed         │ In Defender, analytics rules cannot reopen  │
│ Incidents                │ closed incidents when new alerts match; a   │
│                          │ new incident is triggered instead.          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Incident Tasks           │ Incident tasks are supported in Azure ONLY. │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Multi-Workspace          │ Defender supports ONE primary workspace per │
│ Management               │ tenant; Azure supports managing multiple    │
│                          │ workspaces via Workspace Manager.           │
└──────────────────────────┴─────────────────────────────────────────────┘
```

#### Offboarding a Workspace

If an organization needs to switch its connected workspace:

1. Navigate to `System > Settings > Microsoft Sentinel` in the Defender portal.
2. On the **Workspaces** page, select the connected workspace and select **Disconnect workspace**.
3. Provide a reason for disconnecting and select **Confirm**. Disconnecting removes the Microsoft Sentinel navigation section from the Defender portal sidebar.

---

## Connecting the Dots

To protect a sprawling digital enterprise, every module in Microsoft Sentinel fits together into a unified intelligence agency.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE CENTRAL SURVEILLANCE GRID                             │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Master Command Post   │   │ Workspace RBAC Keys    │   │ Data Retention Tiers    │  │
│  │ (Log Analytics / SIEM │──►│ (Sentinel Reader,      │──►│ (Analytics Hot 30d-2yr /│  │
│  │  vs Data Lake Tiers)  │   │  Responder, Contributor│   │  Data Lake Cold 12 yrs) │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Intelligence Vault    │──►│ VIP Guest Lists        │◄──│ Most Wanted Board       │  │
│  │ (SecurityAlert, CEF,  │   │ (Watchlists via        │   │ (Threat Intel IoCs &    │  │
│  │  XDR Telemetry Tables)│   │  _GetWatchlist)        │   │  STIX Table Transition) │  │
│  └───────────────────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ TEARING DOWN WALLS     │                                │
│                              │ (Unified Operations    │                                │
│                              │  Platform in Defender) │                                │
│                              └────────────────────────┘                                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Architecting the Command Center**: Security teams begin by deploying a **Log Analytics Workspace** to host **Microsoft Sentinel**, choosing between single-workspace, regional, or multi-tenant architectures managed via **Workspace Manager** and **Azure Lighthouse**. Data is divided between the fast **SIEM** operations floor and the long-term **Data Lake** platform.
2. **Permission Boundaries and Log Retention**: Access is restricted using specialized **Azure RBAC** roles (**Microsoft Sentinel Reader**, **Responder**, **Contributor**, and **Logic App Contributor**). Data retention is configured across workspace settings (30 to 730 days) and table plans (**Analytics**, **Basic**, **Auxiliary**), routing cold records to the Data Lake for up to 12 years.
3. **Querying Intelligence and Reference Lists**: Security events stream into structured tables (`SecurityAlert`, `SecurityIncident`, `CommonSecurityLog`, and raw XDR telemetry tables). Analysts enrich event data by building **Watchlists** (CSV imports up to 3.8 MB queried via `_GetWatchlist`) to track VIPs and suppress false alarms, while cross-referencing incoming traffic against **Threat Intelligence** indicators stored in `ThreatIntelligenceIndicator` (transitioning to `ThreatIntelIndicators` and `ThreatIntelObjects`).
4. **Tearing Down Portal Walls**: Finally, administrators connect Sentinel directly to the **Microsoft Defender Portal** to create the **Unified Security Operations Platform**. Connecting the workspace automatically deactivates legacy incident rules, disables the **Fusion** rule in favor of native XDR correlation, and provides analysts with a single, unified command center to hunt threats, analyze logs, and execute automated response playbooks at machine speed.

---

💡 **Next Step**: I can now generate a detailed visual slide deck outline or an interactive quiz on Module 7 to help solidify these deployment steps and KQL configurations for your SC-200 preparation!