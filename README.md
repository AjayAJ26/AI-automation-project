# 📂 End-to-End Multimodal Document Intelligence & Extraction Pipeline: n8n + Google Gemini + Google Drive + Google Sheets

*An enterprise-grade document extraction and parsing pipeline built in n8n that automatically discovers multi-format files in Google Drive, orchestrates routing and validation, uses Google Gemini to extract structured entities, and normalizes tabular data into relational Google Sheets.*

---

## ❗ Problem Statement  

Organizations receive hundreds of unstructured and semi-structured documents daily—such as invoices, receipts, vendor contracts, and PDFs—stored across cloud drives:

- Manual document inspection, copying fields, and entering line items into spreadsheets is slow and labor-intensive  
- Mixed file formats (PDFs, images, scans) require fragmented extraction scripts or manual human review  
- Multi-item documents with nested line items often get flattened incorrectly or lost during basic extraction  
- Lack of file validation causes processing pipelines to fail silently when unreadable files are encountered  

To eliminate these inefficiencies, this project provides a **modular, three-stage autonomous pipeline** that automates the lifecycle from drive discovery to multi-table relational spreadsheet logging.

---

## 📝 Project Overview  

This project demonstrates a production-grade multimodal extraction pipeline built with **n8n** and powered by **Google Gemini Chat Model**. The pipeline processes 30+ items per batch across three distinct workflow stages: **Validation**, **Data Extraction**, and **Data Addition**.

### 🎯 Objective  
- Scan and retrieve batch document files automatically from Google Drive  
- Route and validate files dynamically based on file type and format  
- Utilize Google Gemini as an Information Extractor to convert unstructured documents into strictly structured JSON  
- Split nested line items and normalize records into multiple linked Google Sheets (Header & Itemized Line Items)  

---

## 🚀 Solution  

Architected a three-stage automated data extraction engine:

1. **Validation Stage:** Discovers files via Google Drive API, iterates through items using loop controls, routes file types via a `Switch` node, extracts PDF/text content, and runs custom JavaScript validation.  
2. **Data Extraction Stage:** Directs payloads through conditional logic (`If`), interacts with external endpoints via `HTTP Request`, and prompts the `Information Extractor` backed by the **Google Gemini Chat Model** to isolate target data fields.  
3. **Data Addition Stage:** Appends or updates header-level records in Sheet 1, transforms fields, expands line items using `Split Out`, and appends relational granular items into Sheet 2.

### 🛠️ Tools & Technologies  
- **n8n** (Enterprise Workflow Orchestration)  
- **Google Drive API** (Search Files, Folders & Download)  
- **Google Gemini Chat Model** (Multimodal LLM for Entity Extraction)  
- **n8n Information Extractor** (Structured Schema Extraction)  
- **Google Sheets API** (Relational Data Storage: Sheet 1 Header & Sheet 2 Line Items)  
- **JavaScript** (Custom Payload Formatting & Validation)  

---

## 📸 Workflow & Execution

### 1. Three-Stage Pipeline Architecture
```text
┌────────────────────────────────────────────────────────┐
│                   STAGE 1: VALIDATION                  │
│  Google Drive Search ──► Loop ──► Download ──► Switch  │
│  ├── Extract from File (PDF) ──► If Logic              │
│  └── Code (JavaScript) ────────► If Logic              │
└──────────────────────────┬─────────────────────────────┘
                           │
┌──────────────────────────▼─────────────────────────────┐
│                STAGE 2: DATA EXTRACTION                │
│  HTTP Request / Payload ──► Information Extractor      │
│                                    ▲                   │
│                                    │                   │
│                         Google Gemini Chat Model       │
└──────────────────────────┬─────────────────────────────┘
                           │
┌──────────────────────────▼─────────────────────────────┐
│                 STAGE 3: DATA ADDITION                 │
│  Append/Update Sheet 1 ──► Edit Fields ──► Split Out   │
│                                                 │      │
│                                                 ▼      │
│                                     Append to Sheet 2  │
└────────────────────────────────────────────────────────┘
```

### 2. Workflow
<img width="800" height="249" alt="image" src="https://github.com/user-attachments/assets/1b9e09b2-ba8c-4cff-98eb-76e93ad31909" />

---

## 📌 Detailed Pipeline Stages & Nodes  

### Stage 1: Validation
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **When clicking 'Execute workflow'** | Trigger | Initiates pipeline execution on demand or on a schedule | Manual / Cron trigger |
| **Search files and folders** | Google Drive | Scans designated target Google Drive folder for documents | Filters by folder ID and mime types |
| **Loop Over Items** | Flow Control | Batches and processes files sequentially (tested with 30 items) | Batch processing control |
| **Download file** | Google Drive | Downloads binary file payload from Google Drive | Binary stream handling |
| **Switch** | Logic Router | Routes files based on extension/format (e.g., PDF vs. image) | Rules-based routing |
| **Extract from File** | Text Extraction | Reads and parses text content from PDF/document binaries | Operation: `Extract From PDF` |
| **Code in JavaScript** | Custom Script | Normalizes parsed metadata, checks file sizes, and cleans text | Vanilla JavaScript node |
| **If** | Gatekeeper | Verifies that file contents meet readability criteria before LLM call | Boolean condition check |

### Stage 2: Data Extraction
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **HTTP Request** | Network API | Performs auxiliary API verification and payload preprocessing | `POST` / `GET` operational requests |
| **Information Extractor** | LangChain / AI | Maps unstructured text/binary into a strict JSON schema | Connected to LLM and output schema |
| **Google Gemini Chat Model** | LLM Engine | Provides contextual reasoning and accurate multimodal entity extraction | Configured via Google AI Studio credentials |

### Stage 3: Data Addition
| Node | Type | Purpose | Configuration |
|---|---|---|---|
| **Append or update row in sheet 1** | Google Sheets | Inserts/updates document header records (Invoice Number, Billing Details, Totals) | Match key: `invoice_number` |
| **Edit Fields** | Transformation | Cleans, re-maps, and isolates nested line-item arrays | Field renamer and transformer |
| **Split Out** | Array Flattener | Deconstructs the line items array into separate individual rows | Array path: `line_items` |
| **Append row in sheet 2** | Google Sheets | Logs individual itemized rows linked to the parent `invoice_number` | Sheet 2 itemized mapping |

---

## 📑 Database Output & Sheet Previews

The workflow normalizes unstructured documents into two linked relational sheets: **Invoice Headers** (`sheet 1`) and **Itemized Line Items** (`sheet 2`).

### 1. Invoice Header Table (`sheet 1`)
Captures high-level metadata, billing entities, contact points, tax, and total gross amounts:

#### 📂 Sample Header Schema (`sheet 1`)
<img width="895" height="98" alt="image" src="https://github.com/user-attachments/assets/8fc471c5-b9c6-4053-9905-e1e4a89b0d38" />
---

### 2. Itemized Line Items Table (`sheet 2`)
Captures normalized, granular line items linked back to the parent document via `invoice_number`

#### 📂 Sample Line Items Schema (`sheet 2`)
<img width="838" height="356" alt="image" src="https://github.com/user-attachments/assets/206814b4-9fa4-4e5d-ac80-cc8bdda86914" />
---

## 🌟 Technical Impact  

- **95%+ Manual Effort Eliminated:** Eliminates manual line-by-line spreadsheet entry for high-volume invoices  
- **Normalized Relational Model:** Separates high-level invoice headers and individual products across relational sheets linked by `invoice_number`  
- **Batch Processing Resiliency:** Loop control batches 30+ items without hitting platform memory limits or rate throttles  
- **High Multimodal Accuracy:** Google Gemini extracts complex product names, addresses, quantities, and numeric currency values reliably  

---

## 📁 Repository Structure  

```text
├── workflows/
│   └── document_intelligence_pipeline.json  # Complete exported n8n workflow
├── docs/
│   ├── workflow_canvas.png                  # Canvas screenshot showing all 3 stages
│   ├── sheet1_invoice_headers.png           # Output screenshot of Sheet 1 (Headers)
│   └── sheet2_line_items.png                # Output screenshot of Sheet 2 (Line Items)
├── data/
│   ├── sample_invoices/                     # Sample PDF/image documents for testing
│   ├── sheet1_headers_backup.csv            # Backup CSV of extracted headers
│   └── sheet2_line_items_backup.csv         # Backup CSV of extracted line items
├── scripts/
│   └── validation_script.js                 # Standalone copy of the JS validation code
├── .env.example                             # Environment variables & API template
├── .gitignore
└── README.md
```

---

## ⚙️ How to Run This Project  

### 1. Prerequisites
- An active instance of [n8n](https://n8n.io/) (version supporting LangChain & Advanced AI nodes).
- A Google Cloud Project with **Google Drive API** and **Google Sheets API** enabled.
- An API Key for **Google Gemini** (via Google AI Studio).

### 2. Configure Google Drive & Sheets
1. Create a Google Drive folder where incoming invoices/receipts will be stored.
2. Create a target Google Spreadsheet with two sheets:
   - **Sheet 1 (`Headers`):** `invoice_number`, `date`, `billing_company`, `billing_company_address`, `billing_company_email`, `billing_company_phone`, `customer_company_name`, `total_amount`, `tax`
   - **Sheet 2 (`Line_Items`):** `invoice_number`, `date`, `item_name`, `quantity`, `unit_price`, `total`, `UAN`

### 3. Connect Credentials in n8n
- Add **Google Drive OAuth2** credentials.
- Add **Google Sheets OAuth2** credentials.
- Add **Google Gemini (PaLM / Gemini API)** credentials.

### 4. Import & Configure Workflow
1. In n8n, navigate to **Workflows** > **Import from File**.
2. Select `workflows/document_intelligence_pipeline.json`.
3. In **Search files and folders**, set the source Google Drive folder ID.
4. In the Google Sheets nodes, map your target spreadsheet ID and select **Sheet 1** and **Sheet 2** respectively.
5. Click **Execute workflow** to run the pipeline end-to-end.

---

## 🙏 Acknowledgements  

- [Codebasics](https://codebasics.io/) — Dhaval Patel and Hemanand Vadivel for championing applied, hands-on learning that bridges modern AI with data analytics.  
- [n8n.io](https://n8n.io/) for their modular workflow automation and AI node ecosystem.  
- **Google Gemini** for providing intelligent multimodal document reasoning.  

---

## 📌 Conclusion  

This project demonstrates:  
- Building an end-to-end multimodal document parsing pipeline without custom backend servers  
- Orchestrating automated file discovery, format switching, and custom JavaScript validation  
- Normalizing unstructured multi-item documents into clean, queryable relational schemas  

---

## 📬 Contact  

🔗 **LinkedIn:** www.linkedin.com/in/ajay-jadhav-8381aa347  
📧 **Email:** aj.ajayjadhav01@gmail.com  

---

⭐ *If you found this project helpful, consider giving it a star!*
