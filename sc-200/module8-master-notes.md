# Connecting Logs to Microsoft Sentinel

## The Big Picture

Imagine you have constructed a massive, state-of-the-art security command center for a sprawling corporate campus, complete with high-definition display walls and automated triage desks. However, when you first turn on the system, every monitor screen on the wall is completely blank. A command center is useless if it is not wired directly to the physical security cameras, door sensors, badge readers, and alarms scattered across every office building, server room, and leased facility in your digital estate. Connecting data feeds to **Microsoft Sentinel** (a cloud-native security platform that aggregates, analyzes, and responds to threat data across the entire enterprise network) is the difference between blindly guessing and catching an intruder red-handed.

To illuminate the blank screens on the command wall, security engineers run digital surveillance cables across the enterprise. You visit a digital hardware store to purchase pre-configured wiring kits, flip software switches to stream native cloud telemetry, install tracking agents on everyday office desks, deploy specialized translation booths to interpret foreign firewall dialects, and plug in live threat wiretaps to track known cybercriminals in real time. By establishing these data pipelines, every event—from an executive opening an email to a firewall blocking a port scan—streams directly into structured database vaults, transforming raw, scattered logs into a unified, real-time surveillance grid.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SENTINEL DATA SURVEILLANCE PIPELINE                             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ DIGITAL HARDWARE STORE (Content Hub Solutions & CI/CD Source Control Governance)        │
├───────────────────────────┬────────────────────────────┬───────────────────────────────┤
│ NATIVE CLOUD ALARMS       │ ELITE SECURITY GUARDS      │ OFFICE DESKS & SERVERS        │
│ • Microsoft 365           │ • Microsoft Defender XDR   │ • Windows Security Events     │
│   (OfficeActivity)        │   (Bi-directional Incident │   (AMA + DCR Filtering)       │
│ • Microsoft Entra ID      │    Sync & Raw Telemetry)   │ • Azure Arc Bridges           │
│   (Signin & Audit Logs)   │ • Defender for Cloud       │   (Non-Azure Hosts)           │
│ • Azure Activity          │ • Defender for IoT         │ • Sysmon Event Logs           │
│   (ARM Management Plane)  │                            │   (WEF + ASIM Integration)    │
├───────────────────────────┴────────────────────────────┴───────────────────────────────┤
│ FOREIGN NETWORK TRANSLATION BOOTHS                     LIVE THREAT WIRETAPS            │
│ • CEF via AMA Forwarder (TCP 514 ──► 25226 ──► 443)    │ • TAXII 2.0 / 2.1 Feeds       │
│ • Syslog via AMA Forwarder (TCP 514 ──► 28330 ──► 443) │ • MDTI & Premium MDTI         │
│ • KQL Parser Functions (Unstructured String Processing) │ • STIX Upload API             │
└────────────────────────────────────────────────────────┴───────────────────────────────┘
```

---

## The Core Mechanics

### Purchasing the Wiring Kits: Content Hub Solutions and Source Control Governance

Before running digital cables or connecting remote facilities to the command center, security engineers obtain the proper integration tools and instruction manuals. In Microsoft Sentinel, engineers acquire these resources from the **Content Hub** (`Microsoft Sentinel > Content management > Content hub`), which functions as a digital hardware store for pre-packaged security integrations.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CONTENT HUB SOLUTION COMPONENTS                 │
├───────────────────┬────────────────────────────────────────────────────┤
│ COMPONENT TYPE    │ OPERATIONAL FUNCTION & PURPOSE                     │
├───────────────────┼────────────────────────────────────────────────────┤
│ Data Connectors   │ Data pipelines that ingest logs from cloud services│
│                   │ and third-party appliances into workspace tables.  │
├───────────────────┼────────────────────────────────────────────────────┤
│ Workbooks         │ Interactive visual dashboards displaying charts,   │
│                   │ graphs, and operational metrics.                   │
├───────────────────┼────────────────────────────────────────────────────┤
│ Analytic Rules    │ Automated tripwires and detection queries that     │
│                   │ generate security alerts and incidents.            │
├───────────────────┼────────────────────────────────────────────────────┤
│ Hunting Queries   │ Pre-built investigation queries used by analysts   │
│                   │ to hunt for undetected threats.                    │
├───────────────────┼────────────────────────────────────────────────────┤
│ Playbooks         │ Automated response workflows built on Azure Logic  │
│                   │ Apps to remediate threats at machine speed.        │
└───────────────────┴────────────────────────────────────────────────────┘
```

Security engineers search for solutions by provider, category, or keyword, and select **Install** (or **Install/Update**) to deploy the complete package into their workspace. Once installed, data connectors appear under `Microsoft Sentinel > Configuration > Data connectors`. Selecting a connector and opening its detail pane exposes a split-screen interface:

- **Left Pane**: Displays connector status (**Connected** vs. **Disconnected**), provider name, last log received timestamp, and **Data Types** (a list of specific database tables the connector writes to, such as `OfficeActivity` or `SigninLogs`). Connectors can be deactivated or disconnected, but they cannot be deleted from the workspace.
- **Right Pane (Instructions Tab)**: Contains mandatory **Prerequisites** (workspace read/write permissions, tenant admin roles, or Azure subscription ownership) and **Configuration** steps (toggle switches, policy assignment buttons, or deployment scripts).
- **Right Pane (Next Steps Tab)**: Provides quick links to installed workbooks, sample KQL queries, and analytic rule templates.

```
[ Content Hub ] ──► [ Search Solution ] ──► [ Install Package ] ──► [ Open Connector Page ]
                                                                             │
                                              ┌──────────────────────────────┴──────────────────────────────┐
                                              ▼                                                             ▼
                                    ┌───────────────────┐                                         ┌───────────────────┐
                                    │     LEFT PANE     │                                         │    RIGHT PANE     │
                                    ├───────────────────┤                                         ├───────────────────┤
                                    │ • Connector Status│                                         │ • Prerequisites   │
                                    │ • Last Log Time   │                                         │ • Configuration   │
                                    │ • Target Tables   │                                         │ • Next Steps      │
                                    └───────────────────┘                                         └───────────────────┘
```

Because security configurations and analytic queries change over time, enterprise security teams manage their custom Sentinel content using **Source Control Integration** (`Microsoft Sentinel > Content management > Repositories`). Security engineers connect Sentinel to external version-control repositories—such as **GitHub** or **Azure DevOps**—to store custom KQL queries, analytics templates, workbooks, and automation rules as version-controlled file assets. When an engineer updates a detection rule in the repository, a **CI/CD** (continuous integration and continuous deployment, an automated pipeline that tests and deploys configuration files) pipeline validates the syntax and publishes the updated configuration directly to the Sentinel workspace without manual portal intervention.

Now that our wiring kits are acquired and version-controlled, we can flip the switches to collect telemetry from native cloud services.

---

### Wiring Native Cloud Alarms: Microsoft 365, Microsoft Entra ID, and Azure Activity

The most straightforward surveillance feeds to connect are native Microsoft cloud services. These connectors use cloud service-to-service pipelines that are enabled with a few administrative clicks.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        NATIVE MICROSOFT SERVICE CONNECTORS             │
├─────────────────────┬──────────────────────────┬───────────────────────┤
│ DATA CONNECTOR      │ TARGET LOG TABLE(S)      │ INGESTED TELEMETRY    │
├─────────────────────┼──────────────────────────┼───────────────────────┤
│ Microsoft 365       │ OfficeActivity           │ Exchange, SharePoint, │
│                     │                          │ Teams user actions.   │
├─────────────────────┼──────────────────────────┼───────────────────────┤
│ Microsoft Entra ID  │ SigninLogs, AuditLogs,   │ User/service principal│
│                     │ AADNonInteractiveUser... │ sign-ins, SSPR, group │
│                     │ AADServicePrincipal...   │ and role management.  │
│                     │ AADManagedIdentity...    │                       │
│                     │ AADProvisioningLogs      │                       │
│                     │ ADFSSignInLogs           │                       │
├─────────────────────┼──────────────────────────┼───────────────────────┤
│ Microsoft Entra ID  │ SecurityAlert            │ Identity risk events  │
│ Protection          │                          │ & user risk scores.   │
├─────────────────────┼──────────────────────────┼───────────────────────┤
│ Azure Activity      │ AzureActivity            │ ARM management-plane  │
│                     │                          │ operations & status.  │
└─────────────────────┴──────────────────────────┴───────────────────────┘
```

#### 1. The Microsoft 365 Data Connector

Streams operational logs from Microsoft 365 productivity services into the **`OfficeActivity`** table. It tracks user activities including file downloads, access requests, group modifications, mailbox setting changes (`Set-Mailbox`), and Teams channel interactions.

To configure the Microsoft 365 connector:

1. Navigate to `Microsoft Sentinel > Content management > Content hub`.
2. Search for and select **Microsoft 365**, then select **Install**.
3. Navigate to `Microsoft Sentinel > Configuration > Data connectors`.
4. Select **Microsoft 365** and select **Open connector page**.
5. On the **Instructions** tab under _Configuration_, check the boxes for the record types to collect: **Exchange**, **SharePoint**, and **Teams**.
6. Select **Apply Changes**.

```
[ Content Hub ] ──► [ Install 'Microsoft 365' ] ──► [ Open Connector Page ] ──► [ Check Exchange/SharePoint/Teams ] ──► [ Apply Changes ]
```

#### 2. The Microsoft Entra ID Data Connector

Streams identity authentication and administrative audit events into Microsoft Sentinel. To optimize storage costs, security engineers select specific log types based on operational requirements.

Available log types and target tables include:

- **Interactive User Sign-in Logs** (`SigninLogs`): Tracks user authentications requiring password or MFA prompts.
- **Non-Interactive User Sign-in Logs** (`AADNonInteractiveUserSignInLogs`): Tracks background token refreshes executed on behalf of users.
- **Service Principal Sign-in Logs** (`AADServicePrincipalSignInLogs`): Tracks authentications executed by automated applications and service accounts.
- **Managed Identity Sign-in Logs** (`AADManagedIdentitySignInLogs`): Tracks authentications executed by Azure managed identities assigned to cloud resources.
- **Provisioning Logs** (`AADProvisioningLogs`): Tracks user provisioning sync events from HR tools into Entra ID.
- **AD FS Sign-in Logs** (`ADFSSignInLogs`): Tracks authentication attempts processed through Active Directory Federation Services.
- **Audit Logs** (`AuditLogs`): Tracks Directory administrative changes, including user creations, group modifications, role assignments, and Self-Service Password Reset (SSPR) activity.

To configure the Microsoft Entra ID connector:

1. Navigate to `Content hub > Search "Microsoft Entra ID" > Install`.
2. Open `Data connectors > Microsoft Entra ID > Open connector page`.
3. Check the boxes next to the desired log types (e.g., _Sign-in Logs_, _Audit Logs_).
4. Select **Connect**.

#### 3. The Microsoft Entra ID Protection Data Connector

Streams identity risk detections (e.g., leaked credentials, anonymous IP sign-ins, impossible travel) into the **`SecurityAlert`** table.

Configuration steps:

1. Navigate to `Content hub > Search "Microsoft Entra ID Protection" > Install`.
2. Open `Data connectors > Microsoft Entra ID Protection > Open connector page`.
3. Select **Connect** to stream risk alerts.
4. Under _Create incidents - Recommended!_, select **Enable**. Enabling this toggle automatically activates the built-in analytics rule _"Create incidents based on Microsoft Entra ID Protection alerts"_, which creates Sentinel incidents whenever high-risk identity alerts occur.

#### 4. The Azure Activity Data Connector

Collects subscription-level management-plane operations executed through ARM, including resource creation, resource deletion, administrative writes, service health events, and subscription status updates, streaming data into the **`AzureActivity`** table.

The connector uses **Azure Policy** (an Azure governance service that enforces security standards and automates resource configurations) to assign a log-streaming pipeline across subscriptions.

To configure the Azure Activity connector:

1. Verify prerequisites: The engineer configuring the policy must hold the **Owner** role on the target Azure subscription.
2. Navigate to `Content hub > Search "Azure Activity" > Install`.
3. Open `Data connectors > Azure Activity > Open connector page`.
4. On the **Instructions** tab under _2. Connect your subscriptions..._, select **Launch Azure Policy Assignment Wizard**.
5. On the **Basics** tab, select the ellipsis (**...**) under _Scope_, select the target **Subscription**, and click **Select**.
6. On the **Parameters** tab, select the primary **Log Analytics Workspace** from the dropdown menu.
7. On the **Remediation** tab, check the **Create a remediation task** box (this applies the log-streaming policy to existing subscriptions and resources).
8. Select **Review + Create**, then select **Create**.

```
[ Open Azure Activity Connector ] ──► [ Launch Policy Assignment Wizard ] ──► [ Set Scope Subscription ] ──► [ Select Workspace Parameter ] ──► [ Check 'Create Remediation Task' ] ──► [ Create ]
```

Now that native cloud alarms are connected, we can link specialized security products into our command grid.

---

### Connecting the Elite Security Guards: Microsoft Defender XDR Integration

To stream endpoint, identity, email, and cloud app threat telemetry into Sentinel, security engineers configure the **Microsoft Defender XDR Data Connector** (`Microsoft Sentinel > Configuration > Data connectors > Microsoft Defender XDR`).

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DEFENDER XDR INTEGRATED SERVICES                │
├──────────────────────────┬─────────────────────────────────────────────┤
│ DEFENDER COMPONENT       │ INGESTED TELEMETRY & ATTACK DATA            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Defender for Endpoint    │ EDR alerts, process trees, network sockets, │
│                          │ file system modifications, registry edits.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Defender for Identity    │ Domain controller alerts, Kerberos operations,│
│                          │ LDAP query events, lateral movement paths.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Defender for Office 365  │ Phishing alerts, email delivery events, URL │
│                          │ clicks, zero-hour auto-purge (ZAP) actions. │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Defender for Cloud Apps  │ Shadow IT discovery, cloud app alerts, file │
│                          │ access anomalies, OAuth app permissions.    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Defender for Cloud       │ Multicloud workload alerts & posture data.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Purview DLP / Insider    │ Data Loss Prevention policy matches and     │
│ Risk Management          │ insider risk indicators.                    │
└──────────────────────────┴─────────────────────────────────────────────┘
```

The Defender XDR connector streams high-fidelity alerts alongside raw **Advanced Hunting** telemetry tables—such as `DeviceEvents`, `DeviceFileEvents`, `DeviceProcessEvents`, `DeviceNetworkEvents`, `DeviceLogonEvents`, `EmailEvents`, `EmailUrlInfo`, `EmailAttachmentInfo`, and `IdentityLogonEvents`.

Connecting Defender XDR establishes bi-directional incident synchronization between the Microsoft Defender portal and Microsoft Sentinel. Modifications made to incident attributes in either interface sync immediately to the other:

- **Title**
- **Description**
- **ProductName**
- **Severity**
- **Custom tags**
- **AdditionalData**
- **Comments** (newly added comments sync bi-directionally)
- **LastModifiedBy**

```
┌────────────────────────────────────────────────────────────────────────┐
│                    INTEGRATION ARCHITECTURE OPTIONS                    │
├──────────────────────────┬─────────────────────────────────────────────┤
│ DEFENDER PORTAL          │ Onboarding Sentinel to security.microsoft.com│
│ INTEGRATION              │ automatically configures the Defender XDR   │
│                          │ connector and disconnects legacy alert      │
│                          │ connectors to prevent duplicates.          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ AZURE PORTAL             │ Manually enabling the Defender XDR connector│
│ INTEGRATION              │ in Sentinel within portal.azure.com to      │
│                          │ stream incidents and raw telemetry.         │
└──────────────────────────┴─────────────────────────────────────────────┘
```

When Defender XDR is connected, **Microsoft Incident Creation Rules** for underlying Defender components are automatically disabled. Defender XDR uses its own correlation engine to group alerts into incidents; leaving Sentinel incident creation rules active for these components can cause duplicate incident creation. To tune out unwanted alerts, security engineers configure alert suppression rules in the Defender portal or construct Sentinel automation rules to close matching tickets automatically.

> **Legacy Connectors Warning**: Older standalone connectors (_Microsoft Defender for Cloud Apps_, _Microsoft Defender for Endpoint_, _Microsoft Defender for Identity_, and _Microsoft Defender for Office 365_) are classified as **Legacy Connectors**. Legacy connectors only ingested basic alert headers without raw hunting telemetry, and they required manual incident creation rules that produced un-synced duplicate tickets. Security teams should replace legacy connectors with the unified **Microsoft Defender XDR Connector**.

Additional Defender service connectors are configured independently:

- **Microsoft Defender for Cloud Connector**: Open `Data connectors > Microsoft Defender for Cloud > Open connector page`, select the **Connect** toggle for the target subscription, and enable **Bi-directional sync**.
- **Microsoft Defender for IoT Connector**: Open `Data connectors > Microsoft Defender for IoT > Open connector page`, and select the **Connect** toggle for the target subscription to ingest operational technology (OT) and industrial control system (ICS) alert metrics.

Now that elite security platforms are streaming alert data, we must install software tracking agents on everyday office workstations and servers.

---

### Surveillance on Office Desks: Windows Security Events, AMA, Azure Arc, and Sysmon

To collect granular event logs from Windows virtual machines, physical servers, and on-premises workstations, Microsoft Sentinel utilizes the **Windows Security Events via AMA** connector.

```
┌────────────────────────────────────────────────────────────────────────┐
│                      WINDOWS HOST COLLECTION ARCHITECTURE              │
├────────────────────────────────────────────────────────────────────────┤
│ • Modern Method: Windows Security Events via AMA Connector              │
│   Utilizes Azure Monitor Agent + Data Collection Rules (DCRs).         │
│ • Legacy Method: Security Events via Legacy Agent Connector            │
│   Utilizes Log Analytics Agent (MMA) — DEPRECATED AUGUST 31, 2024.     │
└────────────────────────────────────────────────────────────────────────┘
```

#### 1. Onboarding Non-Azure Windows Hosts via Azure Arc

The Azure Monitor Agent requires an ARM identity to apply configurations. Virtual machines running outside Azure (on-premises physical servers, VMware hosts, or virtual machines in competing clouds) must be onboarded using **Azure Arc** before deploying AMA.

Azure Arc deploys the **Azure Connected Machine Agent** (`azcmagent`), projecting external hosts into Azure as managed resources.

To onboard a non-Azure server using Azure Arc:

1. In the Azure portal, navigate to `Azure Arc > Machines > + Add/Create > Add a single server`.
2. Select **Generate script**, choose the target **Subscription**, **Resource Group**, **Region**, and select **Linux** or **Windows** as the operating system.
3. Download or copy the generated script.
4. Run the script on the target server in an administrative terminal (PowerShell for Windows, Bash for Linux) to install `azcmagent`.
5. Execute the connection command in the terminal:

```
sudo azcmagent connect --resource-group "rg-security" --tenant-id "tenant-guid" --location "eastus" --subscription-id "sub-guid" --cloud "AzureCloud"
```

6. Open a web browser, navigate to `https://microsoft.com/devicelogin`, enter the display code provided in the terminal output, and authenticate with an administrator account.
7. Verify that the server status changes to **Connected** in the Azure Arc portal.

```
[ Azure Arc > Generate Script ] ──► [ Execute Script on Server ] ──► [ Run 'azcmagent connect' ] ──► [ Authenticate via devicelogin ] ──► [ Machine Connected ]
```

#### 2. Configuring Data Collection Rules (DCRs) for Windows Hosts

Data Collection Rules (DCRs) are independent Azure Policy objects that define precisely which event logs **AMA** collects from host endpoints. DCRs enable source-side event filtering, reducing Log Analytics ingestion volume.

To create a DCR for Windows Security Events:

1. Navigate to `Microsoft Sentinel > Configuration > Data connectors`.
2. Select **Windows Security Events via AMA** and select **Open connector page**.
3. Under _Configuration_, select **+ Add data collection rule**.
4. On the **Basics** tab, enter a **Rule Name**, specify the **Subscription** and **Resource Group** where the DCR object will be stored.
5. On the **Resources** tab, select **+ Add resource(s)**, expand the subscription hierarchy, and check the boxes next to target Azure virtual machines and Azure Arc-enabled servers. Selecting a host automatically installs the AMA extension if it is not already present.
6. On the **Collect** tab, select the event collection level:
    - **All Security Events**: Ingests all Windows security events and AppLocker audit logs.
    - **Common**: Ingests a standard audit trail set, including successful sign-ins (`4624`), sign-outs (`4634`), security group changes, Kerberos operations, and domain controller activity.
    - **Minimal**: Ingests a small set of high-fidelity breach indicators, including successful sign-ins (`4624`), failed sign-ins (`4625`), and process creations (`4688`), while excluding sign-out events (`4634`) to reduce log volume.
    - **Custom**: Allows security engineers to define custom **XPath 1.0** filtering expressions to select specific event IDs or log channels.
7. Select **Review + Create**, then **Create**.

```
[ Open Windows Security Events Connector ] ──► [ + Add Data Collection Rule ] ──► [ Select Target VMs & Arc Hosts ] ──► [ Select Event Level (All/Common/Minimal/Custom) ] ──► [ Create ]
```

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DCR EVENT COLLECTION LEVELS                     │
├───────────────┬────────────────────────────────────────────────────────┤
│ LEVEL         │ INGESTION SCOPE & TYPICAL EVENT IDs                    │
├───────────────┼────────────────────────────────────────────────────────┤
│ All Events    │ Complete audit record (Security + AppLocker logs).     │
├───────────────┼────────────────────────────────────────────────────────┤
│ Common        │ Full audit trail including sign-ins (4624), sign-outs │
│               │ (4634), group modifications, and Kerberos operations.  │
├───────────────┼────────────────────────────────────────────────────────┤
│ Minimal       │ Low-volume breach set including sign-ins (4624), failed│
│               │ sign-ins (4625), and process creations (4688).         │
│               │ Excludes high-volume sign-outs (4634).                 │
├───────────────┼────────────────────────────────────────────────────────┤
│ Custom        │ Custom XPath 1.0 query expressions (up to 20 per box, │
│               │ up to 100 boxes per DCR).                              │
└───────────────┴────────────────────────────────────────────────────────┘
```

Security engineers validate custom XPath expressions locally using the PowerShell `Get-WinEvent` cmdlet before entering them into a DCR:

```
# Validate an XPath query expression locally on a Windows host
$XPath = '*[System[EventID=1035]]'
Get-WinEvent -LogName 'Application' -FilterXPath $XPath
```

#### 3. Collecting Sysmon Event Logs

**Sysmon** (System Monitor, a Windows system service and device driver that logs detailed process creations, network connections, and file creation time changes) provides forensic visibility into host activity. Sysmon logs are ingested into Sentinel using the **Windows Forwarded Events** solution via AMA.

Configuration workflow:

1. Install Sysmon on host endpoints and configure a Windows Event Forwarding (WEF) collector machine.
2. In Sentinel, navigate to `Content hub > Search "Windows Forwarded Events" > Install`.
3. Open `Data connectors > Windows Forwarded Events > Open connector page`.
4. Select **+ Create data collection rule**.
5. Define DCR Basics, select target collector hosts on the **Resources** tab, and navigate to the **Collect** tab.
6. Select **Custom** and enter the Sysmon XPath log channel:

```
Microsoft-Windows-Sysmon/Operational!*
```

7. Select **Add**, then select **Review + Create > Create**.
8. To format Sysmon events into standardized schemas, select **Deploy** under _ASIM normalization support_ on the connector page to run an ARM deployment template that installs ASIM unifying parsers.

With Windows endpoints monitored, we must deploy translation infrastructure to ingest security logs from foreign networking gear and Linux firewalls.

---

### Translating Foreign Network Cameras: CEF via AMA Architecture and Deployment

Many third-party network firewalls (e.g., Palo Alto, Check Point, Fortinet, Barracuda) transmit security events using **CEF** (Common Event Format, an industry-standard text format built on top of Syslog that structures log attributes into key-value pairs). To ingest CEF logs, security engineers deploy a dedicated **Linux Log Forwarder** running the Azure Monitor Agent.

```
[ Third-Party Network Appliance ]
               │
               ▼  (CEF over Syslog: TCP/UDP Port 514)
┌────────────────────────────────────────────────────────┐
│ LINUX LOG FORWARDER (Azure VM, Arc, or On-Premises)    │
│                                                        │
│  ┌──────────────────────────────────────────────────┐  │
│  │ Local Syslog Daemon (rsyslog / syslog-ng)        │  │
│  └─────────────────────────┬────────────────────────┘  │
│                            │ (Unix Domain Socket)      │
│                            ▼                           │
│  ┌──────────────────────────────────────────────────┐  │
│  │ Azure Monitor Agent for Linux (AMA)              │  │
│  │ (Listens internally on TCP 28330 / 25226)        │  │
│  └─────────────────────────┬────────────────────────┘  │
└────────────────────────────┼───────────────────────────┘
                             │
                             ▼  (Encrypted HTTPS: TCP Port 443)
[ Microsoft Sentinel Workspace ] ──► (CommonSecurityLog Table)
```

#### 1. Forwarder System Requirements

The dedicated Linux forwarder can be deployed as an Azure virtual machine, an on-premises VM, or a host in another cloud platform.

The system must satisfy these specifications:

- **Operating System**: 64-bit architecture running Amazon Linux 2/2023, Oracle Linux 8/9, RHEL 8/9, Debian 10/11/12, Ubuntu 20.04/22.04/24.04 LTS, or SLES 15.
- **Syslog Daemon**: `rsyslog` (v8) or `syslog-ng` (v2.1 - v3.22.1).
- **Protocols Supported**: Syslog RFC 3164 and RFC 5424.
- **Permissions**: Administrative `sudo` privileges on the Linux host.

#### 2. Deploying the CEF via AMA Data Connector

1. Navigate to `Microsoft Sentinel > Configuration > Data connectors`.
2. Search for and select **Common Event Format (CEF) via AMA**, then select **Open connector page**.
3. Under _Configuration_, select **+ Create data collection rule**.
4. On the **Basics** tab, enter a DCR Name, Subscription, and Resource Group.
5. On the **Resources** tab, select the designated Linux forwarder virtual machine.
6. On the **Collect** tab, verify the CEF log facilities and minimum severity thresholds.
7. Select **Review + Create**, then **Create**. This action automatically installs the **Azure Monitor Linux Agent extension** (`AzureMonitorLinuxAgent`) on the selected forwarder.
8. Configure third-party network appliances to forward CEF Syslog streams to the Linux forwarder's IP address over **UDP or TCP port 514**.

CEF messages received by the forwarder are parsed into structured columns and written directly into the **`CommonSecurityLog`** table in Log Analytics.

```
[ Open CEF via AMA Connector ] ──► [ + Create Data Collection Rule ] ──► [ Select Linux Forwarder VM ] ──► [ Confirm Facilities & Levels ] ──► [ Create ] ──► [ Point Appliances to Port 514 ]
```

> **Log De-duplication Rule**: If a single Linux forwarder is configured to process both plain Syslog and CEF messages, edit the Syslog configuration file (`/etc/rsyslog.conf` or `/etc/syslog-ng/syslog-ng.conf`) on sending source machines to remove facilities assigned to CEF traffic. This prevents duplicate log ingestion into both the `Syslog` and `CommonSecurityLog` tables.

Now that structured CEF translation is configured, we must handle raw, unstructured Syslog streams from Linux operating systems.

---

### Parsing Raw Linux Surveillance Feeds: Syslog via AMA and KQL Infrastructure Parsers

Standard Linux operating systems and legacy appliances transmit unstructured event messages using raw **Syslog**.

```
[ Linux Host / Appliance ]
           │
           ▼  (Raw Syslog: TCP/UDP Port 514)
[ Local Syslog Daemon (rsyslog/syslog-ng) ]
           │
           ▼  (Unix Domain Socket)
[ Azure Monitor Agent for Linux (AMA) ]
           │
           ▼  (Encrypted HTTPS: TCP Port 443)
[ Log Analytics Workspace: Syslog Table ]
```

#### 1. Configuring the Syslog via AMA Data Connector

Syslog messages are ingested using an Azure Monitor Agent Syslog DCR:

- **For Azure Linux Virtual Machines**:
    
    1. Navigate to `Azure Monitor > Settings > Data Collection Rules > + Create`.
    2. Set Platform Type to **Linux**, enter DCR Basics, and navigate to the **Resources** tab.
    3. Select **+ Add resources**, select the target Azure Linux VM, and click **Apply**.
    4. On the **Collect and deliver** tab, select **+ Add data source**, select **Linux Syslog** from the dropdown menu, select facility severity levels, and select **Add data source**.
    5. Select **Review + Create**, then **Create** (this initiates the installation of the `AzureMonitorLinuxAgent` extension on the VM).
- **For Non-Azure Linux Machines**:
    
    1. Onboard the Linux machine to **Azure Arc** using `azcmagent connect`.
    2. Navigate to the existing Syslog DCR in the Azure portal.
    3. Under _Configuration_, select **Resources > + Add**, select the Arc-enabled Linux server, and click **Apply**.

DCR settings allow configuring minimum log levels (`LOG_DEBUG`, `LOG_INFO`, `LOG_WARNING`, `LOG_ERR`, etc.) per Syslog facility (`LOG_AUTH`, `LOG_CRON`, `LOG_DAEMON`, `LOG_KERN`, `LOG_USER`, `LOG_LOCAL0-7`).

#### 2. Authoring KQL Data Parsers for Unstructured Syslog Messages

Unlike CEF data, raw Syslog messages write to the **`Syslog`** table with log contents stored as an unparsed text block in the **`SyslogMessage`** column.

To convert this unparsed text into structured data fields without modifying underlying source logs, security engineers construct reusable **KQL Functions** (query expressions saved as database function aliases that act as virtual table parsers).

```
[ Raw Unstructured Text in SyslogMessage Column ]
                       │
                       ▼
   [ Apply Regex extract() Expressions in KQL ]
                       │
                       ▼
   [ Save KQL Query as a Workspace Function ]
                       │
                       ▼
[ Query Function Alias (MyParser) Like a Native Table ]
```

An engineer builds a KQL parser query using string manipulation and regex extraction functions:

```
// KQL Parser Solution: Extract structured web proxy fields from raw SyslogMessage strings
Syslog
| where ProcessName contains "squid"
| extend URL = extract("(([A-Z]+ [a-z]{4,5}:\\/\\/)|[A-Z]+ )([^ :]*)", 3, SyslogMessage),
         SourceIP = extract("(+ )(({1,3})\\.({1,3})\\.({1,3})\\.({1,3}))", 2, SyslogMessage),
         Status = extract("(TCP_(([A-Z]+)(_[A-Z]+)*)|UDP_(([A-Z]+)(_[A-Z]+)*))", 1, SyslogMessage),
         HTTP_Status_Code = extract("(TCP_(([A-Z]+)(_[A-Z]+)*)|UDP_(([A-Z]+)(_[A-Z]+)*))/({3})", 8, SyslogMessage),
         User = extract("(CONNECT |GET )([^ ]* )([^ ]+)", 3, SyslogMessage),
         RemotePort = extract("(CONNECT |GET )([^ ]*)(:)(*)", 4, SyslogMessage),
         Domain = extract("(([A-Z]+ [a-z]{4,5}:\\/\\/)|[A-Z]+ )([^ :\\/]*)", 3, SyslogMessage)
| extend TLD = extract("\\.[a-z]*$", 0, Domain)
```

To save this parsing query as a reusable function in the portal:

1. In the Sentinel **Logs** window, enter the KQL parsing query.
2. Select **Save** from the top menu and select **As function**.
3. In the side panel, enter a **Function Name** (e.g., `SquidProxyParser`) and a **Legacy Schema Alias** (e.g., `MyParser`).
4. Select **Save**. Analysts can now query `MyParser` directly in hunting queries and analytic rules as if it were a native, pre-parsed database table.

Now that host and network telemetry are streaming into our command center, we must wire up threat intelligence feeds to track active cybercriminals.

---

### Tapping into Live Threat Feeds: TAXII, MDTI, and the Threat Intelligence Upload API

To cross-reference incoming security logs against active global attack infrastructure, Microsoft Sentinel ingests **IoCs** (indicators of compromise, technical indicators like malicious IP addresses, phishing domain names, URLs, and file hashes) using specialized threat intelligence connectors.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   THREAT INTELLIGENCE DATA CONNECTORS                  │
├──────────────────────────┬─────────────────────────────────────────────┤
│ CONNECTOR TYPE           │ INGESTION MECHANISM & SOURCE TYPE           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Microsoft Defender Threat│ High-fidelity public and open-source IOCs   │
│ Intelligence (MDTI)      │ generated natively by Microsoft.            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Premium Defender Threat  │ Advanced commercial threat intelligence feeds│
│ Intelligence             │ requiring the MDTI API Access SKU.          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Threat Intelligence -    │ Automated pull connector connecting to      │
│ TAXII                    │ external TAXII 2.0 / 2.1 servers.           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Threat Intelligence      │ Legacy Graph API integration connecting     │
│ Platforms (TIP)          │ third-party Threat Intelligence Platforms.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Threat Intelligence      │ Direct REST API ingestion pipeline for      │
│ Upload API (Preview)     │ pushing STIX objects without a local agent. │
└──────────────────────────┴─────────────────────────────────────────────┘
```

#### 1. The Defender Threat Intelligence (MDTI) Data Connector

Ingests threat indicators compiled by Microsoft security researchers.

1. Open `Content hub > Search "Threat Intelligence" > Install`.
2. Open `Data connectors > Defender Threat Intelligence > Open connector page`.
3. Select **Connect** to start streaming indicators into the workspace.

#### 2. The Threat Intelligence - TAXII Data Connector

Pulls threat indicators automatically from external **TAXII** (Trusted Automated eXchange of Indicator Information, an automated application protocol used to exchange cyber threat intelligence in STIX format) servers.

To configure the TAXII connector:

1. Navigate to `Microsoft Sentinel > Configuration > Data connectors`.
2. Select **Threat intelligence - TAXII** and select **Open connector page**.
3. On the **Instructions** tab, enter connection details:
    - **Friendly name (for server)**: A descriptive label for the TAXII server.
    - **API root URL**: The base HTTP endpoint URL of the TAXII service.
    - **Collection ID**: The target threat indicator collection identifier.
    - **Username** and **Password**: Authentication credentials for the TAXII server.
    - **Polling frequency**: How often Sentinel checks for new indicators (e.g., _Once an hour_).
4. Select **Add**. Connected TAXII servers appear in the server list, where engineers can view the last indicator received timestamp or remove configurations.

```
[ Data Connectors > Threat Intelligence - TAXII ] ──► [ Open Connector Page ] ──► [ Enter API Root URL, Collection ID, Credentials ] ──► [ Set Polling Frequency ] ──► [ Add ]
```

#### 3. The Threat Intelligence Upload API (Preview)

Ingests custom threat intelligence formatted as **STIX** (Structured Threat Information eXpression, a standardized language for describing cyber threat data) objects directly into Sentinel over a REST API endpoint without deploying connector software.

Configuring the Upload API requires setting up identity permissions in Microsoft Entra ID:

```
[ Entra ID App Registration ] ──► [ Generate Client Secret ] ──► [ Grant 'Sentinel Contributor' Role on Workspace ] ──► [ Configure TIP / API Payload ]
```

1. **Register Entra ID Application**: In the Azure portal, navigate to `Microsoft Entra ID > App registrations > + New registration`. Enter a name, select single-tenant scope, and click **Register**. Copy the **Application (client) ID**. (Requires _Application Administrator_, _Application Developer_, or _Cloud Application Administrator_ permissions if non-admin registration is disabled).
2. **Generate Client Secret**: Under `Certificates & secrets`, select **+ New client secret**, enter a description, set an expiration window, and click **Add**. Copy the secret **Value** immediately.
3. **Assign Workspace Permissions**: Navigate to the target **Log Analytics Workspace** in the Azure portal, select **Access control (IAM)**, select **+ Add > Add role assignment**, choose the **Microsoft Sentinel Contributor** role, and select **User, group, or service principal** under _Assign access to_. Search for the Entra ID application name, select it, and click **Review + Assign**.
4. **Submit Telemetry**: Configure the third-party Threat Intelligence Platform (TIP) or custom API script with the Application Client ID, Client Secret, Tenant ID, OAuth 2.0 access token endpoint, and Sentinel Workspace ID to push STIX objects into the workspace.

```
// Query imported threat indicators using KQL
ThreatIntelligenceIndicator
| where TimeGenerated > ago(7d)
| where Active == true
```

#### 4. The STIX Table Schema Transition Timeline

Threat indicators stream into structured workspace tables for correlation against event logs.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   THREAT INTEL TABLE TRANSITION TIMELINE               │
├────────────────────────────────────────────────────────────────────────┤
│ • April 3, 2025: Public preview of ThreatIntelIndicators &             │
│   ThreatIntelObjects STIX-compliant tables. Data streams to both old   │
│   and new tables simultaneously during the preview window.             │
├────────────────────────────────────────────────────────────────────────┤
│ • July 31, 2025: Hard deprecation date. Data ingestion into the legacy │
│   ThreatIntelligenceIndicator table stops completely. All custom KQL   │
│   queries, analytics rules, workbooks, and automations MUST be updated │
│   to reference ThreatIntelIndicators & ThreatIntelObjects before this  │
│   date.                                                                │
└────────────────────────────────────────────────────────────────────────┘
```

---

## Connecting the Dots

To eliminate blind spots across an enterprise footprint, every ingestion channel in Microsoft Sentinel fits together into a unified surveillance system.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE UNIFIED INGESTION GRID                                │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Digital Hardware Store│   │ Native Service Switches│   │ Elite Guard Pipeline    │  │
│  │ (Content Hub Packages │──►│ (OfficeActivity,       │──►│ (Microsoft Defender XDR │  │
│  │  & CI/CD Source Repos)│   │  SigninLogs, AzureAct) │   │  Bi-directional Sync)   │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Endpoint Surveillance │──►│ Translation Booths     │◄──│ Live Threat Feeds       │  │
│  │ (AMA, Azure Arc, DCRs,│   │ (Linux CEF/Syslog      │   │ (TAXII, MDTI, Upload    │  │
│  │  Sysmon & WEF)        │   │  Forwarders & Parsers) │   │  API & STIX Transition) │  │
│  └───────────────────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ UNIFIED OPERATIONS     │                                │
│                              │ (Real-Time Threat      │                                │
│                              │  Detection Grid)       │                                │
│                              └────────────────────────┘                                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Wiring Kits and Infrastructure Governance**: Security engineers begin by acquiring pre-packaged integrations from the **Content Hub**, deploying bundled data connectors, workbooks, analytic rules, and playbooks. Custom KQL queries, detection logic, and automation templates are managed using **Source Control Repositories** (GitHub or Azure DevOps) to automate configuration deployments via CI/CD pipelines.
2. **Native Cloud and XDR Telemetry Integration**: Engineers flip switches to ingest native cloud telemetry—streaming user activity into `OfficeActivity`, identity logs into `SigninLogs` and `AuditLogs`, subscription operational data into `AzureActivity` via Azure Policy, and risk alerts into `SecurityAlert`. The **Microsoft Defender XDR Connector** links endpoint, identity, email, and cloud app guards, enabling bi-directional incident synchronization while streaming raw **Advanced Hunting** telemetry.
3. **Endpoint Tracking and Multi-Cloud Bridges**: Desk surveillance is established using **Windows Security Events via AMA**, utilizing **Data Collection Rules (DCRs)** to scope host groups and filter event levels (_All_, _Common_, _Minimal_, _Custom XPath_). Non-Azure and on-premises hosts are bridged into Azure ARM using **Azure Arc** (`azcmagent`). **Sysmon** host events are captured using **Windows Forwarded Events** via AMA and normalized using **ASIM** ARM templates.
4. **Foreign Network Translation and Infrastructure Parsing**: Foreign network appliances stream CEF logs to a dedicated Linux log forwarder over Syslog port 514. The Syslog daemon passes traffic via a Unix domain socket to **AMA for Linux**, which encrypts and transmits CEF data over HTTPS port 443 to the `CommonSecurityLog` table. Unstructured raw Syslog messages written to `SyslogMessage` are parsed using KQL expressions (`extract()`) and saved as reusable **KQL Functions** (`MyParser`) to serve as infrastructure data parsers.
5. **Live Threat Intelligence Integration**: Finally, the grid is enriched with threat intelligence by connecting **MDTI**, polling external **TAXII 2.0 / 2.1** servers, or submitting custom STIX objects directly through the **Threat Intelligence Upload API** using a Entra ID application assigned the `Microsoft Sentinel Contributor` role. All IoC streams populate threat tables, transitioning from `ThreatIntelligenceIndicator` to STIX-compliant **`ThreatIntelIndicators`** and **`ThreatIntelObjects`** ahead of the **July 31, 2025** deprecation date, delivering complete visibility across the enterprise.