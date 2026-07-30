# Microsoft Security Copilot Architecture and Workflows

## The Big Picture

Imagine your **Central Security Control Room** is suddenly flooded with high-severity alarms across email, user identities, endpoints, and multi-cloud servers. Your human security guards are overwhelmed, attempting to piece together scattered clues across multiple monitor screens while company executives demand an immediate status report. Now imagine you could instantly hire a **World-Class AI Detective** to sit directly alongside your human guards. This AI detective can read millions of log lines in a fraction of a second, translate obfuscated hacker code into plain English, draft executive briefings before your guards finish pouring their morning coffee, and recommend precise step-by-step remediation commands. In our digital campus, this elite detective is **Microsoft Security Copilot** (an artificial intelligence analysis tool built to help security teams investigate and respond to threats at machine speed). In these master study notes, we will explore how this AI detective's brain works, how to properly fund, hire, and equip them, how to give them clear instructions, and how they assist human guards across both dedicated workspaces and active crime scenes throughout the network.

---

## The Core Mechanics

### The AI Detective's Brain: Generative AI, LLMs, and Tokenization Mechanics

Before human guards can collaborate effectively with their new AI detective, they must understand how the detective's digital brain processes information. The detective operates on **Generative AI** (a form of artificial intelligence that creates original content such as text, summaries, or computer code based on patterns learned from training data).

At the core of this AI brain are **LLMs** (large language models, specialized machine learning models trained on vast text datasets across the internet to generate natural language completions based on starting prompts) and their compact equivalents, **SLMs** (small language models, smaller and more lightweight language models designed for specific tasks). Rather than relying on rigid if-then rules or wizardry, the model calculates mathematical relationships between words and phrases to predict the most probable continuation of a text sequence.

```
[ Raw Text Input: "I heard a dog bark" ]
                      │
                      ▼
 [ Tokenization: Break text into tokens & assign IDs ]
 (I=1, heard=2, a=3, dog=4, bark=5)
                      │
                      ▼
 [ Positional Encoding: Add sequence order markers ]
                      │
                      ▼
 [ Transformer Encoder: Apply Multi-Head Attention ]
 (Weights semantic proximity: "heard" & "dog" heavily weight "bark")
                      │
                      ▼
 [ Vector Embeddings: Convert to multi-dimensional vectors ]
                      │
                      ▼
 [ Transformer Decoder: Apply Masked Attention ]
 (Predicts next token sequence: "at a cat" or "puppy")
```

The process of converting human speech into mathematical data follows a strict sequence:

- **Tokenization** breaks training text into distinct units called **Tokens** (words, sub-words like the "un" in "unbelievable", punctuation marks, or common character sequences) and assigns a unique numerical ID to each token (e.g., `I` = 1, `heard` = 2, `a` = 3, `dog` = 4, `bark` = 5). Modern LLM vocabularies consist of hundreds of thousands of unique tokens.
- **Initial Vectors and Positional Encoding** assign each token an array of numeric values called a **Vector** (an array of numbers representing linguistic traits) along with a **Positional Encoding** (a numerical tag indicating where a token appears in a sentence sequence, ensuring word order is preserved).
- **Transformer Model** processes these vectors through two specialized blocks: an **Encoder** (the transformer block that applies attention mechanisms to construct contextual vector embeddings) and a **Decoder** (the transformer block that evaluates prior tokens to predict the next token in a sequence).
- **Attention and Multi-Head Attention** mechanisms examine each token in the context of surrounding words, assigning higher numerical weights to strongly related words (e.g., when evaluating "bark", the words "heard" and "dog" receive higher attention weights than "I" or "a"). **Multi-Head Attention** executes these evaluations across multiple linguistic dimensions in parallel.
- **Embeddings** are the final output vectors produced by the encoder. In a multi-dimensional vector space, semantically related words point in similar vector directions (e.g., the embeddings for "dog" and "puppy" point in nearly identical directions, close to "cat", but distant from "car" or "skateboard").
- **Cosine Similarity** measures the exact angle between two vector arrows in multi-dimensional space to calculate their semantic proximity.
- **Masked Attention** is used during decoder training to obscure subsequent tokens in a sequence, allowing the model to compare its predicted next token against known text and adjust its internal weights to minimize prediction errors.

To run these heavy language models in an enterprise environment, Microsoft hosts them securely using **Azure OpenAI Services** (a cloud service providing REST API access to OpenAI language models backed by enterprise security, privacy, and compliance controls). Administrators and developers can test model behavior, experiment with system instructions, and evaluate safety guardrails inside the **Chat Playground** (an interactive sandbox testing environment in Azure AI Studio) within **Azure AI Studio** or **Azure AI Foundry** (web-based workspaces used to build, evaluate, and manage custom AI applications). The platform utilizes specialized models including **GPT-4o** (a high-performance multimodal AI model optimized for advanced reasoning, natural chat, and technical task execution), **GPT-3.5 Turbo** (a lightweight language model optimized for fast chat interactions), **Embedding Models** (specialized models that convert text into mathematical vectors for semantic search), and **DALL-E** (a specialized generative AI model that creates visual images from natural language text descriptions).

Now that we understand how the AI detective's brain processes vocabulary and text predictions, we must examine how human guards structure their requests to get accurate, high-quality answers.

### Giving Orders to the Detective: Prompt Engineering, System Prompts, and RAG

If a security guard approaches the AI detective and simply demands, "Find the bad guy," the detective will return a vague or useless answer. To achieve precise results, guards must master **Prompt Engineering** (the practice of carefully crafting text instructions, questions, and parameters to obtain optimal responses from an AI model).

In Security Copilot, a **Prompt** (a natural language statement or question entered into the text prompt bar) must be crafted with four essential ingredients to guide the model's reasoning:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        ELEMENTS OF AN EFFECTIVE PROMPT                 │
├──────────────────┬─────────────────┬───────────────────┬───────────────┤
│       GOAL       │     CONTEXT     │   EXPECTATIONS    │    SOURCE     │
├──────────────────┼─────────────────┼───────────────────┼───────────────┤
│ What specific    │ Why you need    │ What format or    │ Which plugin, │
│ security info    │ the info or how │ target audience   │ data source,  │
│ you need.        │ it will be used.│ to tailor to.     │ or file to use│
│                  │                 │                   │               │
│ Example: "Give me│ Example: "...for│ Example: "Compile │ Example:      │
│ info on incident │ a report to my  │ in a bullet list  │ "Look in      │
│ 15134..."        │ manager."       │ with a summary."  │ Defender."    │
└──────────────────┴─────────────────┴───────────────────┴───────────────┘
```

- **Goal**: The explicit security-related information or action required (e.g., "Summarize incident 15134 in Microsoft Defender XDR").
- **Context**: The background reason or operational scenario explaining why the information is needed (e.g., "...so I can brief my SOC manager on our morning threat status").
- **Expectations**: The required formatting, layout, tone, or target audience constraints (e.g., "...format the output as a single paragraph summary followed by a bulleted list of involved user entities").
- **Source**: The specific security tools, plugins, or uploaded documents the AI should query (e.g., "...use data from Microsoft Defender XDR and my uploaded incident policy file").

Guards must also distinguish between two distinct prompt types: **System Prompts** (background system instructions set by the application that define the AI model's role, persona, tone, safety boundaries, and constraints, such as "You are a helpful security assistant that responds concisely") and **User Prompts** (specific questions or commands entered by a human analyst during an active investigation). To maintain continuity during long conversations, the application tracks **Conversation History** (the stored log of previous user prompts and AI responses passed back into subsequent queries) so the model understands follow-up questions in relation to earlier context.

When asking the detective questions, guards can leverage different learning approaches:

- **Zero-Shot Learning** occurs when the AI successfully completes a task without receiving any prior examples in the prompt.
- **One-Shot Learning** provides the AI with a single formatted example inside the prompt to demonstrate the desired output structure.
- **Few-Shot Learning** provides the AI with multiple concrete examples to teach complex formatting rules or domain-specific classification styles.
- **Grounding Data** involves feeding real-world reference text, log files, or incident reports into the prompt so the AI bases its answer entirely on verified facts rather than guessing.
- **RAG** (retrieval augmented generation, a framework where an application retrieves relevant document passages from an external database and injects them into the prompt to ground the AI's response) ensures that queries regarding corporate policies or specific incident logs return precise, fact-checked completions.

Security analysts should follow core prompting best practices: be clear, specific, and concise; iterate through follow-up prompts to refine results; provide positive instructions that state what _to do_ rather than what _not to do_; directly address the AI as "You" (e.g., "You must extract all IP addresses"); and specify exceptions explicitly (e.g., "List all unmanaged devices, but if their name contains 'test', exclude them").

With prompt engineering principles established, company management must formally onboard, fund, and provision the AI detective before guards can begin typing queries.

### Hiring, Funding, and Badging the Detective: Onboarding, SCUs, Workspaces, and RBAC

To bring Microsoft Security Copilot into the corporate control room, administrators must follow strict onboarding and capacity provisioning procedures.

#### Customer Categories and Licensing

- **Microsoft 365 E5 and E7 Customers**: Security Copilot is included with Microsoft 365 E5 and E7 licenses. Microsoft automatically provisions and onboards eligible tenants via zero-click activation, requiring no manual Azure setup or manual capacity provisioning. Organizations receive a 7-day advance notification prior to automatic activation.
- **Non-Microsoft 365 E5 and E7 Customers**: Organizations without E5 or E7 licenses must execute manual onboarding steps in Azure to provision computing capacity before users can access the service.

#### Capacity Provisioning and Billing

For manual onboarding, Security Copilot operates on a provisioned capacity and usage-based overage model measured in **SCUs** (security compute units, the metric used to measure and bill for the underlying computing power consumed by Security Copilot across standalone and embedded experiences). Provisioned capacity is billed on an hourly basis, while overage capacity is billed based on actual usage.

```
                  ┌─────────────────────────────────────┐
                  │      Azure Subscription Setup       │
                  └──────────────────┬──────────────────┘
                                     │
                                     ▼
                  ┌─────────────────────────────────────┐
                  │    Provision SCU Capacity Resource  │
                  │   (Min 1, Max 100 SCUs per capacity)│
                  └──────────────────┬──────────────────┘
                                     │
             ┌───────────────────────┴───────────────────────┐
             ▼                                               ▼
┌──────────────────────────┐                   ┌──────────────────────────┐
│ Provision via Copilot    │                   │ Provision via Azure      │
│ Setup Wizard             │                   │ Portal Service           │
└────────────┬─────────────┘                   └─────────────┬────────────┘
             │                                               │
             └───────────────────────┬───────────────────────┘
                                     │
                                     ▼
                  ┌─────────────────────────────────────┐
                  │ Create/Attach Copilot Workspaces    │
                  │  (Geographic Data Storage Region)   │
                  └──────────────────┬──────────────────┘
                                     │
                                     ▼
                  ┌─────────────────────────────────────┐
                  │ Assign Copilot RBAC Roles & Permissions│
                  │ (Copilot Owner / Contributor)       │
                  └─────────────────────────────────────┘
```

Administrators can configure **Dynamic Users** (an overage capacity setting allowing Copilot to automatically scale up and bill for on-demand compute units when provisioned SCUs are fully depleted during unexpected workload spikes). Overage settings can be set to unlimited overage or capped at a specific maximum limit. Regardless of whether capacity is provisioned through the Security Copilot setup wizard or directly in the Azure Portal, administrators must select an active **Azure Subscription** (an Azure billing container), specify an **Azure Resource Group** (a logical container for Azure resources), select a geographic region, define a capacity name, and select between 1 and 100 SCUs per capacity resource. For introductory exploration, Microsoft recommends purchasing 3 SCUs with unlimited overage enabled.

To provision capacity, the executing user must hold **Azure Owner** or **Azure Contributor** permissions at the resource group level, combined with **Security Administrator** or higher in Microsoft Entra ID. A **Global Administrator** (the highest administrative role in Microsoft Entra ID) does not automatically possess Azure resource access unless they explicitly elevate access management for Azure subscriptions within the Azure portal.

#### Workspace Architecture

Security Copilot introduces **Workspaces** (separate, isolated work environments within a tenant configured to segment AI security traffic, manage team budgets, and enforce data compliance rules). Using our corporate campus analogy, the Microsoft Entra ID tenant represents a house, while individual Copilot Workspaces represent separate rooms inside that house.

Workspaces provide key operational benefits:

- Tailoring distinct plugins, promptbooks, and file sources to specific team roles (e.g., SOC vs. Compliance).
- Mapping and monitoring SCU costs against individual team budgets.
- Preventing critical workflows from being disrupted by capacity throttling.
- Enforcing **Geo-Residency** compliance by storing session data in specific regional datacenters, including the European Union Data Boundary (**EUDB**), United Kingdom, United States, Australia/New Zealand, Japan, Canada, and South America.

#### Default Environment Configuration

During initial setup or workspace management, users with the **Security Administrator** role configure global settings:

- **Capacity Assignment**: Attaching a provisioned SCU capacity resource to the workspace. Each workspace must be backed by its own capacity assignment.
- **Data Storage Location**: Defining the geographic region where customer data is stored at rest.
- **Prompt Evaluation Location**: Restricting prompt evaluation strictly to the local geographic region or allowing global evaluation across worldwide Azure datacenters during high-traffic periods.
- **Purview Audit Logging**: A global setting (applying tenant-wide across all workspaces) that logs administrative actions, user prompts, and AI responses into **Microsoft Purview Audit** (the centralized compliance audit logging framework).
- **Data-Sharing Preferences**: Workspace toggles that control whether customer data is shared with Microsoft to validate product performance via human review or to build/validate security AI models. Customer data is never used to train foundational AI models.
- **Plugin Management Settings**: Defining whether Contributors or Owners can add custom plugins, whether plugin availability is restricted to Owners only, and toggling the option to **Allow Security Copilot to access data from your Microsoft 365 services** (which is mandatory for the Microsoft Purview plugin to function).

#### Access Control and Role-Based Permissions

Access to Security Copilot is controlled per workspace using two dedicated platform roles that manage AI platform capabilities without granting underlying access to raw security data:

- **Copilot Owner**: Grants full administrative control to manage workspace settings, assign user roles, configure capacity, set data-sharing options, and manage plugins.
- **Copilot Contributor**: Allows analysts to create sessions, run prompts, execute promptbooks, and manage their own custom plugins without accessing administrative settings.

Microsoft Entra ID roles automatically inherit **Copilot Owner** access: **Global Administrator**, **Security Administrator**, **Billing Administrator**, **Intune Administrator**, and **Microsoft Entra Compliance Administrator**. Microsoft Purview roles that inherit Owner access include **Purview Compliance Administrator**, **Purview Data Governance Administrator**, and **Purview Organization Management**.

Administrators can quickly grant platform access to existing security staff by adding the **Recommended Microsoft Security Roles** group (a pre-configured bundle of native Entra security roles) directly into the Copilot Contributor role. Security Copilot operates on the **OBO** (on-behalf-of, an authentication framework where Copilot inherits the exact role permissions and scope tags of the signed-in human user) model. For example, an analyst with Copilot Contributor access can enter prompts, but cannot query incident data via the **Microsoft Sentinel** plugin unless they also hold an explicit service-specific role such as **Microsoft Sentinel Reader** in Azure.

Once badged and assigned to a workspace, the AI detective relies on an internal orchestrator and modular toolkit to execute investigations.

### The Detective's Engine and Toolkit: Orchestrator, Plugins, Sessions, and Promptbooks

When a human guard submits a query, Security Copilot processes the request through a 7-step operational pipeline managed by its internal control engine.

```
[ Step 1: User Submits Prompt in Prompt Bar ]
                      │
                      ▼
[ Step 2: Orchestrator Receives Request & Builds Execution Plan ]
                      │
                      ▼
[ Step 3: Build Context (Executes Plan to Gather Telemetry) ]
                      │
                      ▼
[ Step 4: Plugins & Data Sources Analyzed ]
                      │
                      ▼
[ Step 5: LLM Composes Human-Readable Response ]
                      │
                      ▼
[ Step 6: Responsible AI Safety Checks & Formatting ]
                      │
                      ▼
[ Step 7: User Receives Final Formatted Response ]
```

1. **Submit Prompt**: The analyst inputs a natural language prompt into the prompt bar.
2. **Orchestrator**: The **Orchestrator** (Security Copilot's central planning engine that composes capabilities together to answer queries) evaluates the request, determines context, and builds an execution plan using available skills.
3. **Build Context**: The orchestrator executes the plan to fetch necessary data context from connected security systems.
4. **Plugins**: Copilot analyzes patterns across enabled data sources and plugins to generate intelligent security insights.
5. **Responding**: The LLM synthesizes gathered data into clear, human-readable natural language text.
6. **Responsible AI Review**: The output undergoes safety checks and formatting in compliance with Microsoft's Responsible AI framework.
7. **Receives Response**: The finalized response is delivered to the analyst alongside a visible **Process Log** (an auditable execution trace displaying the exact capabilities, plugins, and safety checks executed during query processing).

#### Sessions and Session Management

A **Session** represents a single continuous conversation within Security Copilot. Within a session, analysts interact with prompt-response pairs using built-in UI controls:

- **Pin**: Saves a prompt-response pair to the **Pin Board** (a split-view panel that collects key investigation evidence, generates automated session summaries, allows title edits, and supports exporting to Microsoft Word, email, or clipboard, as well as link-based sharing with authorized colleagues).
- **Edit, Rerun, and Delete**: Modifies prompt text, re-executes the query, or deletes individual prompts from the active thread.
- **Export and Copy**: Copies response text or exports prompt outputs for external reporting.
- **Feedback Mechanism**: Allows analysts to grade responses using thumbs up/down icons or selecting options (**Looks right**, **Needs improvement**, or **Inappropriate**) to refine model accuracy.

#### Plugins and Capabilities

A **Plugin** is a software connector that links Security Copilot to specific data sources, exposing modular **Capabilities** (individual specialized software functions or skills designed to execute specific tasks, such as summarizing an incident or analyzing code).

Plugins are organized into four categories:

- **Microsoft Plugins**: Preinstalled connectors for Microsoft security tools, including Defender XDR, Microsoft Sentinel, Entra ID Protection, Intune, Purview, Defender for Cloud, Defender for Threat Intelligence, and Defender External Attack Surface Management.
- **Other Plugins**: Integrations for third-party security vendors, including ServiceNow, Splunk, CrowdSec, GreyNoise, Tanium, Netskope, Cyware, and UrlScan, requiring vendor licenses and authentication credentials (e.g., API keys).
- **Website Plugins**: Anonymous, pre-configured connectors that fetch open-source threat intelligence and public web data without requiring setup credentials.
- **Custom Plugins**: Developer-built skills uploaded using a **YAML** (Yet Another Markup Language, a human-readable data format used for configuration files) or **JSON** (JavaScript Object Notation, a lightweight text format used for data interchange) manifest file formatted to OpenAI API or native Copilot specifications.

To use the **Microsoft Sentinel** plugin, users must configure specific setup parameters: the default workspace name, subscription name, and resource group name.

```
Descriptor:
  Name: CustomThreatLookup
  DisplayName: Custom Threat Intelligence Lookup
  Description: Queries internal threat database for IP reputation
Skills:
  - Name: QueryIP
    DisplayName: Query IP Address
    Description: Fetches threat score for a given IP address
    Inputs:
      - Name: IPAddress
        Type: String
        Required: true
```

#### Knowledge Base Connections

Analysts can augment Security Copilot's reasoning by connecting organizational knowledge bases through two methods:

- **File Upload**: Uploading static text files (DOCX, MD, PDF, TXT) up to 3 MB per file, up to a total of 20 MB per user. Uploaded files are stored in the Security Copilot service boundary within the tenant's home geo and remain private to the uploading user account. Prompts must explicitly include phrases like "uploaded files" or the specific file name (e.g., "Based on my uploaded file Contoso_Policy.pdf...") to invoke file reasoning. Owner settings control upload permissions (**No one** vs. **Contributors and Owners**).
- **Azure AI Search Plugin**: Connects Copilot to an **Azure AI Search** (an AI-powered information retrieval service that indexes proprietary documents for vector searching) index. The target index must be configured with a searchable text field, a filterable title field, and a vector field using the `text-embedding-ada-002` embedding model. Prompts must explicitly mention "Azure AI Search" to trigger index searching.

#### Custom Promptbooks

A **Promptbook** is a pre-configured sequence of prompts designed to automate repeatable investigation workflows. Analysts can create custom promptbooks from an existing session by selecting prompt check boxes, clicking **Create promptbook**, defining a title, tag, and description, and parameterizing dynamic inputs using angle brackets without spaces (e.g., `<IncidentID>`, `<number>`, or `<threatactorname>`).

Promptbooks include a **Continue on failure** toggle that allows subsequent prompts in the sequence to run even if an earlier prompt returns no data or fails.

```
[ Select Prompts in Existing Session ]
                 │
                 ▼
[ Click "Create Promptbook" Button ]
                 │
                 ▼
[ Define Name, Description, and Tags ]
                 │
                 ▼
[ Parameterize Inputs: <IncidentID>, <username> ]
                 │
                 ▼
[ Enable "Continue on Failure" Toggle ]
                 │
                 ▼
[ Save & Share with Organization or Keep Private ]
```

#### Standalone Experience UI Landmarks

The standalone experience (`https://securitycopilot.microsoft.com/`) serves as the central landing portal, featuring key UI landmarks:

- **Navigation Panel**: Access to Agents, Promptbooks, Build (preview agent creation workspace), History (retains past sessions; deleted sessions have a 30-day **TTL** [time-to-live, the expiration window before deleted data is permanently purged], while underlying audit logs are retained up to 90 days), Owner settings, Plugin settings, Role assignments, Manage workspaces, Usage monitoring (a 90-day dashboard tracking SCU consumption by workspace, plugin, and user; displays a 90% capacity warning banner to analysts and blocks prompts if provisioned capacity is exceeded without overage), and the **Security Store** (a storefront for discovering and deploying Microsoft and partner AI agents).
- **Ellipses Menu (`...`)**: Bottom-left navigation control providing user Preferences (theme, language, time zone, response debug level), Help widget (self-help articles and ticket submission requiring **Service Support Administrator** or **Helpdesk Administrator** roles), and the **Tenant Switcher** (allowing analysts to switch operational contexts between authorized enterprise tenants).
- **Prompts to Try**: Filterable home screen cards organized by role (CISO, SOC Analyst, Threat Intel Analyst) and plugin.
- **Prompt Bar**: Text input bar containing the Prompt icon (to view system capabilities and prompt suggestions), Sources icon (to manage enabled plugins and uploaded files), and Run icon.

While the standalone interface manages multi-product investigations from a central desk, the detective can also step directly onto active crime scenes embedded inside specific security portals.

### On the Active Crime Scene: The Embedded Experience Across Microsoft Security Tools

The **Embedded Experience** places Security Copilot capabilities directly within native Microsoft security product interfaces. Embedded Copilot directly invokes product-specific capabilities to deliver processing efficiency while allowing analysts to transition seamlessly to the standalone portal by selecting **Open in Security Copilot**.

```
┌────────────────────────────────────────────────────────────────────────┐
│                     SECURITY COPILOT EMBEDDED FOOTPRINT                │
├───────────────────┬───────────────────┬────────────────┬───────────────┤
│   DEFENDER XDR    │  MICROSOFT PURVIEW│ MICROSOFT ENTRA│MSFT INTUNE /  │
│                   │                   │                │DEFENDER CLOUD │
├───────────────────┼───────────────────┼────────────────┼───────────────┤
│• Incident Summaries│• DSPM & DLP Alert │• Risky User    │• Policy       │
│  (up to 100 alerts)│  Summaries        │  Summaries     │  Tooltips &   │
│• Guided Responses │• Insider Risk     │• App Risk      │  Summaries    │
│  (Triage/Remed)   │  Activity Profiles│  Assessments   │• Error Code   │
│• Script Analysis  │• Natural Language │• PIM Just-In-  │  Analyzer     │
│  (PowerShell/Bash)│  Filter Generation│  Time Role     │• IaC Pull     │
│• NL-to-KQL Hunting│• eDiscovery       │  Activation    │  Request Fixes│
│• File Summaries   │  Review Summaries │• Network Usage │  (DevOps)     │
└───────────────────┴───────────────────┴────────────────┴───────────────┘
```

#### 1. Copilot in Microsoft Defender XDR

Embedded within `https://security.microsoft.com/`, Copilot assists threat investigations across core workflows:

- **Incident Summarization**: Automatically synthesizes complex incidents containing up to 100 correlated alerts into a single natural language summary detailing attack progression, impacted assets, threat actor attribution, and **IOCs** (indicators of compromise, technical clues such as file hashes or IP addresses indicating a breach).
- **Guided Responses**: Recommends contextualized, step-by-step response actions categorized into **Triage**, **Containment**, **Investigation**, and **Remediation**. Administrators can upload custom organizational guidelines to tailor recommendations.
- **Script and Command-Line Analysis**: Translates obfuscated PowerShell, Batch, and Bash scripts into plain English, explaining execution intent and mapping activities to the **MITRE ATT&CK** matrix. Analysts can select **Show code** to view matching code lines.
- **Natural Language to KQL**: Converts natural language questions entered in advanced hunting into executable **KQL** (Kusto Query Language, a query language used to search raw security log databases) queries.
- **Create Incident Reports**: Consolidates timestamps, assigned analysts, classifications, automated playbooks, and remediation steps into a formal, auditable report.
- **File, Device, and Identity Summaries**: Summarizes file certificates, string extracts, and API calls on file pages; compiles **ASR** (attack surface reduction, security rules that block common attack vectors) status, tamper protection, and vulnerabilities on device pages; and surfaces account criticality, role changes, and sign-in risks on identity pages.
- **Threat Intelligence Integration**: Consolidates threat analytics reports, actor profiles, and vulnerability disclosures via the Microsoft Defender Threat Intelligence plugin.

#### 2. Copilot in Microsoft Purview

Embedded within `https://purview.microsoft.com/`, Copilot assists compliance and data security admins (requiring the **Allow Security Copilot to access data from your Microsoft 365 services** setting enabled):

- **Data Security Posture Management (DSPM)**: Executes built-in promptbooks (**Risky user investigation** and **Sensitive data protection**) to track unauthorized data transfers across SharePoint, OneDrive, and Exchange.
- **Data Loss Prevention (DLP)**: Summarizes DLP alerts from the alerts queue and explains active policy configurations.
- **Insider Risk Management**: Summarizes user exfiltration patterns, user roles, and activity timelines for high-risk users.
- **Activity Explorer**: Converts natural language prompts (e.g., "Filter files copied to cloud with credit card SIT for past 30 days") into active filter sets.
- **Communication Compliance**: Generates contextual summaries of flagged messages and attachments (for items with a combined length of 100+ words) based on trainable classifiers.
- **eDiscovery**: Summarizes text-extractable evidence items in review sets, executes natural language case searches, and drafts case scope summaries.

#### 3. Copilot in Microsoft Entra

Embedded within `https://entra.microsoft.com/`, Copilot enhances identity administration:

- **Microsoft Entra ID**: Investigates user properties, group ownerships, sign-in failures, and Conditional Access policy coverage gaps.
- **Microsoft Entra ID Protection**: Summarizes risky user profiles and evaluates risk levels for workload identities (service principals and app registrations) to discover excessive permissions or unused apps.
- **Identity Governance**: Analyzes access review decision overrides, queries entitlement management packages, summarizes **PIM** (Privileged Identity Management, an Entra service that controls elevated administrative access) role changes, supports direct just-in-time role activation within chat, and guides **Lifecycle Workflows** for joiner, mover, and leaver scenarios.
- **Global Secure Access**: Analyzes user and device network traffic patterns across Internet Access and Private Access solutions.
- **Data Exploration Grid**: Returns an **Open list** button when query results exceed 10 items, rendering a comprehensive data grid that displays the underlying **Microsoft Graph API** URL.

#### 4. Copilot in Microsoft Intune

Embedded within `https://intune.microsoft.com/` (included with Security Copilot licensing):

- **Policy and Setting Management**: Displays inline Copilot tooltips next to settings in Compliance, Device Configuration, and Endpoint Security policies showing recommended values, conflict checks, and user impact; generates full policy summaries.
- **Device Details and Troubleshooting**: Provides an error analyzer for hex codes, compares device configurations side-by-side, and details app/policy assignments.
- **Data Exploration and Device Query**: Converts natural language requests into device query KQL scripts (requires Intune Advanced Analytics licensing).
- **Specialized Workloads**: Analyzes **EPM** (Endpoint Privilege Management, an Intune feature allowing standard users to run approved administrative tasks) elevation requests, automates Surface device troubleshooting, and delivers performance/resizing recommendations for **Windows 365 Cloud PCs**.

#### 5. Copilot in Microsoft Defender for Cloud

Embedded within the Azure portal for cloud infrastructure security:

- **Dual-Platform Architecture**: Prompts are initially evaluated by Copilot for Azure. If the query requires security analysis, it invokes a Security Copilot skill (which consumes SCUs). Non-security administrative queries handled entirely by Copilot for Azure do not consume SCUs.
- **Recommendations Analysis**: Features an **Analyze with Copilot** control on the recommendations page to filter risks by public exposure, sensitive data, or resource criticality. Enabling the **DCSPM** (Defender Cloud Security Posture Management, an advanced security plan in Defender for Cloud providing attack path analysis) plan unlocks full attack path reasoning.
- **Remediation and Delegation**: Summarizes resource vulnerabilities, generates step-by-step remediation scripts, and drafts delegation emails to resource owners.
- **Infrastructure as Code (IaC) Remediation**: Automatically generates a **PR** (pull request, a formal code change submission in a repository) in Azure DevOps repositories to fix security misconfigurations in Terraform or ARM templates.

Beyond assisting human guards at active crime scenes, the detective can deploy autonomous robotic deputies to execute background security tasks continuously.

### Autonomous Robotic Deputies: Security Copilot Agents and the Security Store

To handle high-volume, repetitive operational tasks without manual human intervention, Security Copilot introduces **Agents** (autonomous, always-on AI routines built to perform specific security and IT management workflows at scale).

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SECURITY COPILOT AGENTS                         │
├──────────────────────────┬─────────────────────────────────────────────┤
│ AGENT NAME               │ CORE OPERATIONAL WORKFLOW                   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Phishing Triage Agent    │ Automatically analyzes, classifies, and     │
│                          │ resolves user-reported phishing emails in   │
│                          │ Defender for Office 365 using LLM reasoning.│
├──────────────────────────┼─────────────────────────────────────────────┤
│ Threat Intelligence      │ Dynamically scans global threat feeds and   │
│ Briefing Agent           │ generates customized vulnerability reports. │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Threat-Hunting Agent     │ Proactively executes background log queries │
│                          │ to surface hidden threat actor activity.    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Conditional Access       │ Identifies policy coverage gaps for new     │
│ Optimization Agent       │ applications and users in Microsoft Entra.  │
└──────────────────────────┴─────────────────────────────────────────────┘
```

Agents operate securely within Microsoft's Zero Trust security framework, learning from analyst feedback and executing tasks based on automated triggers or on-demand invocations.

Organizations manage and deploy agents through the **Security Store** (a security-optimized storefront integrated into the standalone Copilot portal that allows organizations to discover, deploy, and manage Microsoft and partner-built AI agents and solutions). Developers can also build custom agents using the **Build (preview)** interface in the navigation panel, constructing agent logic from scratch or uploading a structured YAML manifest file.

Key pre-built agents include:

- **Phishing Triage Agent**: Triggered automatically when an employee reports a suspicious email in Outlook. Operating under a dedicated **Entra Agent ID** (a dedicated service principal identity for AI agents), the agent evaluates headers, body text, URL detonations, and screenshots using LLMs, auto-resolving false positives while escalating verified phishing threats to human analysts.
- **Threat Intelligence Briefing Agent**: Continuously cross-references tenant vulnerability inventories against global threat actor activity to publish executive briefings on the Threat Analytics dashboard.
- **Threat-Hunting Agent**: Proactively executes background KQL queries across log tables to detect subtle adversary techniques that bypassed standard alert tripwires.
- **Conditional Access Optimization Agent**: Operates within the Microsoft Entra admin center, scanning access logs to identify unmonitored applications or exposed user populations and recommending optimized policy rules.

---

## Connecting the Dots

To transform chaotic security telemetry into clear operational clarity, every component of Microsoft Security Copilot snaps together into a unified AI-powered defense system.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SECURITY COPILOT SYSTEM ARCHITECTURE                            │
│                                                                                        │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ AI Brain & LLMs       │   │ Prompt Engineering     │   │ Capacity & Onboarding   │  │
│  │ (Azure OpenAI GPT-4o) │──►│ (Goal, Context, Source)│──►│ (Provisioned SCUs/Geo)  │  │
│  └───────────────────────┘   └────────────────────────┘   └───────────┬─────────────┘  │
│                                                                       │                │
│                                                                       ▼                │
│  ┌───────────────────────┐   ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Specialized Plugins   │◄──│ Central Orchestrator   │──►│ Workspaces & RBAC       │  │
│  │ (M365, Sentinel, 3rd) │   │ (Execution Planner)    │   │ (Owner / Contributor)   │  │
│  └───────────┬───────────┘   └───────────┬────────────┘   └─────────────────────────┘  │
│              │                           │                                             │
│              ▼                           ▼                                             │
│  ┌────────────────────────┐  ┌────────────────────────┐   ┌─────────────────────────┐  │
│  │ Embedded Experience    │  │ Standalone Portal      │   │ Autonomous Agents       │  │
│  │ (Defender, Purview,    │  │ (Multi-Tool Master     │   │ (Phishing Triage, Threat│  │
│  │  Entra, Intune, MDC)   │  │  Desk & Pin Board)     │   │  Intel Briefing Agent)  │  │
│  └────────────────────────┘  └────────────────────────┘   └─────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **The AI Brain and Prompting**: The system is powered by **LLMs** hosted securely in **Azure OpenAI Services**. Human guards communicate with the detective through **Prompt Engineering**, structuring queries with explicit Goals, Context, Expectations, and Sources while leveraging **RAG** and **Grounding Data** to ensure outputs remain anchored in real incident logs.
2. **Onboarding, Billing, and Governance**: Management provisions computing capacity measured in **SCUs** across specific **Workspaces** to enforce budget control and regional geo-residency compliance. Access is governed using **Copilot Owner** and **Copilot Contributor** platform roles, operating on an **OBO** authentication model that preserves individual user permissions across underlying security databases.
3. **The Central Engine and Modular Toolkit**: When a guard submits a prompt, the **Orchestrator** generates an execution plan, queries connected **Plugins** (Microsoft, third-party, website, or custom YAML manifests), and executes skills. Analysts track investigations within **Sessions**, saving key evidence to the **Pin Board** and creating reusable parameterized **Promptbooks** to automate multi-step workflows.
4. **Embedded and Standalone Execution**: On active crime scenes, the detective operates through the **Embedded Experience**—summarizing multi-alert incidents, reverse-engineering obfuscated PowerShell scripts, converting natural language into **KQL** queries, analyzing **DLP** and **Insider Risk** alerts in Purview, evaluating **Conditional Access** gaps in Entra, generating Intune settings tooltips, and drafting Infrastructure as Code pull requests in Defender for Cloud. For complex cross-product investigations, guards transition to the **Standalone Experience** master portal.
5. **Autonomous Background Patrols**: To handle continuous operational labor, management deploys autonomous **Security Copilot Agents** from the **Security Store**—such as the **Phishing Triage Agent** and **Conditional Access Optimization Agent**—to triage reported emails and optimize identity posture automatically. When all these components work in unison, the security team stops drowning in raw logs and begins defending the digital campus at true machine speed.