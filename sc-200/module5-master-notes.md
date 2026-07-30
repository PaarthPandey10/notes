# Mitigating Threats with Microsoft Defender for Cloud

## The Big Picture

As an enterprise expands across public clouds and private datacenters, managing security is like serving as a **Global Security Director managing a sprawling multi-cloud real estate empire**. You are no longer responsible for a single office building; your footprint spans company-owned headquarters in Azure, leased warehouses in **AWS** (Amazon Web Services, a third-party public cloud platform), and overseas pop-up facilities in **GCP** (Google Cloud Platform, a third-party public cloud platform). To oversee this vast property empire, the director deploys **MDC** (Microsoft Defender for Cloud, a centralized cloud-native application protection platform that provides posture management and workload protection).

The global director manages security through three distinct responsibilities: checking construction blueprints before a building is erected, physically inspecting finished buildings for structural flaws, and posting specialized armed guards inside high-value rooms like server vaults and file archives. Operated through the Azure portal, this global control system continuously evaluates cloud configurations, discovers unmonitored facilities across competing cloud providers, maps out hidden attack routes that burglars could exploit, and deploys real-time threat protection to defend digital assets across the globe.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        MULTI-CLOUD REAL ESTATE EMPIRE (MDC)                            │
│                                                                                        │
│  ┌────────────────────────┐  ┌─────────────────────────┐  ┌─────────────────────────┐  │
│  │ Headquarters (Azure)   │  │ Leased Warehouses (AWS) │  │ Overseas Pop-ups (GCP)  │  │
│  └───────────┬────────────┘  └───────────┬─────────────┘  └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌──────────────────────────────────────────────────────────────────────────────────┐  │
│  │                     CENTRAL SECURITY DIRECTORATE (MDC PORTAL)                    │  │
│  ├─────────────────────────┬───────────────────────────┬────────────────────────────┤  │
│  │ DEVSECOPS               │ CSPM                      │ CWPP                       │  │
│  │ (Blueprint Checks)      │ (Building Inspections)    │ (Specialized Armed Guards) │  │
│  └─────────────────────────┴───────────────────────────┴────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## The Core Mechanics

### The Director's Three Core Roles: DevSecOps, CSPM, and CWPP

Before installing security cameras or hiring guards, the global security director divides security operations into three operational disciplines:

- **DevSecOps** (development security operations, the process of checking application blueprints, source code repositories, and construction templates for security flaws long before a building is physically built) inspects developer code, container images, and **IaC** (infrastructure as code, machine-readable definition files like Terraform or ARM templates used to build cloud environments) definitions to catch vulnerabilities early in the construction lifecycle.
- **CSPM** (cloud security posture management, the process of physically inspecting finished buildings to ensure doors are locked and alarms are wired correctly) continually compares active cloud resources against security best practices, highlighting misconfigurations, measuring overall compliance, and surfacing actionable hardening steps.
- **CWPP** (cloud workload protection platform, the process of stationing specialized armed guards inside specific rooms like server vaults, databases, and container clusters) delivers real-time threat detection and behavioral analytics to block active attacks targeting running workloads.

To fund and activate these capabilities, an administrator holding the **Subscription Owner**, **Subscription Contributor**, or **Security Admin** role navigates to `Microsoft Defender for Cloud > Environment settings` in the Azure portal, selects the target subscription or workspace, and selects **Enable all Microsoft Defender plans** (or toggles individual plans on or off) before saving.

MDC divides posture management features into two capability levels:

- **Foundational CSPM**: Enabled automatically at no extra cost across all registered subscriptions, delivering basic asset discovery, continuous configuration assessments, security recommendations, and a centralized security rating based on the **MCSB** (Microsoft Cloud Security Benchmark, a master set of cloud security guidelines based on industry frameworks like CIS and NIST).
- **Defender CSPM**: A paid plan providing advanced posture capabilities, including agentless vulnerability scanning for virtual machines, data-aware security posture insights, agentless secret scanning, interactive attack path analysis, and the **Cloud Security Graph** (an interconnected graph database mapping relationships between cloud assets, permissions, and network exposures).

To ensure that newly created subscriptions are automatically protected, MDC evaluates tenant subscriptions against built-in initiative assignments managed through **Azure Policy** (an Azure governance service that enforces default security standards and audits compliance across cloud resources). Furthermore, reviewing subscriptions labeled "not covered" on the MDC dashboard allows security teams to identify **Shadow IT** (unmapped or unmanaged cloud subscriptions created without central IT approval) and apply standard governance rules immediately.

Now that the director's core roles and funding models are defined, we must catalogue all existing properties across the digital empire.

```
                      ┌───────────────────────────────────────┐
                      │  Enable Defender for Cloud Plans      │
                      │  (Environment Settings > Select Sub)  │
                      └──────────────────┬────────────────────┘
                                         │
             ┌───────────────────────────┴───────────────────────────┐
             ▼                                                       ▼
┌──────────────────────────┐                            ┌──────────────────────────┐
│ Foundational CSPM (Free) │                            │ Defender CSPM (Billable) │
├──────────────────────────┤                            ├──────────────────────────┤
│• Asset Discovery         │                            │• Agentless VM Scanning   │
│• Continuous Assessment   │                            │• Secret & Data Scanning  │
│• MCSB Recommendations    │                            │• Cloud Security Graph    │
│• Basic Secure Score      │                            │• Attack Path Analysis    │
└──────────────────────────┘                            └──────────────────────────┘
```

### Empire Infrastructure Registration: Asset Inventory and Auto Provisioning

In our multi-cloud estate, the global security director relies on the **Asset Inventory** page (`Microsoft Defender for Cloud > Inventory`) to track every connected building, server, and storage vault. Asset Inventory provides a single pane of glass powered by **ARG** (Azure Resource Graph, an Azure service that allows querying resource property data across subscriptions using KQL) to evaluate security posture across connected environments.

The top summary banner of the Inventory page displays three primary operational metrics:

- **Total Resources**: The cumulative count of all resources connected to MDC across cloud environments.
- **Unhealthy Resources**: Resources containing active, unresolved security recommendations.
- **Unmonitored Resources**: Resources experiencing agent communication failures (e.g., monitoring agents installed but failing to transmit heartbeat signals).

Security analysts filter inventory views by subscription, resource group, resource type, tags, or security finding criteria (e.g., searching for specific **CVE** [common vulnerabilities and exposures, a standardized public list of known cybersecurity weaknesses] IDs). The **Defender for Cloud** status filter categorizes resources into **On** (fully protected by an active Defender plan), **Off** (unprotected by paid Defender plans), or **Partial** (subscriptions where some, but not all, Defender plans are enabled).

From the Inventory toolbar, analysts execute quick administrative actions:

- Assigning corporate metadata tags to filtered resources.
- Onboarding external hosts via the **Add non-Azure servers** button.
- Exporting query results to a CSV file or opening the underlying **KQL** (Kusto Query Language, a database query language used to search raw telemetry and logs) statement directly inside **Azure Resource Graph Explorer**.
- Triggering an **Azure Logic App** (a cloud workflow automation service that executes automated response actions when triggered) to run custom remediation routines on selected assets.

To maintain continuous coverage without manual intervention, administrators configure **Auto Provisioning** (`Environment settings > Settings & monitoring`). Auto provisioning automatically deploys required monitoring agents and extensions to existing and newly created compute resources using Azure Policy `DeployIfNotExists` rules.

Supported auto provisioning components include:

- **Azure Monitor Agent (AMA)**: The modern software agent that collects security logs and OS events from virtual machines via **DCRs** (Data Collection Rules, policy objects defining what data to collect and which Log Analytics workspace to send it to). AMA replaces the retired **MMA** (Microsoft Monitoring Agent, the legacy Log Analytics agent).
- **Microsoft Defender for Endpoint (MDE)**: Automatically deploys the MDE sensor extension to Windows and Linux virtual machines when a Defender for Servers plan is active.
- **Vulnerability Assessment for Machines**: Deploys agentless or integrated vulnerability scanning engines to compute instances.
- **Guest Configuration Agent**: Deploys the Azure Policy Guest Configuration extension to evaluate OS-level security settings and compliance baselines.
- **Agentless Scanning for Machines**: Enables background disk snapshot scanning to inspect software inventories, vulnerabilities, and plain-text secrets without installing endpoint agents.
- **Defender Sensor**: Deploys the container security profile to Kubernetes worker nodes to collect runtime security events.

For non-Azure servers, organizations can utilize **Direct Onboarding with MDE** (`Environment settings > Direct onboarding`). Switching direct onboarding to **On** and selecting a target Azure subscription allows on-premises and multi-cloud servers to connect to MDC directly through the MDE agent without deploying Azure Arc. Direct onboarding provides **Defender for Servers Plan 1** capabilities (licensing, billing, vulnerability data, and EDR alerts), taking up to **24 hours** to appear in the inventory portal. However, advanced management capabilities—such as Azure Policy guest configurations, extension management, and **Defender for Servers Plan 2** features—still require full Azure Arc registration.

When manual AMA deployment is required, administrators create a Data Collection Rule (`Azure Monitor > Data Collection Rules > Create`). They specify rule basics (Name, Subscription, RG, Region, Platform Type), select target VMs on the _Resources_ tab (which automatically installs the AMA extension on associated machines), configure data sources on the _Collect and deliver_ tab (**Windows Events**, **Performance Counters**, **Syslog**, **IIS Logs**, or **Text Logs**), and route data to a designated Log Analytics workspace. Deployment can be executed programmatically via Azure PowerShell or Azure CLI:

```
# Associate a Data Collection Rule with a target Virtual Machine
New-AzDataCollectionRuleAssociation -ResourceUri "/subscriptions/sub-id/resourceGroups/rg-id/providers/Microsoft.Compute/virtualMachines/VM-Prod-01" -RuleId "/subscriptions/sub-id/resourceGroups/rg-id/providers/Microsoft.Insight/dataCollectionRules/DCR-Security-Logs"
```

```
# Associate a Data Collection Rule using Azure CLI
az monitor data-collection rule association create --resource "/subscriptions/sub-id/resourceGroups/rg-id/providers/Microsoft.Compute/virtualMachines/VM-Prod-01" --rule-id "/subscriptions/sub-id/resourceGroups/rg-id/providers/Microsoft.Insight/dataCollectionRules/DCR-Security-Logs" --name "VM-Prod-01-DCR-Assoc"
```

Deployment is verified by querying the `Heartbeat` table in the target Log Analytics workspace. Furthermore, if the workspace is integrated with **Microsoft Sentinel** (Microsoft's cloud-native SIEM platform), security event collection must be configured in _either_ MDC or Microsoft Sentinel—never both simultaneously—to prevent duplicate event ingestion. Administrators choose between **Minimal** (collects low-volume breach indicators like logon event IDs `4624` and `4625`), **Common** (collects full user audit trails including sign-outs `4634` and group changes), or **All Events**.

Now that our local Azure assets are registered and monitored, we must extend our security boundary to connect foreign properties across the multi-cloud empire.

---

### Bringing Outside Warehouses into the Central Control Room: Multi-Cloud Onboarding (Azure Arc, AWS, GCP)

In our corporate real estate empire, the director frequently manages remote facilities not built natively on Azure land. To bring these hybrid and multi-cloud properties under central control, MDC uses specialized connectors and bridge software.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              MULTI-CLOUD ONBOARDING ARCHITECTURE                       │
├──────────────────────────┬─────────────────────────────┬───────────────────────────────┤
│ HYBRID / ON-PREMISES     │ AMAZON WEB SERVICES (AWS)   │ GOOGLE CLOUD PLATFORM (GCP)   │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ Azure Arc Bridge         │ AWS Native Connector        │ GCP Native Connector          │
│ (Connected Machine Agent)│ (AWS CloudFormation Stack)  │ (GCloud Script / Shell)       │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ Project non-Azure hosts  │ Integrates AWS Security Hub │ Integrates GCP Security       │
│ into ARM with Resource   │ & IAM Role:                 │ Command Center & Workload     │
│ IDs and Azure Policy.    │ CspmMonitorAws              │ Identity Providers.           │
└──────────────────────────┴─────────────────────────────┴───────────────────────────────┘
```

#### 1. Hybrid and On-Premises Machines via Azure Arc

**Azure Arc** (a management service that extends Azure security, policy, and monitoring tools to non-Azure, on-premises, and multi-cloud servers) acts as a software bridge. Installing the **Azure Connected Machine Agent** on an external Windows or Linux server projects that machine into **ARM** (Azure Resource Manager, the management layer used to deploy, update, and manage Azure resources) as a native Azure resource. Each connected machine receives an Azure Resource ID, belongs to a resource group, and accepts Azure Policy definitions, tags, and extensions.

Azure Arc supports onboarding across multiple operational channels:

- **Interactive Deployment**: Generating an installation script in the Azure portal and executing it on individual machines.
- **At-Scale Deployment**: Using a **Service Principal** (an automated service identity in Microsoft Entra ID used by deployment scripts) to onboard machines non-interactively via Group Policy, Microsoft Configuration Manager task sequences, or PowerShell.
- **Virtualization Fabric Onboarding**: Onboarding VMware vSphere or System Center Virtual Machine Manager (SCVMM) management servers directly to discover and enroll virtual machines at scale.

Azure Arc connected machines must maintain active communication; if an Arc-enabled server becomes disconnected or expires, MDC automatically purges the stale Azure Arc entity after **7 days** to keep inventory views clean.

#### 2. Onboarding Amazon Web Services (AWS) Accounts

Connecting an AWS account integrates **AWS Security Hub** (Amazon's central security finding aggregator) directly into MDC (`Environment settings > Add environment > Amazon Web Services`).

The AWS onboarding setup spans four operational steps:

1. **Account Details**: Define a connector name, select scope (**Single account** vs. **Management account** to autodiscover all member accounts in an AWS Organization), select target Azure subscription and location, specify AWS Account IDs, select monitored AWS regions, and set an environment scan interval (**4, 6, 12, or 24 hours**).
2. **Select Defender Plans**: Toggle desired multi-cloud protection plans (**Foundational CSPM**, **Defender CSPM**, **Servers**, **Containers** for Amazon EKS clusters, and **Databases** for EC2 SQL and RDS instances). Read-only API calls executed by the `CspmMonitorAws` role are logged in AWS CloudTrail; to optimize CloudTrail export costs in external SIEMs, administrators can filter out read-only calls from `arn:aws:iam::[accountId]:role/CspmMonitorAws`.
3. **Configure Access**: Choose permissions type (**Default access** allowing automatic adoption of future capabilities vs. **Least privilege access** granting current required roles only) and select a deployment method (**AWS CloudFormation** or **Terraform**). Running the generated CloudFormation template creates required IAM roles (`CspmMonitorAws`) in the AWS account. When onboarding an AWS Management Account, the template must be executed as both a CloudFormation Stack and StackSet.
4. **Review and Generate**: Validate parameters and generate the security connector. Scan data and recommendations populate within a few hours.

#### 3. Onboarding Google Cloud Platform (GCP) Projects

Connecting a GCP project integrates **GCP Security Command Center** into MDC (`Environment settings > Add environment > Google Cloud Platform`).

The GCP onboarding setup spans four operational steps:

1. **Project Details**: Define a connector name, select scope (**Single project** vs. **Organization** to onboard all underlying GCP projects while defining optional project/folder exclusion lists), assign subscription and location, enter the GCP Project Number, GCP Project ID, and Organization ID, and define the scan interval (**4, 6, 12, or 24 hours**).
2. **Select Plans**: Select protection coverage across **Foundational CSPM**, **Defender CSPM**, **Servers**, **Databases**, and **Containers**.
3. **Configure Access**: Select permission types and choose a deployment method (**GCloud Shell script** or **Terraform**). Executing the generated GCloud script in GCP Cloud Shell provisions required architecture components: a **Workload Identity Pool**, a **Workload Identity Provider** (per plan), dedicated **Service Accounts**, and project-level policy bindings.
4. **Review and Generate**: Validate and create the security connector. Onboarding requires enabling five mandatory Google APIs: `iam.googleapis.com`, `sts.googleapis.com`, `cloudresourcemanager.googleapis.com`, `iamcredentials.googleapis.com`, and `compute.googleapis.com`. Initial recommendations appear in MDC within **6 hours**.

With all global facilities connected to our control room, the global security director must establish a standardized building inspection system to measure structural safety.

---

### Inspecting Finished Buildings: Secure Score, MCSB, and Regulatory Compliance

To evaluate safety across the multi-cloud empire, the global director reviews building inspection ratings surfaced through **Secure Score** (`Microsoft Defender for Cloud > Security posture`).

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SECURE SCORE CALCULATION                        │
├────────────────────────────────────────────────────────────────────────┤
│ • Security Policy (Azure Policy Definition)                            │
│   └── Combined into Security Initiative (e.g., MCSB)                   │
│       └── Evaluates Resources and Triggers Security Recommendations    │
│           └── Recommendations Grouped into Security Controls            │
│               └── CONTROL SCORE = Max Points ONLY when ALL resources   │
│                   comply with ALL recommendations in that control!     │
└────────────────────────────────────────────────────────────────────────┘
```

#### Understanding Policies, Initiatives, and Controls

- **Security Policy**: An Azure Policy definition specifying precise security conditions that must be controlled (e.g., requiring storage accounts to restrict public network access).
- **Security Initiative**: A logical grouping of Azure Policy definitions targeted toward a unified security goal. The default built-in initiative assigned automatically to every subscription registered in MDC is the **Microsoft Cloud Security Benchmark (MCSB)**. The MCSB provides cloud-centric security guidelines derived from **CIS** (Center for Internet Security) and **NIST** (National Institute of Standards and Technology) frameworks.
- **Security Recommendation**: Actionable hardening tasks generated when resources fail an underlying policy check within an assigned initiative.
- **Security Control**: A logical grouping of related security recommendations reflecting a specific vulnerable attack surface (e.g., _Enable MFA_, _Secure management ports_, or _Apply system updates_).

Each security control carries a max point value. **Secure Score is calculated as a percentage**, where an organization earns the points assigned to a security control **ONLY when every single resource within the scope complies with ALL recommendations contained inside that specific control**. Resolving partial recommendations within a control yields zero point increase until the entire control is fully remediated.

#### Regulatory Compliance Management

In addition to the default MCSB posture, organizations track compliance against industry regulations using the **Regulatory Compliance Dashboard** (`Microsoft Defender for Cloud > Regulatory compliance`). Each regulatory standard is modeled as an Azure Policy initiative mapped to specific regulatory controls.

Supported regulatory standards include:

- **PCI-DSS v3.2.1** (Payment Card Industry Data Security Standard)
- **ISO 27001:2013** (International information security management standard)
- **NIST SP 800-53 R4 / R5** and **NIST SP 800-171 R2** (US federal security frameworks)
- **SOC TSP** (Service Organization Control Trust Services Criteria)
- **HIPAA / HITRUST** (Healthcare data privacy standards)
- **FedRAMP High / Moderate** (US Federal Risk and Authorization Management Program)
- **CMMC Level 3** (Cybersecurity Maturity Model Certification)
- **Azure CIS 1.1.0 / 1.3.0** and **SWIFT CSP CSCF-v2020**

To add a regulatory standard to the dashboard, a user holding **Subscription Owner** or **Policy Contributor** permissions opens the Regulatory Compliance page, selects **Manage compliance policies**, expands _Industry & regulatory standards_, selects **Add more standards**, searches for the required regulation, and assigns the initiative to the target subscription or management group scope.

#### Visualizing Posture with Azure Workbooks

To track posture trends over time and present executive reports, MDC integrates natively with **Azure Workbooks** (`Microsoft Defender for Cloud > Workbooks`).

The Workbooks gallery provides pre-built interactive templates:

- **Secure Score Over Time**: Tracks subscription score changes and recommendation resolution progress over custom date ranges.
- **System Updates**: Highlights missing OS security patches categorized by severity and host.
- **Vulnerability Assessment Findings**: Visualizes vulnerability scan outputs across compute instances and container registries.
- **Compliance Over Time**: Displays regulatory compliance passing rates over time.
- **Active Alerts**: Categorizes security alerts by severity, MITRE ATT&CK tactics, tags, and geographic location.

While basic building inspections catch standard misconfigurations, clever burglars string together multiple minor flaws to break into high-value vaults. The director must adopt an adversary's mindset to expose these hidden breach routes.

---

### Thinking Like a Burglar: Cloud Security Graph, Attack Path Analysis, and Cloud Security Explorer

Standard compliance lists evaluate resources in isolation—such as checking whether a virtual machine is missing a patch or whether a storage account allows public access. However, real-world attackers combine isolated weaknesses into complex attack chains. To expose these multi-hop threat routes, MDC leverages the **Cloud Security Graph** (available with the Defender CSPM plan).

The Cloud Security Graph is an interactive graph database that ingests multi-cloud telemetry—compute configurations, network topologies, identity permissions, internet exposure points, and vulnerability scan outputs—to model complex relationships between assets.

```
[ Internet Exposure Point ]
            │
            ▼
[ Vulnerable Web Application (CVE-2023-XXXX) ]
            │
  (Over-Privileged Service Principal)
            │
            ▼
[ Key Vault Instance Containing Production Secrets ]
            │
  (Extracted Administrative Credentials)
            │
            ▼
[ Production Database Vault (Data Exfiltration) ]
```

Using the graph engine, MDC executes **Attack Path Analysis** (`Cloud Security > Attack path analysis`). Attack path analysis automatically simulates adversary techniques to surface high-risk paths leading to critical assets (e.g., domain controllers, database servers, or key vaults). Each surfaced attack path displays an interactive visual map detailing entry points, intermediate hops, exploit techniques, affected resources, and a **Target Risk Score**. Resolving a recommendation on a single choke-point node along the path breaks the entire attack chain and neutralizes the threat.

Security analysts conduct custom graph investigations using **Cloud Security Explorer** (`Cloud Security > Cloud Security Explorer`). Cloud Security Explorer provides a visual query builder that translates natural language or dropdown selections into graph queries against the underlying database.

Analysts can build targeted security queries to answer complex risk questions:

- _"Show all internet-exposed Virtual Machines possessing high-severity vulnerabilities and elevated identity permissions."_
- _"Show all AWS EC2 instances running unpatched software with direct access to production S3 storage buckets."_
- _"Show all Kubernetes pods running privileged containers connected to sensitive key vaults."_

Now that the director has inspected finished buildings and mapped out potential burglar routes, it is time to deploy specialized armed guards to protect specific high-value rooms.

---

### Stationing Specialized Armed Guards: Workload Protections (CWPP Deep-Dive)

In our corporate estate, **CWPP** plans act as specialized armed guards stationed inside distinct rooms. Each Defender plan is tailored to protect a specific workload architecture.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        MDC WORKLOAD PROTECTION PLANS                   │
├─────────────────────┬──────────────────────────────────────────────────┤
│ WORKLOAD PLAN       │ CORE CAPABILITIES & PROTECTION MECHANISMS        │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for        │ Plan 1: MDE integration, EDR, hourly billing.    │
│ Servers             │ Plan 2: Adds agentless vulnerability/malware/    │
│                     │ secret scanning, FIM, JIT, Azure Update Manager, │
│                     │ Network Map, 500MB free data ingestion/node/day. │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for App    │ Inspects incoming web HTTP traffic, gateway logs,│
│ Service             │ and underlying VM sandboxes to block web exploits.│
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for        │ Protects Blob, Files, ADLS Gen2. Uses Hash       │
│ Storage             │ Reputation Analysis to catch malware uploads.    │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for        │ Protects Azure SQL, SQL on VMs, OSS RDB, and     │
│ Databases           │ Cosmos DB against SQL injection and brute force. │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for Key    │ Monitors Key Vault API calls to detect secret    │
│ Vault               │ theft and unauthorized certificate access.       │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for        │ Monitors ARM operations via Activity Logs to stop│
│ Resource Manager    │ exploitation tools (Microburst, PowerZure).      │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for        │ Hardens AKS/EKS via Azure Policy, scans ACR      │
│ Containers          │ images, and monitors runtime node auditd logs.   │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for APIs   │ Protects web APIs against OWASP API Top 10 risks;│
│                     │ discovers unauthenticated and dormant APIs.      │
├─────────────────────┼──────────────────────────────────────────────────┤
│ Defender for        │ Scans IaC templates and code repos; inserts PR   │
│ DevOps              │ annotations directly into developer workflows.   │
└────────────────┘─────────────────────────────────────────────────┘
```

#### 1. Defender for Servers

Protects Windows and Linux physical servers, virtual machines, and scale sets across Azure, AWS, GCP, and on-premises hosts.

Defender for Servers is offered in two plan tiers:

- **Plan 1**: Onboards host devices to MDE automatically. Charges licensing fees on an hourly per-server basis rather than static monthly subscriptions, lowering costs for dynamic auto-scaling server pools. Delivers EDR capabilities, OS-level threat detection, software inventory, and agent-based vulnerability scanning.
- **Plan 2**: Includes all Plan 1 features plus agentless vulnerability scanning, agentless malware scanning, agentless plain-text secret scanning, OS baseline misconfiguration auditing via the Azure Machine Configuration extension, **FIM** (file integrity monitoring, a capability that examines critical system files and registry keys for unauthorized changes using the MDE sensor), **JIT** (just-in-time, a security feature that locks down management ports like RDP `3389` and SSH `22`, opening them only upon request for authorized source IPs and limited time windows), **Network Map** visualization, integrated DNS threat protection (monitoring DNS queries for data tunneling and C2 communications), and **500 MB of free Log Analytics data ingestion per day per node**.

#### 2. Defender for App Service

Provides native, transparent threat protection for web applications and APIs hosted on dedicated Azure App Service plans without requiring code modifications or agent installations.

App Service gateways inspect incoming HTTP/HTTPS traffic before routing requests to application sandboxes. MDC analyzes internal gateway logs and underlying VM sandboxes to identify attack methodologies—such as widespread vulnerability scanning, malicious web crawlers, webshell deployment attempts, and distributed web exploits originating from suspicious IP addresses.

#### 3. Defender for Storage

Delivers cloud-native security intelligence for data stored in Azure Blob Storage, Azure Files, and Azure Data Lake Storage Gen2. It detects anomalous access patterns, unauthorized permission changes, data exfiltration attempts, and access originating from Tor anonymizing exit nodes.

To identify uploaded malware, Defender for Storage utilizes **Hash Reputation Analysis** supported by Microsoft Threat Intelligence. The engine does **not** read or scan the actual text content of stored files; instead, it inspects storage log telemetry, extracts file hashes of newly uploaded items, and compares those hashes against a global database of known viruses, trojans, and ransomware. To automate malware cleanup, administrators deploy a workflow automation triggered by alerts containing `"Potential malware uploaded to a storage account"`, which automatically deletes or quarantines the infected file.

#### 4. Defender for Databases

Protects multi-cloud database infrastructure across four specialized plans:

- **Defender for Azure SQL Databases**: Secures Azure SQL Database, Azure SQL Managed Instance, and dedicated SQL pools in Azure Synapse Analytics.
- **Defender for SQL Servers on Machines**: Extends protection to SQL Server running on Azure VMs, AWS EC2, GCP compute, or on-premises servers (connected via Azure Arc or standalone Windows setups).
- **Defender for Open-Source Relational Databases**: Secures community database engines including Azure Database for PostgreSQL, Azure Database for MySQL, and Azure Database for MariaDB.
- **Defender for Azure Cosmos DB**: Continuously analyzes the native telemetry stream of Azure Cosmos DB accounts to detect SQL injection variations, anomalous key-listing patterns, credential theft, and suspicious data extraction without impacting database query performance.

Database plans combine **Vulnerability Assessment** (periodic scanning to identify database misconfigurations and missing security patches) with **Advanced Threat Protection** (real-time behavioral monitoring detecting SQL injection attacks, password spraying, brute-force attempts, and unauthorized administrative privilege abuse).

#### 5. Defender for Key Vault

Provides native threat protection for **Azure Key Vault** instances safeguarding digital certificates, database connection strings, encryption keys, and administrative secrets. MDC monitors Key Vault management and data plane operation logs, raising high-severity alerts upon detecting unusual access patterns, unauthorized secret enumeration, key-listing activities, or traffic originating from malicious IP addresses.

#### 6. Defender for Resource Manager

Monitors the management plane of Azure by analyzing **ARM** deployment operations and Azure Activity Logs. Because ARM manages resource creation and administrative permissions, it represents a primary target for threat actors.

Defender for Resource Manager uses advanced behavioral analytics to detect suspicious management activities—such as disabling antimalware extensions across multiple VMs, executing malicious scripts via VM extensions, deploying exploitation toolkits like **Microburst** or **PowerZure**, or attempting lateral movement from the cloud management plane down into the workload data plane.

#### 7. Defender for Containers

Delivers cloud-native container security across Azure Kubernetes Service (AKS), Amazon EKS, and unmanaged Kubernetes clusters connected via Azure Arc.

Container protection spans three operational pillars:

- **Environment Hardening**: Uses **Azure Policy for Kubernetes** (an admission controller plugin) to evaluate cluster configuration requests against security best practices, blocking non-compliant deployment requests (e.g., preventing the creation of privileged containers).
- **Vulnerability Assessment**: Scans container images stored inside **ACR** (Azure Container Registry, a private cloud registry service that stores container images) upon push or import, while continuously scanning active running images on AKS worker nodes using the Defender security profile extension.
- **Runtime Threat Protection**: Monitors Linux worker nodes and Kubernetes cluster control planes in real time using 60+ container-aware analytics mapped to the **MITRE ATT&CK for Containers** matrix. It detects exposed Kubernetes dashboards, high-privileged role assignments, sensitive host directory mounts, and anomalous container process spawns.

#### 8. Defender for APIs and Defender for DevOps

- **Defender for APIs**: Discovers, monitors, and protects managed web APIs against the **OWASP** (Open Web Application Security Project) API Top 10 security risks. It continuously scans API traffic to discover unauthenticated APIs, inactive or dormant APIs, and endpoints leaking sensitive data.
- **Defender for DevOps**: Unifies security management across code repositories in GitHub, Azure DevOps, and GitLab. It evaluates IaC templates, source code weaknesses, and container build manifests, surfacing **PR Annotations** (pull request comments inserted directly into developer code reviews) so developers can fix security flaws before code is merged into production branches.

Now that our specialized armed guards are actively monitoring every room in the estate, we must establish clear procedures for handling security alarms when an intruder is detected.

---

### Triage, Threat Intelligence Reports, and Workload Incident Response

When security guards detect malicious behavior inside any room of our estate, MDC generates a **Security Alert**. To prevent analysts from getting overwhelmed by scattered warnings, MDC uses **Cloud Smart Alert Correlation** to bundle related alerts and low-fidelity signals into a unified **Security Incident**. Security incidents present a single, connected attack campaign view showing attacker progression across impacted resources.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        MDC ALERT SEVERITY MATRIX                       │
├───────────────┬────────────────────────────────────────────────────────┤
│ ALERT SEVERITY│ OPERATIONAL DEFINITION & CONFIDENCE LEVEL              │
├───────────────┼────────────────────────────────────────────────────────┤
│ High          │ High probability of active compromise; high confidence │
│               │ of malicious intent (e.g., Mimikatz execution).        │
├───────────────┼────────────────────────────────────────────────────────┤
│ Medium        │ Likely suspicious activity; medium-to-high confidence  │
│               │ (e.g., anomalous sign-in or ML anomaly detection).     │
├───────────────┼────────────────────────────────────────────────────────┤
│ Low           │ Likely benign positive or blocked attack; low-to-medium│
│               │ confidence (e.g., administrative log clear action).    │
├───────────────┼────────────────────────────────────────────────────────┤
│ Informational │ Contextual background event nested inside incidents.   │
└───────────────┴────────────────────────────────────────────────────────┘
```

Alerts are mapped to standard **MITRE ATT&CK Tactics**: _PreAttack_, _InitialAccess_, _Persistence_, _PrivilegeEscalation_, _DefenseEvasion_, _CredentialAccess_, _Discovery_, _LateralMovement_, _Execution_, _Collection_, _Exfiltration_, _CommandAndControl_, and _Impact_.

Selecting an alert opens the Alert Details side pane. Navigating to the **Take Action** tab provides four standardized response options:

1. **Mitigate the Threat**: Provides specific, manual remediation instructions to contain the active threat on the affected resource.
2. **Prevent Future Attacks**: Lists security recommendations to harden the resource and eliminate underlying vulnerabilities.
3. **Trigger Automated Response**: Allows analysts to execute a pre-configured Azure Logic App manually against the alert.
4. **Suppress Similar Alerts**: Launches the suppression rule wizard to dismiss future matching alerts automatically.

To assist threat investigators, MDC generates downloadable **Threat Intelligence Reports** in PDF format directly from alert pages.

Threat intelligence reports are available in three formats:

- **Activity Group Report**: Delivers deep dives into specific threat actor groups, their known motivations, and operational TTPs.
- **Campaign Report**: Focuses on the mechanics of specific, active global malware campaigns.
- **Threat Summary Report**: Combines actor profiles, campaign details, associated **IoCs** (indicators of compromise, technical evidence like malicious IP addresses or file hashes), and global victimology metrics.

When responding to alerts on specific cloud workloads, analysts follow standardized containment procedures:

- **Key Vault Alerts**:
    1. Review alert details to identify caller IP address, **UPN** (user principal name), and accessed secret Object IDs.
    2. Verify if traffic originated from a recognized internal application or IP range.
    3. If unauthorized, immediately restrict access: if Key Vault uses Azure RBAC, remove the suspicious principal's role assignment under `Access control (IAM)`; if using legacy access policies, delete the principal from Vault Access Policies; enable Key Vault Firewall to block untrusted IP ranges.
    4. Immediately rotate, disable, or delete all accessed secrets, keys, and certificates.
- **DNS Alerts**:
    1. Contact the resource owner to confirm if DNS query patterns (e.g., high-volume requests to unknown domains) were intentional.
    2. If unexpected, treat the host as compromised and immediately isolate the virtual machine from the network.
    3. Run a full antimalware scan, audit installed software packages, reimage the host from a verified malware-free image, and apply pending MDC security recommendations.
- **Resource Manager Alerts**:
    1. Open Azure Activity Log and filter by subscription, timestamp, and caller identity.
    2. If an administrative user account is compromised, reset credentials or delete rogue user accounts created by the attacker.
    3. Audit Azure Automation accounts and delete unfamiliar runbooks.
    4. Review IAM role assignments at the subscription level and remove unauthorized permissions.
    5. Reimage or redeploy affected virtual machines.

While manual response procedures contain active breaches, maintaining enterprise security at scale requires automated robotic dispatch systems.

---

### Automated Response Patrols and Alarm Silencing: Logic Apps and Suppression Rules

To handle high-volume alarms without exhausting human analysts, MDC provides two automation mechanisms: **Workflow Automation** and **Alert Suppression Rules**.

#### Workflow Automation with Azure Logic Apps

**Workflow Automation** (`Microsoft Defender for Cloud > Workflow automation`) automatically executes background playbooks when MDC generates a security alert or security recommendation. Workflow automations are built using **Azure Logic Apps Designer**, providing a visual drag-and-drop canvas to configure triggers, conditions, and actions (e.g., sending email notifications, posting Microsoft Teams messages, creating ServiceNow tickets, or executing automated mitigation scripts).

```
┌────────────────────────────────────────────────────────────────────────┐
│                    WORKFLOW AUTOMATION CONFIGURATION                   │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Navigate to Workflow Automation > Add workflow automation.          │
│ 2. Enter Rule Name, Description, Subscription, and Resource Group.     │
│ 3. Define Trigger Conditions:                                          │
│    • Defender Data Type: [ Security Alert ] or [ Recommendation ]      │
│    • Alert Name Contains: String filter (e.g., "SQL")                  │
│    • Alert Severity: [ High ] [ Medium ] [ Low ]                       │
│ 4. Select Target Logic App (must contain MDC trigger connectors).      │
│ 5. Save Rule (Triggers automatically on matching events).              │
└────────────────────────────────────────────────────────────────────────┘
```

MDC supports two native Logic App triggers:

- `When a Microsoft Defender for Cloud Alert is created or triggered`: Fires when an alert matching configured severity levels or name substrings is generated.
- `When a Microsoft Defender for Cloud Recommendation is created or triggered`: Fires when a new security recommendation is raised across evaluated assets.

Workflow automations can also be executed manually by opening an alert or recommendation page and selecting **Trigger Logic App**.

#### Alert Suppression Rules

When legitimate administrative actions or known testing routines repeatedly trigger false-positive warnings, analysts build **Suppression Rules** (`Security alerts > Suppression rules > Create new suppression rule`). Suppression rules automatically set matching future alerts to a **Dismissed** state, hiding them from active queue views without disabling underlying detection algorithms.

Configuring a suppression rule requires defining specific parameters:

- **Rule Name**: Must contain 2 to 50 characters (letters, numbers, hyphens, and underscores only).
- **Rule State**: Set to **Enabled** or **Disabled**.
- **Reason**: Select built-in categories (_False positive_, _Too many alerts_, or _Other_).
- **Expiration Date**: Defines a mandatory end date for the suppression rule (rules run for a maximum window of **6 months** before requiring manual renewal).
- **Rule Conditions**: Scopes suppression to all resources or specific matching criteria (e.g., filtering by specific source IP addresses, process names, user accounts, Azure resource IDs, or geographic locations).

Analysts can select **Simulate** before saving to test how many historical alerts would have been dismissed under the proposed rule logic. Dismissed alerts remain searchable in the portal by modifying queue filter options to display `State == Dismissed`.

---

## Connecting the Dots

To protect a sprawling multi-cloud estate, every security discipline in Microsoft Defender for Cloud snaps together into a unified defense system.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE MULTI-CLOUD SURVEILLANCE GRID                         │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Multi-Cloud Empire     │   │ Empire Registration    │   │ Building Inspections    │  │
│  │ (DevSecOps / CSPM /   │──►│ (Asset Inventory / Arc /│──►│ (Secure Score / MCSB /  │  │
│  │  CWPP Distinctions)   │   │  AWS / GCP Connectors) │   │  Regulatory Compliance) │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └───────────┬─────────────┘  │
│              │                           │                            │                │
│              └─────────────────┐         │         ┌──────────────────┘                │
│                                ▼         ▼         ▼                                   │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Burglar Route Mapping │──►│ Stationed Armed Guards │◄──│ Alert Triage & Response │  │
│  │ (Cloud Security Graph │   │ (CWPP Workload Plans:  │   │ (Cloud Smart Correlation│  │
│  │  & Attack Paths)      │   │  Servers, DBs, Storage)│   │  & Threat Intel Reports)│  │
│  └───────────────────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│                                          │                                             │
│                                          ▼                                             │
│                              ┌────────────────────────┐                                │
│                              │ AUTOMATED PATROLS      │                                │
│                              │ (Workflow Automation / │                                │
│                              │  Suppression Rules)    │                                │
│                              └────────────────────────┘                                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Empire Organization and Onboarding**: The global security director structures operations across **DevSecOps** (code/blueprint checks), **CSPM** (building inspections), and **CWPP** (armed guards). All empire properties are registered in the **Asset Inventory** using **Auto Provisioning** (AMA, MDE, vulnerability scanners), connecting external facilities via **Azure Arc**, **Direct Onboarding**, and native multi-cloud connectors for **AWS** (CloudFormation) and **GCP** (GCloud Shell).
2. **Building Inspections and Burglar Mapping**: Finished cloud assets are inspected using **Secure Score** and the **Microsoft Cloud Security Benchmark (MCSB)**, measuring legal compliance against standards like PCI-DSS and ISO 27001. To catch sophisticated threat actors, the director uses the **Cloud Security Graph** and **Attack Path Analysis** to expose multi-hop breach chains leading to sensitive vaults, querying graph relations using **Cloud Security Explorer**.
3. **Stationing Workload Guards**: Specialized **CWPP** guards are stationed inside specific rooms—deploying **Defender for Servers** (Plan 1 MDE EDR vs. Plan 2 agentless scanning, FIM, JIT, and free data ingestion), **Defender for App Service**, **Defender for Storage** (Hash Reputation Analysis malware checks), **Defender for Databases** (Azure SQL, OSS RDB, Cosmos DB), **Defender for Key Vault**, **Defender for Resource Manager** (ARM Activity Log monitoring), **Defender for Containers** (ACR scanning, AKS/EKS runtime auditd monitoring), **Defender for APIs** (OWASP Top 10), and **Defender for DevOps** (IaC scanning and PR annotations).
4. **Alert Triaging, Response, and Automation**: When intruders trigger alarms, **Cloud Smart Alert Correlation** groups warnings into unified **Security Incidents**. Analysts review severity levels, consult **Threat Intelligence Reports**, and execute workload-specific containment (Key Vault secret rotation, VM isolation, Activity Log audits). Finally, high-volume tasks are dispatched to **Workflow Automation** using **Azure Logic Apps**, while recurring false alarms are silenced for up to 6 months using **Alert Suppression Rules**, providing total multi-cloud visibility and automated threat mitigation across the globe.