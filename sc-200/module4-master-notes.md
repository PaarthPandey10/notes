  
# Mitigating Threats with Microsoft Defender for Endpoint

## The Big Picture

While central control rooms and camera feeds monitor overall campus movement, protecting an enterprise digital estate ultimately comes down to securing individual employee workstations, laptops, mobile phones, and cloud servers. In our campus analogy, **Microsoft Defender for Endpoint** (an endpoint detection and response platform that helps security teams prevent, detect, investigate, and respond to advanced threats across enterprise devices) acts as a **specialized security squad stationed directly at every desk and door of the digital campus to stop intruders at the device level**. Rather than waiting for a breach to spread across the entire network, this frontline squad locks down compromised endpoints, blocks suspicious software behaviors in milliseconds, conducts deep forensic crime-scene investigations, and automatically repairs damaged operating system files before an attacker can steal sensitive corporate data.

  

## The Core Mechanics

### Campus Desk Registration: Initial Setup, Device Discovery, and Onboarding

Before our specialized security squad can guard an office desk, every computer and mobile device must be badged and registered with the central security service. Setting up **MDE** (Microsoft Defender for Endpoint, an endpoint detection and response platform that protects user devices) begins in the **Microsoft Defender Portal** (`[https://security.microsoft.com/](https://security.microsoft.com/) > Settings > Endpoints`).

  

During initial tenant configuration, an administrator holding the **Security Administrator** role defines core global preferences:

  

- **Data Storage Location**: Specifies the geographic region hosting customer security telemetry (**United States**, **European Union**, or **United Kingdom**). This setting is permanent and cannot be changed after initial setup.
    
      
    
- **Data Retention**: Defines how long raw security logs remain searchable in the cloud. The default retention window is **6 months** (180 days).
    
      
    
- **Enable Preview Features**: Toggles early access to upcoming product capabilities and dashboard features.
    
      
    

Endpoints communicate with cloud analytics servers using embedded OS behavioral sensors. These sensors run under the **LocalSystem Account** (a high-privilege Windows system account used by operating system services) and transmit data using **WinHTTP** (Windows HTTP Services, a software interface that enables applications to communicate over HTTP/HTTPS protocols). Because WinHTTP operates independently of browser proxy settings, the sensor discovers proxy servers using auto-discovery methods including **Transparent Proxy** (a network setup where web traffic is redirected through a proxy without client configuration) or **WPAD** (web proxy autodiscovery protocol, an automated network protocol used by computers to locate proxy configuration files).

  

To locate unmanaged equipment connected to corporate Wi-Fi or local subnets, administrators configure **Device Discovery** (`Settings > Endpoints > Device discovery` or `Settings > Device discovery`). Device discovery operates in two modes:

  

- **Basic Discovery**: Endpoints passively listen to local network traffic using the `SenseNDR.exe` binary, extracting basic device metadata without generating extra network traffic.
    
      
    
- **Standard Discovery (Recommended)**: Endpoints actively probe local networks using multicast queries and smart probing protocols. Standard discovery enriches device details, identifies unmanaged workstations, servers, printers, and IoT devices, and can be configured to detect specific library vulnerabilities such as **Log4j2** (`CVE-2021-44228`, a critical remote code execution vulnerability in a popular Java logging framework).
    
      
    

Discovered endpoints appear in the **Device Inventory** under four distinct **Onboarding Status** categories: **Onboarded** (monitored by MDE), **Can be onboarded** (supported OS discovered but agent not enrolled), **Unsupported** (discovered device running an incompatible OS), or **Insufficient info** (requires additional standard discovery probing to determine OS compatibility).

  

MDE supports onboarding across major enterprise operating systems:

  

- **Windows**: Supports Windows 7 SP1 (requires **ESU** [extended security updates, paid security patches for legacy operating systems]), Windows 8.1, Windows 10, Windows 11, Windows Server 2008 R2 SP1 (requires ESU), Windows Server 2012 R2, 2016, 201803+, 2019, 2022, 2025, and **AVD** (Azure Virtual Desktop, a cloud-based desktop virtualization service).
    
      
    
- **macOS**: Delivers antivirus, **EDR** (endpoint detection and response, tools that continuously monitor device behaviors to catch advanced attacks), and vulnerability management for the three latest released versions of macOS, deployed via **Intune** (Microsoft Intune, a cloud-based endpoint management service) or **Jamf** (a third-party management platform for Apple devices), and updated via **MAU** (Microsoft AutoUpdate, software that updates Microsoft applications on macOS).
    
      
    
- **Linux**: Protects six major Linux server distributions (**RHEL 7.2+**, **CentOS 7.2+**, **Ubuntu 16 LTS+**, **SLES 12+**, **Debian 9+**, and **Oracle Linux 7.2+**) using command-line controls deployed through **Puppet** or **Ansible** (popular open-source software tools for automated IT configuration management).
    
      
    
- **Android**: Secures Android 6.0+ devices across Work Profile and Device Administrator modes, providing web protection, anti-phishing, **PUA** (potentially unwanted application, software that degrades system performance or displays unsolicited ads) scanning, and Conditional Access integration.
    
      
    
- **iOS**: Secures iOS 11.0+ supervised and unsupervised devices, delivering web protection, anti-phishing, custom network indicators, and jailbreak detection.
    
      
    

Administrators deploy onboarding packages to Windows endpoints (`Settings > Endpoints > Device management > Onboarding`) using five deployment options:

  

1. **Local Script**: Designed for testing up to **10 devices** using a local batch package.
    
      
    
2. **Group Policy**: Deploys onboarding scripts across domain-joined devices using **GPO** (Group Policy Object, a collection of settings that control computer configurations in Active Directory).
    
      
    
3. **Microsoft Configuration Manager**: Deploys settings across enterprise fleets managed by Configuration Manager current branch or legacy versions.
    
      
    
4. **Mobile Device Management (Intune)**: Enrolls cloud-managed endpoints automatically via Intune device configuration profiles.
    
      
    
5. **VDI Onboarding Script**: Configures non-persistent **VDI** (virtual desktop infrastructure, virtualized desktop environments hosted on central servers) virtual machine pools.
    
      
    

Deployment is verified by running a detection test script in Command Prompt (`powershell.exe -NoExit -ExecutionPolicy Bypass ...`), which triggers a synthetic test alert on the device page within minutes. If a machine is decommissioned, administrators download an offboarding package (`Settings > Endpoints > Device management > Offboarding`); offboarding packages automatically expire **30 days** after generation to prevent unauthorized agent removal.

  

Plaintext

```
[ Unmanaged Devices on Corporate Subnet ]
                    │
                    ▼
┌────────────────────────────────────────────────────────┐
│             DEVICE DISCOVERY CONFIGURATION             │
├──────────────────────────┬─────────────────────────────┤
│ Basic Discovery          │ Standard Discovery (Rec.)   │
│ (Passive SenseNDR.exe)   │ (Active Multicast Probing)  │
└───────────┬──────────────┴──────────────┬──────────────┘
            │                             │
            └──────────────┬──────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│                EVALUATE ONBOARDING STATUS              │
├───────────────┬──────────────────┬───────────┬─────────┤
│ Onboarded     │ Can be onboarded │Unsupported│No Info  │
└───────────────┴─────────┬────────┴───────────┴─────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│             DEPLOY ONBOARDING PACKAGE METHOD           │
├───────────────┬──────────────┬─────────────────────────┤
│ Local Script  │ Group Policy │ Intune / Config Manager │
│ (<= 10 Hosts) │ (Active Dir.)│ (Enterprise Cloud Fleet)│
└───────────────┴──────────────┴─────────────────────────┘
```

Plaintext

```
1. CAMPUS DESK REGISTRATION: SETUP, DISCOVERY & ONBOARDING
├── Global Tenant Preferences (Configured by Security Administrator)
│   ├── Data Storage Location (United States, European Union, or United Kingdom)
│   ├── Data Retention (Default 6 months / 180 days)
│   └── Enable Preview Features
├── OS Sensor & Telemetry Communication
│   ├── LocalSystem Account (a high-privilege Windows system account used by OS services)
│   └── WinHTTP (Windows HTTP Services, software interface enabling HTTP/HTTPS communication)
│       ├── Transparent Proxy (network setup redirecting web traffic without client configuration)
│       └── WPAD (web proxy autodiscovery protocol, automated protocol to locate proxy files)
├── Device Discovery
│   ├── Basic Discovery (Passively listens using SenseNDR.exe binary)
│   └── Standard Discovery (Recommended; actively probes via multicast/smart probing protocols)
│       └── Log4j2 (CVE-2021-44228, a critical remote code execution vulnerability detected by Standard Discovery)
├── Onboarding Status Categories
│   ├── Onboarded (Monitored by MDE)
│   ├── Can be onboarded (Supported OS discovered but agent not enrolled)
│   ├── Unsupported (Discovered device running an incompatible OS)
│   └── Insufficient info (Requires additional standard discovery probing)
├── Supported Platforms & Technologies
│   ├── Windows (Requires ESU [extended security updates] for legacy; supports AVD [Azure Virtual Desktop])
│   ├── macOS (Delivers EDR [endpoint detection and response]; deployed via Intune or Jamf; updated via MAU [Microsoft AutoUpdate])
│   ├── Linux (RHEL, CentOS, Ubuntu, SLES, Debian, Oracle Linux deployed via Puppet or Ansible)
│   ├── Android (Secures devices providing PUA [potentially unwanted application] scanning)
│   └── iOS
└── Windows Deployment Methods
    ├── Local Script (Testing up to 10 devices)
    ├── Group Policy (Deploys scripts via GPO [Group Policy Object])
    ├── Microsoft Configuration Manager
    ├── Mobile Device Management (Intune)
    ├── VDI Onboarding Script (For non-persistent VDI [virtual desktop infrastructure])
    └── Offboarding Package (Automatically expires 30 days after generation)
```

### Badging the Squad: Unified Access Control and Device Grouping

To manage who can inspect specific desk stations and execute containment commands, MDE implements strict access control frameworks. Starting February 16, 2025, new MDE tenants default exclusively to **URBAC** (unified role-based access control, a single centralized permission framework across Defender tools), while existing tenants preserve legacy roles or map them into unified permissions.

  

Under legacy MDE RBAC, global Entra ID roles grant default portal access:

  

- **Global Administrator** and **Security Administrator**: Receive full unrestricted access to all devices and management settings regardless of device group scopes.
    
      
    
- **Security Reader**: Receives read-only access across alerts, devices, and vulnerability dashboards.
    
      
    

Custom MDE roles are configured by navigating to `Settings > Endpoints > Permissions > Roles > Turn on roles > + Add item`. Administrators assign granular permissions across functional areas:

  

- **View Data**: Grants read access to security operations data or vulnerability management dashboards.
    
      
    
- **Active Remediation Actions**: Permits analysts to approve or dismiss pending AIR actions, manage allowed/blocked indicator lists, handle vulnerability exceptions, or block vulnerable applications.
    
      
    
- **Alerts Investigation**: Allows managing alert statuses, running antivirus scans, collecting investigation packages, managing device tags, and downloading **PE** (portable executable, standard Windows executable file formats like EXE and DLL) files.
    
      
    
- **Manage Security Settings in Security Center**: Grants authority to configure alert suppression rules, automation folder exclusions, email notifications, and onboarding settings.
    
      
    
- **Manage Endpoint Security Settings in Microsoft Intune**: Grants full access to Intune Endpoint Security policies and compliance configurations.
    
      
    
- **Live Response Capabilities**: Divided into **Basic Commands** (starting a remote shell and running read-only inspection commands) and **Advanced Commands** (downloading PE/non-PE files, uploading files to the library, and running remote PowerShell scripts).
    
      
    

To restrict analyst visibility to specific computers, administrators configure **Device Groups** (`Settings > Endpoints > Permissions > Device groups > + Add device group`). Device groups serve three critical operational functions:

  

1. Restricting alert and device visibility to specific **Entra ID** (Microsoft Entra ID, the cloud-based identity and access management service) user groups assigned to matching RBAC roles.
    
      
    
2. Assigning distinct automated remediation levels during automated investigations.
    
      
    
3. Filtering device lists and incident graphs during security investigations.
    
      
    

Creating a device group involves defining a group name, setting an **Automation Level** (e.g., Full automation vs. Semi-automation), setting dynamic **Matching Rules** (evaluating device name prefixes, domain names, custom device tags, or OS platforms), assigning user access rights to specific Entra security groups, and defining a numeric **Rank**. If an endpoint matches criteria across multiple device groups, it is assigned exclusively to the **highest-ranked device group**.

  

Once permissions and device groups are configured, administrators enable global feature switches by navigating to **Advanced Features** (`Settings > Endpoints > Advanced features`):

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ADVANCED FEATURES TOGGLE MATRIX                    │
├──────────────────────────┬─────────────────────────────────────────────┤
│ ADVANCED FEATURE TOGGLE  │ OPERATIONAL PURPOSE & FUNCTION              │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Automated Investigation  │ Enables AIR playbooks to analyze alerts.    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Autoresolve Remediated   │ Automatically closes alerts when AIR finds  │
│ Alerts                   │ "No threats found" or "Remediated".         │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Live Response & Live     │ Enables remote interactive command shell    │
│ Response for Servers     │ sessions on endpoints and server OS hosts.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Live Response Unsigned   │ Permits executing unsigned PowerShell       │
│ Script Execution         │ scripts from the tenant library.            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Always Remediate PUA     │ Automatically cleans potentially unwanted   │
│                          │ software tenant-wide regardless of local AV.│
├──────────────────────────┼─────────────────────────────────────────────┤
│ Enable EDR in Block Mode │ Blocks post-breach malicious artifacts even │
│                          │ when third-party AV is primary active.      │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Custom Network           │ Enforces allow/block rules on custom IP,    │
│ Indicators             │ URL, and domain lists via Network Protection│
├──────────────────────────┼─────────────────────────────────────────────┤
│ Tamper Protection        │ Locks Defender AV settings against local    │
│                          │ registry, script, or malware modification.  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Share Endpoint Alerts    │ Forwards endpoint alerts to Purview for     │
│ with Purview             │ Insider Risk Management policy scoring.     │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Microsoft Intune         │ Shares device risk scores with Intune for   │
│ Connection               │ device risk-based Conditional Access.       │
└──────────────────────────┴─────────────────────────────────────────────┘
```

Plaintext

```
2. BADGING THE SQUAD: ACCESS CONTROL & DEVICE GROUPING
├── Unified Access Control 
│   ├── URBAC (unified role-based access control, a single centralized permission framework across Defender tools)
│   └── Legacy MDE RBAC (Global Administrator, Security Administrator, Security Reader)
├── Custom MDE Roles
│   ├── View Data (Read access to security/vulnerability data)
│   ├── Active Remediation Actions (Approve/dismiss AIR, manage block lists)
│   ├── Alerts Investigation (Manage alerts, run AV, download PE [portable executable] files)
│   ├── Manage Security Settings in Security Center (Configure suppression, emails, onboarding)
│   ├── Manage Endpoint Security Settings in Microsoft Intune
│   └── Live Response Capabilities (Basic Commands, Advanced Commands)
├── Device Groups
│   ├── Operational Functions
│   │   ├── 1. Restrict visibility mapped to Entra ID (the cloud-based identity and access management service) user groups
│   │   ├── 2. Assign distinct automated remediation levels
│   │   └── 3. Filter device lists and incident graphs
│   └── Configuration Parameters
│       ├── Group Name, Automation Level, Matching Rules, User Access Rights, Rank
│       └── Highest-ranked device group (Exclusive assignment if endpoint matches multiple groups)
└── Advanced Features Toggles
    ├── Automated Investigation & Autoresolve Remediated Alerts
    ├── Live Response & Live Response for Servers
    ├── Live Response Unsigned Script Execution
    ├── Always Remediate PUA
    ├── Enable EDR in Block Mode
    ├── Custom Network Indicators
    ├── Tamper Protection
    ├── Share Endpoint Alerts with Purview
    └── Microsoft Intune Connection
```

### Hardening the Office Doors: Attack Surface Reduction (ASR) Rules and Protection Stack

In our campus analogy, **ASR** (attack surface reduction, a set of software hardening capabilities that block common entry vectors used by malware) acts as physical reinforcement on office doors, windows, and mail slots. ASR stops threats before execution by constraining software behaviors commonly abused by attackers.

  

The overall ASR protection stack comprises eight core security components:

  

1. **ASR Rules**: Intelligent rules that target software behaviors commonly abused by malware, such as Office apps launching child processes or running obfuscated scripts.
    
      
    
2. **Hardware-Based Isolation**: Uses **VBS** (virtualization-based security, hardware-assisted isolated memory environments) to protect system integrity at startup and isolate web browsing sessions inside **Application Guard** (a container isolation feature that runs Microsoft Edge inside an isolated hypervisor).
    
      
    
3. **Application Control**: Enforces **WDAC** (Windows Defender Application Control, enterprise code integrity policies that restrict executable files), shifting from a default-trust model to an explicit trust model where applications must prove safety before running.
    
      
    
4. **Exploit Protection**: Applies memory mitigations (e.g., **DEP** [data execution prevention], **ASLR** [address space layout randomization]) to operating systems and individual apps; functions alongside third-party antivirus.
    
      
    
5. **Network Protection**: Extends **SmartScreen** (Microsoft Defender SmartScreen, a cloud-based reputation service that blocks malicious websites and downloads) reputation filtering to all outbound network traffic and socket calls across non-Microsoft browsers.
    
      
    
6. **Web Protection**: Blocks access to phishing domains, untrusted web content, and web content categories defined by corporate policies.
    
      
    
7. **Controlled Folder Access**: Prevents unauthorized applications and file-encrypting **Ransomware** (extortion malware that encrypts files and demands payment for decryption keys) from modifying files inside protected system folders (e.g., `Documents`, `Pictures`, `Desktop`).
    
      
    
8. **Device Control**: Monitors and regulates access to physical media, such as USB drives, removable storage devices, and Bluetooth transfers.
    
      
    

Individual **ASR Rules** target specific software abuse patterns across four distinct operational modes:

  

- **Disable (0)**: Turns off the specific ASR rule.
    
      
    
- **Block (1)**: Actively prevents the suspicious behavior from executing.
    
      
    
- **Audit (2)**: Evaluates and logs the behavior in Windows Event Viewer without blocking, allowing admins to assess line-of-business application impact before full deployment.
    
      
    
- **Warn (6)**: Blocks the behavior initially but presents a notification allowing end users to bypass the restriction if necessary.
    
      
    

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CORE ASR RULES AND OBJECTIVES                   │
├────────────────────────────────────────────────────────────────────────┤
│ • Block executable content from email client and webmail               │
│ • Block all Office applications from creating child processes          │
│ • Block Office applications from creating executable content           │
│ • Block Office applications from injecting code into other processes   │
│ • Block JavaScript or VBScript from launching downloaded executables   │
│ • Block execution of potentially obfuscated scripts                    │
│ • Block Win32 API calls from Office macros                             │
│ • Use advanced protection against ransomware                           │
│ • Block credential stealing from Windows LSASS (lsass.exe)             │
│ • Block process creations originating from PSExec and WMI commands     │
│ • Block untrusted and unsigned processes that run from USB             │
│ • Block executable files unless they meet prevalence or age criteria   │
│ • Block Office communication applications from creating child processes│
│ • Block Adobe Reader from creating child processes                     │
│ • Block persistence through WMI event subscription                     │
│ • Block abuse of exploited vulnerable signed drivers                   │
└────────────────────────────────────────────────────────────────────────┘
```

ASR rules can be deployed across Windows 10/11 and Windows Server (2016 through 2025) using five management channels:

  

- **Intune Endpoint Security Policy**: `Endpoint Security > Attack surface reduction > Create Policy`.
    
      
    
- **Intune Device Configuration Profiles**: `Device configuration > Profiles > Endpoint protection > Windows Defender Exploit Guard > Attack Surface Reduction`.
    
      
    
- **MDM CSP**: Configured via **OMA-URI** (Open Mobile Alliance Uniform Resource Identifier, a standardized path syntax used in device management policies) `./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionRules` passing rule **GUIDs** (globally unique identifiers) and state values (e.g., `GUID=1|GUID=2`).
    
      
    
- **Group Policy**: `Computer Configuration > Administrative Templates > Windows Components > Microsoft Defender Antivirus > Windows Defender Exploit Guard > Attack surface reduction`.
    
      
    
- **PowerShell**: Configured using `Set-MpPreference` or `Add-MpPreference`:
    
      
    

PowerShell

```
# Enable ASR Rule 1 in Block Mode and Rule 2 in Audit Mode
Add-MpPreference -AttackSurfaceReductionRules_Ids 75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84, 3B576869-A4EC-4529-8536-B80A7769E899 -AttackSurfaceReductionRules_Actions Enabled, AuditMode

# Add File Path Exclusion to ASR Rules
Add-MpPreference -AttackSurfaceReductionOnlyExclusions "C:\Program Files\LOBApp\app.exe"
```

The `Set-MpPreference` cmdlet overwrites existing ASR rule lists, while `Add-MpPreference` appends new rules and exclusions to existing configurations. All ASR trigger events are logged locally under `Applications and Services Logs > Microsoft > Windows > Windows Defender / Exploit Guard`.

  

Plaintext

```
[ Software Initiates Action (e.g., Macro Spawns Child Process) ]
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    EVALUATE ASR RULE MODE                    │
├──────────────┬───────────────┬────────────────┬──────────────┤
│ Disable (0)  │   Block (1)   │   Audit (2)    │   Warn (6)   │
├──────────────┼───────────────┼────────────────┼──────────────┤
│ Allows       │ Blocks Action │ Logs Event to  │ Blocks Action│
│ Execution    │ Instantly &   │ Event Viewer & │ but Prompts  │
│ Unchecked    │ Raises Alert  │ Portal Dashboard User Bypass  │
└──────────────┴───────────────┴────────────────┴──────────────┘
```

Plaintext

```
3. HARDENING THE OFFICE DOORS: ASR & PROTECTION STACK
├── 8 Core Security Components
│   ├── 1. ASR Rules (Intelligent rules that target software behaviors commonly abused by malware)
│   ├── 2. Hardware-Based Isolation
│   │   ├── VBS (virtualization-based security, hardware-assisted isolated memory environments)
│   │   └── Application Guard (a container isolation feature that runs Microsoft Edge inside an isolated hypervisor)
│   ├── 3. Application Control
│   │   └── WDAC (Windows Defender Application Control, enterprise code integrity policies that restrict executable files)
│   ├── 4. Exploit Protection (Applies memory mitigations like DEP [data execution prevention] and ASLR [address space layout randomization])
│   ├── 5. Network Protection
│   │   └── SmartScreen (Microsoft Defender SmartScreen, a cloud-based reputation service that blocks malicious websites)
│   ├── 6. Web Protection
│   ├── 7. Controlled Folder Access
│   │   └── Ransomware (extortion malware that encrypts files and demands payment for decryption keys)
│   └── 8. Device Control
├── ASR Operational Modes
│   ├── Disable (0) (Turns off rule)
│   ├── Block (1) (Prevents execution actively)
│   ├── Audit (2) (Logs behavior in Event Viewer without blocking)
│   └── Warn (6) (Blocks initially but allows end-user bypass)
└── ASR Deployment Channels
    ├── Intune Endpoint Security Policy
    ├── Intune Device Configuration Profiles
    ├── MDM CSP
    │   ├── OMA-URI (Open Mobile Alliance Uniform Resource Identifier, standardized path syntax)
    │   └── GUIDs (globally unique identifiers)
    ├── Group Policy
    └── PowerShell (Using Set-MpPreference to overwrite or Add-MpPreference to append)
```

### Inspecting the Crime Scene: Device Investigation, Behavioral Blocking, and EDR in Block Mode

When an alarm trips on a workstation, analysts open the **Device Inventory Page** (`Assets > Devices`) to review the compromised host. Operating under an "assume breach" mindset, MDE continuously streams behavioral cyber telemetry—kernel activities, memory operations, process trees, registry edits, network sockets, and file changes—retaining records for 6 months.

  

The Device Inventory displays key status indicators:

  

- **Risk Level**: Evaluates overall operational risk (**High**, **Medium**, **Low**, or **Informational**) based on active alert severities. Resolving alerts or approving AIR remediations lowers device risk.
    
      
    
- **Exposure Level**: Measures vulnerability vulnerability impact (**High**, **Medium**, **Low**) based on pending security recommendations. Shows "No data available" if the host has been inactive for >30 days, runs an unsupported OS, or uses a stale agent.
    
      
    
- **Health State**: Categorized as **Active** (actively reporting), **Inactive** (stopped sending signals for >7 days), or **Misconfigured** (sub-classified as _No sensor data_ or _Impaired communications_).
    
      
    
- **Antivirus Status**: Categorized as **Disabled**, **Not reporting**, or **Not updated**.
    
      
    

Selecting an endpoint opens the **Device Page**, which provides seven specialized investigation tabs:

  

- **Overview Tab**: Features summary cards for Active Alerts (grouped by New/In progress across 30 days), Logged on Users (first/last seen and sign-in types), and Security Assessments.
    
      
    
- **Alerts Tab**: Filtered queue displaying all alerts associated with the specific host.
    
      
    
- **Timeline Tab**: Chronological view of raw device events and alerts over the past 30 days. Analysts filter events by date range, search keywords, or event groups; flag specific events to build a breach timeline; export up to **7 days** of timeline events to CSV; or click **Hunt for related events** to instantly load the event context into an **Advanced Hunting** query.
    
      
    
- **Security Recommendations Tab**: Lists prioritized vulnerability fixes generated by Defender Vulnerability Management.
    
      
    
- **Software Inventory Tab**: Displays installed software, versions, and associated weakness counts.
    
      
    
- **Discovered Vulnerabilities Tab**: Lists specific **CVE** (common vulnerabilities and exposures, a standardized public list of known cybersecurity weaknesses) IDs impacting the host alongside **CVSS** (common vulnerability scoring system, an industry-standard numeric rating system measuring vulnerability severity) ratings.
    
      
    
- **Missing KBs Tab**: Highlights missing Windows security patches and **KB** (Knowledge Base, Microsoft software update documentation numbers) updates.
    
      
    

To stop fileless and polymorphic threats in real time, MDE uses **Behavioral Blocking and Containment**.

  

Plaintext

```
[ Process Initiates Execution ]
              │
              ▼
[ Client Behavioral Blocking (Defender AV) ] ──(Malicious)──► [ Instant Local Block ]
              │
              ▼
 [ Cloud Protection & ML Classifiers ]       ──(Malicious)──► [ Rapid Feedback-Loop Block ]
              │
              ▼
 [ Post-Breach EDR In Block Mode ]           ──(Malicious)──► [ Remediates Running Artifact ]
```

Behavioral blocking operates across three integrated defense layers:

  

1. **Client Behavioral Blocking**: Defender AV monitors process trees and in-memory behaviors locally, sending suspicious patterns to cloud machine learning classifiers. Within milliseconds, the cloud protection service evaluates the artifact and blocks execution. Detections follow MITRE ATT&CK naming conventions (e.g., `Behavior:Win32/CredentialAccess.*!ml` or `Behavior:Win32/LateralMovement.*!ml`).
    
      
    
2. **Feedback-Loop Blocking**: Also known as rapid protection. When an artifact is confirmed malicious on one endpoint, the cloud engine immediately transmits protection signatures tenant-wide and globally to block the threat on all other machines before execution begins.
    
      
    
3. **EDR in Block Mode**: Extends behavioral containment post-breach. When **EDR in block mode** is enabled (`Settings > Endpoints > Advanced features`), MDE remediates malicious files, scripts, or memory artifacts detected by post-breach EDR capabilities, **even if a third-party antivirus solution is active and Defender AV is running in Passive Mode**. Completed blocks are logged in the Action Center as _Blocked_ or _Prevented_.
    
      
    

Plaintext

```
4. INSPECTING THE CRIME SCENE: DEVICE INVESTIGATION & BEHAVIORAL BLOCKING
├── Device Inventory Page Status Indicators
│   ├── Risk Level (High, Medium, Low, Informational based on active alerts)
│   ├── Exposure Level (High, Medium, Low based on vulnerabilities)
│   ├── Health State (Active, Inactive, Misconfigured [No sensor data, Impaired communications])
│   └── Antivirus Status (Disabled, Not reporting, Not updated)
├── Device Page Investigation Tabs
│   ├── Overview Tab & Alerts Tab
│   ├── Timeline Tab (Raw device events over 30 days)
│   │   ├── 7 days CSV export limit
│   │   └── Hunt for related events (Instant pivot to Advanced Hunting)
│   ├── Security Recommendations Tab & Software Inventory Tab
│   ├── Discovered Vulnerabilities Tab
│   │   ├── CVE (common vulnerabilities and exposures, a standardized public list of known weaknesses)
│   │   └── CVSS (common vulnerability scoring system, numeric rating system measuring severity)
│   └── Missing KBs Tab
│       └── KB (Knowledge Base, Microsoft software update documentation numbers)
└── Behavioral Blocking and Containment
    ├── 1. Client Behavioral Blocking (Defender AV local monitoring -> ML Classifiers -> Instant Local Block)
    ├── 2. Feedback-Loop Blocking (Rapid protection signature transmission globally)
    └── 3. EDR in Block Mode (Extends behavioral containment post-breach even if Defender AV is in Passive Mode)
```

### Frontline Containment and Evidence Collection: Live Response and Investigation Packages

When investigating a compromised machine, analysts execute **Response Actions** directly from the top toolbar of the Device Page.

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DEVICE RESPONSE ACTIONS                         │
├───────────────────┬────────────────────────────────────────────────────┤
│ RESPONSE ACTION   │ OPERATIONAL MECHANISM & PLATFORM SUPPORT           │
├───────────────────┼────────────────────────────────────────────────────┤
│ Isolate Device    │ Disconnects host from network while preserving MDE │
│                   │ cloud connectivity. Supports Selective Isolation   │
│                   │ (enabling Outlook, Teams, Skype for Business).     │
│                   │ Supported on Windows, macOS, and Linux.            │
├───────────────────┼────────────────────────────────────────────────────┤
│ Restrict App      │ Applies a WDAC code integrity policy that permits  │
│ Execution         │ only files signed by a Microsoft certificate to    │
│                   │ run. Supported on Windows 10 (1709+) and Win 11.   │
├───────────────────┼────────────────────────────────────────────────────┤
│ Run Antivirus     │ Remotely triggers a Quick or Full scan. Respects   │
│ Scan              │ ScanAvgCPULoadFactor (default 50% CPU limit).      │
│                   │ Runs even if Defender AV is in Passive Mode.       │
├───────────────────┼────────────────────────────────────────────────────┤
│ Contain Device    │ Targets unmanaged/rogue devices. Instructs         │
│                   │ surrounding managed endpoints to reject network    │
│                   │ connections from the contained device IP/MAC.      │
├───────────────────┼────────────────────────────────────────────────────┤
│ Collect Investi-  │ Downloads a password-protected .zip archive containing│
│ gation Package    │ 14 forensic folders (autoruns, prefetch, logs).    │
├───────────────────┼────────────────────────────────────────────────────┤
│ Initiate Live     │ Establishes a remote interactive command-line shell│
│ Response Session  │ to run basic or advanced forensic commands.        │
└───────────────────┴────────────────────────────────────────────────────┘
```

#### Forensic Investigation Package Collection

Selecting **Collect investigation package** downloads a password-protected `.zip` archive containing 14 detailed forensic snapshots:

  

- `Autoruns`: Registry entries across known **ASEP** (auto-start entry points, registry locations used by software to run automatically at startup) locations to identify persistence.
    
      
    
- `Installed programs`: CSV list of installed software compiled via Win32 product classes.
    
      
    
- `Network connections`: Active TCP/IP connections (`ActiveNetConnections.txt`), ARP table (`Arp.txt`), DNS resolver cache (`DnsCache.txt`), TCP/IP adapter settings (`IpConfig.txt`), and firewall logs (`FirewallExecutionLog.txt` / `pfirewall.log`).
    
      
    
- `Prefetch files`: Copy of Windows prefetch files from `%SystemRoot%\Prefetch` and file listing (`PrefetchFilesList.txt`) to track recently executed applications.
    
      
    
- `Processes`: CSV list of running processes, parent process IDs, and memory states.
    
      
    
- `Scheduled tasks`: CSV dump of active scheduled tasks.
    
      
    
- `Security event log`: Windows Security Event Log (`.evtx`) recording authentication events.
    
      
    
- `Services`: CSV dump of installed system services and startup configurations.
    
      
    
- `SMB sessions`: Inbound (`SMBInboundSessions`) and outbound (`SMBOutboundSession`) network file sharing connections.
    
      
    
- `System information`: `SystemInformation.txt` detailing OS build, hotfixes, and hardware.
    
      
    
- `Temp directories`: Text dumps listing dropped files in `%Temp%` across all user profiles.
    
      
    
- `Users and groups`: Local user accounts and group memberships.
    
      
    
- `WdSupportLogs`: Defender AV support logs (`MpCmdRunLog.txt` and `MPSupportFiles.cab`).
    
      
    
- `CollectionSummaryReport.xls`: Execution summary report detailing extracted data points, extraction commands, and error codes.
    
      
    

#### Live Response Sessions

**Live Response** provides an interactive remote shell connection to the endpoint. Live Response sessions enforce strict operational constraints:

  

- **Session Limits**: Inactivity timeout occurs after **5 minutes**; maximum of **10 concurrent sessions** tenant-wide; an individual user can initiate only 1 session at a time; an individual device can participate in only 1 session at a time.
    
      
    
- **File Size Limits**: Background download (`getfile`) limit is **3 GB**; file metadata inspection (`fileinfo`) limit is **10 GB**; library upload limit is **250 MB**.
    
      
    
- **Prerequisites**: Enabled in Advanced features, device assigned an Automation Remediation level (at least minimum level), and user provisioned with appropriate permissions (**Security operations > Security data > Response (manage)** in URBAC or Standalone RBAC Live Response roles). Unsigned PowerShell script execution requires enabling the **Live response unsigned script execution** setting in Advanced features.
    
      
    

Live Response commands are divided into Basic and Advanced categories:

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        LIVE RESPONSE COMMAND SET                       │
├───────────────────┬────────────────────────────────────────────────────┤
│ COMMAND CATEGORY  │ COMMANDS AND FUNCTIONAL DESCRIPTION                │
├───────────────────┼────────────────────────────────────────────────────┤
│ Basic Commands    │ cd, cls, connect, connections, dir, drivers,       │
│ (Read-Only        │ fileinfo, findfile, help, persistence, processes,  │
│ Forensic Inspection) registry, scheduledtasks, services, trace.       │
│                   │ getfile <path>: Downloads file in background;      │
│                   │ Ctrl+Z sends to background, fg <id> restores.      │
├───────────────────┼────────────────────────────────────────────────────┤
│ Advanced Commands │ analyze: Runs incrimination engines on file/PID.   │
│ (Remediation &    │ getfile -auto: Auto-runs fileinfo prerequisite.    │
│ Script Execution) │ run <script>: Executes script from library.        │
│                   │ library: Lists uploaded tenant library scripts.    │
│                   │ putfile <file>: Uploads library file to device     │
│                   │ working directory (deleted on reboot by default).  │
│                   │ remediate: Remediates entity; supports -auto flag. │
│                   │ undo: Restores remediated file or registry entity. │
└───────────────────┴────────────────────────────────────────────────────┘
```

Commands support output piping to text files (`[command] > [filename].txt`) and format modifiers (`-output json` or `-output table`). All executed shell commands are audited under the **Command Log** tab.

  

Plaintext

```
5. FRONTLINE CONTAINMENT: RESPONSE ACTIONS & LIVE RESPONSE
├── Device Response Actions
│   ├── Isolate Device (Supports Selective Isolation for Outlook/Teams)
│   ├── Restrict App Execution (WDAC policy permitting only MS-signed files)
│   ├── Run Antivirus Scan (Quick/Full scan respects ScanAvgCPULoadFactor)
│   ├── Contain Device (Targets unmanaged/rogue devices)
│   ├── Collect Investigation Package (14 forensic folders)
│   └── Initiate Live Response Session
├── Forensic Investigation Package (14 Snapshots)
│   ├── Autoruns (ASEP [auto-start entry points, registry locations used by software to run at startup])
│   ├── Installed programs, Network connections, Prefetch files, Processes, Scheduled tasks
│   ├── Security event log, Services, SMB sessions, System information, Temp directories
│   └── Users and groups, WdSupportLogs, CollectionSummaryReport.xls
└── Live Response Sessions
    ├── Operational Constraints
    │   ├── Session Limits (5 mins inactivity timeout, 10 concurrent tenant-wide max)
    │   ├── File Limits (3 GB getfile, 10 GB fileinfo, 250 MB library upload)
    │   └── Prerequisites (Automation Remediation level assigned, URBAC permissions, Unsigned Script toggle)
    ├── Command Categories
    │   ├── Basic Commands (cd, cls, connect, connections, dir, drivers, fileinfo, findfile, help, persistence, processes, registry, scheduledtasks, services, trace, getfile)
    │   └── Advanced Commands (analyze, getfile -auto, run, library, putfile, remediate, undo)
    └── Auditing
        └── Command Log (Records all executed shell commands)
```

### Deep Evidence Analysis: Investigating Files, Users, IPs, and Domains

When an investigation involves specific artifacts, analysts navigate to dedicated profile pages for **Files**, **User Accounts**, **IP Addresses**, and **Domains/URLs**.

  

#### 1. File Investigation

Selecting a file hash opens the **File Profile Page**, displaying SHA1, SHA256, MD5, file size, digital signer verification, **VirusTotal** detection ratio, Defender AV verdict, and organizational/worldwide prevalence.

  

The page features four primary tabs:

  

- **Alerts Tab**: Filtered queue displaying alerts associated with the file.
    
      
    
- **Observed in Organization Tab**: Displays devices where the file was observed within a selected date range (up to 100 devices; exporting to CSV surfaces all impacted hosts).
    
      
    
- **Deep Analysis Tab**: Submits portable executable files (`.exe` or `.dll`) to a secure cloud hypervisor detonation chamber. Deep analysis executes the file, observes behavioral actions, and generates a report detailing dropped files, registry edits, process injections, and contacted IP addresses. Sample submission requires local registry key `HKLM\SOFTWARE\Policies\Microsoft\Windows Advanced Threat Protection` `AllowSampleCollection=1`.
    
      
    
- **File Names Tab**: Lists all file names used by the binary across the organization.
    
      
    

File response actions along the top toolbar include:

  

- **Stop and Quarantine File**: Stops running processes, quarantines the binary, and deletes persistent registry keys across up to **1,000 devices** (requires Windows 10 1703+ or Windows 11, and Defender AV in Active or Passive mode).
    
      
    
- **Add Indicator**: Creates a custom IoC to block or allow the file hash.
    
      
    
- **Download File**: Downloads a password-protected `.zip` archive containing the sample. If the file is not in the cloud backend, a **Collect file** button appears (available if the file was observed within the past 30 days).
    
      
    

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FILE PROFILE PAGE LAYOUT                        │
├───────────────────┬───────────────────┬────────────────┬───────────────┤
│   FILE DETAILS    │      ALERTS       │   OBSERVED IN  │ DEEP ANALYSIS │
│                   │                   │  ORGANIZATION  │               │
├───────────────────┼───────────────────┼────────────────┼───────────────┤
│ Hashes (SHA256),  │ Associated alerts │ Devices seen   │ Cloud sandbox │
│ VirusTotal ratio, │ in queue.         │ (up to 100 in  │ detonation for│
│ signer status,    │                   │ UI; export CSV │ PE files      │
│ prevalence data.  │                   │ for complete). │ (.exe / .dll).│
└───────────────────┴───────────────────┴────────────────┴───────────────┘
```

#### 2. User Account Investigation

Selecting a user account opens the **User Account Page**, displaying SAM account name, **SID** (security identifier, a unique alphanumeric code assigned to user and group accounts in Windows), sign-in risk level, **MFA** (multi-factor authentication, requiring multiple forms of verification) status, last sign-in timestamp, and Active Directory group memberships.

  

#### 3. IP Address Investigation

Selecting an external IP address opens the **IP Address Page**, surfacing **ASN** (autonomous system number, a unique identifier for internet network routing domains), reverse DNS hostnames, organizational prevalence, and a chronological list of endpoints that communicated with the IP.

  

#### 4. Domain and URL Investigation

Selecting a domain opens the **URL Page**, displaying URL registration contacts, nameservers, prevalence charts spanning 1 day to 6 months, active alerts, and observed device timelines.

  

Plaintext

```
6. DEEP EVIDENCE ANALYSIS: FILES, USERS, IPS & DOMAINS
├── 1. File Profile Page
│   ├── File Properties (SHA1, SHA256, MD5, VirusTotal detection ratio)
│   ├── Page Tabs
│   │   ├── Alerts Tab
│   │   ├── Observed in Organization Tab (UI capped at 100 devices; full export via CSV)
│   │   ├── Deep Analysis Tab (Cloud sandbox detonation for .exe / .dll files)
│   │   └── File Names Tab
│   └── Toolbar Actions
│       ├── Stop and Quarantine File (Executes across up to 1,000 devices)
│       ├── Add Indicator
│       └── Download File / Collect file
├── 2. User Account Page
│   └── Properties (SAM account name, SID [security identifier, a unique alphanumeric code assigned to accounts], MFA [multi-factor authentication])
├── 3. IP Address Page
│   └── Properties (ASN [autonomous system number, unique identifier for network routing domains], Reverse DNS)
└── 4. Domain and URL Page
    └── Properties (Registration contacts, nameservers, prevalence charts)
```

### Automated Robotic Guards: AIR Mechanics, Folder Exclusions, and Risk-Based Conditional Access

To prevent alert fatigue, **AIR** (automated investigation and remediation, automated software playbooks that analyze and clean up threats) uses inspection algorithms to evaluate alerts, build investigation graphs, and execute cleanup tasks automatically.

  

AIR configuration is governed by **Automation Levels** assigned to specific Device Groups:

  

- **Full - Remediate Threats Automatically**: Executes all remediation actions automatically on malicious artifacts.
    
      
    
- **Semi - Require Approval for Any Remediation**: Pauses all remediation actions and places them in the Pending tab awaiting human approval.
    
      
    
- **Semi - Require Approval for Core Folders Remediation**: Automatically executes remediation on non-core folders, but requires explicit human approval for actions targeting operating system directories (e.g., `\windows*`).
    
      
    
- **Semi - Require Approval for Non-Temp Folders Remediation**: Automatically executes remediation on temporary directories (e.g., `\temp*`, `\downloads*`, `\program files*`), but requires approval for files in all other system locations.
    
      
    
- **No Automated Response**: Disables AIR completely on targeted endpoints, significantly degrading security posture.
    
      
    

Administrators tune AIR upload and folder behaviors in Settings (`Settings > Endpoints > Rules`):

  

- **Automation Uploads**: **File Content Analysis** automatically uploads suspicious files with specific extensions (e.g., `exe,bat`) to the cloud during AIR runs. **Memory Content Analysis** permits uploading process memory content for deep inspection.
    
      
    
- **Automation Folder Exclusions**: Configures specific folders, subfolders, file extensions, or file names to be skipped by AIR playbooks during automated investigations.
    
      
    

To block compromised endpoints from corporate resources, organizations integrate MDE with Intune to enforce **Device Risk-Based Conditional Access**.

  

Plaintext

```
[ Device Risk Level Rises to High ]
                 │
                 ▼
[ MDE Transmits Risk Score to Intune ]
                 │
                 ▼
 [ Intune Compliance Policy Evaluates Host ]
 (Machine Risk Score Exceeds "Medium" Threshold)
                 │
                 ▼
 [ Intune Marks Device as Noncompliant ]
                 │
                 ▼
[ Entra ID Conditional Access Policy Blocks Access ]
```

Configuring risk-based Conditional Access follows a 5-step implementation workflow:

  

1. **Turn on Intune Connection in MDE**: Toggle **Microsoft Intune connection** to **On** in MDE Advanced features.
    
      
    
2. **Enable MDE Integration in Intune**: Set **Allow Microsoft Defender for Endpoint to enforce Endpoint Security Configurations** to **On**.
    
      
    
3. **Create Intune Compliance Policy**: Set **Require the device to be at or under the machine risk score** to a target threshold:
    
      
    - **Clear**: Most secure; host must have zero active threats.
        
          
        
    - **Low**: Compliant if only low-severity threats exist.
        
          
        
    - **Medium**: Compliant if low or medium-severity threats exist.
        
          
        
    - **High**: Least secure; allows all threat levels.
        
          
        
4. **Assign Policy**: Assign to targeted user or device security groups.
    
      
    
5. **Create Entra ID Conditional Access Policy**: Under Grant, select **Require device to be marked as compliant**.
    
      
    

Plaintext

```
7. AUTOMATED ROBOTIC GUARDS: AIR & RISK-BASED CONDITIONAL ACCESS
├── AIR (automated investigation and remediation, automated software playbooks that analyze and clean up threats)
├── Automation Levels (Assigned via Device Groups)
│   ├── Full - Remediate Threats Automatically
│   ├── Semi - Require Approval for Any Remediation
│   ├── Semi - Require Approval for Core Folders Remediation
│   ├── Semi - Require Approval for Non-Temp Folders Remediation
│   └── No Automated Response (Disables AIR completely)
├── Automation Exclusions & Uploads
│   ├── Automation Uploads (File Content Analysis, Memory Content Analysis)
│   └── Automation Folder Exclusions (Skips specific folders/extensions during investigation)
└── Device Risk-Based Conditional Access (5-Step Workflow)
    ├── 1. Turn on Intune Connection in MDE (Advanced features)
    ├── 2. Enable MDE Integration in Intune (Endpoint security)
    ├── 3. Create Intune Compliance Policy
    │   └── Target Thresholds: Clear (zero threats), Low, Medium, High (least secure)
    ├── 4. Assign Policy (To user or device groups)
    └── 5. Create Entra ID Conditional Access Policy (Require device to be marked as compliant)
```

### Fine-Tuning the Alarms: Alert Notifications, Suppression Rules, and Custom IoCs

#### Alert Email Notifications

Administrators configure automated email alerts to inform specific personnel immediately. Notification rules define targeted device groups and minimum alert severities (**High**, **Medium**, **Low**, **Informational**). Recipients must be validated using the **Send test email** button before saving.

  

#### Alert Suppression Rules

To eliminate false positives caused by legitimate tools, analysts build **Alert Suppression Rules**. Suppression rules hide known innocuous alerts from the queue without disabling underlying detection engines.

  

#### Indicators of Compromise (IoCs)

Security teams manage custom **IoCs** (indicators of compromise, technical evidence such as file hashes, IP addresses, or certificates indicating malicious activity) to enforce explicit allow, audit, or block actions. IoC indicators are honored across three detection engines: the MDE **Cloud Detection Engine**, the **Endpoint Prevention Engine** (Defender AV), and the **AIR Engine**.

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SUPPORTED IOC TYPES & ACTIONS                   │
├───────────────┬──────────────────────────────────┬─────────────────────┤
│ IOC TYPE      │ SUPPORTED ACTIONS                │ PREREQUISITES       │
├───────────────┼──────────────────────────────────┼─────────────────────┤
│ Files (PE)    │ Allow, Audit, Warn, Block        │ Defender AV Cloud   │
│               │ execution, Block & remediate.    │ Protection enabled; │
│               │                                  │ Antimalware 4.18.1901+│
├───────────────┼──────────────────────────────────┼─────────────────────┤
│ IP Addresses, │ Allow, Audit, Warn, Block.       │ Network Protection  │
│ URLs / Domains│ (No CIDR notation; single IPs    │ in Block Mode;      │
│               │  and FQDNs only).                │ Antimalware 4.18.1906+│
├───────────────┼──────────────────────────────────┼─────────────────────┤
│ Certificates  │ Allow, Block. (Leaf certificates │ Defender AV Cloud   │
│               │ chained to trusted Root CA;      │ Protection enabled; │
│               │ CER or PEM extensions).          │ CER/PEM files.      │
└───────────────┴──────────────────────────────────┴─────────────────────┘
```

A tenant can store up to **15,000 indicators**. Indicators can be uploaded in bulk using a CSV file. File blocks take effect within minutes, while URL/IP and certificate indicators may take up to **2 to 3 hours** to propagate across all endpoint sensors.

  

Plaintext

```
8. FINE-TUNING THE ALARMS: NOTIFICATIONS, SUPPRESSION & IOCS
├── Alert Email Notifications (Scopes by severity/device groups; Send test email required)
├── Alert Suppression Rules (Hides known innocuous alerts without disabling detection engines)
└── IoCs (indicators of compromise, technical evidence indicating malicious activity)
    ├── Supported Detection Engines
    │   └── Cloud Detection Engine, Endpoint Prevention Engine, AIR Engine
    ├── IoC Types & Actions
    │   ├── Files (PE) (Allow, Audit, Warn, Block execution, Block & remediate)
    │   ├── IP Addresses, URLs / Domains (Allow, Audit, Warn, Block)
    │   └── Certificates (Allow, Block - requires CER or PEM extensions)
    └── Management Constraints
        ├── Tenant Limit (15,000 indicators)
        ├── CSV Import format (indicatorType, indicatorValue, action, title, expirationTime, severity, rbacGroupNames, category, MITRE techniques)
        └── Propagation Time (Files = minutes; URLs/IPs/Certs = 2 to 3 hours to propagate)
```

### Proactive Campus Inspections: Defender Vulnerability Management and Intune Remediation

Rather than waiting for an attack to occur, **Microsoft Defender Vulnerability Management** continuously discovers vulnerabilities and software misconfigurations in real time using the built-in, agentless MDE sensor.

  

Defender Vulnerability Management is available across three capability tiers:

  

- **Core Vulnerability Management**: Included natively in **Microsoft Defender for Endpoint Plan 2**.
    
      
    
- **Vulnerability Management Add-on**: Optional add-on for MDE Plan 2. Adds **Security baselines assessment profiles**, **Block vulnerable applications**, **Browser extensions assessment**, **Digital certificate assessment**, and **Network share analysis**.
    
      
    
- **Vulnerability Management Standalone**: Full standalone vulnerability management platform for un-agented environments.
    
      
    

Vulnerability metrics are monitored through five primary portal blades:

  

1. **Dashboard**: Surfaces the **Exposure Score** (a metric reflecting overall organizational vulnerability risk based on security recommendations) and **Configuration Score** (measuring security benchmark compliance).
    
      
    
2. **Recommendations**: Prioritizes security fixes by fusing vulnerability data with dynamic threat context:
    
      
    - **Active Exploits in the Wild**: Prioritizes CVEs currently being weaponized.
        
          
        
    - **Active Breach Correlation**: Cross-references EDR detections to highlight vulnerabilities being exploited inside the organization.
        
          
        
    - **High-Value Assets**: Prioritizes exposed devices carrying confidential data.
        
          
        
3. **Remediation**: Tracks active remediation tasks, ticket statuses, and completion progress.
    
      
    
4. **Inventories**: Displays installed software, weakness counts, certificates, and network shares.
    
      
    
5. **Weaknesses**: Lists specific CVE IDs, severity ratings, and CVSS scores.
    
      
    

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                   VULNERABILITY REMEDIATION WORKFLOW                   │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Security Admin selects recommendation in MDE Recommendations tab.   │
│ 2. Clicks "Remediation options" and completes request form.            │
│ 3. Form defines scope, target device groups, priority, and due date.   │
│ 4. Submitting creates a Security Task in MDE & a Ticket in Intune.     │
│ 5. IT Admin approves ticket in Intune & deploys software update/patch. │
│ 6. Real-time MDE sensor detects update & automatically closes task.    │
└────────────────────────────────────────────────────────────────────────┘
```

To initiate remediation, a security analyst selects a security recommendation, clicks **Remediation options**, selects the target IT service management tool (**Intune for AAD joined machines** or **ServiceNow**), sets priority and due date, and submits the request. Submitting the request automatically provisions a **Security Task** in MDE and generates an actionable **Remediation Ticket** inside Intune. As soon as the MDE sensor detects the updated binary on the endpoint, the remediation task and exposure score update automatically in real time.

  

Plaintext

```
9. PROACTIVE CAMPUS INSPECTIONS: VULNERABILITY MANAGEMENT
├── Microsoft Defender Vulnerability Management Tiers
│   ├── Core Vulnerability Management (Included in MDE Plan 2)
│   ├── Vulnerability Management Add-on (Security baselines, Block vulnerable applications, Cert/Network analysis)
│   └── Vulnerability Management Standalone (For un-agented environments)
├── Vulnerability Portal Blades
│   ├── Dashboard
│   │   ├── Exposure Score (A metric reflecting overall organizational vulnerability risk)
│   │   └── Configuration Score (Measuring security benchmark compliance)
│   ├── Recommendations
│   │   ├── Active Exploits in the Wild (Weaponized CVEs)
│   │   ├── Active Breach Correlation (Cross-referenced with EDR)
│   │   └── High-Value Assets
│   ├── Remediation, Inventories, Weaknesses
└── Remediation Workflow
    ├── 1. Select recommendation -> "Remediation options"
    ├── 2. Submit form -> creates Security Task (in MDE) & Remediation Ticket (in Intune/ServiceNow)
    ├── 3. IT Admin deploys patch via Intune
    └── 4. Real-time MDE sensor detects update and auto-closes task
```

## Connecting the Dots

To protect every desk and door across our enterprise digital campus, every component of Microsoft Defender for Endpoint snaps together into a unified frontline defense system.

  

Plaintext

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE FRONT-LINE DESK SQUAD                                 │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Campus Desk Registration│   │ Squad Access & Groups  │   │ Hardening Office Doors  │  │
│  │ (Device Onboarding)   │──►│ (URBAC / Device Groups)│──►│ (Attack Surface Reduct.)│  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Crime Scene Inspection│──►│ Remote Containment     │◄──│ Evidence Profile Pages  │  │
│  │ (Device Timeline / EDR│   │ (Live Response / AIR)  │   │ (Files, Users, IPs)     │  │
│  └───────────────────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ PROACTIVE INSPECTIONS  │                                │
│                              │ (Defender Vulnerability│                                │
│                              │  Management & Intune)  │                                │
│                              └────────────────────────┘                                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Desk Registration and Squad Governance**: Endpoints are onboarded into **Microsoft Defender for Endpoint** using deployment scripts, Group Policy, or **Intune**, discovering unmanaged devices via **Standard Discovery**. Devices are organized into ranked **Device Groups** and assigned to analysts using **URBAC** permissions and **Advanced Features** settings.
    
      
    
2. **Door Hardening and Behavioral Containment**: Workstations are hardened against intrusion using **Attack Surface Reduction (ASR)** rules enforced via Intune, MDM CSPs, or PowerShell. When malware bypasses pre-execution locks, **Client Behavioral Blocking**, **Feedback-Loop Blocking**, and **EDR in Block Mode** contain malicious behaviors in real time.
    
      
    
3. **Crime Scene Forensics and Remote Containment**: Analysts investigate compromised hosts using the **Device Timeline** and **Behavioral Cyber Telemetry**. Threat responders execute immediate containment using **Device Isolation** (with selective Outlook/Teams communications), **Restrict App Execution**, or **Contain Device** (for unmanaged rogue hosts). Deep evidence is gathered by collecting a 14-folder **Investigation Package** or opening a **Live Response** remote shell session to run basic and advanced commands.
    
      
    
4. **Evidence Triaging and Automated Remediation**: Physical evidence is analyzed on profile pages for **Files** (running cloud sandbox **Deep Analysis** on `.exe`/`.dll` binaries), **User Accounts**, **IP Addresses**, and **Domains**. High-volume alerts trigger **AIR** playbooks operating under defined **Automation Levels**, while unhealthy endpoints are blocked from corporate apps using **Intune Compliance Policies** and **Entra ID Conditional Access**.
    
      
    
5. **Alarm Tuning and Proactive Patching**: SOC teams eliminate false positives using **Alert Suppression Rules**, send targeted **Email Notifications**, and import custom **IoC Indicators** (files, URLs, certificates). Finally, **Defender Vulnerability Management** scans endpoints for software weaknesses, routing prioritized **Remediation Tasks** to Intune IT administrators to close security gaps before intruders can exploit them, ensuring absolute device-level security across the entire enterprise campus.
    

Plaintext

```
MASTER MAP: MITIGATING THREATS WITH MICROSOFT DEFENDER FOR ENDPOINT
├── 1. CAMPUS DESK REGISTRATION (SETUP & ONBOARDING)
│   ├── Configuration (Security Administrator sets Geo Location, 6-Month Retention, Preview features)
│   ├── Telemetry Mechanics (LocalSystem Account, WinHTTP, WPAD, Transparent Proxy)
│   ├── Device Discovery
│   │   ├── Basic Discovery (SenseNDR.exe passive listening)
│   │   └── Standard Discovery (Active multicast probing, detects Log4j2 CVE-2021-44228)
│   └── Onboarding Deployment
│       ├── Windows (Local Script, GPO, Configuration Manager, Intune, VDI)
│       └── Cross-Platform (macOS via Jamf/MAU, Linux via Puppet/Ansible, iOS, Android PUA)
├── 2. BADGING THE SQUAD (ACCESS CONTROL & DEVICE GROUPS)
│   ├── URBAC (Unified Role-Based Access Control)
│   ├── Custom Roles (View Data, Active Remediation, Alerts Inv., Live Response)
│   ├── Device Groups (Filters devices by Entra ID groups, sets Automation Levels, exclusive Highest-Rank assignment)
│   └── Advanced Features (Automated Investigation, Live Response Unsigned Scripts, EDR in Block Mode, Tamper Protection)
├── 3. HARDENING THE OFFICE DOORS (ATTACK SURFACE REDUCTION)
│   ├── 8 Core Security Components
│   │   ├── ASR Rules, Hardware Isolation (VBS, Application Guard)
│   │   ├── Application Control (WDAC), Exploit Protection (DEP, ASLR)
│   │   └── Network/Web Protection (SmartScreen), Controlled Folder Access (Ransomware), Device Control
│   ├── ASR Operational Modes (Disable [0], Block [1], Audit [2], Warn [6])
│   └── Deployment (Intune, MDM CSP via OMA-URI/GUIDs, Group Policy, PowerShell Add-MpPreference)
├── 4. INSPECTING THE CRIME SCENE (DEVICE INVESTIGATION & BEHAVIORAL BLOCKING)
│   ├── Device Inventory (Risk Level, Exposure Level, Health State, AV Status)
│   ├── Device Page Tabs (Overview, Alerts, Timeline [7 days CSV], Discovered Vulnerabilities [CVE/CVSS])
│   └── Behavioral Containment
│       └── (1. Client Behavioral Blocking -> 2. Feedback-Loop Blocking -> 3. EDR in Block Mode)
├── 5. FRONTLINE CONTAINMENT (RESPONSE ACTIONS & LIVE RESPONSE)
│   ├── Response Actions (Isolate Device, Restrict App Execution, Run AV, Contain Device)
│   ├── Investigation Package (14 folders including Autoruns [ASEP], Prefetch, Services, WdSupportLogs)
│   └── Live Response Sessions
│       ├── Constraints (5 mins timeout, 10 concurrent, 3 GB getfile, 250 MB upload)
│       └── Commands (Basic read-only vs. Advanced script execution and analyze/remediate)
├── 6. DEEP EVIDENCE ANALYSIS (ARTIFACT PROFILES)
│   ├── File Profile Page (SHA256, VirusTotal, Deep Analysis detonation, Stop & Quarantine up to 1,000 devices)
│   ├── User Account Page (SID, MFA status)
│   ├── IP Address Page (ASN tracking)
│   └── Domain and URL Page
├── 7. AUTOMATED ROBOTIC GUARDS (AIR & CONDITIONAL ACCESS)
│   ├── AIR Automation Levels (Full, Semi-Core Folders, Semi-Non-Temp Folders, No Response)
│   ├── Automation Adjustments (Folder Exclusions, File/Memory Content Uploads)
│   └── Risk-Based Conditional Access (Intune connection -> Intune Compliance Policy -> Entra ID Block)
├── 8. FINE-TUNING THE ALARMS (NOTIFICATIONS, SUPPRESSION, IOCS)
│   ├── Tuning Controls (Email Notifications, Alert Suppression Rules)
│   └── IoCs (Indicators of compromise)
│       ├── Types (Files, IPs, URLs, Certificates)
│       └── Management (Max 15,000, 2-3 hour propagation, CSV imports)
└── 9. PROACTIVE CAMPUS INSPECTIONS (VULNERABILITY MANAGEMENT)
    ├── Defender Vulnerability Tiers (Core [MDE Plan 2], Add-on, Standalone)
    ├── Dashboards (Exposure Score, Configuration Score, Active Exploits, Active Breach)
    └── Remediation Loop (MDE Security Task -> Intune Remediation Ticket -> IT Patch Deploy -> Sensor auto-close)
```