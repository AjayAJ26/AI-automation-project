# 🛡️ Autonomous Data Quality Anomaly Detection & Self-Healing Pipeline with Human-in-the-Loop: n8n + PostgreSQL + Google Gemini + Google Workspace

*An enterprise AI Agent workflow built in n8n that autonomously audits PostgreSQL databases for anomalies, drafts remedial SQL corrections, requests human approval via interactive Gmail notifications, executes verified fixes, and generates executive briefing reports in Google Docs.*

---

## ❗ Problem Statement  

Data integrity issues—such as orphaned keys, broken constraints, missing records, and schema discrepancies—are widespread in dynamic data warehouses:

- Database health audits are typically manual, scheduled intermittently, or detected only after downstream dashboards break  
- Diagnosing the root cause of complex relational inconsistencies requires senior engineering bandwidth  
- Fully autonomous automated remediation without human oversight introduces major risks of accidental data corruption or catastrophic deletion  
- Documenting incidents, resolution steps, and audit logs for leadership is tedious and frequently neglected  

To resolve these challenges, this project introduces a **multi-agent, self-healing pipeline with Human-in-the-Loop (HITL) safeguards**, balancing autonomous root-cause remediation with mandatory human verification.

---

## 📝 Project Overview  

This project demonstrates a production-grade multi-agent workflow orchestrated in **n8n** and powered by **Google Gemini Chat Models**. The pipeline spans four distinct modular phases:
1. **Investigate the DB & list down the issues**
2. **HITL approval process**
3. **Fix the issues using SQL**
4. **Create an executive summary**

### 🎯 Objective  
- Autonomously query and evaluate relational database tables (PostgreSQL) for anomalies  
- Leverage an AI Agent to perform deep root-cause diagnosis and write remediation SQL scripts  
- Enforce strict Human-in-the-Loop governance via interactive Gmail approval nodes before changes apply  
- Safely execute approved SQL fixes and publish automated incident summaries to Google Docs and leadership emails  

---

## 🚀 Solution  

Architected an end-to-end self-healing architecture:

1. **Investigation Phase:** Executes initial schema checks on PostgreSQL, loops through anomalies, and deploys an **AI Agent** (backed by Google Gemini and a PostgreSQL execution tool) to inspect tables, write diagnostic code in JavaScript, and log issues.  
2. **HITL Approval Phase:** Splits detected anomalies, filters critical actions, routes via `If` conditional logic, and sends an interactive **Gmail message with wait-for-response** to the data team for authorization.  
3. **Remediation Phase:** Upon affirmative human approval, loops through approved correction tasks, passes execution logic through **AI Agent 1**, and runs verified remediation scripts via `Execute a SQL query in Postgres`.  
4. **Executive Summary Phase:** Merges and aggregates audit streams, engages **AI Agent 2** to draft an executive resolution debrief, creates/shares a **Google Doc** via Drive, and dispatches a final summary alert via Gmail.

### 🛠️ Tools & Technologies  
- **n8n** (Multi-Agent Workflow Orchestration & Human-in-the-Loop State Engine)  
- **Google Gemini Chat Model** (Cognitive Diagnostic, Remediation & Incident Synthesis Agents)  
- **PostgreSQL Database** (Data Warehouse & Targeted Anomaly Inspection/Repair)  
- **Gmail (n8n Interactive Nodes)** (Approval Gates & Stakeholder Alerting)  
- **Google Docs & Google Drive API** (Automated Report Creation & Document Sharing)  
- **JavaScript** (Custom Payload Normalization & Condition Logic)  

---

## 📸 Workflow & Execution

### 1. Four-Stage Architecture Flow
```text
┌────────────────────────────────────────────────────────┐
│      PHASE 1: INVESTIGATE THE DB & LIST ISSUES         │
│  PostgreSQL Check ──► Loop ──► AI Agent (Gemini)       │
│  └── Connected Tools: Postgres Query + JS Scripts      │
└──────────────────────────┬─────────────────────────────┘
                           │
┌──────────────────────────▼─────────────────────────────┐
│             PHASE 2: HITL APPROVAL PROCESS             │
│  Split Out ──► Filter / If ──► Gmail (Wait Response)   │
└──────────────┬───────────────────────────┬─────────────┘
               │ (Approved)                │ (Branch / Audit)
┌──────────────▼─────────────┐ ┌───────────▼─────────────┐
│  PHASE 3: FIX USING SQL    │ │ PHASE 4: EXEC SUMMARY   │
│  Loop ──► AI Agent 1       │ │ Merge ──► Aggregate     │
│  └── Execute SQL (Postgres)│ │ ──► AI Agent 2 (Gemini) │
│                            │ │ ──► Create Google Doc   │
│                            │ │ ──► Send Final Gmail    │
└────────────────────────────┘ └─────────────────────────┘
```

### 2. workflow
<img width="800" height="205" alt="image" src="https://github.com/user-attachments/assets/75501f3a-0ccd-4fc5-ae13-cab49c0a6e59" />

### 3. Output report
<img width="299" height="289" alt="image" src="https://github.com/user-attachments/assets/7355024d-5300-44ac-a75f-3910ee0cb2ed" />

---

## 📌 Detailed Pipeline Stages & Nodes  

### Phase 1: Investigate the DB & List Down the Issues
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **When clicking 'Execute workflow'** | Trigger | Manually or periodically triggers the database health audit | Schedule / On-Demand |
| **Execute a SQL query** | Database | Runs diagnostic checks to isolate dirty rows, orphans, or constraint faults | Targeted validation queries |
| **Loop Over Items** | Flow Control | Iterates through each identified data anomaly batch | Batching controller |
| **AI Agent** | Cognitive Agent | Reason over error contexts, query schemas, and propose fixes | Conversational diagnostic agent |
| **Google Gemini Chat Model** | LLM Engine | Provides analytical reasoning for SQL anomaly evaluation | Connected to Google AI Studio |
| **Execute a SQL query in Postgres**| Agent Tool | Grants the AI agent read-only access to verify table state | Secondary schema interrogation |
| **Code in JavaScript** | Transformation | Cleans and formats structured diagnostic payloads | JSON data restructuring |
| **Wait** | Flow Control | Rate limits and stabilizes iterative LLM inspection runs | Timed throttling buffer |

### Phase 2: HITL Approval Process
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **Split Out** | Deconstruct | Unpacks arrays of detected issues into individual items | Field expansion |
| **Filter** | Logic | Filters critical versus non-critical anomalies | Priority criteria rules |
| **Send message and wait for response** | Gmail HITL | Sends notification with approval buttons and pauses execution | Webhook callback listener |
| **If** | Conditional | Branches workflow based on human action (`Approved` vs `Rejected`) | Boolean state gatekeeper |
| **No Operation, do nothing** | Flow Control | Gracefully exits or logs rejected operations | Fallback endpoint |
| **Edit Fields** | Transformation | Normalizes payload parameters for remediation execution | Metadata tagging |

### Phase 3: Fix the Issues Using SQL
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **Split Out 1 & Loop Over Items 1** | Flow Control | Iterates over confirmed remediation actions | Batch queue |
| **Send message and wait for response 1** | Gmail | Secondary validation or execution confirmation alert | Confirmation safeguard |
| **If 1** | Conditional | Ensures remediation parameters match database sanity checks | Final safety check |
| **AI Agent 1** | Cognitive Agent | Generates deterministic, safe DDL/DML correction syntax | Safe mode code generator |
| **Google Gemini Chat Model 1** | LLM Engine | Formulates targeted rollback and commit SQL logic | Code generation |
| **Execute a SQL query in Postgres 1**| Database Tool | Executes verified SQL corrections directly on the target table | DML execution |

### Phase 4: Create an Executive Summary
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **Merge & Aggregate** | Data Consolidator | Combines run logs from Phase 1, Phase 2, and Phase 3 | Aggregates execution stream |
| **AI Agent 2** | Cognitive Agent | Compiles technical audit logs into business-friendly executive debriefs | Reporting synthesis agent |
| **Google Gemini Chat Model 2** | LLM Engine | Summarizes root causes, business impact, and resolution metrics | Markdown synthesis |
| **Create / Update Google Docs** | Google Docs Tool | Generates a new documented audit log in Google Docs | Drive folder placement |
| **Share the File in Google Drive** | Google Drive Tool| Adjusts sharing permissions for designated management stakeholders | Read permissions granted |
| **Send a message** | Gmail | Dispatches direct email briefing containing Google Doc link to stakeholders| Executive notification email |

---

## 📑 Remediation & Reporting Audit Trail

### 1. Sample Anomaly Log & Remediation Action
| Incident_ID | Target_Table | Anomaly_Type | Severity | Proposed Remediation | Human Approval | Execution Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `INC-8801` | `dim_customer` | Orphaned records without Market mapping | High | Reassign NULL market keys to default region code `N/A` | APPROVED | COMPLETED |
| `INC-8802` | `fact_sales_monthly`| Negative transaction unit price values | Critical | Set unit price to baseline standard catalogue price | APPROVED | COMPLETED |
| `INC-8803` | `fact_forecast_monthly`| Duplicate monthly forecast records | Medium | Drop secondary duplicate rows retaining latest timestamp | REJECTED | ABORTED |

---

## 🌟 Technical Impact  

- **Autonomous Threat Discovery:** Identifies hidden data corruption and anomaly spikes instantly without manual analyst queries  
- **Safe HITL Operations:** Prevents catastrophic automated updates by forcing human approval via email before any `UPDATE` or `DELETE` executes  
- **Full Traceability:** Every step—from initial diagnosis to operator approval and final SQL execution—is tracked end-to-end  
- **Executive Communication:** Closes the gap between engineering and leadership by turning raw SQL logs into readable Google Docs summaries  

---

## 📁 Repository Structure  

```text
├── workflows/
│   └── autonomous_dq_remediation_pipeline.json  # Exported n8n workflow file
├── docs/
│   └── workflow_canvas.png                     # Canvas screenshot of the multi-agent pipeline
├── sql/
│   ├── sample_anomalies_schema.sql             # Test tables with intentionally introduced dirty data
│   └── audit_queries.sql                       # Base inspection queries run by Phase 1
├── templates/
│   └── executive_summary_template.md           # Prompt template used by AI Agent 2
├── .env.example                                # Template for API keys & DB credentials
├── .gitignore
└── README.md
```

---

## ⚙️ How to Run This Project  

### 1. Prerequisites
- An active instance of [n8n](https://n8n.io/) (version supporting Advanced AI, LangChain, and Wait nodes).
- A connected **PostgreSQL** database with read/write credentials.
- A **Google Gemini API** key (via Google AI Studio).
- A Google account with **Gmail**, **Google Docs**, and **Google Drive** APIs enabled.

### 2. Configure Credentials in n8n
1. **PostgreSQL Credentials:** Host, Database, Port (`5432`), Username, and Password.
2. **Google Gemini (PaLM / Gemini API):** API Key.
3. **Gmail OAuth2:** Connect account for dispatching approval requests and executive alerts.
4. **Google Docs & Google Drive OAuth2:** Connect account for creating and sharing audit reports.

### 3. Import & Configure Workflow
1. In n8n, navigate to **Workflows** > **Import from File**.
2. Select `workflows/autonomous_dq_remediation_pipeline.json`.
3. In Phase 2 and Phase 3, update the **Send message and wait for response** nodes with your designated approver's email address.
4. In Phase 4, configure your target Google Drive folder ID for saving incident reports.
5. Click **Execute workflow** to run an initial end-to-end test.

---

## 🙏 Acknowledgements  

- [Codebasics](https://codebasics.io/) — Dhaval Patel and Hemanand Vadivel for championing practical AI automation and applied enterprise analytics architectures.  
- [n8n.io](https://n8n.io/) for their modular AI Agent and Human-in-the-Loop orchestration tooling.  
- **Google Gemini** for providing fast reasoning, Text-to-SQL logic, and multi-agent incident reporting capabilities.  

---

## 📌 Conclusion  

This project demonstrates:  
- Building an autonomous, self-healing data engineering system using visual agent orchestration  
- Implementing enterprise Human-in-the-Loop (HITL) checkpoints to safely approve AI-generated code  
- Streamlining database maintenance and executive reporting across heterogeneous modern tools  

---

## 📬 Contact  

🔗 **LinkedIn:** www.linkedin.com/in/ajay-jadhav-8381aa347  
📧 **Email:** aj.ajayjadhav01@gmail.com  

---

⭐ *If you found this project helpful, consider giving it a star!*
