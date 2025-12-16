# AI-Powered Change Documentation & Approval Agent 🏗️🤖

**Automating Scope Change Management for the AEC Industry**

![n8n](https://img.shields.io/badge/Workflow-n8n-orange?style=flat-square)
![Docker](https://img.shields.io/badge/Container-Docker-blue?style=flat-square)
![AI](https://img.shields.io/badge/AI-Gemini%201.5%20Pro-4285F4?style=flat-square)
![Status](https://img.shields.io/badge/Status-Operational-green?style=flat-square)

## 🎥 Demo

> **[Insert Demo Video Here]**
> *Watch the agent process a client request, calculate costs, and generate a signed PDF in real-time.*

---

## 📖 Executive Summary

[cite_start]This project involves the design and deployment of an automated system to streamline scope change management in construction and architecture projects[cite: 5]. [cite_start]Hosted locally on Docker (Windows/WSL2), the system intercepts change requests, retrieves project data, and uses AI (Gemini 1.5 Pro) to generate formal approval documentation[cite: 6, 7].

## 🚩 The Problem Statement

[cite_start]In the Architecture, Engineering, and Construction (AEC) industry, scope changes are frequently communicated verbally or via informal messaging[cite: 9]. [cite_start]This lack of formal documentation leads to[cite: 10]:
* [cite_start]**Financial Discrepancies:** Unrecorded costs for material and labor[cite: 11].
* [cite_start]**Timeline Conflicts:** Unapproved extensions to project deadlines[cite: 11].
* [cite_start]**Payment Disputes:** Lack of signed evidence for completed variations[cite: 12].

## 💡 The Solution

[cite_start]The objective is to automate the creation of "Change Orders" to ensure all modifications are financially quantified, documented, and approved before execution[cite: 13].

### System Architecture
[cite_start]The system is hosted locally to ensure data privacy and control, utilizing a containerized architecture for stability[cite: 15].

![Agent Architecture](Agent%20Architecture.png)

**Workflow Overview:**
1.  [cite_start]**Input:** Client submits a request via a Mobile Web Form.
2.  [cite_start]**Data Acquisition:** The agent fetches project context and material rates from a structured Google Sheets database[cite: 25, 26, 31].
3.  [cite_start]**Processing (AI Core):** Data is merged and sent to **Gemini 1.5 Pro**, which generates the natural language justification and cost breakdown[cite: 32, 33].
4.  [cite_start]**Document Generation:** A formal document is created in Google Docs, populated with AI text, and converted to PDF[cite: 35, 36, 37].
5.  [cite_start]**Delivery & Logging:** The PDF is emailed to the client/manager, and the transaction is logged in the database[cite: 38, 40].

---

## ⚙️ Technical Stack

* [cite_start]**Host Machine:** ASUS TUF A14 (AMD Ryzen Processor)[cite: 16].
* [cite_start]**Virtualization:** Windows Subsystem for Linux 2 (WSL2)[cite: 17].
* [cite_start]**Containerization:** Docker Desktop for Windows[cite: 18].
* [cite_start]**Automation Engine:** n8n (Enterprise-grade workflow automation)[cite: 19].
* [cite_start]**LLM/AI:** Google Gemini 1.5 Pro.
* [cite_start]**Database:** Google Sheets (Structured data storage)[cite: 20].
* [cite_start]**Authentication:** Google Cloud IAM (Service Account with JSON Key)[cite: 21].

---

## 🗄️ Database Architecture

[cite_start]The system relies on a "Ground Truth" Google Sheet with three distinct datasets[cite: 53, 54]:

1.  [cite_start]**`Project_Tasks`**: Baseline data including Task ID, Description, Original Cost, and Duration[cite: 55].
2.  [cite_start]**`Rates`**: Lookup table for unit costs (e.g., cost per window, cost per sq. meter) and delay impacts[cite: 56].
3.  [cite_start]**`Change_Logs`**: A designated output register where the AI records generated change orders and approval statuses[cite: 57].

---

## 🚀 Implementation Methodology

### [cite_start]1. Environment Configuration [cite: 42]
[cite_start]To avoid version conflicts and dependency issues, a containerized approach was selected[cite: 43].
* [cite_start]**BIOS Configuration:** Enabled Virtualization (SVM Mode) to support the Linux subsystem[cite: 44].
* [cite_start]**WSL2 Deployment:** Installed Windows Subsystem for Linux (Version 2) to provide a native Linux kernel[cite: 45].
* [cite_start]**Network Configuration:** Reset WSL network stack and Docker network bridge to resolve initial DNS connectivity issues[cite: 46].

### 2. Docker Deployment
[cite_start]The n8n instance was deployed using the following persistent configuration[cite: 47]:

```bash
docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 -e N8N_HOST="127.0.0.1" \
 --restart unless-stopped \
 n8nio/n8n
```

 * [cite_start]**Port Mapping:** `5678:5678` (Localhost access)[cite: 48].
* [cite_start]**Volume Mounting:** A dedicated Docker volume (`n8n_data`) ensures workflow and credential data persists across system reboots[cite: 49].
* [cite_start]**Restart Policy:** `unless-stopped` for high availability[cite: 51].

### 3. Cloud Integration & Security
[cite_start]Server-to-Server authentication was implemented to allow secure database interaction without manual login[cite: 59, 60].

* [cite_start]**Google Cloud Project:** Created project `n8n-construction-agent`[cite: 61].
* [cite_start]**API Enablement:** Enabled Google Sheets API[cite: 62].
* [cite_start]**Service Account:** Created an IAM Service Account (`n8n-bot`) to act as a "Robot Identity"[cite: 63].
* [cite_start]**Key Generation:** Generated a secure JSON private key[cite: 64].
* [cite_start]**Access Control:** The Google Sheet was shared with the Service Account email with "Editor" privileges, adhering to the Principle of Least Privilege[cite: 65].

---

## 🧩 Workflow Logic

[cite_start]The n8n workflow follows this logic path[cite: 22, 25, 29, 27, 34]:

1.  [cite_start]**On Form Submission:** Triggers the flow[cite: 22].
2.  [cite_start]**Get Row(s):** Parallel execution fetches `Project Tasks` and `Material Rates`[cite: 26, 31].
3.  [cite_start]**Merge:** Combines task context with pricing data[cite: 32].
4.  [cite_start]**Basic LLM Chain:** Sends the merged context to Gemini 1.5 Pro to draft the Change Order content[cite: 33].
5.  [cite_start]**Append Row:** Logs the request in `Change_Logs`[cite: 40].
6.  [cite_start]**Create/Update Document:** Uses a Google Doc template to create the formal letter[cite: 35].
7.  [cite_start]**Download & Send:** Converts to PDF and emails the stakeholder[cite: 37, 38].

---

## ✅ Current System Status

* [cite_start]**Connectivity:** The local Docker container successfully establishes outbound connections to the internet[cite: 68].
* [cite_start]**Authentication:** n8n is successfully authenticated with Google Cloud using Service Account credentials[cite: 69].
* [cite_start]**Data Retrieval:** The workflow correctly fetches and parses project data into JSON format[cite: 70, 71].

---

## 🔮 Next Steps

1.  [cite_start]**Data Merging:** Combine "Task" data with "Rates" data to create a unified context[cite: 79].
2.  [cite_start]**AI Integration:** Connect the workflow to the Large Language Model (LLM) to generate natural language documentation[cite: 80].
3.  [cite_start]**Output Generation:** Configure the system to write the final Change Order back to the database[cite: 81].

---

**Author:** Yashwanth
[cite_start]**Date:** November 20, 2025 [cite: 2, 3]
