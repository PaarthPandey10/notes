```
1. CAMPUS DESK REGISTRATION: SETUP, DISCOVERY & ONBOARDING
├── Big Picture: protecting the enterprise digital estate comes down to securing individual employee workstations, laptops, mobile phones, and cloud servers
│   ├── Microsoft Defender for Endpoint (an endpoint detection and response platform that helps security teams prevent, detect, investigate, and respond to advanced threats across enterprise devices)
│   │   └── Analogy: specialized security squad stationed directly at every desk and door of the digital campus to stop intruders at the device level
│   └── Frontline squad actions: locks down compromised endpoints, blocks suspicious software behaviors in milliseconds, conducts deep forensic crime-scene investigations, automatically repairs damaged operating system files before an attacker can steal sensitive corporate data
├── MDE setup entry point: Microsoft Defender Portal (https://security.microsoft.com/ > Settings > Endpoints)
│   └── Every computer and mobile device must be badged and registered with the central security service before the squad can guard it
├── Global Tenant Preferences (Configured by Security Administrator)
│   ├── Data Storage Location (United States, European Union, or United Kingdom)
│   │   ├── Specifies the geographic region hosting customer security telemetry
│   │   └── Permanent: cannot be changed after initial setup
│   ├── Data Retention (Default 6 months / 180 days)
│   │   └── Defines how long raw security logs remain searchable in the cloud
│   └── Enable Preview Features (toggles early access to upcoming product capabilities and dashboard features)
├── OS Sensor & Telemetry Communication
│   ├── Endpoints communicate with cloud analytics servers using embedded OS behavioral sensors
│   ├── LocalSystem Account (a high-privilege Windows system account used by OS services)
│   │   └── Sensors run under this account
│   └── WinHTTP (Windows HTTP Services, software interface enabling HTTP/HTTPS communication)
│       ├── Sensors transmit data using WinHTTP
│       ├── WinHTTP operates independently of browser proxy settings, so the sensor discovers proxy servers using auto-discovery methods
│       ├── Transparent Proxy (network setup redirecting web traffic without client configuration)
│       └── WPAD (web proxy autodiscovery protocol, automated protocol to locate proxy files)
│           └── Automated network protocol used by computers to locate proxy configuration files
├── Device Discovery (locates unmanaged equipment connected to corporate Wi-Fi or local subnets)
│   ├── Path: Settings > Endpoints > Device discovery OR Settings > Device discovery
│   ├── Basic Discovery (Passively listens using SenseNDR.exe binary)
│   │   └── Extracts basic device metadata without generating extra network traffic
│   └── Standard Discovery (Recommended; actively probes via multicast/smart probing protocols)
│       ├── Actively probes local networks using multicast queries and smart probing protocols
│       ├── Enriches device details; identifies unmanaged workstations, servers, printers, and IoT devices
│       ├── Can be configured to detect specific library vulnerabilities
│       └── Log4j2 (CVE-2021-44228, a critical remote code execution vulnerability detected by Standard Discovery)
│           └── Critical remote code execution vulnerability in a popular Java logging framework
├── Onboarding Status Categories (discovered endpoints appear in the Device Inventory under four categories)
│   ├── Onboarded (Monitored by MDE)
│   ├── Can be onboarded (Supported OS discovered but agent not enrolled)
│   ├── Unsupported (Discovered device running an incompatible OS)
│   └── Insufficient info (Requires additional standard discovery probing)
│       └── Needed to determine OS compatibility
├── Supported Platforms & Technologies (onboarding across major enterprise operating systems)
│   ├── Windows (Requires ESU [extended security updates] for legacy; supports AVD [Azure Virtual Desktop])
│   │   ├── ESU (extended security updates, paid security patches for legacy operating systems)
│   │   ├── AVD (Azure Virtual Desktop, a cloud-based desktop virtualization service)
│   │   ├── Client: Windows 7 SP1 (requires ESU), Windows 8.1, Windows 10, Windows 11
│   │   └── Server: Windows Server 2008 R2 SP1 (requires ESU), 2012 R2, 2016, 201803+, 2019, 2022, 2025
│   ├── macOS (Delivers EDR [endpoint detection and response]; deployed via Intune or Jamf; updated via MAU [Microsoft AutoUpdate])
│   │   ├── Delivers antivirus, EDR, and vulnerability management for the three latest released versions of macOS
│   │   ├── EDR (endpoint detection and response, tools that continuously monitor device behaviors to catch advanced attacks)
│   │   ├── Intune (Microsoft Intune, a cloud-based endpoint management service)
│   │   ├── Jamf (a third-party management platform for Apple devices)
│   │   └── MAU (Microsoft AutoUpdate, software that updates Microsoft applications on macOS)
│   ├── Linux (RHEL, CentOS, Ubuntu, SLES, Debian, Oracle Linux deployed via Puppet or Ansible)
│   │   ├── Six major Linux server distributions: RHEL 7.2+, CentOS 7.2+, Ubuntu 16 LTS+, SLES 12+, Debian 9+, Oracle Linux 7.2+
│   │   ├── Command-line controls
│   │   └── Puppet or Ansible (popular open-source software tools for automated IT configuration management)
│   ├── Android (Secures devices providing PUA [potentially unwanted application] scanning)
│   │   ├── Android 6.0+ devices across Work Profile and Device Administrator modes
│   │   ├── Provides web protection, anti-phishing, PUA scanning, and Conditional Access integration
│   │   └── PUA (potentially unwanted application, software that degrades system performance or displays unsolicited ads)
│   └── iOS
│       └── iOS 11.0+ supervised and unsupervised devices: web protection, anti-phishing, custom network indicators, jailbreak detection
├── Discovery-to-Deployment Workflow (Unmanaged Devices on Corporate Subnet -> Device Discovery Configuration [Basic vs Standard] -> Evaluate Onboarding Status -> Deploy Onboarding Package Method)
│   └── Deploy method options: Local Script (<= 10 hosts), Group Policy (Active Directory), Intune / Config Manager (Enterprise Cloud Fleet)
└── Windows Deployment Methods (path: Settings > Endpoints > Device management > Onboarding; five deployment options)
    ├── Local Script (Testing up to 10 devices)
    │   └── Uses a local batch package
    ├── Group Policy (Deploys scripts via GPO [Group Policy Object])
    │   ├── Deploys onboarding scripts across domain-joined devices
    │   └── GPO (Group Policy Object, a collection of settings that control computer configurations in Active Directory)
    ├── Microsoft Configuration Manager
    │   └── Deploys settings across enterprise fleets managed by Configuration Manager current branch or legacy versions
    ├── Mobile Device Management (Intune)
    │   └── Enrolls cloud-managed endpoints automatically via Intune device configuration profiles
    ├── VDI Onboarding Script (For non-persistent VDI [virtual desktop infrastructure])
    │   ├── Configures non-persistent VDI virtual machine pools
    │   └── VDI (virtual desktop infrastructure, virtualized desktop environments hosted on central servers)
    ├── Deployment Verification
    │   └── Detection test script in Command Prompt (powershell.exe -NoExit -ExecutionPolicy Bypass ...) triggers a synthetic test alert on the device page within minutes
    └── Offboarding Package (Automatically expires 30 days after generation)
        ├── Path: Settings > Endpoints > Device management > Offboarding
        ├── Used when a machine is decommissioned
        └── 30-day expiry prevents unauthorized agent removal

2. BADGING THE SQUAD: ACCESS CONTROL & DEVICE GROUPING
├── Purpose: manage who can inspect specific desk stations and execute containment commands
├── Unified Access Control 
│   ├── URBAC (unified role-based access control, a single centralized permission framework across Defender tools)
│   │   └── Starting February 16, 2025, new MDE tenants default exclusively to URBAC; existing tenants preserve legacy roles or map them into unified permissions
│   └── Legacy MDE RBAC (Global Administrator, Security Administrator, Security Reader)
│       ├── Global Entra ID roles grant default portal access
│       ├── Global Administrator and Security Administrator: full unrestricted access to all devices and management settings regardless of device group scopes
│       └── Security Reader: read-only access across alerts, devices, and vulnerability dashboards
├── Custom MDE Roles
│   ├── Path: Settings > Endpoints > Permissions > Roles > Turn on roles > + Add item
│   ├── Administrators assign granular permissions across functional areas
│   ├── View Data (Read access to security/vulnerability data)
│   │   └── Read access to security operations data or vulnerability management dashboards
│   ├── Active Remediation Actions (Approve/dismiss AIR, manage block lists)
│   │   └── Approve or dismiss pending AIR actions, manage allowed/blocked indicator lists, handle vulnerability exceptions, block vulnerable applications
│   ├── Alerts Investigation (Manage alerts, run AV, download PE [portable executable] files)
│   │   ├── Manage alert statuses, run antivirus scans, collect investigation packages, manage device tags, download PE files
│   │   └── PE (portable executable, standard Windows executable file formats like EXE and DLL)
│   ├── Manage Security Settings in Security Center (Configure suppression, emails, onboarding)
│   │   └── Configure alert suppression rules, automation folder exclusions, email notifications, and onboarding settings
│   ├── Manage Endpoint Security Settings in Microsoft Intune
│   │   └── Full access to Intune Endpoint Security policies and compliance configurations
│   └── Live Response Capabilities (Basic Commands, Advanced Commands)
│       ├── Basic Commands (starting a remote shell and running read-only inspection commands)
│       └── Advanced Commands (downloading PE/non-PE files, uploading files to the library, running remote PowerShell scripts)
├── Device Groups
│   ├── Path: Settings > Endpoints > Permissions > Device groups > + Add device group
│   ├── Purpose: restrict analyst visibility to specific computers
│   ├── Operational Functions (three critical functions)
│   │   ├── 1. Restrict visibility mapped to Entra ID (the cloud-based identity and access management service) user groups
│   │   │   └── Restricts alert and device visibility to specific Entra ID user groups assigned to matching RBAC roles
│   │   │       └── Entra ID (Microsoft Entra ID, the cloud-based identity and access management service)
│   │   ├── 2. Assign distinct automated remediation levels
│   │   │   └── During automated investigations
│   │   └── 3. Filter device lists and incident graphs
│   │       └── During security investigations
│   └── Configuration Parameters
│       ├── Group Name
│       ├── Automation Level (e.g., Full automation vs. Semi-automation)
│       ├── Matching Rules (dynamic; evaluating device name prefixes, domain names, custom device tags, or OS platforms)
│       ├── User Access Rights (assigned to specific Entra security groups)
│       ├── Rank (numeric)
│       └── Highest-ranked device group (Exclusive assignment if endpoint matches multiple groups)
└── Advanced Features Toggles (path: Settings > Endpoints > Advanced features; global feature switches)
    ├── Automated Investigation & Autoresolve Remediated Alerts
    │   ├── Automated Investigation (enables AIR playbooks to analyze alerts)
    │   └── Autoresolve Remediated Alerts (automatically closes alerts when AIR finds "No threats found" or "Remediated")
    ├── Live Response & Live Response for Servers
    │   └── Enables remote interactive command shell sessions on endpoints and server OS hosts
    ├── Live Response Unsigned Script Execution
    │   └── Permits executing unsigned PowerShell scripts from the tenant library
    ├── Always Remediate PUA
    │   └── Automatically cleans potentially unwanted software tenant-wide regardless of local AV
    ├── Enable EDR in Block Mode
    │   └── Blocks post-breach malicious artifacts even when third-party AV is primary active
    ├── Custom Network Indicators
    │   └── Enforces allow/block rules on custom IP, URL, and domain lists via Network Protection
    ├── Tamper Protection
    │   └── Locks Defender AV settings against local registry, script, or malware modification
    ├── Share Endpoint Alerts with Purview
    │   └── Forwards endpoint alerts to Purview for Insider Risk Management policy scoring
    └── Microsoft Intune Connection
        └── Shares device risk scores with Intune for device risk-based Conditional Access

3. HARDENING THE OFFICE DOORS: ASR & PROTECTION STACK
├── ASR (attack surface reduction, a set of software hardening capabilities that block common entry vectors used by malware)
│   ├── Analogy: physical reinforcement on office doors, windows, and mail slots
│   └── Stops threats before execution by constraining software behaviors commonly abused by attackers
├── 8 Core Security Components (the overall ASR protection stack)
│   ├── 1. ASR Rules (Intelligent rules that target software behaviors commonly abused by malware)
│   │   └── Examples: Office apps launching child processes or running obfuscated scripts
│   ├── 2. Hardware-Based Isolation
│   │   ├── VBS (virtualization-based security, hardware-assisted isolated memory environments)
│   │   │   └── Protects system integrity at startup
│   │   └── Application Guard (a container isolation feature that runs Microsoft Edge inside an isolated hypervisor)
│   │       └── Isolates web browsing sessions
│   ├── 3. Application Control
│   │   └── WDAC (Windows Defender Application Control, enterprise code integrity policies that restrict executable files)
│   │       └── Shifts from a default-trust model to an explicit trust model where applications must prove safety before running
│   ├── 4. Exploit Protection (Applies memory mitigations like DEP [data execution prevention] and ASLR [address space layout randomization])
│   │   ├── Applied to operating systems and individual apps
│   │   ├── DEP (data execution prevention)
│   │   ├── ASLR (address space layout randomization)
│   │   └── Functions alongside third-party antivirus
│   ├── 5. Network Protection
│   │   ├── Extends SmartScreen reputation filtering to all outbound network traffic and socket calls across non-Microsoft browsers
│   │   └── SmartScreen (Microsoft Defender SmartScreen, a cloud-based reputation service that blocks malicious websites)
│   │       └── Also blocks malicious downloads
│   ├── 6. Web Protection
│   │   └── Blocks access to phishing domains, untrusted web content, and web content categories defined by corporate policies
│   ├── 7. Controlled Folder Access
│   │   ├── Prevents unauthorized applications and file-encrypting ransomware from modifying files inside protected system folders (e.g., Documents, Pictures, Desktop)
│   │   └── Ransomware (extortion malware that encrypts files and demands payment for decryption keys)
│   └── 8. Device Control
│       └── Monitors and regulates access to physical media, such as USB drives, removable storage devices, and Bluetooth transfers
├── Core ASR Rules and Objectives (16 rules)
│   ├── Block executable content from email client and webmail
│   ├── Block all Office applications from creating child processes
│   ├── Block Office applications from creating executable content
│   ├── Block Office applications from injecting code into other processes
│   ├── Block JavaScript or VBScript from launching downloaded executables
│   ├── Block execution of potentially obfuscated scripts
│   ├── Block Win32 API calls from Office macros
│   ├── Use advanced protection against ransomware
│   ├── Block credential stealing from Windows LSASS (lsass.exe)
│   ├── Block process creations originating from PSExec and WMI commands
│   ├── Block untrusted and unsigned processes that run from USB
│   ├── Block executable files unless they meet prevalence or age criteria
│   ├── Block Office communication applications from creating child processes
│   ├── Block Adobe Reader from creating child processes
│   ├── Block persistence through WMI event subscription
│   └── Block abuse of exploited vulnerable signed drivers
├── ASR Operational Modes (four modes per individual rule)
│   ├── Disable (0) (Turns off rule)
│   │   └── Allows execution unchecked
│   ├── Block (1) (Prevents execution actively)
│   │   └── Blocks action instantly & raises alert
│   ├── Audit (2) (Logs behavior in Event Viewer without blocking)
│   │   ├── Evaluates and logs the behavior in Windows Event Viewer & portal dashboard
│   │   └── Lets admins assess line-of-business application impact before full deployment
│   └── Warn (6) (Blocks initially but allows end-user bypass)
│       └── Blocks the behavior initially but presents a notification allowing end users to bypass the restriction if necessary
└── ASR Deployment Channels (Windows 10/11 and Windows Server 2016 through 2025; five management channels)
    ├── Intune Endpoint Security Policy
    │   └── Path: Endpoint Security > Attack surface reduction > Create Policy
    ├── Intune Device Configuration Profiles
    │   └── Path: Device configuration > Profiles > Endpoint protection > Windows Defender Exploit Guard > Attack Surface Reduction
    ├── MDM CSP
    │   ├── OMA-URI (Open Mobile Alliance Uniform Resource Identifier, standardized path syntax)
    │   │   └── Standardized path syntax used in device management policies
    │   │       └── Path: ./Vendor/MSFT/Policy/Config/Defender/AttackSurfaceReductionRules
    │   └── GUIDs (globally unique identifiers)
    │       └── Rule GUIDs and state values passed together (e.g., GUID=1|GUID=2)
    ├── Group Policy
    │   └── Path: Computer Configuration > Administrative Templates > Windows Components > Microsoft Defender Antivirus > Windows Defender Exploit Guard > Attack surface reduction
    └── PowerShell (Using Set-MpPreference to overwrite or Add-MpPreference to append)
        ├── Set-MpPreference overwrites existing ASR rule lists
        ├── Add-MpPreference appends new rules and exclusions to existing configurations
        ├── Example (Rule 1 Block, Rule 2 Audit): Add-MpPreference -AttackSurfaceReductionRules_Ids 75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84, 3B576869-A4EC-4529-8536-B80A7769E899 -AttackSurfaceReductionRules_Actions Enabled, AuditMode
        ├── Example (file path exclusion): Add-MpPreference -AttackSurfaceReductionOnlyExclusions "C:\Program Files\LOBApp\app.exe"
        └── Logging: all ASR trigger events logged locally under Applications and Services Logs > Microsoft > Windows > Windows Defender / Exploit Guard

4. INSPECTING THE CRIME SCENE: DEVICE INVESTIGATION & BEHAVIORAL BLOCKING
├── Device Inventory Page (path: Assets > Devices; analysts review the compromised host)
│   ├── "Assume breach" mindset: MDE continuously streams behavioral cyber telemetry
│   │   ├── Kernel activities, memory operations, process trees, registry edits, network sockets, and file changes
│   │   └── Records retained for 6 months
│   └── Status Indicators
│       ├── Risk Level (High, Medium, Low, Informational based on active alerts)
│       │   └── Evaluates overall operational risk; resolving alerts or approving AIR remediations lowers device risk
│       ├── Exposure Level (High, Medium, Low based on vulnerabilities)
│       │   ├── Measures vulnerability impact based on pending security recommendations
│       │   └── Shows "No data available" if host inactive >30 days, runs an unsupported OS, or uses a stale agent
│       ├── Health State (Active, Inactive, Misconfigured [No sensor data, Impaired communications])
│       │   ├── Active (actively reporting)
│       │   ├── Inactive (stopped sending signals for >7 days)
│       │   └── Misconfigured (sub-classified as No sensor data or Impaired communications)
│       └── Antivirus Status (Disabled, Not reporting, Not updated)
├── Device Page Investigation Tabs (selecting an endpoint opens the Device Page; seven specialized tabs)
│   ├── Overview Tab & Alerts Tab
│   │   ├── Overview Tab: summary cards for Active Alerts (grouped by New/In progress across 30 days), Logged on Users (first/last seen and sign-in types), and Security Assessments
│   │   └── Alerts Tab: filtered queue displaying all alerts associated with the specific host
│   ├── Timeline Tab (Raw device events over 30 days)
│   │   ├── Chronological view of raw device events and alerts over the past 30 days
│   │   ├── Filter by date range, search keywords, or event groups
│   │   ├── Flag specific events to build a breach timeline
│   │   ├── 7 days CSV export limit
│   │   └── Hunt for related events (Instant pivot to Advanced Hunting)
│   │       └── Loads the event context into an Advanced Hunting query
│   ├── Security Recommendations Tab & Software Inventory Tab
│   │   ├── Security Recommendations: prioritized vulnerability fixes generated by Defender Vulnerability Management
│   │   └── Software Inventory: installed software, versions, and associated weakness counts
│   ├── Discovered Vulnerabilities Tab
│   │   ├── Lists specific CVE IDs impacting the host alongside CVSS ratings
│   │   ├── CVE (common vulnerabilities and exposures, a standardized public list of known weaknesses)
│   │   └── CVSS (common vulnerability scoring system, numeric rating system measuring severity)
│   │       └── Industry-standard numeric rating system measuring vulnerability severity
│   └── Missing KBs Tab
│       ├── Highlights missing Windows security patches and KB updates
│       └── KB (Knowledge Base, Microsoft software update documentation numbers)
└── Behavioral Blocking and Containment (stops fileless and polymorphic threats in real time)
    ├── Flow: Process Initiates Execution -> Client Behavioral Blocking (Defender AV) [Malicious -> Instant Local Block] -> Cloud Protection & ML Classifiers [Malicious -> Rapid Feedback-Loop Block] -> Post-Breach EDR In Block Mode [Malicious -> Remediates Running Artifact]
    ├── 1. Client Behavioral Blocking (Defender AV local monitoring -> ML Classifiers -> Instant Local Block)
    │   ├── Defender AV monitors process trees and in-memory behaviors locally, sending suspicious patterns to cloud machine learning classifiers
    │   ├── Within milliseconds, the cloud protection service evaluates the artifact and blocks execution
    │   └── Detections follow MITRE ATT&CK naming conventions (e.g., Behavior:Win32/CredentialAccess.*!ml or Behavior:Win32/LateralMovement.*!ml)
    ├── 2. Feedback-Loop Blocking (Rapid protection signature transmission globally)
    │   ├── Also known as rapid protection
    │   └── When an artifact is confirmed malicious on one endpoint, the cloud engine immediately transmits protection signatures tenant-wide and globally to block the threat on all other machines before execution begins
    └── 3. EDR in Block Mode (Extends behavioral containment post-breach even if Defender AV is in Passive Mode)
        ├── Enabled at Settings > Endpoints > Advanced features
        ├── Remediates malicious files, scripts, or memory artifacts detected by post-breach EDR capabilities
        ├── Works even if a third-party antivirus solution is active and Defender AV is running in Passive Mode
        └── Completed blocks are logged in the Action Center as Blocked or Prevented

5. FRONTLINE CONTAINMENT: RESPONSE ACTIONS & LIVE RESPONSE
├── Device Response Actions (executed from the top toolbar of the Device Page)
│   ├── Isolate Device (Supports Selective Isolation for Outlook/Teams)
│   │   ├── Disconnects host from network while preserving MDE cloud connectivity
│   │   ├── Selective Isolation enables Outlook, Teams, and Skype for Business
│   │   └── Supported on Windows, macOS, and Linux
│   ├── Restrict App Execution (WDAC policy permitting only MS-signed files)
│   │   ├── Applies a WDAC code integrity policy that permits only files signed by a Microsoft certificate to run
│   │   └── Supported on Windows 10 (1709+) and Windows 11
│   ├── Run Antivirus Scan (Quick/Full scan respects ScanAvgCPULoadFactor)
│   │   ├── Remotely triggers a Quick or Full scan
│   │   ├── ScanAvgCPULoadFactor (default 50% CPU limit)
│   │   └── Runs even if Defender AV is in Passive Mode
│   ├── Contain Device (Targets unmanaged/rogue devices)
│   │   └── Instructs surrounding managed endpoints to reject network connections from the contained device IP/MAC
│   ├── Collect Investigation Package (14 forensic folders)
│   │   └── Downloads a password-protected .zip archive containing 14 forensic folders (autoruns, prefetch, logs)
│   └── Initiate Live Response Session
│       └── Establishes a remote interactive command-line shell to run basic or advanced forensic commands
├── Forensic Investigation Package (14 Snapshots; password-protected .zip)
│   ├── Autoruns (ASEP [auto-start entry points, registry locations used by software to run at startup])
│   │   └── Registry entries across known ASEP locations to identify persistence
│   ├── Installed programs, Network connections, Prefetch files, Processes, Scheduled tasks
│   │   ├── Installed programs: CSV list of installed software compiled via Win32 product classes
│   │   ├── Network connections: ActiveNetConnections.txt (active TCP/IP connections), Arp.txt (ARP table), DnsCache.txt (DNS resolver cache), IpConfig.txt (TCP/IP adapter settings), FirewallExecutionLog.txt / pfirewall.log (firewall logs)
│   │   ├── Prefetch files: copy of Windows prefetch files from %SystemRoot%\Prefetch and file listing (PrefetchFilesList.txt) to track recently executed applications
│   │   ├── Processes: CSV list of running processes, parent process IDs, and memory states
│   │   └── Scheduled tasks: CSV dump of active scheduled tasks
│   ├── Security event log, Services, SMB sessions, System information, Temp directories
│   │   ├── Security event log: Windows Security Event Log (.evtx) recording authentication events
│   │   ├── Services: CSV dump of installed system services and startup configurations
│   │   ├── SMB sessions: inbound (SMBInboundSessions) and outbound (SMBOutboundSession) network file sharing connections
│   │   ├── System information: SystemInformation.txt detailing OS build, hotfixes, and hardware
│   │   └── Temp directories: text dumps listing dropped files in %Temp% across all user profiles
│   └── Users and groups, WdSupportLogs, CollectionSummaryReport.xls
│       ├── Users and groups: local user accounts and group memberships
│       ├── WdSupportLogs: Defender AV support logs (MpCmdRunLog.txt and MPSupportFiles.cab)
│       └── CollectionSummaryReport.xls: execution summary report detailing extracted data points, extraction commands, and error codes
└── Live Response Sessions (interactive remote shell connection to the endpoint)
    ├── Operational Constraints
    │   ├── Session Limits (5 mins inactivity timeout, 10 concurrent tenant-wide max)
    │   │   ├── An individual user can initiate only 1 session at a time
    │   │   └── An individual device can participate in only 1 session at a time
    │   ├── File Limits (3 GB getfile, 10 GB fileinfo, 250 MB library upload)
    │   │   ├── getfile: background download limit 3 GB
    │   │   ├── fileinfo: file metadata inspection limit 10 GB
    │   │   └── Library upload limit 250 MB
    │   └── Prerequisites (Automation Remediation level assigned, URBAC permissions, Unsigned Script toggle)
    │       ├── Enabled in Advanced features
    │       ├── Device assigned an Automation Remediation level (at least minimum level)
    │       ├── User provisioned with Security operations > Security data > Response (manage) in URBAC, or Standalone RBAC Live Response roles
    │       └── Unsigned PowerShell script execution requires enabling Live response unsigned script execution in Advanced features
    ├── Command Categories
    │   ├── Basic Commands (cd, cls, connect, connections, dir, drivers, fileinfo, findfile, help, persistence, processes, registry, scheduledtasks, services, trace, getfile)
    │   │   ├── Read-Only Forensic Inspection
    │   │   └── getfile <path>: downloads file in background; Ctrl+Z sends to background, fg <id> restores
    │   └── Advanced Commands (analyze, getfile -auto, run, library, putfile, remediate, undo)
    │       ├── Remediation & Script Execution
    │       ├── analyze: runs incrimination engines on file/PID
    │       ├── getfile -auto: auto-runs fileinfo prerequisite
    │       ├── run <script>: executes script from library
    │       ├── library: lists uploaded tenant library scripts
    │       ├── putfile <file>: uploads library file to device working directory (deleted on reboot by default)
    │       ├── remediate: remediates entity; supports -auto flag
    │       └── undo: restores remediated file or registry entity
    ├── Output handling
    │   ├── Piping to text files ([command] > [filename].txt)
    │   └── Format modifiers (-output json or -output table)
    └── Auditing
        └── Command Log (Records all executed shell commands)

6. DEEP EVIDENCE ANALYSIS: FILES, USERS, IPS & DOMAINS
├── Analysts navigate to dedicated profile pages for Files, User Accounts, IP Addresses, and Domains/URLs
├── 1. File Profile Page (selecting a file hash opens it)
│   ├── File Properties (SHA1, SHA256, MD5, VirusTotal detection ratio)
│   │   └── Also: file size, digital signer verification, Defender AV verdict, organizational/worldwide prevalence
│   ├── Page Tabs (four primary tabs)
│   │   ├── Alerts Tab
│   │   │   └── Filtered queue displaying alerts associated with the file
│   │   ├── Observed in Organization Tab (UI capped at 100 devices; full export via CSV)
│   │   │   └── Devices where the file was observed within a selected date range; exporting to CSV surfaces all impacted hosts
│   │   ├── Deep Analysis Tab (Cloud sandbox detonation for .exe / .dll files)
│   │   │   ├── Submits portable executable files (.exe or .dll) to a secure cloud hypervisor detonation chamber
│   │   │   ├── Executes the file, observes behavioral actions, generates a report detailing dropped files, registry edits, process injections, and contacted IP addresses
│   │   │   └── Sample submission requires registry key HKLM\SOFTWARE\Policies\Microsoft\Windows Advanced Threat Protection, AllowSampleCollection=1
│   │   └── File Names Tab
│   │       └── Lists all file names used by the binary across the organization
│   └── Toolbar Actions
│       ├── Stop and Quarantine File (Executes across up to 1,000 devices)
│       │   ├── Stops running processes, quarantines the binary, and deletes persistent registry keys
│       │   └── Requires Windows 10 1703+ or Windows 11, and Defender AV in Active or Passive mode
│       ├── Add Indicator
│       │   └── Creates a custom IoC to block or allow the file hash
│       └── Download File / Collect file
│           ├── Download File: password-protected .zip archive containing the sample
│           └── Collect file button appears if the file is not in the cloud backend (available if observed within the past 30 days)
├── 2. User Account Page
│   └── Properties (SAM account name, SID [security identifier, a unique alphanumeric code assigned to accounts], MFA [multi-factor authentication])
│       ├── Also: sign-in risk level, last sign-in timestamp, Active Directory group memberships
│       ├── SID (security identifier, a unique alphanumeric code assigned to user and group accounts in Windows)
│       └── MFA (multi-factor authentication, requiring multiple forms of verification) status
├── 3. IP Address Page (selecting an external IP address)
│   └── Properties (ASN [autonomous system number, unique identifier for network routing domains], Reverse DNS)
│       ├── ASN (autonomous system number, a unique identifier for internet network routing domains)
│       ├── Reverse DNS hostnames
│       ├── Organizational prevalence
│       └── Chronological list of endpoints that communicated with the IP
└── 4. Domain and URL Page (selecting a domain opens the URL Page)
    └── Properties (Registration contacts, nameservers, prevalence charts)
        ├── Prevalence charts spanning 1 day to 6 months
        └── Active alerts and observed device timelines

7. AUTOMATED ROBOTIC GUARDS: AIR & RISK-BASED CONDITIONAL ACCESS
├── AIR (automated investigation and remediation, automated software playbooks that analyze and clean up threats)
│   ├── Purpose: prevent alert fatigue
│   └── Uses inspection algorithms to evaluate alerts, build investigation graphs, and execute cleanup tasks automatically
├── Automation Levels (Assigned via Device Groups)
│   ├── Full - Remediate Threats Automatically
│   │   └── Executes all remediation actions automatically on malicious artifacts
│   ├── Semi - Require Approval for Any Remediation
│   │   └── Pauses all remediation actions and places them in the Pending tab awaiting human approval
│   ├── Semi - Require Approval for Core Folders Remediation
│   │   └── Auto-remediates non-core folders; requires explicit human approval for operating system directories (e.g., \windows*)
│   ├── Semi - Require Approval for Non-Temp Folders Remediation
│   │   └── Auto-remediates temporary directories (e.g., \temp*, \downloads*, \program files*); requires approval for files in all other system locations
│   └── No Automated Response (Disables AIR completely)
│       └── Disables AIR completely on targeted endpoints, significantly degrading security posture
├── Automation Exclusions & Uploads (tuned at Settings > Endpoints > Rules)
│   ├── Automation Uploads (File Content Analysis, Memory Content Analysis)
│   │   ├── File Content Analysis: automatically uploads suspicious files with specific extensions (e.g., exe,bat) to the cloud during AIR runs
│   │   └── Memory Content Analysis: permits uploading process memory content for deep inspection
│   └── Automation Folder Exclusions (Skips specific folders/extensions during investigation)
│       └── Configures specific folders, subfolders, file extensions, or file names to be skipped by AIR playbooks during automated investigations
└── Device Risk-Based Conditional Access (5-Step Workflow)
    ├── Integrates MDE with Intune to block compromised endpoints from corporate resources
    ├── Flow: Device Risk Level Rises to High -> MDE Transmits Risk Score to Intune -> Intune Compliance Policy Evaluates Host (Machine Risk Score Exceeds "Medium" Threshold) -> Intune Marks Device as Noncompliant -> Entra ID Conditional Access Policy Blocks Access
    ├── 1. Turn on Intune Connection in MDE (Advanced features)
    │   └── Toggle Microsoft Intune connection to On
    ├── 2. Enable MDE Integration in Intune (Endpoint security)
    │   └── Set Allow Microsoft Defender for Endpoint to enforce Endpoint Security Configurations to On
    ├── 3. Create Intune Compliance Policy
    │   ├── Setting: Require the device to be at or under the machine risk score
    │   └── Target Thresholds: Clear (zero threats), Low, Medium, High (least secure)
    │       ├── Clear: most secure; host must have zero active threats
    │       ├── Low: compliant if only low-severity threats exist
    │       ├── Medium: compliant if low or medium-severity threats exist
    │       └── High: least secure; allows all threat levels
    ├── 4. Assign Policy (To user or device groups)
    │   └── Assign to targeted user or device security groups
    └── 5. Create Entra ID Conditional Access Policy (Require device to be marked as compliant)
        └── Under Grant, select Require device to be marked as compliant

8. FINE-TUNING THE ALARMS: NOTIFICATIONS, SUPPRESSION & IOCS
├── Alert Email Notifications (Scopes by severity/device groups; Send test email required)
│   ├── Automated email alerts inform specific personnel immediately
│   ├── Notification rules define targeted device groups and minimum alert severities (High, Medium, Low, Informational)
│   └── Recipients must be validated using the Send test email button before saving
├── Alert Suppression Rules (Hides known innocuous alerts without disabling detection engines)
│   └── Built by analysts to eliminate false positives caused by legitimate tools; hides known innocuous alerts from the queue
└── IoCs (indicators of compromise, technical evidence indicating malicious activity)
    ├── Technical evidence such as file hashes, IP addresses, or certificates indicating malicious activity
    ├── Enforce explicit allow, audit, or block actions
    ├── Supported Detection Engines
    │   └── Cloud Detection Engine, Endpoint Prevention Engine, AIR Engine
    │       ├── MDE Cloud Detection Engine
    │       ├── Endpoint Prevention Engine (Defender AV)
    │       └── AIR Engine
    ├── IoC Types & Actions
    │   ├── Files (PE) (Allow, Audit, Warn, Block execution, Block & remediate)
    │   │   └── Prerequisites: Defender AV Cloud Protection enabled; Antimalware 4.18.1901+
    │   ├── IP Addresses, URLs / Domains (Allow, Audit, Warn, Block)
    │   │   ├── No CIDR notation; single IPs and FQDNs only
    │   │   └── Prerequisites: Network Protection in Block Mode; Antimalware 4.18.1906+
    │   └── Certificates (Allow, Block - requires CER or PEM extensions)
    │       ├── Leaf certificates chained to trusted Root CA
    │       └── Prerequisites: Defender AV Cloud Protection enabled; CER/PEM files
    └── Management Constraints
        ├── Tenant Limit (15,000 indicators)
        ├── CSV Import format (indicatorType, indicatorValue, action, title, expirationTime, severity, rbacGroupNames, category, MITRE techniques)
        │   └── Indicators can be uploaded in bulk using a CSV file
        └── Propagation Time (Files = minutes; URLs/IPs/Certs = 2 to 3 hours to propagate)
            └── Propagation across all endpoint sensors

9. PROACTIVE CAMPUS INSPECTIONS: VULNERABILITY MANAGEMENT
├── Microsoft Defender Vulnerability Management (continuously discovers vulnerabilities and software misconfigurations in real time using the built-in, agentless MDE sensor)
├── Microsoft Defender Vulnerability Management Tiers (three capability tiers)
│   ├── Core Vulnerability Management (Included in MDE Plan 2)
│   │   └── Included natively in Microsoft Defender for Endpoint Plan 2
│   ├── Vulnerability Management Add-on (Security baselines, Block vulnerable applications, Cert/Network analysis)
│   │   ├── Optional add-on for MDE Plan 2
│   │   ├── Security baselines assessment profiles
│   │   ├── Block vulnerable applications
│   │   ├── Browser extensions assessment
│   │   ├── Digital certificate assessment
│   │   └── Network share analysis
│   └── Vulnerability Management Standalone (For un-agented environments)
│       └── Full standalone vulnerability management platform
├── Vulnerability Portal Blades (five primary portal blades)
│   ├── Dashboard
│   │   ├── Exposure Score (A metric reflecting overall organizational vulnerability risk)
│   │   │   └── Based on security recommendations
│   │   └── Configuration Score (Measuring security benchmark compliance)
│   ├── Recommendations
│   │   ├── Prioritizes security fixes by fusing vulnerability data with dynamic threat context
│   │   ├── Active Exploits in the Wild (Weaponized CVEs)
│   │   │   └── Prioritizes CVEs currently being weaponized
│   │   ├── Active Breach Correlation (Cross-referenced with EDR)
│   │   │   └── Cross-references EDR detections to highlight vulnerabilities being exploited inside the organization
│   │   └── High-Value Assets
│   │       └── Prioritizes exposed devices carrying confidential data
│   ├── Remediation, Inventories, Weaknesses
│   │   ├── Remediation: tracks active remediation tasks, ticket statuses, and completion progress
│   │   ├── Inventories: installed software, weakness counts, certificates, and network shares
│   │   └── Weaknesses: specific CVE IDs, severity ratings, and CVSS scores
└── Remediation Workflow
    ├── 1. Select recommendation -> "Remediation options"
    │   ├── Security Admin / security analyst selects a recommendation in the MDE Recommendations tab and clicks Remediation options
    │   └── Selects the target IT service management tool: Intune for AAD joined machines or ServiceNow
    ├── 2. Submit form -> creates Security Task (in MDE) & Remediation Ticket (in Intune/ServiceNow)
    │   ├── Request form defines scope, target device groups, priority, and due date
    │   └── Submitting provisions a Security Task in MDE and an actionable Remediation Ticket inside Intune
    ├── 3. IT Admin deploys patch via Intune
    │   └── IT Admin approves ticket in Intune and deploys software update/patch
    └── 4. Real-time MDE sensor detects update and auto-closes task
        └── Remediation task and exposure score update automatically in real time

MASTER MAP: MITIGATING THREATS WITH MICROSOFT DEFENDER FOR ENDPOINT
├── 1. CAMPUS DESK REGISTRATION (SETUP & ONBOARDING)
│   ├── Microsoft Defender for Endpoint (an endpoint detection and response platform that helps security teams prevent, detect, investigate, and respond to advanced threats across enterprise devices)
│   │   └── Specialized security squad at every desk and door (workstations, laptops, mobile phones, cloud servers)
│   ├── Configuration (Security Administrator sets Geo Location, 6-Month Retention, Preview features)
│   │   └── Portal: https://security.microsoft.com/ > Settings > Endpoints; Data Storage Location (US, EU, UK) is permanent
│   ├── Telemetry Mechanics (LocalSystem Account, WinHTTP, WPAD, Transparent Proxy)
│   ├── Device Discovery
│   │   ├── Basic Discovery (SenseNDR.exe passive listening)
│   │   └── Standard Discovery (Active multicast probing, detects Log4j2 CVE-2021-44228)
│   ├── Onboarding Status (Onboarded, Can be onboarded, Unsupported, Insufficient info)
│   └── Onboarding Deployment
│       ├── Windows (Local Script [up to 10 devices], GPO, Configuration Manager, Intune, VDI)
│       │   └── Offboarding package expires 30 days after generation; deployment verified by detection test script
│       └── Cross-Platform (macOS via Jamf/MAU, Linux via Puppet/Ansible, iOS, Android PUA)
│           └── Windows (ESU, AVD), macOS (three latest versions), Linux (RHEL, CentOS, Ubuntu, SLES, Debian, Oracle Linux), Android 6.0+, iOS 11.0+
├── 2. BADGING THE SQUAD (ACCESS CONTROL & DEVICE GROUPS)
│   ├── URBAC (Unified Role-Based Access Control)
│   │   └── Default for new MDE tenants from February 16, 2025; legacy MDE RBAC (Global Administrator, Security Administrator, Security Reader)
│   ├── Custom Roles (View Data, Active Remediation, Alerts Inv., Live Response)
│   │   └── Also: Manage Security Settings in Security Center, Manage Endpoint Security Settings in Microsoft Intune; PE (portable executable) downloads
│   ├── Device Groups (Filters devices by Entra ID groups, sets Automation Levels, exclusive Highest-Rank assignment)
│   │   └── Matching Rules (name prefixes, domains, tags, OS platform) + Rank
│   └── Advanced Features (Automated Investigation, Live Response Unsigned Scripts, EDR in Block Mode, Tamper Protection)
│       └── Also: Autoresolve Remediated Alerts, Always Remediate PUA, Custom Network Indicators, Share Endpoint Alerts with Purview, Microsoft Intune Connection
├── 3. HARDENING THE OFFICE DOORS (ATTACK SURFACE REDUCTION)
│   ├── ASR (attack surface reduction, a set of software hardening capabilities that block common entry vectors used by malware)
│   ├── 8 Core Security Components
│   │   ├── ASR Rules, Hardware Isolation (VBS, Application Guard)
│   │   ├── Application Control (WDAC), Exploit Protection (DEP, ASLR)
│   │   └── Network/Web Protection (SmartScreen), Controlled Folder Access (Ransomware), Device Control
│   ├── ASR Operational Modes (Disable [0], Block [1], Audit [2], Warn [6])
│   └── Deployment (Intune, MDM CSP via OMA-URI/GUIDs, Group Policy, PowerShell Add-MpPreference)
│       └── Workstations hardened against intrusion via Intune, MDM CSPs, or PowerShell; Set-MpPreference overwrites vs Add-MpPreference appends
├── 4. INSPECTING THE CRIME SCENE (DEVICE INVESTIGATION & BEHAVIORAL BLOCKING)
│   ├── Device Inventory (Risk Level, Exposure Level, Health State, AV Status)
│   │   └── Path: Assets > Devices; behavioral cyber telemetry retained 6 months
│   ├── Device Page Tabs (Overview, Alerts, Timeline [7 days CSV], Discovered Vulnerabilities [CVE/CVSS])
│   │   └── Also: Security Recommendations, Software Inventory, Missing KBs (KB); Device Timeline and Behavioral Cyber Telemetry
│   └── Behavioral Containment
│       └── (1. Client Behavioral Blocking -> 2. Feedback-Loop Blocking -> 3. EDR in Block Mode)
│           └── Contain malicious behaviors in real time when malware bypasses pre-execution locks
├── 5. FRONTLINE CONTAINMENT (RESPONSE ACTIONS & LIVE RESPONSE)
│   ├── Response Actions (Isolate Device, Restrict App Execution, Run AV, Contain Device)
│   │   └── Device Isolation (with selective Outlook/Teams communications); Contain Device for unmanaged rogue hosts
│   ├── Investigation Package (14 folders including Autoruns [ASEP], Prefetch, Services, WdSupportLogs)
│   └── Live Response Sessions
│       ├── Constraints (5 mins timeout, 10 concurrent, 3 GB getfile, 250 MB upload)
│       └── Commands (Basic read-only vs. Advanced script execution and analyze/remediate)
│           └── Remote shell session; Command Log audit
├── 6. DEEP EVIDENCE ANALYSIS (ARTIFACT PROFILES)
│   ├── File Profile Page (SHA256, VirusTotal, Deep Analysis detonation, Stop & Quarantine up to 1,000 devices)
│   │   └── Deep Analysis runs cloud sandbox on .exe/.dll binaries
│   ├── User Account Page (SID, MFA status)
│   ├── IP Address Page (ASN tracking)
│   └── Domain and URL Page
├── 7. AUTOMATED ROBOTIC GUARDS (AIR & CONDITIONAL ACCESS)
│   ├── AIR Automation Levels (Full, Semi-Core Folders, Semi-Non-Temp Folders, No Response)
│   │   └── High-volume alerts trigger AIR playbooks under defined Automation Levels; also Semi - Require Approval for Any Remediation
│   ├── Automation Adjustments (Folder Exclusions, File/Memory Content Uploads)
│   └── Risk-Based Conditional Access (Intune connection -> Intune Compliance Policy -> Entra ID Block)
│       └── Unhealthy endpoints blocked from corporate apps using Intune Compliance Policies and Entra ID Conditional Access
├── 8. FINE-TUNING THE ALARMS (NOTIFICATIONS, SUPPRESSION, IOCS)
│   ├── Tuning Controls (Email Notifications, Alert Suppression Rules)
│   │   └── SOC teams eliminate false positives using Alert Suppression Rules and send targeted Email Notifications
│   └── IoCs (Indicators of compromise)
│       ├── Types (Files, IPs, URLs, Certificates)
│       └── Management (Max 15,000, 2-3 hour propagation, CSV imports)
│           └── Detection engines: Cloud Detection Engine, Endpoint Prevention Engine, AIR Engine
├── 9. PROACTIVE CAMPUS INSPECTIONS (VULNERABILITY MANAGEMENT)
│   ├── Defender Vulnerability Tiers (Core [MDE Plan 2], Add-on, Standalone)
│   ├── Dashboards (Exposure Score, Configuration Score, Active Exploits, Active Breach)
│   └── Remediation Loop (MDE Security Task -> Intune Remediation Ticket -> IT Patch Deploy -> Sensor auto-close)
│       └── Prioritized Remediation Tasks routed to Intune IT administrators to close security gaps before intruders can exploit them
└── END STATE (connecting the dots)
    └── Every component snaps together into a unified frontline defense system, ensuring absolute device-level security across the entire enterprise campus
```