```
1. THE BIG PICTURE: CATCHING THE GHOST WITH THE KEYS
├── The Scenario & The Problem
│   ├── The Illusion of Perimeter Security: It's 3 AM on a Sunday. Perimeter firewalls are holding strong, Microsoft Defender shows zero external threats, and physical guards are drinking coffee. Everything looks perfectly quiet.
│   ├── The Nightmare: At that exact moment, your most trusted senior developer ("Jane") is quietly downloading your unreleased Q4 source code onto a personal thumb drive.
│   └── The Blind Spot: Front-gate guards are completely useless because they are looking outward. You cannot catch a "ghost" that already has the keys to the building by waiting for a perimeter alarm to blink red.
├── The Solution: Microsoft Purview (Looking Inward)
│   ├── The overarching umbrella brand for managing data security, risk, and compliance (accessed at purview.microsoft.com).
│   ├── Focuses on *actions and content*, not just malware code (e.g., "Why is the CFO's assistant photocopying the earnings report at 3 AM?").
│   └── The Dual Architecture
│       ├── Internal Affairs: The compliance and data security team monitoring employee behaviors for policy violations and data theft.
│       └── Corporate Archive: The unified audit logging and evidence preservation vault that securely records every digital action for legal review.
2. THE MAILROOM CHECKPOINT: DATA LOSS PREVENTION (DLP)
├── Concept: Automated inspection guards standing at every office door, mail chute, and USB port, examining every package before it leaves the building.
├── Identification Method 1: Sensitive Information Types (SITs)
│   ├── The guard opens the envelope to look for specific text patterns (e.g., Credit Card Numbers).
│   └── Preventing False Positives (The Granular Check):
│       ├── Primary Evidence: Regex patterns matching the structural format.
│       ├── Checksum Validation: Runs a mathematical algorithm (e.g., Luhn algorithm for credit cards) before alarming.
│       └── Secondary Evidence: Checks for proximity keywords (e.g., "Visa", "Expiration Date" within 300 characters).
├── Identification Method 2: Sensitivity Labels
│   ├── The digital "Highly Confidential" red watermark stamped directly on the outside of the file.
│   ├── Label Inheritance: If a user copies text from a labeled document and pastes it into a blank file, the new file automatically inherits the "Highly Confidential" status (the label travels with the data).
│   └── Downgrade Protection: If an employee tries to change a label from "Confidential" to "Public" to sneak it out, the system forces them to type out a written justification, instantly generating a high-severity audit log.
├── Managing Alert Fatigue (Single vs. Aggregate)
│   ├── Single-Event Alerts: Triggered immediately for undeniable high-risk actions (e.g., emailing 5,000 patient health records).
│   └── Aggregate-Event Alerts: Triggered only when a threshold is met (e.g., 15 project codes shared in 60 minutes) to highlight deliberate exfiltration while ignoring accidental human error.
├── The Alert Lifecycle (6 Phases)
│   └── 1. Trigger -> 2. Notify (Purview & Defender XDR) -> 3. Triage -> 4. Investigate (Activity Explorer, Content Explorer) -> 5. Remediate -> 6. Tune.
└── The Automated Sorting Clerk: DLP Triage Agent (Preview)
├── An AI agent powered by Security Copilot that evaluates active alerts.
├── Analyzes Content Risk, Exfiltration Risk (destination matters), and Policy Risk.
└── Categorizes the massive queue into: [Needs Attention], [Less Urgent], and [Not Categorized] so humans focus on critical threats first.
3. WATCHING INTERNAL AFFAIRS: INSIDER RISK MANAGEMENT
├── Concept: Shifting from transactional events (DLP) to tracking behavioral patterns ("low and slow" data theft) over time.
├── Privacy Protection: Pseudonymization
│   ├── A strict GDPR/privacy requirement. By default, investigators do not see "Jane Smith"; they see an anonymized string like "User 8X9".
│   └── Prevents investigator bias. True identity is only unmasked if the behavioral threat is definitively confirmed and HR/Legal grants approval.
├── The 5-Step Alert Generation Engine
│   ├── 1. Settings Configured: Establish privacy, audit logs, and risk indicators.
│   ├── 2. Policy Created: Tell the system who/what to watch (e.g., "Data theft by departing employees").
│   ├── 3. Triggering Event: The system wakes up (e.g., HR portal registers Jane submitting her two-week resignation notice).
│   ├── 4. User Activity Evaluated & Scored: Machine learning compares current actions against historical baselines (e.g., Jane usually downloads 5 files a day; today she downloaded 500. Risk score spikes).
│   └── 5. Alert Generated: Fired into the queue only if the accumulated risk score breaches the threshold.
├── Visual Investigation UI (Parsing the Data)
│   ├── All Risk Factors Tab: The executive summary (top exfiltration activities).
│   ├── Activity Explorer Tab: The raw forensic ledger.
│   └── User Activity Tab (The Scatter Plot Timeline)
│       ├── An interactive visual map spanning 1, 3, or 6 months.
│       ├── Bubbles = Scored events (height = risk score; size = volume; color = activity type like blue for benign access, red for exfiltration).
│       └── Connecting Lines (Risk Sequences) = Visually connects the intent (e.g., Accesses code -> downgrades label -> transfers to personal cloud). Demystifies the entire narrative.
└── Escalation
└── Confirm all alerts & create Case: Formally transitions the event from a security investigation to an incident response, bringing in HR and Legal.
4. THE LEGAL VAULT: eDISCOVERY & LEGAL HOLDS
├── Concept: When Legal/HR gets involved, a screenshot of a scatter plot isn't enough. They need hard, legally defensible, tamper-proof evidence (Chain of Custody).
├── Securing the Vault Doors (RBAC Isolation)
│   ├── Principle of Least Privilege: Even a Global IT Administrator does NOT have access to read the CEO's emails by default.
│   ├── eDiscovery Manager: Can create cases and search data, but only sees cases they created or were invited to.
│   └── eDiscovery Administrator: Can view all cases tenant-wide.
├── The 4-Phase Search Process
│   ├── 1. Define Criteria: Name and initial scope.
│   ├── 2. Identify Sources: Narrow the haystack (target specific OneDrive URLs or mailboxes instead of searching a 10,000-person company).
│   ├── 3. Build the Query: Uses KeyQL (Keyword Query Language).
│   │   └── Copilot Translation: Instead of risking a missed parenthesis in complex boolean logic, investigators type natural language ("Find emails from Jane about Project Phoenix"), and Copilot generates perfect KeyQL.
│   └── 4. Run & Review: System provides statistical samples to check your work (ensuring you didn't accidentally pull 40,000 irrelevant newsletters) before exporting.
└── Preserving Chain of Custody (Export & Cryptographic Hashes)
├── Export Format: PST (Personal Storage Table) bundling emails, metadata, and folders into one file. Fixes 259-character path limits with "Condense paths".
└── Cryptographic Hash (The Digital Wax Seal):
├── Runs the exported file through SHA-256 to spit out a fixed mathematical fingerprint.
└── If opposing counsel claims IT altered a single comma to make the employee look guilty, rehashing the file will produce a radically different string. If the hashes match, the evidence is mathematically proven to be perfectly intact.
5. SECURITY CAMERA TAPES: MICROSOFT PURVIEW AUDIT
├── Concept: The underlying engine recording every click, download, and login. If you need to know exactly who did what, you rewind these tapes.
├── Fencing Visibility: Administrative Units (AUs)
│   └── A logical container that ensures the Berlin IT auditor can only rewind tapes for the German campus, keeping them cryptographically blind to the Seattle CEO's logs.
├── Audit Tiers & Retention (Cost vs. Risk)
│   ├── Standard Audit: 180-day retention. Good for immediate response, but fails against long-term, patient insiders whose entry logs get overwritten.
│   └── Premium Audit: 1-year default, custom policies up to 10 years. (Storing 10 years of logs for every intern is computationally/financially impossible; apply strategically to high-value targets).
├── Deep Forensics: MailItemsAccessed (The Holy Grail)
│   ├── Prevents catastrophic "assume all emails were read" legal breach notifications by proving exactly which emails the attacker actually opened.
│   ├── Sync Access: Background infrastructural download (machine-to-machine).
│   ├── Bind Access: The smoking gun. Proves human interaction (actively clicking/viewing a single specific email).
│   └── 24-Hour Throttling: If a malicious script rapidly opens 10,000 emails, Purview logs the initial Bind event and suppresses the rest for 24 hours to prevent the raw log database from bloating and crashing.
└── AI Surveillance (Watching the Robots)
├── If an employee asks AI to summarize a secret file before stealing the summary, Purview watches the AI.
├── Record Types: CopilotInteraction (1st party), ConnectedAIAppInteraction (Custom/Entra apps; requires DSPM onboarding), AIAppInteraction (3rd party; requires Browser Extension).
└── Metadata: Logs AppIdentity, AccessedResources (what the AI read), and JailbreakDetected (flagging prompt injections/safety bypasses). Does NOT log the raw prompt text for privacy.
6. PARSING EVIDENCE: LOG TRANSFORMATIONS & POWER QUERY
├── The Problem: The "Russian Nesting Doll"
│   └── When you export the raw camera tapes, all crucial details (IPs, file paths) are crammed into a single unreadable CSV cell called `AuditData`, formatted as nested JSON.
├── Method 1: Excel Power Query Editor (Visual Wizardry)
│   ├── 1. Open CSV -> Transform Data
│   ├── 2. Right-click the `AuditData` column -> Transform -> JSON.
│   └── 3. Expand properties: Magically explodes the nested brackets into dozens of neat, distinct columns (SiteUrl, ClientIPAddress), generating a flattened, sortable report.
└── Method 2: PowerShell (`Search-UnifiedAuditLog`)
├── Used by tech-savvy investigators for massive, automated log extraction. Implements Pagination (`-SessionId`) to bypass 5,000-row limits.
└── The Legal Trap (`-Encoding UTF8`):
└── You absolutely MUST append the `-Encoding UTF8` parameter to the export script. If omitted, foreign characters (German umlauts, Mandarin characters) corrupt into gibberish. Handing a spreadsheet of gibberish to a judge renders your evidence legally inadmissible.
7. MASTER MAP: MITIGATING INTERNAL THREATS WITH PURVIEW
├── 1. THE MAILROOM CHECKPOINT (DLP)
│   ├── Tools: SITs (Regex + Checksums), Sensitivity Labels (Downgrade protection/Inheritance).
│   └── Operations: Single vs. Aggregate alerts; AI Triage Agent sorting the queue.
├── 2. INTERNAL AFFAIRS (INSIDER RISK MANAGEMENT)
│   ├── Mindset: Behavioral "low and slow" sequences.
│   ├── Privacy: Pseudonymization (User 8X9).
│   └── Visuals: Scatter Plot Timeline drawing lines between anomalous spikes to reveal intent.
├── 3. THE LEGAL VAULT (eDISCOVERY)
│   ├── Security: Strict RBAC (eDiscovery Manager/Admin).
│   ├── Workflow: 4-Phase Search (Scope, Source, KeyQL/Copilot Query, Sample Review).
│   └── Evidence: Exported as PSTs with SHA-256 Cryptographic Hashes for an airtight Chain of Custody.
├── 4. THE SECURITY CAMERA TAPES (PURVIEW AUDIT)
│   ├── Tiers: Standard (180 days) vs. Premium (1-10 years, High-Value Events).
│   ├── Evidence: `MailItemsAccessed` (Bind vs. Sync; 24-hr throttling) & AI Surveillance logs (`JailbreakDetected`).
│   └── Boundaries: Administrative Units (AUs) scope visibility.
└── 5. PARSING EVIDENCE
└── Extracting nested JSON from the `AuditData` column using Excel Power Query or PowerShell (mandating UTF8 encoding to prevent legally inadmissible gibberish).


```