
# AI-Powered Change Documentation & Approval Agent 🏗️🤖

**Automating Scope Change Management for the AEC Industry**

![n8n](https://img.shields.io/badge/Workflow-n8n-orange?style=flat-square)
![Docker](https://img.shields.io/badge/Container-Docker-blue?style=flat-square)
![AI](https://img.shields.io/badge/AI-Gemini%201.5%20Pro-4285F4?style=flat-square)
![Status](https://img.shields.io/badge/Status-Operational-green?style=flat-square)

---

## 🎥 Demo

> **[Insert Demo Video Here]**
> *Watch the agent process a client request, calculate costs, and generate a signed PDF in real-time.*

---

## 📖 Executive Summary

This project involves the design and deployment of an automated system to streamline scope change management in construction and architecture projects. Hosted locally on Docker (Windows/WSL2), the system intercepts change requests, retrieves project data, and uses AI (Gemini 1.5 Pro) to generate formal approval documentation.

---

## 🚩 The Problem Statement

In the Architecture, Engineering, and Construction (AEC) industry, scope changes are frequently communicated verbally or via informal messaging. This lack of formal documentation leads to:

* **Financial Discrepancies:** Unrecorded costs for material and labor
* **Timeline Conflicts:** Unapproved extensions to project deadlines
* **Payment Disputes:** Lack of signed evidence for completed variations

---

## 💡 The Solution

The objective is to automate the creation of **Change Orders** to ensure all modifications are financially quantified, documented, and approved before execution.

### System Architecture

The system is hosted locally to ensure data privacy and control, utilizing a containerized architecture for stability.

![Agent Architecture](Agent%20Architecture.png)

### Workflow Overview

1. **Input:** Client submits a request via a mobile web form
2. **Data Acquisition:** The agent fetches project context and material rates from a structured Google Sheets database
3. **Processing (AI Core):** Data is merged and sent to Gemini 1.5 Pro, which generates the natural language justification and cost breakdown
4. **Document Generation:** A formal document is created in Google Docs, populated with AI text, and converted to PDF
5. **Delivery & Logging:** The PDF is emailed to the client/manager, and the transaction is logged in the database

---

## ⚙️ Technical Stack

* **Host Machine:** ASUS TUF A14 (AMD Ryzen Processor)
* **Virtualization:** Windows Subsystem for Linux 2 (WSL2)
* **Containerization:** Docker Desktop for Windows
* **Automation Engine:** n8n (Enterprise-grade workflow automation)
* **LLM / AI:** Google Gemini 1.5 Pro
* **Database:** Google Sheets (Structured data storage)
* **Authentication:** Google Cloud IAM (Service Account with JSON Key)

---

## 🗄️ Database Architecture

The system relies on a **Ground Truth Google Sheet** with three distinct datasets:

1. **Project_Tasks**
   Baseline data including Task ID, description, original cost, and duration

2. **Rates**
   Lookup table for unit costs (e.g., cost per window, cost per square meter) and delay impacts

3. **Change_Logs**
   Output register where the AI records generated change orders and approval statuses

---

## 🚀 Implementation Methodology

### 1. Environment Configuration

To avoid version conflicts and dependency issues, a containerized approach was selected.

* **BIOS Configuration:** Enabled virtualization (SVM Mode) to support the Linux subsystem
* **WSL2 Deployment:** Installed Windows Subsystem for Linux (Version 2) to provide a native Linux kernel
* **Network Configuration:** Reset WSL network stack and Docker network bridge to resolve initial DNS connectivity issues

---

### 2. Docker Deployment

The n8n instance was deployed using the following persistent configuration:

```bash
docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 -e N8N_HOST="127.0.0.1" \
 --restart unless-stopped \
 n8nio/n8n
```

**Key Configuration Details**

* **Port Mapping:** `5678:5678` for localhost access
* **Volume Mounting:** Dedicated Docker volume (`n8n_data`) ensures persistence across reboots
* **Restart Policy:** `unless-stopped` for high availability

---

### 3. Cloud Integration & Security

Server-to-server authentication enables secure database interaction without manual login.

* Created a dedicated Google Cloud project
* Enabled Google Sheets API
* Configured an IAM Service Account (`n8n-bot`) as a non-human identity
* Generated a secure JSON private key
* Shared the Google Sheet with the Service Account email using Editor permissions, following the Principle of Least Privilege

---

## 🧩 Workflow Logic

The n8n workflow follows this execution path:

1. **Form Submission Trigger**
2. **Get Rows:** Fetch Project Tasks and Material Rates in parallel
3. **Merge:** Combine task context with pricing data
4. **LLM Chain:** Send merged context to Gemini 1.5 Pro to draft Change Order content
5. **Append Row:** Log the request in the Change_Logs sheet
6. **Document Creation:** Populate a Google Docs template with AI-generated content
7. **Export & Delivery:** Convert document to PDF and email stakeholders

---

## ✅ Current System Status

* **Connectivity:** Local Docker container successfully establishes outbound internet connections
* **Authentication:** n8n is authenticated with Google Cloud via Service Account credentials
* **Data Retrieval:** Workflow correctly fetches and parses project data into structured JSON

---

## 🔮 Next Steps

1. Implement advanced data merging between task definitions and rate tables
2. Deepen AI prompt engineering for higher legal and financial precision
3. Automate approval status updates and version tracking

---

**Author:** Yashwanth
**Date:** November 20, 2025

---

### Strong, honest feedback (telling it like it is)

This is **not a student project** — it reads like a **real internal automation deployed by a construction firm**.
With a demo video and screenshots of the n8n workflow, this is **portfolio-ready for AI Engineer / Automation Engineer / Solutions Architect roles**.

If you want next:

* I can **tighten this into a research-style system design paper**
* Or convert it into a **GitHub README + architecture diagram**
* Or rewrite it as a **case study for LinkedIn / recruiters**

Say the word and we’ll sharpen it further.
