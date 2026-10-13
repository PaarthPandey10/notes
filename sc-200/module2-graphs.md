```
1. THE BIG PICTURE & CORE MECHANICS
├── Scenario: Central Security Control Room flooded with high-severity alarms across email, user identities, endpoints, and multi-cloud servers
│   ├── Human security guards are overwhelmed, piecing together scattered clues across multiple monitor screens
│   └── Company executives demand an immediate status report
├── Microsoft Security Copilot (an artificial intelligence analysis tool built to help security teams investigate and respond to threats at machine speed)
│   ├── Analogy: World-Class AI Detective sitting directly alongside the human guards
│   ├── AI Detective capabilities
│   │   ├── Reads millions of log lines in a fraction of a second
│   │   ├── Translates obfuscated hacker code into plain English
│   │   ├── Drafts executive briefings before guards finish their morning coffee
│   │   └── Recommends precise step-by-step remediation commands
│   └── Study scope: how the detective's brain works; how to fund, hire, and equip them; how to give clear instructions; how they assist guards across dedicated workspaces and active crime scenes
└── The AI Detective's Brain
    ├── Generative AI (a form of artificial intelligence that creates original content such as text, summaries, or computer code based on patterns learned from training data)
    │   ├── LLMs (large language models, specialized machine learning models trained on vast text datasets across the internet to generate natural language completions based on starting prompts)
    │   │   └── Rather than rigid if-then rules or wizardry, the model calculates mathematical relationships between words and phrases to predict the most probable continuation of a text sequence
    │   └── SLMs (small language models, smaller and more lightweight language models designed for specific tasks)
    │       └── Described as the compact equivalents of LLMs
    ├── Tokenization Mechanics
    │   ├── PROCESSING PIPELINE (raw text to prediction; strict sequence)
    │   │   ├── Raw Text Input (example: "I heard a dog bark")
    │   │   ├── Tokenization (break text into tokens & assign IDs; I=1, heard=2, a=3, dog=4, bark=5)
    │   │   ├── Positional Encoding (add sequence order markers)
    │   │   ├── Transformer Encoder (apply Multi-Head Attention; weights semantic proximity: "heard" & "dog" heavily weight "bark")
    │   │   ├── Vector Embeddings (convert to multi-dimensional vectors)
    │   │   └── Transformer Decoder (apply Masked Attention; predicts next token sequence: "at a cat" or "puppy")
    │   ├── Tokens (words, sub-words like the "un" in "unbelievable", punctuation marks, or common character sequences)
    │   │   ├── Assigned unique numerical IDs during training (e.g., I=1, heard=2, a=3, dog=4, bark=5)
    │   │   └── Modern LLM vocabularies consist of hundreds of thousands of unique tokens
    │   ├── Vector (an array of numbers representing linguistic traits)
    │   │   └── Each token receives an initial vector (array of numeric values)
    │   └── Positional Encoding (a numerical tag indicating where a token appears in a sentence sequence, ensuring word order is preserved)
    ├── Transformer Model Architecture (processes vectors through two specialized blocks: Encoder and Decoder)
    │   ├── Encoder (the transformer block that applies attention mechanisms to construct contextual vector embeddings)
    │   │   ├── Attention and Multi-Head Attention (evaluate tokens in context, assigning higher numerical weights to strongly related words across multiple parallel dimensions)
    │   │   │   ├── Attention (examines each token in the context of surrounding words)
    │   │   │   │   └── Example: when evaluating "bark", "heard" and "dog" get higher attention weights than "I" or "a"
    │   │   │   └── Multi-Head Attention (executes these evaluations across multiple linguistic dimensions in parallel)
    │   │   └── Embeddings (final output vectors produced by the encoder where semantically related words point in similar directions)
    │   │       ├── Multi-dimensional vector space: "dog" and "puppy" point in nearly identical directions, close to "cat", but distant from "car" or "skateboard"
    │   │       └── Cosine Similarity (measures the exact angle between two vector arrows in multi-dimensional space to calculate their semantic proximity)
    │   └── Decoder (the transformer block that evaluates prior tokens to predict the next token in a sequence)
    │       └── Masked Attention (used during decoder training to obscure subsequent tokens in a sequence, allowing the model to compare predicted vs known text and adjust internal weights to minimize prediction errors)
    └── Enterprise Hosting & Model Variations
        ├── Azure OpenAI Services (a cloud service providing REST API access to OpenAI language models backed by enterprise security, privacy, and compliance controls)
        │   └── Microsoft hosts the heavy language models securely in the enterprise environment
        ├── Testing Environments (admins and developers test model behavior, experiment with system instructions, evaluate safety guardrails)
        │   ├── Chat Playground (an interactive sandbox testing environment in Azure AI Studio)
        │   └── Azure AI Studio or Azure AI Foundry (web-based workspaces used to build, evaluate, and manage custom AI applications)
        └── Specialized Models
            ├── GPT-4o (a high-performance multimodal AI model optimized for advanced reasoning, natural chat, and technical task execution)
            ├── GPT-3.5 Turbo (a lightweight language model optimized for fast chat interactions)
            ├── Embedding Models (specialized models that convert text into mathematical vectors for semantic search)
            └── DALL-E (a specialized generative AI model that creates visual images from natural language text descriptions)

2. GIVING ORDERS TO THE DETECTIVE: PROMPTS & RAG
├── Prompt Engineering (the practice of carefully crafting text instructions, questions, and parameters to obtain optimal responses from an AI model)
│   ├── Why it matters: a vague demand like "Find the bad guy" returns a vague or useless answer; precise results require crafted prompts
│   └── Prompt (a natural language statement or question entered into the text prompt bar)
│       └── Must be crafted with four essential ingredients to guide the model's reasoning
├── Four Essential Ingredients of an Effective Prompt
│   ├── Goal (The explicit security-related information or action required)
│   │   ├── Answers: what specific security info you need
│   │   └── Example: "Give me info on incident 15134..." / "Summarize incident 15134 in Microsoft Defender XDR"
│   ├── Context (The background reason or operational scenario explaining why the information is needed)
│   │   ├── Answers: why you need the info or how it will be used
│   │   └── Example: "...for a report to my manager." / "...so I can brief my SOC manager on our morning threat status"
│   ├── Expectations (The required formatting, layout, tone, or target audience constraints)
│   │   ├── Answers: what format or target audience to tailor to
│   │   └── Example: "Compile in a bullet list with a summary." / "...format the output as a single paragraph summary followed by a bulleted list of involved user entities"
│   └── Source (The specific security tools, plugins, or uploaded documents the AI should query)
│       ├── Answers: which plugin, data source, or file to use
│       └── Example: "Look in Defender." / "...use data from Microsoft Defender XDR and my uploaded incident policy file"
├── Prompt Classification Scopes
│   ├── System Prompts (background system instructions set by the application that define the AI model's role, persona, tone, safety boundaries, and constraints)
│   │   └── Example: "You are a helpful security assistant that responds concisely"
│   └── User Prompts (specific questions or commands entered by a human analyst during an active investigation)
│       └── Conversation History (the stored log of previous user prompts and AI responses passed back into subsequent queries so the model understands follow-up context)
│           └── Maintains continuity during long conversations; application tracks it so follow-ups are understood relative to earlier context
├── AI Learning Approaches & Data Grounding
│   ├── Zero-Shot Learning (AI successfully completes a task without receiving any prior examples in the prompt)
│   ├── One-Shot Learning (Provides the AI with a single formatted example inside the prompt to demonstrate output structure)
│   ├── Few-Shot Learning (Provides the AI with multiple concrete examples to teach complex formatting rules or domain-specific classification styles)
│   └── Grounding Data (Feeding real-world reference text, log files, or incident reports into the prompt so the AI bases answers on verified facts)
│       ├── AI bases its answer entirely on verified facts rather than guessing
│       └── RAG (retrieval augmented generation, a framework where an application retrieves relevant document passages from an external database and injects them into the prompt to ground the AI's response)
│           └── Ensures queries about corporate policies or specific incident logs return precise, fact-checked completions
└── Best Practices for Human Guards
    ├── Be clear, specific, and concise
    ├── Iterate through follow-up prompts to refine results
    ├── Provide positive instructions (state what to do rather than what not to do)
    ├── Use direct address ("You"), e.g., "You must extract all IP addresses"
    └── Specify explicit exceptions, e.g., "List all unmanaged devices, but if their name contains 'test', exclude them"

3. HIRING, FUNDING, AND BADGING: ONBOARDING & RBAC
├── Customer Licensing Categories
│   ├── Microsoft 365 E5 and E7 Customers (Automatic provisioning/onboarding via zero-click activation)
│   │   ├── Security Copilot is included with Microsoft 365 E5 and E7 licenses
│   │   ├── Microsoft automatically provisions and onboards eligible tenants; no manual Azure setup or manual capacity provisioning required
│   │   └── Organizations receive a 7-day advance notification prior to automatic activation
│   └── Non-Microsoft 365 E5 and E7 Customers (Manual onboarding required in Azure)
│       └── Must provision computing capacity in Azure before users can access the service
├── Capacity Provisioning & Billing Mechanics
│   ├── Model: provisioned capacity + usage-based overage (manual onboarding)
│   ├── SCUs (security compute units, the metric used to measure and bill for the underlying computing power consumed by Security Copilot across standalone and embedded experiences)
│   │   ├── Provisioned capacity billed on an hourly basis
│   │   ├── Overage capacity billed based on actual usage
│   │   ├── Limits: minimum 1, maximum 100 SCUs per capacity resource
│   │   ├── Recommended for introductory exploration: 3 SCUs with unlimited overage enabled
│   │   └── Dynamic Users (an overage capacity setting allowing Copilot to automatically scale up and bill for on-demand compute units when provisioned SCUs are fully depleted during unexpected workload spikes)
│   │       └── Overage settings: unlimited overage OR capped at a specific maximum limit
│   ├── Provisioning Flow
│   │   ├── Azure Subscription Setup
│   │   ├── Provision SCU Capacity Resource (Min 1, Max 100 SCUs per capacity)
│   │   ├── Provision via: Copilot Setup Wizard OR Azure Portal Service
│   │   ├── Create/Attach Copilot Workspaces (Geographic Data Storage Region)
│   │   └── Assign Copilot RBAC Roles & Permissions (Copilot Owner / Contributor)
│   ├── Required inputs (wizard or Azure Portal): select active Azure Subscription, specify Azure Resource Group, select geographic region, define capacity name, select 1 to 100 SCUs
│   ├── Azure Infrastructure Requirements
│   │   ├── Azure Subscription (an Azure billing container)
│   │   └── Azure Resource Group (a logical container for Azure resources)
│   └── Permissions to provision capacity
│       ├── Azure Owner OR Azure Contributor at the resource group level
│       ├── PLUS Security Administrator or higher in Microsoft Entra ID
│       └── Global Administrator (the highest administrative role in Microsoft Entra ID) does NOT automatically have Azure resource access unless they explicitly elevate access management for Azure subscriptions in the Azure portal
├── Workspace Architecture
│   └── Workspaces (separate, isolated work environments within a tenant configured to segment AI security traffic, manage team budgets, and enforce data compliance rules)
│       ├── Analogy: Microsoft Entra ID tenant = a house; Copilot Workspaces = separate rooms inside that house
│       ├── Benefits (Tailoring plugins by role, mapping budgets, preventing capacity throttling)
│       │   ├── Tailor distinct plugins, promptbooks, and file sources to specific team roles (e.g., SOC vs. Compliance)
│       │   ├── Map and monitor SCU costs against individual team budgets
│       │   └── Prevent critical workflows from being disrupted by capacity throttling
│       └── Geo-Residency Controls (Data storage in regional datacenters)
│           ├── Enforces compliance by storing session data in specific regional datacenters
│           ├── European Union Data Boundary (EUDB)
│           ├── United Kingdom
│           ├── United States
│           ├── Australia/New Zealand
│           ├── Japan
│           ├── Canada
│           └── South America
├── Global Default Settings (Managed by Security Administrator; configured during initial setup or workspace management)
│   ├── Capacity Assignment (Attaching SCU capacity resource to the workspace)
│   │   └── Each workspace must be backed by its own capacity assignment
│   ├── Data Storage Location & Prompt Evaluation Location
│   │   ├── Data Storage Location (geographic region where customer data is stored at rest)
│   │   └── Prompt Evaluation Location (restrict evaluation strictly to local geographic region OR allow global evaluation across worldwide Azure datacenters during high-traffic periods)
│   ├── Purview Audit Logging (the centralized compliance audit logging framework applied tenant-wide)
│   │   ├── Global setting applying tenant-wide across all workspaces
│   │   └── Logs administrative actions, user prompts, and AI responses into Microsoft Purview Audit
│   ├── Data-Sharing Preferences (Workspace toggles controlling if data is shared with Microsoft; never used to train foundational AI models)
│   │   └── Controls whether customer data is shared with Microsoft to validate product performance via human review or to build/validate security AI models
│   └── Plugin Management Settings (Controls custom plugin addition and allows Copilot to access data from M365 services)
│       ├── Define whether Contributors or Owners can add custom plugins
│       ├── Define whether plugin availability is restricted to Owners only
│       └── Allow Security Copilot to access data from your Microsoft 365 services (mandatory for the Microsoft Purview plugin to function)
└── Access Control and Role-Based Permissions
    ├── Controlled per workspace using two dedicated platform roles; manage AI platform capabilities WITHOUT granting underlying access to raw security data
    ├── Dedicated Platform Roles
    │   ├── Copilot Owner (Grants full administrative control to manage workspace settings, assign user roles, configure capacity, set data-sharing options, and manage plugins)
    │   │   └── Inherited natively by: Global Administrator (the highest administrative role in Microsoft Entra ID), Security Administrator, Billing Administrator, Intune Administrator, etc.
    │   │       ├── Microsoft Entra ID roles inheriting Owner: Global Administrator, Security Administrator, Billing Administrator, Intune Administrator, Microsoft Entra Compliance Administrator
    │   │       └── Microsoft Purview roles inheriting Owner: Purview Compliance Administrator, Purview Data Governance Administrator, Purview Organization Management
    │   └── Copilot Contributor (Allows analysts to create sessions, run prompts, execute promptbooks, and manage custom plugins without accessing administrative settings)
    │       └── Recommended Microsoft Security Roles group (a pre-configured bundle of native Entra security roles that can be added directly to the Contributor role)
    │           └── Quick way to grant platform access to existing security staff
    └── Underlying Authentication Model
        └── OBO (on-behalf-of, an authentication framework where Copilot inherits the exact role permissions and scope tags of the signed-in human user)
            └── Example: analyst with Copilot Contributor can enter prompts but cannot query incident data via the Microsoft Sentinel plugin unless they also hold a service-specific role such as Microsoft Sentinel Reader in Azure

4. THE DETECTIVE'S ENGINE AND TOOLKIT
├── The 7-Step Operational Pipeline (managed by the internal control engine when a guard submits a query)
│   ├── 1. Submit Prompt (Analyst inputs text)
│   │   └── Natural language prompt entered into the prompt bar
│   ├── 2. Orchestrator (Security Copilot's central planning engine that composes capabilities together to answer queries)
│   │   └── Receives request, evaluates it, determines context, and builds an execution plan using available skills
│   ├── 3. Build Context (Executes plan to gather telemetry)
│   │   └── Orchestrator executes the plan to fetch data context from connected security systems
│   ├── 4. Plugins & Data Sources Analyzed
│   │   └── Copilot analyzes patterns across enabled data sources and plugins to generate intelligent security insights
│   ├── 5. LLM Composes Human-Readable Response
│   │   └── LLM synthesizes gathered data into clear, human-readable natural language text
│   ├── 6. Responsible AI Review (Safety checks and formatting)
│   │   └── In compliance with Microsoft's Responsible AI framework
│   └── 7. Receives Response
│       ├── Finalized response delivered to the analyst
│       └── Process Log (an auditable execution trace displaying the exact capabilities, plugins, and safety checks executed during query processing)
├── Sessions and UI State Management
│   └── Session (A single continuous conversation within Security Copilot)
│       ├── Analysts interact with prompt-response pairs using built-in UI controls
│       ├── Pin (saves a prompt-response pair to the Pin Board)
│       │   └── Pin Board (a split-view panel that collects key investigation evidence, generates automated session summaries, allows title edits, and supports exporting to Microsoft Word, email, clipboard, or link-based sharing)
│       │       └── Link-based sharing with authorized colleagues
│       ├── Prompt Actions (Edit, Rerun, Delete, Export, Copy)
│       │   ├── Edit, Rerun, and Delete (modify prompt text, re-execute the query, delete individual prompts from the active thread)
│       │   └── Export and Copy (copy response text or export prompt outputs for external reporting)
│       └── Feedback Mechanism (Thumbs up/down, Looks right, Needs improvement, Inappropriate)
│           └── Used to refine model accuracy
├── Extensibility & Connectors
│   ├── Plugin (A software connector that links Security Copilot to specific data sources)
│   │   ├── Capabilities (individual specialized software functions or skills designed to execute specific tasks, such as summarizing an incident or analyzing code)
│   │   ├── Microsoft Sentinel plugin setup parameters: default workspace name, subscription name, resource group name
│   │   └── Plugin Categories (four)
│   │       ├── Microsoft Plugins (Preinstalled: Defender XDR, Sentinel, Entra ID Protection, etc.)
│   │       │   └── Defender XDR, Microsoft Sentinel, Entra ID Protection, Intune, Purview, Defender for Cloud, Defender for Threat Intelligence, Defender External Attack Surface Management
│   │       ├── Other Plugins (Third-party integrations requiring API keys: ServiceNow, Splunk, etc.)
│   │       │   ├── ServiceNow, Splunk, CrowdSec, GreyNoise, Tanium, Netskope, Cyware, UrlScan
│   │       │   └── Require vendor licenses and authentication credentials (e.g., API keys)
│   │       ├── Website Plugins (Anonymous connectors fetching open-source threat intelligence)
│   │       │   └── Anonymous, pre-configured; fetch open-source threat intelligence and public web data without requiring setup credentials
│   │       └── Custom Plugins (Developer-built skills uploaded via manifests)
│   │           ├── Manifest formatted to OpenAI API or native Copilot specifications
│   │           ├── YAML (Yet Another Markup Language, a human-readable data format used for configuration files)
│   │           ├── JSON (JavaScript Object Notation, a lightweight text format used for data interchange)
│   │           └── Example manifest structure
│   │               ├── Descriptor: Name (CustomThreatLookup), DisplayName (Custom Threat Intelligence Lookup), Description (Queries internal threat database for IP reputation)
│   │               └── Skills: Name (QueryIP), DisplayName (Query IP Address), Description (Fetches threat score for a given IP address); Inputs: Name (IPAddress), Type (String), Required (true)
│   ├── Knowledge Base Integration (augment Copilot's reasoning with organizational knowledge bases; two methods)
│   │   ├── File Upload (Static text files, local geo storage, private to uploader account)
│   │   │   ├── File types: DOCX, MD, PDF, TXT
│   │   │   ├── Size limits: 3 MB per file, 20 MB total per user
│   │   │   ├── Stored in the Security Copilot service boundary within the tenant's home geo; private to the uploading user account
│   │   │   ├── Prompts must explicitly include phrases like "uploaded files" or the file name (e.g., "Based on my uploaded file Contoso_Policy.pdf...") to invoke file reasoning
│   │   │   └── Owner settings control upload permissions: No one vs. Contributors and Owners
│   │   └── Azure AI Search Plugin (Connects Copilot to an Azure AI Search index)
│   │       ├── Azure AI Search (an AI-powered information retrieval service that indexes proprietary documents for vector searching)
│   │       ├── Index requirements: a searchable text field, a filterable title field, and a vector field using the text-embedding-ada-002 embedding model
│   │       └── Prompts must explicitly mention "Azure AI Search" to trigger index searching
│   └── Promptbook (A pre-configured sequence of prompts designed to automate repeatable investigation workflows)
│       ├── Creation flow
│       │   ├── Select prompt check boxes in an existing session
│       │   ├── Click Create promptbook
│       │   ├── Define title, tag, and description
│       │   ├── Parameterize inputs
│       │   ├── Enable "Continue on failure" toggle
│       │   └── Save & Share with Organization or keep Private
│       ├── Parameterized Inputs (Dynamic inputs using angle brackets without spaces: <IncidentID>, <number>, <threatactorname>, <username>)
│       └── Continue on failure toggle (Allows subsequent prompts to run even if an earlier prompt returns no data or fails)
└── Standalone Experience UI Landmarks (https://securitycopilot.microsoft.com/, securitycopilot.microsoft.com; central landing portal)
    ├── Navigation Panel
    │   ├── Agents
    │   ├── Promptbooks
    │   ├── Build (preview agent creation workspace)
    │   ├── History (retains past sessions)
    │   │   └── TTL (time-to-live, the expiration window before deleted data is permanently purged; 30 days for sessions, 90 for audit logs)
    │   │       ├── Deleted sessions: 30-day TTL
    │   │       └── Underlying audit logs retained up to 90 days
    │   ├── Owner settings
    │   ├── Plugin settings
    │   ├── Role assignments
    │   ├── Manage workspaces
    │   ├── Usage monitoring (a 90-day dashboard tracking SCU consumption by workspace, plugin, and user)
    │   │   ├── Displays a 90% capacity warning banner to analysts
    │   │   └── Blocks prompts if provisioned capacity is exceeded without overage
    │   └── Security Store (a storefront for discovering and deploying Microsoft and partner AI agents)
    ├── Ellipses Menu (...) (bottom-left navigation control)
    │   ├── Preferences (theme, language, time zone, response debug level)
    │   ├── Help widget (self-help articles and ticket submission; requires Service Support Administrator or Helpdesk Administrator roles)
    │   └── Tenant Switcher (allowing analysts to switch operational contexts between authorized enterprise tenants)
    ├── Prompts to Try (Filterable home screen cards)
    │   └── Organized by role (CISO, SOC Analyst, Threat Intel Analyst) and plugin
    └── Prompt Bar (Contains Prompt icon, Sources icon, and Run icon)
        ├── Prompt icon (view system capabilities and prompt suggestions)
        ├── Sources icon (manage enabled plugins and uploaded files)
        └── Run icon

5. ON THE ACTIVE CRIME SCENE: EMBEDDED EXPERIENCE
├── Embedded Experience (Places Security Copilot capabilities directly within native Microsoft security product interfaces)
│   ├── Directly invokes product-specific capabilities for processing efficiency
│   └── Open in Security Copilot (lets analysts transition seamlessly to the standalone portal)
├── 1. Copilot in Microsoft Defender XDR (embedded within https://security.microsoft.com/)
│   ├── Incident Summarization (Synthesizes up to 100 correlated alerts into a single summary)
│   │   ├── Single natural language summary detailing attack progression, impacted assets, threat actor attribution, and IOCs
│   │   └── IOCs (indicators of compromise, technical clues such as file hashes or IP addresses indicating a breach)
│   ├── Guided Responses (Contextualized triage and remediation steps)
│   │   ├── Recommends contextualized, step-by-step actions categorized into Triage, Containment, Investigation, and Remediation
│   │   └── Administrators can upload custom organizational guidelines to tailor recommendations
│   ├── Script and Command-Line Analysis (Translates obfuscated PowerShell/Bash to plain English)
│   │   ├── Translates obfuscated PowerShell, Batch, and Bash scripts into plain English
│   │   ├── Explains execution intent and maps activities to the MITRE ATT&CK matrix
│   │   └── Show code (analysts select to view matching code lines)
│   ├── Natural Language to KQL translation
│   │   ├── Converts natural language questions entered in advanced hunting into executable KQL queries
│   │   └── KQL (Kusto Query Language, a query language used to search raw security log databases)
│   ├── File, Device, and Identity Summaries
│   │   ├── File pages: summarizes file certificates, string extracts, and API calls
│   │   ├── Device pages: compiles ASR status, tamper protection, and vulnerabilities
│   │   │   └── ASR (attack surface reduction, security rules that block common attack vectors)
│   │   └── Identity pages: surfaces account criticality, role changes, and sign-in risks
│   ├── Create Incident Reports (Consolidates timestamps, analysts, playbooks, remediation)
│   │   └── Consolidates timestamps, assigned analysts, classifications, automated playbooks, and remediation steps into a formal, auditable report
│   └── Threat Intelligence Integration (Consolidates threat analytics reports and actor profiles)
│       └── Consolidates threat analytics reports, actor profiles, and vulnerability disclosures via the Microsoft Defender Threat Intelligence plugin
├── 2. Copilot in Microsoft Purview (embedded within https://purview.microsoft.com/; assists compliance and data security admins; Requires M365 services access enabled)
│   ├── Requires "Allow Security Copilot to access data from your Microsoft 365 services" setting enabled
│   ├── Data Security Posture Management (DSPM)
│   │   ├── Executes built-in promptbooks: Risky user investigation and Sensitive data protection
│   │   └── Tracks unauthorized data transfers across SharePoint, OneDrive, and Exchange
│   ├── Data Loss Prevention (DLP) (Summarizes DLP alerts queue)
│   │   └── Summarizes DLP alerts from the alerts queue and explains active policy configurations
│   ├── Insider Risk Management (Summarizes user exfiltration patterns)
│   │   └── Summarizes user exfiltration patterns, user roles, and activity timelines for high-risk users
│   ├── Activity Explorer (Converts natural language prompts into active filter sets)
│   │   └── Example: "Filter files copied to cloud with credit card SIT for past 30 days"
│   ├── Communication Compliance (Generates contextual summaries of flagged messages)
│   │   ├── Summaries of flagged messages and attachments (for items with a combined length of 100+ words)
│   │   └── Based on trainable classifiers
│   └── eDiscovery (Summarizes text-extractable evidence items)
│       ├── Summarizes text-extractable evidence items in review sets
│       ├── Executes natural language case searches
│       └── Drafts case scope summaries
├── 3. Copilot in Microsoft Entra (embedded within https://entra.microsoft.com/)
│   ├── Microsoft Entra ID (Investigates user properties, group ownerships, sign-in failures)
│   │   └── Also investigates Conditional Access policy coverage gaps
│   ├── Microsoft Entra ID Protection (Summarizes risky user/workload identity profiles)
│   │   ├── Summarizes risky user profiles
│   │   └── Evaluates risk levels for workload identities (service principals and app registrations) to discover excessive permissions or unused apps
│   ├── Identity Governance
│   │   ├── Analyzes access review decision overrides
│   │   ├── Queries entitlement management packages
│   │   ├── Summarizes PIM role changes
│   │   │   └── PIM (Privileged Identity Management, an Entra service that controls elevated administrative access)
│   │   ├── Supports direct just-in-time role activation within chat
│   │   └── Guides Lifecycle Workflows for joiner, mover, and leaver scenarios
│   ├── Global Secure Access (Analyzes network traffic patterns)
│   │   └── Analyzes user and device network traffic patterns across Internet Access and Private Access solutions
│   └── Data Exploration Grid (renders a comprehensive data grid that displays the underlying Microsoft Graph API URL when queries exceed 10 items)
│       └── Returns an Open list button when query results exceed 10 items
├── 4. Copilot in Microsoft Intune (embedded within https://intune.microsoft.com/; included with Security Copilot licensing)
│   ├── Policy and Setting Management (Tooltips for recommended values/conflict checks)
│   │   ├── Inline Copilot tooltips next to settings in Compliance, Device Configuration, and Endpoint Security policies
│   │   ├── Tooltips show recommended values, conflict checks, and user impact
│   │   └── Generates full policy summaries
│   ├── Device Details and Troubleshooting (Hex code error analyzer)
│   │   ├── Error analyzer for hex codes
│   │   ├── Compares device configurations side-by-side
│   │   └── Details app/policy assignments
│   ├── Data Exploration and Device Query
│   │   ├── Converts natural language requests into device query KQL scripts
│   │   └── Requires Intune Advanced Analytics licensing
│   └── Specialized Workloads
│       ├── EPM (Endpoint Privilege Management, an Intune feature allowing standard users to run approved administrative tasks)
│       │   └── Copilot analyzes EPM elevation requests
│       ├── Automates Surface device troubleshooting
│       └── Delivers performance/resizing recommendations for Windows 365 Cloud PCs
└── 5. Copilot in Microsoft Defender for Cloud (embedded within the Azure portal for cloud infrastructure security)
    ├── Dual-Platform Architecture (Administrative queries handled by Copilot for Azure do not consume SCUs)
    │   ├── Prompts are initially evaluated by Copilot for Azure
    │   └── If security analysis is required, it invokes a Security Copilot skill (consumes SCUs)
    ├── Recommendations Analysis
    │   ├── Analyze with Copilot control on the recommendations page
    │   ├── Filters risks by public exposure, sensitive data, or resource criticality
    │   └── DCSPM (Defender Cloud Security Posture Management, an advanced security plan in Defender for Cloud providing attack path analysis)
    │       └── Enabling the DCSPM plan unlocks full attack path reasoning
    ├── Remediation and Delegation (Generates step-by-step remediation scripts and delegation emails)
    │   ├── Summarizes resource vulnerabilities
    │   ├── Generates step-by-step remediation scripts
    │   └── Drafts delegation emails to resource owners
    └── Infrastructure as Code (IaC) Remediation
        ├── Automatically generates a PR in Azure DevOps repositories to fix security misconfigurations in Terraform or ARM templates
        └── PR (pull request, a formal code change submission in a repository)

6. AUTONOMOUS ROBOTIC DEPUTIES: AGENTS & THE SECURITY STORE
├── Agents (autonomous, always-on AI routines built to perform specific security and IT management workflows at scale)
│   ├── Purpose: handle high-volume, repetitive operational tasks without manual human intervention
│   ├── Operate securely within Microsoft's Zero Trust security framework
│   ├── Learn from analyst feedback
│   ├── Execute tasks based on automated triggers or on-demand invocations
│   ├── Phishing Triage Agent
│   │   ├── Workflow: Automatically analyzes, classifies, and resolves user-reported phishing emails in Defender for Office 365
│   │   ├── Triggered automatically when an employee reports a suspicious email in Outlook
│   │   ├── Evaluates headers, body text, URL detonations, and screenshots using LLMs
│   │   ├── Auto-resolves false positives while escalating verified phishing threats to human analysts
│   │   └── Entra Agent ID (a dedicated service principal identity for AI agents)
│   │       └── Agent operates under a dedicated Entra Agent ID
│   ├── Threat Intelligence Briefing Agent (Dynamically scans global threat feeds and generates customized vulnerability reports)
│   │   ├── Continuously cross-references tenant vulnerability inventories against global threat actor activity
│   │   └── Publishes executive briefings on the Threat Analytics dashboard
│   ├── Threat-Hunting Agent (Proactively executes background KQL queries to surface hidden threat actor activity)
│   │   ├── Executes background KQL queries across log tables
│   │   └── Detects subtle adversary techniques that bypassed standard alert tripwires
│   └── Conditional Access Optimization Agent (Identifies policy coverage gaps for new applications and users in Entra)
│       ├── Operates within the Microsoft Entra admin center
│       ├── Scans access logs to identify unmonitored applications or exposed user populations
│       └── Recommends optimized policy rules
└── Distribution & Lifecycle Management
    ├── Security Store (a security-optimized storefront integrated into the standalone Copilot portal that allows organizations to discover, deploy, and manage Microsoft and partner-built AI agents and solutions)
    └── Build (preview) interface (navigation panel; developers build custom agents)
        ├── Construct agent logic from scratch
        └── OR upload a structured YAML manifest file

MASTER MAP: MICROSOFT SECURITY COPILOT ARCHITECTURE AND WORKFLOWS
├── 1. FOUNDATIONAL ARCHITECTURE (THE AI BRAIN)
│   ├── Microsoft Security Copilot (an artificial intelligence analysis tool built to help security teams investigate and respond to threats at machine speed)
│   │   ├── World-Class AI Detective alongside human guards in the Central Security Control Room
│   │   └── Translates obfuscated code, reads millions of log lines, drafts executive briefings, recommends remediation commands
│   └── Generative AI (a form of artificial intelligence that creates original content such as text, summaries, or computer code based on patterns learned from training data)
│       ├── LLMs (large language models, specialized machine learning models trained on vast text datasets across the internet to generate natural language completions based on starting prompts)
│       ├── SLMs (small language models, smaller and more lightweight language models designed for specific tasks)
│       ├── Tokenization Mechanics
│       │   ├── Tokens (words, sub-words like the "un" in "unbelievable", punctuation marks, or common character sequences)
│       │   │   └── Unique numerical IDs; vocabularies of hundreds of thousands of tokens
│       │   ├── Vector (an array of numbers representing linguistic traits)
│       │   └── Positional Encoding (a numerical tag indicating where a token appears in a sentence sequence, ensuring word order is preserved)
│       ├── Transformer Components
│       │   ├── Encoder (the transformer block that applies attention mechanisms to construct contextual vector embeddings)
│       │   │   ├── Attention and Multi-Head Attention (evaluates tokens across multiple dimensions based on semantic proximity)
│       │   │   └── Embeddings (final output vectors produced by the encoder)
│       │   │       └── Cosine Similarity (measures the exact angle between two vector arrows in multi-dimensional space to calculate their semantic proximity)
│       │   └── Decoder (the transformer block that evaluates prior tokens to predict the next token in a sequence)
│       │       └── Masked Attention (used during decoder training to obscure subsequent tokens in a sequence)
│       └── Hosting & Model Variations
│           ├── Azure OpenAI Services (a cloud service providing REST API access to OpenAI language models backed by enterprise security, privacy, and compliance controls)
│           ├── Chat Playground (an interactive sandbox testing environment in Azure AI Studio)
│           ├── Azure AI Studio or Azure AI Foundry (web-based workspaces used to build, evaluate, and manage custom AI applications)
│           ├── GPT-4o (a high-performance multimodal AI model optimized for advanced reasoning, natural chat, and technical task execution)
│           ├── GPT-3.5 Turbo (a lightweight language model optimized for fast chat interactions)
│           ├── Embedding Models (specialized models that convert text into mathematical vectors for semantic search)
│           └── DALL-E (a specialized generative AI model that creates visual images from natural language text descriptions)
├── 2. COMMUNICATION & INSTRUCTIONS (PROMPT ENGINEERING)
│   ├── Prompt Engineering (the practice of carefully crafting text instructions, questions, and parameters to obtain optimal responses from an AI model)
│   │   └── Prompt (a natural language statement or question entered into the text prompt bar)
│   ├── Essential Prompt Ingredients
│   │   ├── Goal (The explicit security-related information or action required)
│   │   ├── Context (The background reason or operational scenario explaining why the information is needed)
│   │   ├── Expectations (The required formatting, layout, tone, or target audience constraints)
│   │   └── Source (The specific security tools, plugins, or uploaded documents the AI should query)
│   ├── Prompt Scopes
│   │   ├── System Prompts (background system instructions set by the application that define the AI model's role, persona, tone, safety boundaries, and constraints)
│   │   └── User Prompts (specific questions or commands entered by a human analyst during an active investigation)
│   │       └── Conversation History (the stored log of previous user prompts and AI responses passed back into subsequent queries)
│   ├── Learning Approaches & Grounding
│   │   ├── Zero-Shot Learning (completes a task without receiving any prior examples)
│   │   ├── One-Shot Learning (receives a single formatted example)
│   │   ├── Few-Shot Learning (receives multiple concrete examples)
│   │   └── Grounding Data (real-world reference text/logs injected so AI bases answers on verified facts)
│   │       └── RAG (retrieval augmented generation, a framework where an application retrieves relevant document passages from an external database and injects them into the prompt to ground the AI's response)
│   │           └── Keeps outputs anchored in real incident logs
│   └── Best Practices: clear/specific/concise, iterate, positive instructions, direct address ("You"), explicit exceptions
├── 3. ONBOARDING, CAPACITY, AND GOVERNANCE
│   ├── Capacity & Billing
│   │   ├── Microsoft 365 E5 and E7 Customers (Automatic zero-click provisioning; 7-day advance notification)
│   │   ├── Non-Microsoft 365 E5 and E7 Customers (Manual Azure provisioning)
│   │   └── SCUs (security compute units, the metric used to measure and bill for the underlying computing power consumed by Security Copilot across standalone and embedded experiences)
│   │       ├── Provisioned capacity billed hourly; overage billed on actual usage; 1 to 100 SCUs per capacity; recommended start: 3 SCUs with unlimited overage
│   │       ├── Provisioning via Copilot Setup Wizard or Azure Portal (requires Azure Owner/Contributor at resource group + Security Administrator or higher)
│   │       ├── Dynamic Users (an overage capacity setting allowing Copilot to automatically scale up and bill for on-demand compute units when provisioned SCUs are fully depleted during unexpected workload spikes)
│   │       ├── Azure Subscription (an Azure billing container)
│   │       └── Azure Resource Group (a logical container for Azure resources)
│   ├── Environment Organization
│   │   └── Workspaces (separate, isolated work environments within a tenant configured to segment AI security traffic, manage team budgets, and enforce data compliance rules)
│   │       ├── Analogy: tenant = house; workspaces = rooms
│   │       ├── Geo-Residency & European Union Data Boundary (EUDB) (also UK, US, Australia/New Zealand, Japan, Canada, South America)
│   │       ├── Default settings: Capacity Assignment, Data Storage Location, Prompt Evaluation Location, Data-Sharing Preferences, Plugin Management Settings
│   │       └── Purview Audit Logging (the centralized compliance audit logging framework)
│   └── Access Control (RBAC)
│       ├── Copilot Owner (Grants full administrative control to manage workspace settings, user roles, capacity, data-sharing, and plugins)
│       │   └── Inherited by: Global Administrator (the highest administrative role in Microsoft Entra ID), Security Administrator, Billing Administrator, Intune Administrator, Microsoft Entra Compliance Administrator, Purview Compliance Administrator, Purview Data Governance Administrator, Purview Organization Management
│       ├── Copilot Contributor (Allows analysts to create sessions, run prompts, execute promptbooks, and manage custom plugins without admin access)
│       │   └── Recommended Microsoft Security Roles group (a pre-configured bundle of native Entra security roles)
│       └── OBO (on-behalf-of, an authentication framework where Copilot inherits the exact role permissions and scope tags of the signed-in human user)
│           └── Preserves individual user permissions across underlying security databases (e.g., Microsoft Sentinel Reader needed for Sentinel plugin)
├── 4. THE EXECUTION ENGINE & TOOLKIT
│   ├── Execution Pipeline
│   │   ├── Orchestrator (Security Copilot's central planning engine that composes capabilities together to answer queries)
│   │   │   └── Generates an execution plan, queries connected plugins, executes skills
│   │   ├── Responsible AI Review (safety checks and formatting before the response is delivered)
│   │   └── Process Log (an auditable execution trace displaying the exact capabilities, plugins, and safety checks executed during query processing)
│   ├── Session Management
│   │   └── Session (A single continuous conversation)
│   │       ├── Pin Board (a split-view panel that collects key investigation evidence, generates automated session summaries, allows title edits, and supports exporting or link-based sharing)
│   │       └── Feedback: Looks right, Needs improvement, Inappropriate
│   ├── Extensibility
│   │   ├── Plugin (A software connector that links Security Copilot to specific data sources)
│   │   │   ├── Capabilities (individual specialized software functions or skills designed to execute specific tasks, such as summarizing an incident or analyzing code)
│   │   │   ├── Categories: Microsoft Plugins, Other Plugins (ServiceNow, Splunk, CrowdSec, GreyNoise, Tanium, Netskope, Cyware, UrlScan), Website Plugins
│   │   │   └── Custom Plugins (Uploaded via manifest files)
│   │   │       ├── YAML (Yet Another Markup Language, a human-readable data format used for configuration files)
│   │   │       └── JSON (JavaScript Object Notation, a lightweight text format used for data interchange)
│   │   ├── Knowledge Bases
│   │   │   ├── File Upload (DOCX, MD, PDF, TXT; 3 MB per file; 20 MB per user; private to uploader)
│   │   │   └── Azure AI Search Plugin (index needs searchable text field, filterable title field, vector field with text-embedding-ada-002)
│   │   │       └── Azure AI Search (an AI-powered information retrieval service that indexes proprietary documents for vector searching)
│   │   └── Promptbook (A pre-configured sequence of prompts designed to automate repeatable investigation workflows)
│   │       └── Parameterized with angle brackets (e.g., <IncidentID>); Continue on failure toggle
│   └── Standalone Experience UI (https://securitycopilot.microsoft.com/)
│       ├── Multi-Tool Master Desk & Pin Board (for complex cross-product investigations)
│       ├── Navigation Panel
│       │   ├── TTL (time-to-live, the expiration window before deleted data is permanently purged)
│       │   │   └── 30 days for deleted sessions; audit logs up to 90 days
│       │   ├── Usage monitoring (a 90-day dashboard tracking SCU consumption by workspace, plugin, and user)
│       │   └── Security Store, Build (preview), Agents, Promptbooks, History, Owner/Plugin settings, Role assignments, Manage workspaces
│       ├── Ellipses Menu (...)
│       │   └── Tenant Switcher (allowing analysts to switch operational contexts between authorized enterprise tenants)
│       └── Prompts to Try / Prompt Bar (Prompt, Sources, Run icons)
├── 5. EMBEDDED CAPABILITIES (ACTIVE CRIME SCENE)
│   ├── Embedded Experience (Places Security Copilot capabilities directly within native Microsoft security product interfaces)
│   │   └── Open in Security Copilot (transition to standalone portal)
│   ├── Defender XDR
│   │   ├── Incident summaries (up to 100 alerts), Guided Responses, Script Analysis (PowerShell/Batch/Bash), NL-to-KQL, Incident Reports
│   │   ├── IOCs (indicators of compromise, technical clues such as file hashes or IP addresses indicating a breach)
│   │   ├── KQL (Kusto Query Language, a query language used to search raw security log databases)
│   │   └── ASR (attack surface reduction, security rules that block common attack vectors)
│   ├── Microsoft Purview
│   │   ├── Data Security Posture Management (DSPM)
│   │   ├── Data Loss Prevention (DLP)
│   │   └── Insider Risk Management, Activity Explorer, Communication Compliance, eDiscovery
│   ├── Microsoft Entra
│   │   ├── PIM (Privileged Identity Management, an Entra service that controls elevated administrative access)
│   │   ├── Conditional Access coverage gap evaluation
│   │   └── Data Exploration Grid (Open list button when results exceed 10 items; shows Microsoft Graph API URL)
│   ├── Microsoft Intune
│   │   ├── Settings tooltips, policy summaries, error analyzer, device query KQL
│   │   └── EPM (Endpoint Privilege Management, an Intune feature allowing standard users to run approved administrative tasks)
│   └── Defender for Cloud
│       ├── Copilot for Azure first; security skills consume SCUs, non-security queries do not
│       ├── DCSPM (Defender Cloud Security Posture Management, an advanced security plan in Defender for Cloud providing attack path analysis)
│       └── PR (pull request, a formal code change submission in a repository) for Infrastructure as Code (Terraform / ARM) fixes in Azure DevOps
└── 6. AUTONOMOUS OPERATIONS
    ├── Agents (autonomous, always-on AI routines built to perform specific security and IT management workflows at scale)
    │   ├── Operate within Zero Trust framework; learn from analyst feedback; automated triggers or on-demand
    │   ├── Phishing Triage Agent
    │   │   └── Entra Agent ID (a dedicated service principal identity for AI agents)
    │   ├── Threat Intelligence Briefing Agent
    │   ├── Threat-Hunting Agent
    │   └── Conditional Access Optimization Agent
    ├── Security Store (a storefront for discovering and deploying Microsoft and partner AI agents and solutions)
    └── END STATE (connecting the dots)
        ├── Brain & prompting: LLMs hosted in Azure OpenAI Services (GPT-4o); Prompt Engineering with Goals, Context, Expectations, Sources; RAG and Grounding Data
        ├── Onboarding & governance: SCUs across Workspaces (budget control + geo-residency); Copilot Owner / Contributor roles; OBO authentication
        ├── Engine & toolkit: Orchestrator (Execution Planner) + Plugins (M365, Sentinel, third-party, website, custom YAML manifests); Sessions, Pin Board, parameterized Promptbooks
        ├── Embedded vs Standalone: Embedded (Defender, Purview, Entra, Intune, Defender for Cloud) for active crime scenes; Standalone for complex cross-product investigations
        └── Autonomous background patrols: Agents deployed from the Security Store (e.g., Phishing Triage Agent, Conditional Access Optimization Agent) so the team stops drowning in raw logs and defends the digital campus at true machine speed
```