# 💱 Automated Currency Exchange Rate Ingestion Pipeline: n8n + Google Sheets

*An automated data extraction and logging pipeline built in n8n that fetches live foreign exchange rates via REST API and automatically appends structured financial data into Google Sheets for analytics and reporting.*

---

## ❗ Problem Statement  

Financial analysts and data teams often need up-to-date foreign exchange (FX) rates to track pricing, calculate currency adjustments, and normalize international revenue:

- Manually looking up daily exchange rates and copying values into spreadsheets is time-consuming and error-prone  
- Inconsistent update schedules lead to outdated figures across financial reports and dashboards  
- Manual data entry creates gaps and lacks an auditable historical trace  

To eliminate these inefficiencies, this project provides a **lightweight, scheduled automation pipeline** to fetch live currency rates and log them systematically without manual intervention.

---

## 📝 Project Overview  

This project demonstrates how to build an end-to-end data ingestion pipeline using **n8n** that integrates an external financial REST API with **Google Sheets** for real-time tracking and downstream reporting.

### 🎯 Objective  
- Automate real-time FX rate extraction via API  
- Eliminate manual spreadsheet updates  
- Build a persistent, historical audit trail of currency rate fluctuations  
- Provide clean, pre-structured tabular data ready for analysis in Excel or Power BI  

---

## 🚀 Solution  

Designed an **n8n automated workflow** that initiates an HTTP GET request to the ExchangeRate API (`v6.exchangerate-api.com`), extracts currency rate metrics from the JSON response, and automatically appends each payload as a new row in a target Google Sheet.

### 🛠️ Tools & Technologies  
- **n8n** (Workflow Orchestration & Automation)  
- **HTTP Request / REST API** (ExchangeRate API — `GET: https://v6.exchangerate-api.com/...`)  
- **Google Sheets** (Cloud Spreadsheet Storage / Data Destination)  
- **JSON** (Data Serialization & Payload Parsing)  

---

## 📸 Workflow & Execution

### 1. n8n Pipeline Architecture
The workflow starts with a trigger node, performs a GET call to the ExchangeRate API endpoint, and appends the resulting payload into Google Sheets:

---

## 📌 Workflow Nodes & Logic  

| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **When clicking 'Execute workflow'** | Trigger | Initiates pipeline execution on demand (or via cron/interval) | Manual / Scheduled trigger |
| **HTTP Request** | Data Fetch | Calls the ExchangeRate API endpoint | `GET` request fetching JSON exchange rate data |
| **Append row in sheet** | Data Destination | Appends response fields into designated columns | Target Spreadsheet ID + Column Mapping |

---

## 📑 Output Data & Sheet Preview
### Workflow
<img width="723" height="337" alt="image" src="https://github.com/user-attachments/assets/f16d8a0e-d0b0-426a-805c-8b67b40d5827" />

# Workflow output
<img width="530" height="178" alt="image" src="https://github.com/user-attachments/assets/e75d14c2-bb59-4311-9e73-2218158a08f0" />


### 📂 Sample Output Schema

| Timestamp | Base_Currency | Target_Currency | Conversion_Rate | 
| :--- | :--- | :--- | :--- | 
| `2026-09-05 18:45:00` | USD | INR | 83.45 | 
| `2026-09-05 18:45:00` | USD | EUR | 0.92 | 
| `2026-09-05 18:45:00` | USD | GBP | 0.79 | 

---

## 🌟 Technical Impact  

- **Hands-Off Maintenance:** Automatically syncs exchange rates without recurring human effort  
- **Real-Time Visibility:** Ensures downstream models always reference the latest currency metrics  
- **Extensibility:** Easily expandable with cron schedules (e.g., run daily at 9:00 AM) or notification webhooks (Slack/Email alerts)  

---

## 📁 Repository Structure  

```text
├── workflows/
│   └── currency_exchange_automation.json  # Exported n8n workflow file
├── data/
│   └── exchange_rates_output.xlsx         # Exported Google Sheets data backup
├── docs/
│   ├── workflow_canvas.png                # Screenshot of the n8n editor canvas
│   └── sheets_output_preview.png          # Screenshot of the output Google Sheet
├── .env.example                           # Template for API credentials
├── .gitignore
└── README.md
```

---

## ⚙️ How to Run This Project  

### 1. Prerequisites
- An active instance of [n8n](https://n8n.io/) (n8n Cloud or self-hosted via Docker).
- An API key from [ExchangeRate-API](https://www.exchangerate-api.com/).
- A Google account with access to Google Sheets.

### 2. Configure Google Sheets & API
1. Create a Google Sheet with the following headers in Row 1:  
   `Timestamp`, `Base_Currency`, `Target_Currency`, `Conversion_Rate`, `Status`.
2. Connect your Google credentials inside n8n (**Settings** > **Credentials** > **Google Sheets OAuth2 API**).

### 3. Import & Configure n8n Workflow
1. In n8n, go to **Workflows** > **Import from File**.
2. Select `workflows/currency_exchange_automation.json`.
3. Open the **HTTP Request** node and insert your ExchangeRate API endpoint URL and API Key.
4. Open the **Append row in sheet** node, map your target Google Spreadsheet ID and Sheet name.
5. Click **Execute workflow** to test the run and verify the new row appended to your sheet.

---

## 🙏 Acknowledgements  

- [Codebasics](https://codebasics.io/) — Dhaval Patel and Hemanand Vadivel for practical guidance on connecting modern automation tools to data analytics workflows.  
- [n8n.io](https://n8n.io/) for workflow orchestration capabilities.  

---

## 📌 Conclusion  

This project demonstrates:  
- Connecting external REST APIs to persistent data destinations using visual automation  
- Processing JSON payloads within n8n  
- Building production-grade automation patterns applicable to any recurring data collection task  

---

## 📬 Contact  

🔗 **LinkedIn:** www.linkedin.com/in/ajay-jadhav-8381aa347  
📧 **Email:** aj.ajayjadhav01@gmail.com  

---

⭐ *If you found this project helpful, consider giving it a star!*
