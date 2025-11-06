# 🧩 Task 1 — RFQ → CRM Automation (Alrouf Lighting)

## 📘 Project Overview
This workflow automates the complete Request-for-Quotation (RFQ) handling process for **Alrouf Lighting** — from email receipt to CRM opportunity creation, attachment archival, auto-acknowledgment, and team alerting.

**Orchestration Platform:** n8n  
**Implementation Date:** November 6 2025  
**Goal:** Eliminate manual RFQ handling while improving speed, traceability, and customer communication.

---

## 🎯 Objectives

| Objective | Description |
|------------|-------------|
| **Email Capture** | Automatically detect incoming RFQ/quotation emails through IMAP. |
| **Data Extraction (LLM)** | Parse unstructured RFQ emails into structured JSON using Google Gemini LLM. |
| **Data Persistence** | Record all messages in Google Sheets (`RFQ_Master_Log` & `RFQ_Parsed_Data`). |
| **CRM Integration (Mock)** | Create opportunities using a simulated CRM module with unique IDs. |
| **Attachment Archival** | Upload received files to Google Drive under `/RFQ_Attachments/`. |
| **Customer Auto-Reply** | Send bilingual acknowledgment email (English / Arabic ready). |
| **Internal Notification** | Post structured alert to Slack channel `#all-alrouf-project`. |
| **Error Tracking** | Maintain logs for failed executions and recovery attempts. |
| **Deduplication** | Prevent duplicate processing using `messageId` lookup. |

---

## ⚙️ Architecture Summary

### Stage 1 — Email Capture & Deduplication
- **IMAP Trigger** → Monitors inbox for unseen emails (subject/body contains *RFQ* or Arabic equivalents).  
- **Sheets Lookup + IF Node** → Checks for existing `messageId`; stops duplicates.

### Stage 2 — LLM-Powered Extraction
- **Google Gemini LLM → Structured Output Parser** → Converts free-text email into JSON.  
- **Function Node (Unify Output)** → Merges metadata (from, subject, date) with parsed data.

### Stage 3 — Data Persistence
- **Append to Google Sheets (Master Log)** → Logs raw message.  
- **Extract Parsed Fields → Append Parsed Sheet** → Creates analytic dataset.  
- **Status Update** → Marks record as “Parsed”.

### Stage 4 — CRM Mock & File Archival (Parallel)
- **Mock CRM Node** → Generates `crmId` and timestamps.  
- **Google Drive Upload** → Saves attachments under `/RFQ_Attachments/`.  
- **Merge Node** → Joins both outputs for unified status update.

### Stage 5 — Communication & Notification
- **Gmail Auto-Responder** → Sends acknowledgment.  
- **Slack Message Node** → Notifies sales channel with key details.  
- **Final Sheet Update** → Marks workflow “Alert Posted”.

---

## 🧠 Tech Stack

| Category | Tools / APIs |
|-----------|--------------|
| **Orchestration** | n8n v1.x |
| **Email Handling** | Gmail (IMAP trigger + OAuth2) |
| **Data Logging** | Google Sheets API |
| **File Storage** | Google Drive API |
| **AI / NLP** | Google Gemini LLM via LangChain nodes |
| **Notification** | Slack API |
| **Mock CRM** | JavaScript Function Node |
| **Error Tracking** | Custom JSON (`Error_Log.json`) |

---

## 📂 Repository Structure

├── Task_1_RFQ_to_CRM_Automation/
│   │
│   ├── Workflow_JSON/
│   │   ├── RFQ_to_CRM_Workflow.json             
│   │   ├── CRM_Mock_Log.json                    
│   │   ├── Error_Log.json                       
│   │   ├── Example_Input_Email.json               
│   │   └── Sample_Output_Data.json                
│   │
│   ├── Documentation/
│   │   ├── README.md                                
│   │   ├── .env.example                             
│   │   ├── Workflow_Blueprint.pdf                 
│   │   ├── Data_Mapping_Table.xlsx               
│   │   ├── Node_Explanation.md                      
│   │   ├── Security_and_Error_Handling.md           
│   │   └── Setup_Guide.md                           
│   │
│   ├── Screenshots/
│   │   ├── IMAP_Trigger_Config.png
│   │   ├── LLM_Translation_Node.png
│   │   ├── Google_Sheets_Insert_Node.png
│   │   ├── Drive_Archive_Node.png
│   │   ├── CRM_Mock_Function_Node.png
│   │   ├── Gmail_AutoReply_Node.png
│   │   ├── Slack_Alert_Node.png
│   │   ├── Sheet_Log_Sample.png
│   │   └── Workflow_Execution_Proof.png
│   │
│   ├── Video_Walkthrough/
│   │   ├── RFQ_to_CRM_Automation_Demo.mp4        
│   │   └── README.txt                  
│   │
│   ├── Auto_Reply_Sample.txt               
│   ├── Sample_Raw_Email.eml                  
│   ├── Requirements_Summary.txt   
│   ├── Integration_Notes.txt                        
│   ├── Tech_Stack_Info.txt                         
│   └── README.md                                  
│
│
└── Master_README.md  


---

## 🚀 Setup & Execution Guide

### 1️⃣ Prerequisites
- n8n installed locally or cloud workspace  
- Active Google API accounts (Gmail / Sheets / Drive)  
- Slack workspace with bot token  
- Gemini API key (or mock mode)

### 2️⃣ Import Workflow
1. Open n8n → Import `RFQ_to_CRM_Workflow.json`.  
2. Connect your credentials (replace placeholders from `.env.example`).  
3. Verify Google Sheet and Drive folder IDs.  
4. Enable workflow and test with sample email.



📈 Performance

| Metric                 | Result                      |
| ---------------------- | --------------------------- |
| Workflow Nodes         | 20 (1 trigger + 19 actions) |
| Average Execution Time | < 5 seconds                 |
| CRM Latency            | ~0.8 s (mock)               |
| Slack Alert Delivery   | Instant (< 1 s)             |
| Success Rate           | 100% tested samples         |



🧩 Future Enhancements

- Real Salesforce integration via OAuth API.
- Arabic template auto-selection.
- Retry logic for transient errors.
- OCR for attachment spec extraction.
- Analytics dashboard for RFQ volume tracking.



📦 Deliverables Included
- Sanitized Workflow JSON
- PDF Documentation
- Example Sheets and Drive screenshots
- CRM Mock and Error Logs
- Auto-Reply sample + Slack alert proof



🏁 Conclusion

- The RFQ → CRM Automation system demonstrates a production-ready, multi-system integration 
  pipeline.
- It proves full automation of Alrouf’s RFQ intake with:
  --> Reliable email capture
  --> Intelligent field extraction via Gemini LLM,
  --> Transparent data logging,
  --> Instant acknowledgment and alerts.
- This project serves as a scalable foundation for future Salesforce or Odoo CRM 
  integrations.