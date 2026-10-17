```
1. THE BIG PICTURE & THE DIRECTOR'S THREE CORE ROLES
├── Scenario: enterprise expands across public clouds and private datacenters
│   └── Role analogy: Global Security Director managing a sprawling multi-cloud real estate empire (no longer a single office building)
├── The Empire Footprint
│   ├── Headquarters (Azure) (company-owned)
│   ├── Leased Warehouses (AWS) (AWS: Amazon Web Services, a third-party public cloud platform)
│   └── Overseas Pop-ups (GCP) (GCP: Google Cloud Platform, a third-party public cloud platform)
├── MDC (Microsoft Defender for Cloud, a centralized cloud-native application protection platform that provides posture management and workload protection)
│   ├── Deployed by the director to oversee the vast property empire
│   ├── Operated through the Azure portal (global control system / Central Security Directorate MDC Portal)
│   └── Continuous functions
│       ├── Evaluates cloud configurations
│       ├── Discovers unmonitored facilities across competing cloud providers
│       ├── Maps out hidden attack routes that burglars could exploit
│       └── Deploys real-time threat protection to defend digital assets across the globe
├── The Director's Three Responsibilities (checking blueprints before building, inspecting finished buildings, posting armed guards in high-value rooms like server vaults and file archives)
│   ├── DevSecOps (development security operations, the process of checking application blueprints, source code repositories, and construction templates for security flaws long before a building is physically built)
│   │   ├── Analogy: Blueprint Checks
│   │   ├── Inspects developer code, container images, and IaC definitions to catch vulnerabilities early in the construction lifecycle
│   │   └── IaC (infrastructure as code, machine-readable definition files like Terraform or ARM templates used to build cloud environments)
│   ├── CSPM (cloud security posture management, the process of physically inspecting finished buildings to ensure doors are locked and alarms are wired correctly)
│   │   ├── Analogy: Building Inspections
│   │   └── Continually compares active cloud resources against security best practices; highlights misconfigurations, measures overall compliance, surfaces actionable hardening steps
│   └── CWPP (cloud workload protection platform, the process of stationing specialized armed guards inside specific rooms like server vaults, databases, and container clusters)
│       ├── Analogy: Specialized Armed Guards
│       └── Delivers real-time threat detection and behavioral analytics to block active attacks targeting running workloads
├── Enabling Defender Plans (funding and activating capabilities)
│   ├── Required roles: Subscription Owner, Subscription Contributor, or Security Admin
│   ├── Path: Microsoft Defender for Cloud > Environment settings (Azure portal)
│   ├── Select the target subscription or workspace
│   ├── Select Enable all Microsoft Defender plans (or toggle individual plans on or off)
│   └── Save
├── Posture Management Capability Levels
│   ├── Foundational CSPM (Free)
│   │   ├── Enabled automatically at no extra cost across all registered subscriptions
│   │   ├── Asset Discovery (basic)
│   │   ├── Continuous Assessment (continuous configuration assessments)
│   │   ├── MCSB Recommendations (security recommendations)
│   │   ├── Basic Secure Score (centralized security rating based on the MCSB)
│   │   └── MCSB (Microsoft Cloud Security Benchmark, a master set of cloud security guidelines based on industry frameworks like CIS and NIST)
│   └── Defender CSPM (Billable; paid plan with advanced posture capabilities)
│       ├── Agentless Vulnerability Scanning for virtual machines
│       ├── Data-aware security posture insights
│       ├── Secret & Data Scanning (agentless secret scanning)
│       ├── Attack Path Analysis (interactive)
│       └── Cloud Security Graph (an interconnected graph database mapping relationships between cloud assets, permissions, and network exposures)
└── Automatic Protection & Shadow IT Discovery
    ├── Azure Policy (an Azure governance service that enforces default security standards and audits compliance across cloud resources)
    │   └── MDC evaluates tenant subscriptions against built-in initiative assignments managed through Azure Policy so newly created subscriptions are automatically protected
    └── Reviewing subscriptions labeled "not covered" on the MDC dashboard
        └── Shadow IT (unmapped or unmanaged cloud subscriptions created without central IT approval); apply standard governance rules immediately

2. EMPIRE INFRASTRUCTURE REGISTRATION: ASSET INVENTORY & AUTO PROVISIONING
├── Asset Inventory (path: Microsoft Defender for Cloud > Inventory; tracks every connected building, server, and storage vault)
│   ├── Single pane of glass powered by ARG to evaluate security posture across connected environments
│   │   └── ARG (Azure Resource Graph, an Azure service that allows querying resource property data across subscriptions using KQL)
│   ├── Top Summary Banner (three primary operational metrics)
│   │   ├── Total Resources (cumulative count of all resources connected to MDC across cloud environments)
│   │   ├── Unhealthy Resources (resources containing active, unresolved security recommendations)
│   │   └── Unmonitored Resources (resources experiencing agent communication failures, e.g., monitoring agents installed but failing to transmit heartbeat signals)
│   ├── Inventory Filters
│   │   ├── Filter by subscription, resource group, resource type, tags, or security finding criteria
│   │   │   └── e.g., searching for specific CVE IDs (CVE: common vulnerabilities and exposures, a standardized public list of known cybersecurity weaknesses)
│   │   └── Defender for Cloud status filter
│   │       ├── On (fully protected by an active Defender plan)
│   │       ├── Off (unprotected by paid Defender plans)
│   │       └── Partial (subscriptions where some, but not all, Defender plans are enabled)
│   └── Inventory Toolbar Actions
│       ├── Assign corporate metadata tags to filtered resources
│       ├── Onboard external hosts via the Add non-Azure servers button
│       ├── Export query results to a CSV file
│       ├── Open the underlying KQL statement inside Azure Resource Graph Explorer
│       │   └── KQL (Kusto Query Language, a database query language used to search raw telemetry and logs)
│       └── Trigger an Azure Logic App to run custom remediation routines on selected assets
│           └── Azure Logic App (a cloud workflow automation service that executes automated response actions when triggered)
├── Auto Provisioning (path: Environment settings > Settings & monitoring)
│   ├── Purpose: maintain continuous coverage without manual intervention
│   ├── Automatically deploys required monitoring agents and extensions to existing and newly created compute resources using Azure Policy DeployIfNotExists rules
│   └── Supported Components
│       ├── Azure Monitor Agent (AMA)
│       │   ├── Modern software agent that collects security logs and OS events from virtual machines via DCRs
│       │   ├── DCRs (Data Collection Rules, policy objects defining what data to collect and which Log Analytics workspace to send it to)
│       │   └── Replaces MMA (Microsoft Monitoring Agent, the legacy Log Analytics agent; retired)
│       ├── Microsoft Defender for Endpoint (MDE)
│       │   └── Automatically deploys the MDE sensor extension to Windows and Linux virtual machines when a Defender for Servers plan is active
│       ├── Vulnerability Assessment for Machines
│       │   └── Deploys agentless or integrated vulnerability scanning engines to compute instances
│       ├── Guest Configuration Agent
│       │   └── Deploys the Azure Policy Guest Configuration extension to evaluate OS-level security settings and compliance baselines
│       ├── Agentless Scanning for Machines
│       │   └── Background disk snapshot scanning to inspect software inventories, vulnerabilities, and plain-text secrets without installing endpoint agents
│       └── Defender Sensor
│           └── Deploys the container security profile to Kubernetes worker nodes to collect runtime security events
├── Direct Onboarding with MDE (for non-Azure servers; path: Environment settings > Direct onboarding)
│   ├── Switch direct onboarding to On and select a target Azure subscription
│   ├── On-premises and multi-cloud servers connect to MDC directly through the MDE agent WITHOUT deploying Azure Arc
│   ├── Provides Defender for Servers Plan 1 capabilities (licensing, billing, vulnerability data, and EDR alerts)
│   ├── Takes up to 24 hours to appear in the inventory portal
│   └── Limitation: advanced management (Azure Policy guest configurations, extension management, Defender for Servers Plan 2 features) still requires full Azure Arc registration
├── Manual AMA Deployment via Data Collection Rule
│   ├── Path: Azure Monitor > Data Collection Rules > Create
│   ├── Rule basics: Name, Subscription, RG (resource group), Region, Platform Type
│   ├── Resources tab: select target VMs (automatically installs the AMA extension on associated machines)
│   ├── Collect and deliver tab: configure data sources
│   │   ├── Windows Events
│   │   ├── Performance Counters
│   │   ├── Syslog
│   │   ├── IIS Logs
│   │   └── Text Logs
│   ├── Route data to a designated Log Analytics workspace
│   ├── Programmatic deployment
│   │   ├── Azure PowerShell: New-AzDataCollectionRuleAssociation -ResourceUri "/subscriptions/sub-id/resourceGroups/rg-id/providers/Microsoft.Compute/virtualMachines/VM-Prod-01" -RuleId "/subscriptions/sub-id/resourceGroups/rg-id/providers/Microsoft.Insight/dataCollectionRules/DCR-Security-Logs"
│   │   └── Azure CLI: az monitor data-collection rule association create --resource "<VM resource ID>" --rule-id "<DCR resource ID>" --name "VM-Prod-01-DCR-Assoc"
│   └── Verification: query the Heartbeat table in the target Log Analytics workspace
└── Security Event Collection & Microsoft Sentinel
    ├── Microsoft Sentinel (Microsoft's cloud-native SIEM platform)
    ├── Rule: if the workspace is integrated with Sentinel, configure security event collection in EITHER MDC or Sentinel, never both simultaneously (prevents duplicate event ingestion)
    └── Collection tiers
        ├── Minimal (collects low-volume breach indicators like logon event IDs 4624 and 4625)
        ├── Common (collects full user audit trails including sign-outs 4634 and group changes)
        └── All Events

3. BRINGING OUTSIDE WAREHOUSES INTO THE CONTROL ROOM: MULTI-CLOUD ONBOARDING
├── Multi-Cloud Onboarding Architecture (specialized connectors and bridge software)
│   ├── Hybrid / On-Premises: Azure Arc Bridge (Connected Machine Agent) -> projects non-Azure hosts into ARM with Resource IDs and Azure Policy
│   ├── Amazon Web Services (AWS): AWS Native Connector (AWS CloudFormation Stack) -> integrates AWS Security Hub & IAM Role: CspmMonitorAws
│   └── Google Cloud Platform (GCP): GCP Native Connector (GCloud Script / Shell) -> integrates GCP Security Command Center & Workload Identity Providers
├── 1. Hybrid and On-Premises Machines via Azure Arc
│   ├── Azure Arc (a management service that extends Azure security, policy, and monitoring tools to non-Azure, on-premises, and multi-cloud servers)
│   │   └── Acts as a software bridge
│   ├── Azure Connected Machine Agent
│   │   ├── Installed on an external Windows or Linux server
│   │   └── Projects the machine into ARM as a native Azure resource
│   ├── ARM (Azure Resource Manager, the management layer used to deploy, update, and manage Azure resources)
│   │   └── Each connected machine receives an Azure Resource ID, belongs to a resource group, and accepts Azure Policy definitions, tags, and extensions
│   ├── Onboarding Channels
│   │   ├── Interactive Deployment (generate an installation script in the Azure portal and execute on individual machines)
│   │   ├── At-Scale Deployment
│   │   │   ├── Service Principal (an automated service identity in Microsoft Entra ID used by deployment scripts) onboards machines non-interactively
│   │   │   └── Via Group Policy, Microsoft Configuration Manager task sequences, or PowerShell
│   │   └── Virtualization Fabric Onboarding (onboard VMware vSphere or System Center Virtual Machine Manager [SCVMM] management servers directly to discover and enroll virtual machines at scale)
│   └── Disconnection Rule: if an Arc-enabled server becomes disconnected or expires, MDC automatically purges the stale Azure Arc entity after 7 days to keep inventory views clean
├── 2. Onboarding Amazon Web Services (AWS) Accounts
│   ├── AWS Security Hub (Amazon's central security finding aggregator) integrates directly into MDC
│   ├── Path: Environment settings > Add environment > Amazon Web Services
│   └── Four Operational Steps
│       ├── 1. Account Details
│       │   ├── Define a connector name
│       │   ├── Scope: Single account vs. Management account (autodiscovers all member accounts in an AWS Organization)
│       │   ├── Select target Azure subscription and location
│       │   ├── Specify AWS Account IDs and select monitored AWS regions
│       │   └── Environment scan interval: 4, 6, 12, or 24 hours
│       ├── 2. Select Defender Plans
│       │   ├── Foundational CSPM, Defender CSPM, Servers
│       │   ├── Containers (for Amazon EKS clusters)
│       │   ├── Databases (for EC2 SQL and RDS instances)
│       │   └── Read-only API calls executed by the CspmMonitorAws role are logged in AWS CloudTrail; to optimize CloudTrail export costs in external SIEMs, filter out read-only calls from arn:aws:iam::[accountId]:role/CspmMonitorAws
│       ├── 3. Configure Access
│       │   ├── Permissions type: Default access (automatic adoption of future capabilities) vs. Least privilege access (current required roles only)
│       │   ├── Deployment method: AWS CloudFormation or Terraform
│       │   ├── Running the generated CloudFormation template creates required IAM roles (CspmMonitorAws) in the AWS account
│       │   └── Management Account onboarding: template must be executed as both a CloudFormation Stack and StackSet
│       └── 4. Review and Generate
│           ├── Validate parameters and generate the security connector
│           └── Scan data and recommendations populate within a few hours
└── 3. Onboarding Google Cloud Platform (GCP) Projects
    ├── GCP Security Command Center integrates into MDC
    ├── Path: Environment settings > Add environment > Google Cloud Platform
    └── Four Operational Steps
        ├── 1. Project Details
        │   ├── Define a connector name
        │   ├── Scope: Single project vs. Organization (onboards all underlying GCP projects; optional project/folder exclusion lists)
        │   ├── Assign subscription and location
        │   ├── Enter GCP Project Number, GCP Project ID, and Organization ID
        │   └── Scan interval: 4, 6, 12, or 24 hours
        ├── 2. Select Plans
        │   └── Foundational CSPM, Defender CSPM, Servers, Databases, and Containers
        ├── 3. Configure Access
        │   ├── Select permission types
        │   ├── Deployment method: GCloud Shell script or Terraform
        │   └── Executing the generated GCloud script in GCP Cloud Shell provisions: a Workload Identity Pool, a Workload Identity Provider (per plan), dedicated Service Accounts, and project-level policy bindings
        └── 4. Review and Generate
            ├── Validate and create the security connector
            ├── Mandatory Google APIs (five): iam.googleapis.com, sts.googleapis.com, cloudresourcemanager.googleapis.com, iamcredentials.googleapis.com, compute.googleapis.com
            └── Initial recommendations appear in MDC within 6 hours

4. INSPECTING FINISHED BUILDINGS: SECURE SCORE, MCSB & REGULATORY COMPLIANCE
├── Secure Score (path: Microsoft Defender for Cloud > Security posture)
│   ├── Calculation Chain
│   │   ├── Security Policy (Azure Policy Definition)
│   │   ├── Combined into Security Initiative (e.g., MCSB)
│   │   ├── Evaluates Resources and Triggers Security Recommendations
│   │   └── Recommendations Grouped into Security Controls
│   │       └── CONTROL SCORE = Max Points ONLY when ALL resources comply with ALL recommendations in that control
│   ├── Security Policy (an Azure Policy definition specifying precise security conditions that must be controlled, e.g., requiring storage accounts to restrict public network access)
│   ├── Security Initiative (a logical grouping of Azure Policy definitions targeted toward a unified security goal)
│   │   ├── Default built-in initiative assigned automatically to every subscription registered in MDC: Microsoft Cloud Security Benchmark (MCSB)
│   │   ├── MCSB provides cloud-centric security guidelines derived from CIS (Center for Internet Security) and NIST (National Institute of Standards and Technology) frameworks
│   ├── Security Recommendation (actionable hardening tasks generated when resources fail an underlying policy check within an assigned initiative)
│   ├── Security Control (a logical grouping of related security recommendations reflecting a specific vulnerable attack surface)
│   │   └── Examples: Enable MFA, Secure management ports, Apply system updates
│   └── Scoring Rules
│       ├── Each security control carries a max point value
│       ├── Secure Score is calculated as a percentage
│       ├── Points earned ONLY when every single resource within the scope complies with ALL recommendations in that specific control
│       └── Resolving partial recommendations within a control yields zero point increase until the entire control is fully remediated
├── Regulatory Compliance Management
│   ├── Regulatory Compliance Dashboard (path: Microsoft Defender for Cloud > Regulatory compliance)
│   │   └── Each regulatory standard is modeled as an Azure Policy initiative mapped to specific regulatory controls
│   ├── Supported Regulatory Standards
│   │   ├── PCI-DSS v3.2.1 (Payment Card Industry Data Security Standard)
│   │   ├── ISO 27001:2013 (International information security management standard)
│   │   ├── NIST SP 800-53 R4 / R5 and NIST SP 800-171 R2 (US federal security frameworks)
│   │   ├── SOC TSP (Service Organization Control Trust Services Criteria)
│   │   ├── HIPAA / HITRUST (Healthcare data privacy standards)
│   │   ├── FedRAMP High / Moderate (US Federal Risk and Authorization Management Program)
│   │   ├── CMMC Level 3 (Cybersecurity Maturity Model Certification)
│   │   └── Azure CIS 1.1.0 / 1.3.0 and SWIFT CSP CSCF-v2020
│   └── Adding a Standard
│       ├── Required permissions: Subscription Owner or Policy Contributor
│       ├── Open Regulatory Compliance page > Manage compliance policies
│       ├── Expand Industry & regulatory standards > Add more standards
│       ├── Search for the required regulation
│       └── Assign the initiative to the target subscription or management group scope
└── Visualizing Posture with Azure Workbooks (path: Microsoft Defender for Cloud > Workbooks)
    ├── Tracks posture trends over time and presents executive reports; integrates natively with Azure Workbooks
    └── Pre-built interactive templates (Workbooks gallery)
        ├── Secure Score Over Time (tracks subscription score changes and recommendation resolution progress over custom date ranges)
        ├── System Updates (missing OS security patches categorized by severity and host)
        ├── Vulnerability Assessment Findings (vulnerability scan outputs across compute instances and container registries)
        ├── Compliance Over Time (regulatory compliance passing rates over time)
        └── Active Alerts (security alerts by severity, MITRE ATT&CK tactics, tags, and geographic location)

5. THINKING LIKE A BURGLAR: CLOUD SECURITY GRAPH, ATTACK PATHS & CLOUD SECURITY EXPLORER
├── Problem: standard compliance lists evaluate resources in isolation (e.g., VM missing a patch, storage account allowing public access); attackers combine isolated weaknesses into complex multi-hop attack chains
├── Cloud Security Graph (available with the Defender CSPM plan)
│   ├── Interactive graph database
│   └── Ingests multi-cloud telemetry: compute configurations, network topologies, identity permissions, internet exposure points, and vulnerability scan outputs to model complex relationships between assets
├── Attack Path Analysis (path: Cloud Security > Attack path analysis)
│   ├── Uses the graph engine to automatically simulate adversary techniques
│   ├── Surfaces high-risk paths leading to critical assets (e.g., domain controllers, database servers, key vaults)
│   ├── Each attack path displays an interactive visual map detailing: entry points, intermediate hops, exploit techniques, affected resources, and a Target Risk Score
│   ├── Choke-point remediation: resolving a recommendation on a single node along the path breaks the entire attack chain and neutralizes the threat
│   └── Example Attack Chain
│       ├── Internet Exposure Point
│       ├── Vulnerable Web Application (CVE-2023-XXXX)
│       ├── (Over-Privileged Service Principal)
│       ├── Key Vault Instance Containing Production Secrets
│       ├── (Extracted Administrative Credentials)
│       └── Production Database Vault (Data Exfiltration)
└── Cloud Security Explorer (path: Cloud Security > Cloud Security Explorer)
    ├── Visual query builder that translates natural language or dropdown selections into graph queries against the underlying database
    └── Example targeted queries
        ├── "Show all internet-exposed Virtual Machines possessing high-severity vulnerabilities and elevated identity permissions"
        ├── "Show all AWS EC2 instances running unpatched software with direct access to production S3 storage buckets"
        └── "Show all Kubernetes pods running privileged containers connected to sensitive key vaults"

6. STATIONING SPECIALIZED ARMED GUARDS: WORKLOAD PROTECTIONS (CWPP DEEP-DIVE)
├── CWPP plans act as specialized armed guards stationed inside distinct rooms; each Defender plan is tailored to a specific workload architecture
├── 1. Defender for Servers
│   ├── Protects Windows and Linux physical servers, virtual machines, and scale sets across Azure, AWS, GCP, and on-premises hosts
│   ├── Plan 1
│   │   ├── Onboards host devices to MDE automatically (MDE integration)
│   │   ├── Charges licensing fees on an hourly per-server basis rather than static monthly subscriptions (lowers costs for dynamic auto-scaling server pools)
│   │   └── Delivers EDR capabilities, OS-level threat detection, software inventory, and agent-based vulnerability scanning
│   └── Plan 2 (includes all Plan 1 features plus)
│       ├── Agentless vulnerability scanning
│       ├── Agentless malware scanning
│       ├── Agentless plain-text secret scanning
│       ├── OS baseline misconfiguration auditing via the Azure Machine Configuration extension
│       ├── FIM (file integrity monitoring, a capability that examines critical system files and registry keys for unauthorized changes using the MDE sensor)
│       ├── JIT (just-in-time, a security feature that locks down management ports like RDP 3389 and SSH 22, opening them only upon request for authorized source IPs and limited time windows)
│       ├── Azure Update Manager
│       ├── Network Map visualization
│       ├── Integrated DNS threat protection (monitors DNS queries for data tunneling and C2 communications)
│       └── 500 MB of free Log Analytics data ingestion per day per node
├── 2. Defender for App Service
│   ├── Native, transparent threat protection for web applications and APIs hosted on dedicated Azure App Service plans
│   ├── No code modifications or agent installations required
│   ├── App Service gateways inspect incoming HTTP/HTTPS traffic before routing requests to application sandboxes
│   └── MDC analyzes internal gateway logs and underlying VM sandboxes to identify attack methodologies: widespread vulnerability scanning, malicious web crawlers, webshell deployment attempts, distributed web exploits from suspicious IP addresses
├── 3. Defender for Storage
│   ├── Protects Azure Blob Storage, Azure Files, and Azure Data Lake Storage Gen2 (ADLS Gen2)
│   ├── Detects anomalous access patterns, unauthorized permission changes, data exfiltration attempts, and access from Tor anonymizing exit nodes
│   ├── Hash Reputation Analysis (supported by Microsoft Threat Intelligence; catches malware uploads)
│   │   ├── Does NOT read or scan the actual text content of stored files
│   │   └── Inspects storage log telemetry, extracts file hashes of newly uploaded items, compares them against a global database of known viruses, trojans, and ransomware
│   └── Automated malware cleanup: workflow automation triggered by alerts containing "Potential malware uploaded to a storage account" automatically deletes or quarantines the infected file
├── 4. Defender for Databases (four specialized plans; multi-cloud database infrastructure)
│   ├── Defender for Azure SQL Databases (Azure SQL Database, Azure SQL Managed Instance, dedicated SQL pools in Azure Synapse Analytics)
│   ├── Defender for SQL Servers on Machines (SQL Server on Azure VMs, AWS EC2, GCP compute, or on-premises servers connected via Azure Arc or standalone Windows setups)
│   ├── Defender for Open-Source Relational Databases (Azure Database for PostgreSQL, MySQL, and MariaDB)
│   ├── Defender for Azure Cosmos DB
│   │   └── Continuously analyzes the native telemetry stream of Cosmos DB accounts to detect SQL injection variations, anomalous key-listing patterns, credential theft, and suspicious data extraction without impacting database query performance
│   └── Combined capabilities
│       ├── Vulnerability Assessment (periodic scanning to identify database misconfigurations and missing security patches)
│       └── Advanced Threat Protection (real-time behavioral monitoring detecting SQL injection attacks, password spraying, brute-force attempts, and unauthorized administrative privilege abuse)
├── 5. Defender for Key Vault
│   ├── Native threat protection for Azure Key Vault instances safeguarding digital certificates, database connection strings, encryption keys, and administrative secrets
│   └── MDC monitors Key Vault management and data plane operation logs; raises high-severity alerts on unusual access patterns, unauthorized secret enumeration, key-listing activities, or traffic from malicious IP addresses
├── 6. Defender for Resource Manager
│   ├── Monitors the management plane of Azure by analyzing ARM deployment operations and Azure Activity Logs
│   │   └── ARM manages resource creation and administrative permissions, a primary target for threat actors
│   └── Advanced behavioral analytics detect: disabling antimalware extensions across multiple VMs, executing malicious scripts via VM extensions, deploying exploitation toolkits like Microburst or PowerZure, lateral movement from the cloud management plane down into the workload data plane
├── 7. Defender for Containers
│   ├── Cloud-native container security across Azure Kubernetes Service (AKS), Amazon EKS, and unmanaged Kubernetes clusters connected via Azure Arc
│   └── Three Operational Pillars
│       ├── Environment Hardening
│       │   └── Azure Policy for Kubernetes (an admission controller plugin) evaluates cluster configuration requests against best practices and blocks non-compliant deployments (e.g., preventing creation of privileged containers)
│       ├── Vulnerability Assessment
│       │   ├── Scans container images stored inside ACR (Azure Container Registry, a private cloud registry service that stores container images) upon push or import
│       │   └── Continuously scans active running images on AKS worker nodes using the Defender security profile extension
│       └── Runtime Threat Protection
│           ├── Monitors Linux worker nodes and Kubernetes cluster control planes in real time (node auditd logs)
│           ├── 60+ container-aware analytics mapped to the MITRE ATT&CK for Containers matrix
│           └── Detects exposed Kubernetes dashboards, high-privileged role assignments, sensitive host directory mounts, anomalous container process spawns
└── 8. Defender for APIs and Defender for DevOps
    ├── Defender for APIs
    │   ├── Discovers, monitors, and protects managed web APIs against the OWASP (Open Web Application Security Project) API Top 10 security risks
    │   └── Continuously scans API traffic to discover unauthenticated APIs, inactive or dormant APIs, and endpoints leaking sensitive data
    └── Defender for DevOps
        ├── Unifies security management across code repositories in GitHub, Azure DevOps, and GitLab
        ├── Evaluates IaC templates, source code weaknesses, and container build manifests
        └── PR Annotations (pull request comments inserted directly into developer code reviews) so developers fix flaws before code is merged into production branches

7. TRIAGE, THREAT INTELLIGENCE REPORTS & WORKLOAD INCIDENT RESPONSE
├── Alert Packaging
│   ├── Security Alert (generated when MDC detects malicious behavior inside any room of the estate)
│   ├── Cloud Smart Alert Correlation (bundles related alerts and low-fidelity signals; prevents analysts from being overwhelmed by scattered warnings)
│   └── Security Incident (single, connected attack campaign view showing attacker progression across impacted resources)
├── MDC Alert Severity Matrix
│   ├── High (high probability of active compromise; high confidence of malicious intent, e.g., Mimikatz execution)
│   ├── Medium (likely suspicious activity; medium-to-high confidence, e.g., anomalous sign-in or ML anomaly detection)
│   ├── Low (likely benign positive or blocked attack; low-to-medium confidence, e.g., administrative log clear action)
│   └── Informational (contextual background event nested inside incidents)
├── MITRE ATT&CK Tactics (alerts mapped to standard tactics)
│   └── PreAttack, InitialAccess, Persistence, PrivilegeEscalation, DefenseEvasion, CredentialAccess, Discovery, LateralMovement, Execution, Collection, Exfiltration, CommandAndControl, Impact
├── Alert Details Side Pane > Take Action tab (four standardized response options)
│   ├── 1. Mitigate the Threat (specific, manual remediation instructions to contain the active threat on the affected resource)
│   ├── 2. Prevent Future Attacks (security recommendations to harden the resource and eliminate underlying vulnerabilities)
│   ├── 3. Trigger Automated Response (manually execute a pre-configured Azure Logic App against the alert)
│   └── 4. Suppress Similar Alerts (launches the suppression rule wizard to dismiss future matching alerts automatically)
├── Threat Intelligence Reports (downloadable PDF, generated directly from alert pages; three formats)
│   ├── Activity Group Report (deep dives into specific threat actor groups, known motivations, and operational TTPs)
│   ├── Campaign Report (mechanics of specific, active global malware campaigns)
│   └── Threat Summary Report (combines actor profiles, campaign details, associated IoCs, and global victimology metrics)
│       └── IoCs (indicators of compromise, technical evidence like malicious IP addresses or file hashes)
└── Workload Containment Procedures
    ├── Key Vault Alerts
    │   ├── 1. Review alert details to identify caller IP address, UPN (user principal name), and accessed secret Object IDs
    │   ├── 2. Verify whether traffic originated from a recognized internal application or IP range
    │   ├── 3. If unauthorized, immediately restrict access
    │   │   ├── Key Vault uses Azure RBAC: remove the suspicious principal's role assignment under Access control (IAM)
    │   │   ├── Legacy access policies: delete the principal from Vault Access Policies
    │   │   └── Enable Key Vault Firewall to block untrusted IP ranges
    │   └── 4. Immediately rotate, disable, or delete all accessed secrets, keys, and certificates
    ├── DNS Alerts
    │   ├── 1. Contact the resource owner to confirm whether DNS query patterns (e.g., high-volume requests to unknown domains) were intentional
    │   ├── 2. If unexpected, treat the host as compromised and immediately isolate the virtual machine from the network
    │   └── 3. Run a full antimalware scan, audit installed software packages, reimage the host from a verified malware-free image, and apply pending MDC security recommendations
    └── Resource Manager Alerts
        ├── 1. Open Azure Activity Log and filter by subscription, timestamp, and caller identity
        ├── 2. If an administrative user account is compromised, reset credentials or delete rogue user accounts created by the attacker
        ├── 3. Audit Azure Automation accounts and delete unfamiliar runbooks
        ├── 4. Review IAM role assignments at the subscription level and remove unauthorized permissions
        └── 5. Reimage or redeploy affected virtual machines

8. AUTOMATED RESPONSE PATROLS & ALARM SILENCING: LOGIC APPS & SUPPRESSION RULES
├── Two automation mechanisms (handle high-volume alarms without exhausting human analysts): Workflow Automation and Alert Suppression Rules
├── Workflow Automation (path: Microsoft Defender for Cloud > Workflow automation)
│   ├── Automatically executes background playbooks when MDC generates a security alert or security recommendation
│   ├── Built using Azure Logic Apps Designer (visual drag-and-drop canvas to configure triggers, conditions, and actions)
│   │   └── Example actions: send email notifications, post Microsoft Teams messages, create ServiceNow tickets, execute automated mitigation scripts
│   ├── Configuration Steps
│   │   ├── 1. Navigate to Workflow Automation > Add workflow automation
│   │   ├── 2. Enter Rule Name, Description, Subscription, and Resource Group
│   │   ├── 3. Define Trigger Conditions
│   │   │   ├── Defender Data Type: Security Alert or Recommendation
│   │   │   ├── Alert Name Contains: string filter (e.g., "SQL")
│   │   │   └── Alert Severity: High, Medium, Low
│   │   ├── 4. Select Target Logic App (must contain MDC trigger connectors)
│   │   └── 5. Save Rule (triggers automatically on matching events)
│   ├── Native Logic App Triggers
│   │   ├── "When a Microsoft Defender for Cloud Alert is created or triggered" (fires when an alert matching configured severity levels or name substrings is generated)
│   │   └── "When a Microsoft Defender for Cloud Recommendation is created or triggered" (fires when a new security recommendation is raised across evaluated assets)
│   └── Manual execution: open an alert or recommendation page and select Trigger Logic App
└── Alert Suppression Rules (path: Security alerts > Suppression rules > Create new suppression rule)
    ├── Built when legitimate administrative actions or known testing routines repeatedly trigger false-positive warnings
    ├── Automatically sets matching future alerts to a Dismissed state, hiding them from active queue views without disabling underlying detection algorithms
    ├── Configuration Parameters
    │   ├── Rule Name (2 to 50 characters; letters, numbers, hyphens, and underscores only)
    │   ├── Rule State (Enabled or Disabled)
    │   ├── Reason (built-in categories: False positive, Too many alerts, or Other)
    │   ├── Expiration Date (mandatory end date; rules run for a maximum window of 6 months before requiring manual renewal)
    │   └── Rule Conditions (scopes suppression to all resources or specific matching criteria: source IP addresses, process names, user accounts, Azure resource IDs, or geographic locations)
    ├── Simulate (select before saving to test how many historical alerts would have been dismissed under the proposed rule logic)
    └── Dismissed alerts remain searchable by modifying queue filter options to display State == Dismissed

MASTER MAP: MITIGATING THREATS WITH MICROSOFT DEFENDER FOR CLOUD
├── 1. THE MULTI-CLOUD EMPIRE & CORE ROLES
│   ├── Global Security Director managing Azure (HQ), AWS (leased warehouses), GCP (overseas pop-ups) via MDC (centralized cloud-native application protection platform)
│   ├── DevSecOps (blueprint checks: code, container images, IaC such as Terraform/ARM templates)
│   ├── CSPM (building inspections: misconfigurations, compliance, hardening)
│   ├── CWPP (specialized armed guards: real-time threat detection for running workloads)
│   ├── Enablement (Subscription Owner / Contributor / Security Admin; Environment settings > Enable all Microsoft Defender plans)
│   ├── Foundational CSPM (free: asset discovery, continuous assessment, MCSB recommendations, basic Secure Score)
│   ├── Defender CSPM (paid: agentless VM scanning, data-aware insights, secret scanning, attack path analysis, Cloud Security Graph)
│   └── Azure Policy initiatives auto-protect new subscriptions; "not covered" subscriptions reveal Shadow IT
├── 2. EMPIRE REGISTRATION (ASSET INVENTORY & AUTO PROVISIONING)
│   ├── Asset Inventory (Inventory page, ARG-powered; Total / Unhealthy / Unmonitored Resources; status On / Off / Partial)
│   ├── Toolbar: tags, Add non-Azure servers, CSV export, ARG Explorer (KQL), Azure Logic App trigger
│   ├── Auto Provisioning (AMA with DCRs replacing MMA, MDE, Vulnerability Assessment, Guest Configuration Agent, Agentless Scanning, Defender Sensor; Azure Policy DeployIfNotExists)
│   ├── Direct Onboarding with MDE (Plan 1 capabilities, up to 24 hours, no Azure Arc; advanced features still need Arc)
│   └── DCR deployment (PowerShell / Azure CLI; verify via Heartbeat table; Sentinel vs MDC event collection never both; Minimal 4624/4625, Common, All Events)
├── 3. MULTI-CLOUD ONBOARDING
│   ├── Azure Arc (Connected Machine Agent -> ARM resource; interactive, at-scale via Service Principal, vSphere/SCVMM; stale entity purged after 7 days)
│   ├── AWS (Security Hub; CloudFormation/Terraform; CspmMonitorAws role; Management account needs Stack + StackSet; scan interval 4/6/12/24 hours)
│   └── GCP (Security Command Center; GCloud Shell/Terraform; Workload Identity Pool & Provider; five mandatory APIs; recommendations within 6 hours)
├── 4. BUILDING INSPECTIONS (SECURE SCORE & COMPLIANCE)
│   ├── Secure Score (Policy -> Initiative [MCSB: CIS, NIST] -> Recommendations -> Security Controls; points only when ALL resources comply with ALL recommendations in a control)
│   ├── Regulatory Compliance Dashboard (PCI-DSS, ISO 27001, NIST SP 800-53 / 800-171, SOC TSP, HIPAA / HITRUST, FedRAMP, CMMC Level 3, Azure CIS, SWIFT CSP; add via Owner / Policy Contributor)
│   └── Azure Workbooks (Secure Score Over Time, System Updates, Vulnerability Assessment Findings, Compliance Over Time, Active Alerts)
├── 5. BURGLAR ROUTE MAPPING
│   ├── Cloud Security Graph (Defender CSPM; compute, network, identity, exposure, vulnerability telemetry)
│   ├── Attack Path Analysis (entry points, hops, exploit techniques, Target Risk Score; fix a choke point to break the chain)
│   └── Cloud Security Explorer (visual query builder for graph queries)
├── 6. STATIONED ARMED GUARDS (CWPP PLANS)
│   ├── Servers (Plan 1: MDE EDR, hourly per-server billing; Plan 2: agentless scanning, FIM, JIT, Azure Update Manager, Network Map, DNS protection, 500 MB/node/day free ingestion)
│   ├── App Service (gateway HTTP/HTTPS inspection, no agents)
│   ├── Storage (Blob, Files, ADLS Gen2; Hash Reputation Analysis; "Potential malware uploaded to a storage account" automation)
│   ├── Databases (Azure SQL, SQL on machines, Open-Source Relational DBs, Cosmos DB; Vulnerability Assessment + Advanced Threat Protection)
│   ├── Key Vault (secret/key/certificate access monitoring)
│   ├── Resource Manager (ARM + Activity Logs; Microburst, PowerZure)
│   ├── Containers (AKS/EKS/Arc; Azure Policy for Kubernetes, ACR scanning, 60+ runtime analytics, MITRE ATT&CK for Containers)
│   ├── APIs (OWASP API Top 10; unauthenticated and dormant APIs)
│   └── DevOps (GitHub, Azure DevOps, GitLab; IaC scanning; PR Annotations)
├── 7. ALERT TRIAGE & RESPONSE
│   ├── Cloud Smart Alert Correlation (alerts -> Security Incidents)
│   ├── Severity (High, Medium, Low, Informational) and MITRE ATT&CK tactics (PreAttack ... Impact)
│   ├── Take Action tab (Mitigate the Threat, Prevent Future Attacks, Trigger Automated Response, Suppress Similar Alerts)
│   ├── Threat Intelligence Reports PDF (Activity Group, Campaign, Threat Summary with IoCs)
│   └── Workload Containment (Key Vault: restrict access + rotate secrets; DNS: isolate + reimage; Resource Manager: Activity Log audit + remove rogue accounts/runbooks/roles)
├── 8. AUTOMATED PATROLS
│   ├── Workflow Automation (Azure Logic Apps; alert/recommendation triggers; manual Trigger Logic App)
│   └── Suppression Rules (Dismissed state; Rule Name 2-50 characters; expiration max 6 months; Simulate; State == Dismissed)
└── END STATE (connecting the dots)
    └── Total multi-cloud visibility and automated threat mitigation across the globe: Auto Provisioning + Arc/AWS/GCP connectors -> Secure Score/MCSB/compliance -> Cloud Security Graph attack paths -> CWPP guards -> Cloud Smart Alert Correlation -> Workflow Automation and Suppression Rules
```