# 📬 Automated Email-to-Database ETL Pipeline: n8n + Gmail + Supabase (PostgreSQL)

*An event-driven ETL automation pipeline built with n8n that listens for incoming CSV email attachments in Gmail, parses and transforms tabular data, and automatically inserts structured records into a Supabase (PostgreSQL) database.*

---

## ❗ Problem Statement  

Data analysts and operations teams frequently receive recurring business reports, transactional exports, and vendor datasets via email:

- Manually downloading attachments, opening spreadsheets, and running database inserts wastes hours of analytical time  
- Delayed manual data entry causes lags between operational transactions and live dashboard updates  
- Human errors during manual file processing introduce dirty records, schema mismatches, and duplicate data  

To overcome this bottleneck, this project introduces a **fully automated, hands-off ingestion pipeline** that processes inbound file attachments and synchronizes records directly into a persistent relational database.

---

## 📝 Project Overview  

This project demonstrates how to build an end-to-end data integration pipeline using **n8n** connected to **Gmail** and **Supabase (PostgreSQL)**, shifting routine data collection into a completely autonomous process as part of my learning journey with **Codebasics**.

### 🎯 Objective  
- Automate file ingestion directly from inbox to database  
- Extract, parse, and structure incoming CSV attachment data automatically  
- Eliminate manual file downloads and manual SQL `INSERT` workflows  
- Ensure analytics data warehouses are updated in near real-time  

---

## 🚀 Solution  

Designed an **n8n event-driven pipeline** that listens for incoming emails with attachments via the Gmail Trigger, extracts the CSV payload into structured JSON items using the Extract from File node, and performs bulk insertion into a target table in Supabase (PostgreSQL).

### 🛠️ Tools & Technologies  
- **n8n** (Workflow Orchestration & Automation Engine)  
- **Gmail Trigger** (OAuth2 Event-driven Email Ingestion)  
- **Extract from File Node** (Binary CSV Parsing & Transformation)  
- **Supabase / PostgreSQL** (Cloud Relational Database & Backend Storage)  
- **SQL** (Table Schema Definition & Constraints)  

---

## 📸 Workflow & Execution

### workflow
The workflow activates on receiving an email, extracts row items from the attached CSV, and streams the records directly into the Supabase database:

```text
[ Gmail Trigger: New Email with Attachment ]
                      │
                      ▼ (1 Item)
         [ Extract from File: CSV ]
                      │
                      ▼ (10 Items)
       [ Insert rows in a table: Supabase ]
                      │
                      ▼ (10 Items)
           [ PostgreSQL Updated ]
```

### 2. worlflow
<img width="800" height="368" alt="image" src="https://github.com/user-attachments/assets/6c6afddb-90a1-49a8-9d4d-4ce2d6045613" />

---

## 📌 Workflow Nodes & Logic  

| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **Gmail Trigger** | Event Trigger | Listens for new incoming emails with file attachments | Filters on subject tags, sender, or unread status |
| **Extract from File** | Data Transformation | Decodes binary data and converts CSV rows into JSON objects | Operation: `Extract From CSV` |
| **Insert rows in a table** | Data Destination | Performs batch inserts into the Supabase PostgreSQL table | Selected Table: Target database table mapping |

---

## 🔢 Database Schema & Setup (Supabase)  

**Primary Engine:** Supabase (Cloud PostgreSQL)  

### 📂 Sample SQL Schema  

```sql
-- Schema for automated transaction or operational records
CREATE TABLE IF NOT EXISTS customer_orders (
    order_id VARCHAR(50) PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    order_date DATE NOT NULL,
    amount NUMERIC(10, 2) NOT NULL,
    status VARCHAR(50) DEFAULT 'Pending',
    ingested_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## 📑 Output Data & Verification

### 📂 Sample Ingested Records

| order_id | customer_name | order_date | amount | status | ingested_at |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ORD-9021` | Acme Corp | `2026-09-01` | 1450.00 | Completed | `2026-09-05 18:45:00` |
| `ORD-9022` | Nexus Retail | `2026-09-02` | 820.50 | Completed | `2026-09-05 18:45:00` |
| `ORD-9023` | Global Tech | `2026-09-03` | 2300.00 | Completed | `2026-09-05 18:45:00` |

---

## 🌟 Technical Impact  

- **Zero-Touch ETL:** Automatically captures and loads CSV records the moment an email lands in the inbox  
- **Scalable Batch Processing:** Handles multi-row batches (demonstrated with 10 records per execution) without performance drops  
- **Error Reduction:** Removes data entry errors by streaming raw CSV data directly into strongly-typed PostgreSQL schemas  

---

## 📁 Repository Structure  

```text
├── workflows/
│   └── email_csv_to_supabase_workflow.json   # Exported n8n workflow file
├── sql/
│   └── schema.sql                            # Supabase table definitions and schemas
├── docs/
│   └── workflow_canvas.png                   # Canvas screenshot of the n8n pipeline
├── data/
│   └── sample_input.csv                      # Sample input file used for pipeline testing
├── .env.example                              # Template for database & API credentials
├── .gitignore
└── README.md
```

---

## ⚙️ How to Run This Project  

### 1. Prerequisites
- An active instance of [n8n](https://n8n.io/) (n8n Cloud or self-hosted via Docker).
- A [Supabase](https://supabase.com/) project with PostgreSQL access.
- A Google account with Gmail API access enabled.

### 2. Configure Database
1. Open the **SQL Editor** in your Supabase dashboard.
2. Run the table creation script from `sql/schema.sql` to initialize your destination table.

### 3. Connect Credentials in n8n
- **Gmail OAuth2:** Connect your Gmail account under **Settings** > **Credentials**.
- **Supabase / PostgreSQL:** Add your Supabase host, database name, port (`5432`), user, and password credentials.

### 4. Import & Activate Workflow
1. In n8n, navigate to **Workflows** > **Import from File**.
2. Select `workflows/email_csv_to_supabase_workflow.json`.
3. In the **Gmail Trigger** node, set your filter criteria (e.g., label or subject line filter).
4. In the **Insert rows in a table** node, select your target database table and map the column fields.
5. Toggle the workflow to **Active** to begin live listening.

---

## 🙏 Acknowledgements  

- [Codebasics](https://codebasics.io/) — Dhaval Patel and Hemanand Vadivel for hands-on, industry-oriented training combining Data Analytics with modern AI automation architectures.  
- [n8n.io](https://n8n.io/) and [Supabase](https://supabase.com/) for developer-friendly data pipeline and orchestration tooling.  

---

## 📌 Conclusion  

This project highlights:  
- Building production-grade, event-driven data ingestion without writing complex server code  
- Handling binary file attachments and decoding them into tabular database entities  
- Bridging communication tools (Gmail) with analytical data warehouses (Supabase/PostgreSQL)  

---

## 📬 Contact  

🔗 **LinkedIn:** www.linkedin.com/in/ajay-jadhav-8381aa347  
📧 **Email:** aj.ajayjadhav01@gmail.com  

---

⭐ *If you found this project useful, consider giving it a star!*
