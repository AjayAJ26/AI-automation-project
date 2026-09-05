# 🤖 Conversational AI Data Analyst Agent: n8n + Google Gemini + SQL Tooling

*An intelligent AI Agent built in n8n that bridges natural language business questions with relational SQL databases using Google Gemini, contextual memory, and automated query execution tools.*

---

## ❗ Problem Statement  

Non-technical stakeholders and business leaders frequently need immediate answers to ad-hoc operational questions:

- Business users must wait for data analysts to write and run manual SQL queries for routine KPIs  
- Slicing through relational tables (such as markets, sales volumes, and historical metrics) requires technical query syntax that non-analysts cannot write  
- Analysts spend valuable hours answering recurring, basic data lookup requests instead of working on deep exploratory analysis  

To resolve this, this project introduces a **Conversational AI Data Analyst Agent** capable of translating plain-English questions into executable SQL queries, querying the database in real time, and returning precise, conversational business insights.

---

## 📝 Project Overview  

This project explores building autonomous AI workflows using **n8n's Advanced AI Agent framework**. Powered by **Google Gemini**, session memory, and SQL execution tools, the agent acts as an autonomous assistant for business intelligence.

### 🎯 Objective  
- Enable natural language querying of relational databases (Text-to-SQL)  
- Maintain multi-turn conversational context using session memory  
- Execute SQL queries securely via agentic tool calling  
- Return formatted, natural-language business answers with exact numbers  

---

## 🚀 Solution  

Developed an **n8n AI Agent architecture** using:
- **When chat message received:** Serves as the interactive conversational interface
- **AI Agent Node:** Serves as the cognitive orchestrator deciding when to consult memory vs. call external tools
- **Google Gemini Chat Model:** Drives reasoning, natural language understanding, and dynamic SQL generation
- **Simple Memory:** Preserves conversation history across user interactions
- **Execute a SQL Query (Tool):** Allows the agent to independently inspect schemas, run analytical SQL queries, and interpret output records

### 🛠️ Tools & Technologies  
- **n8n** (AI Agent Framework & Workflow Orchestration)  
- **Google Gemini** (LLM for Reasoning & Text-to-SQL Translation)  
- **SQL / Relational Database** (Data Warehouse / Analytics Engine)  
- **Simple Memory Buffer** (Multi-turn Context Persistence)  

---

## 📸 Workflow & Execution

### 1. Agent Architecture & Live Query Execution
The AI agent receives the user's question, identifies the required metrics, queries the database via the SQL tool, and formulates the natural-language response:

```text
[ User Prompt via Chat Interface ]
                 │
                 ▼
       ┌──────────────────┐
       │     AI Agent     │ ◄── Google Gemini (LLM)
       └────────┬─────────┘ ◄── Simple Memory (Context)
                │
                ▼ (Tool Call)
    ┌─────────────────────────┐
    │   Execute a SQL Query   │ ──► [ Database Execution ]
    └───────────┬─────────────┘
                │
                ▼ (Result Interpreted)
   [ Final Conversational Answer ]
```

### workflow with the output
<img width="800" height="450" alt="image" src="https://github.com/user-attachments/assets/84ff920a-58e5-4e08-8e07-72510a4477c1" />

> *The agent dynamically processes ad-hoc questions like: "Which market had the highest total sold quantity in 2020, and what was the quantity?" and retrieves exact metrics instantly.*

---

## 📌 Workflow Nodes & Agent Components  

| Component | Node Type | Purpose | Configuration |
|---|---|---|---|
| **When chat message received** | Chat Trigger | Provides the conversational chat UI and session management | Chat interface entry point |
| **AI Agent** | Core Agent | Coordinates reasoning, memory retrieval, and tool invocation | Mode: Conversational Agent |
| **Google Gemini** | Chat Model | Synthesizes queries and crafts natural-language responses | Google Gemini Chat Model integration |
| **Simple Memory** | Memory Node | Retains dialogue history for multi-turn conversations | Session ID-based memory buffer |
| **Execute a SQL Query** | Agent Tool | Allows the agent to query database tables dynamically | Connected to analytical database |
| **Sub-Workflow Tool** | Agent Tool | Dispatches complex auxiliary processes | Secondary workflow invocation |

---

## 💬 Sample Interaction & Query Result  

### User Prompt
> *"Which market had the highest total sold quantity in 2020, and what was the quantity?"*

### Agent Internal Tool Execution (Simulated SQL)
```sql
SELECT 
    market, 
    SUM(sold_quantity) AS total_sold_quantity
FROM fact_sales_monthly
WHERE fiscal_year = 2020
GROUP BY market
ORDER BY total_sold_quantity DESC
LIMIT 1;
```

### Agent Response
> **"The market with the highest total sold quantity in 2020 was India, with a total of 438,376 units sold."**

---

## 🌟 Business & Technical Impact  

- **Instant Ad-Hoc Analytics:** Reduces time-to-insight from hours to seconds for non-technical stakeholders  
- **Self-Service BI:** Eliminates manual ticket backlogs for data analyst teams  
- **Context-Aware:** Memory integration allows follow-up questions without repeating query parameters  
- **Safe & Auditable:** All queries and agent steps are logged in real time within the n8n execution monitor  

---

## 📁 Repository Structure  

```text
├── workflows/
│   └── ai_sql_analyst_agent.json      # Exported n8n agent workflow file
├── docs/
│   └── workflow_canvas.png            # Screenshot of the agent canvas and chat logs
├── sql/
│   └── schema.sql                     # Database schema definition queried by the agent
├── .env.example                       # API keys template (Gemini API, Database credentials)
├── .gitignore
└── README.md
```

---

## ⚙️ How to Run This Project  

### 1. Prerequisites
- An active instance of [n8n](https://n8n.io/) (version supporting Advanced AI / LangChain nodes).
- A Google AI Studio API Key for **Google Gemini**.
- A connected SQL database (PostgreSQL, MySQL, etc.).

### 2. Configure Credentials in n8n
1. Navigate to **Settings** > **Credentials** in your n8n workspace.
2. Add your **Google Gemini API** key.
3. Add your **Database Credentials** (Host, User, Password, Port, Database).

### 3. Import & Configure the Workflow
1. Go to **Workflows** > **Import from File**.
2. Select `workflows/ai_sql_analyst_agent.json`.
3. Open the **Execute a SQL Query** node and ensure the connection matches your analytical database.
4. Click **Chat** to test questions directly inside the built-in n8n test console.

---

## 🙏 Acknowledgements  

- [Codebasics](https://codebasics.io/) — Dhaval Patel and Hemanand Vadivel for leading practical education combining real-world business data with AI-driven automation.  
- [n8n.io](https://n8n.io/) for their modular AI Agent framework.  
- **Google Gemini** for providing fast, instruction-following LLM intelligence.  

---

## 📌 Conclusion  

This project demonstrates the transition from traditional, static analytics reporting to **autonomous, conversational agentic systems** that democratize access to database metrics across organizations.

---

## 📬 Contact  

🔗 **LinkedIn:** www.linkedin.com/in/ajay-jadhav-8381aa347  
📧 **Email:** aj.ajayjadhav01@gmail.com  

---

⭐ *If you found this project helpful, consider giving it a star!*
