
#AI-Powered Change Documentation & Approval Agent 🏗️🤖**Automating Scope Change Management for the AEC Industry**

##🎥 Demo> **[Insert Demo Video Here]**
> *Watch the agent process a client request, calculate costs, and generate a signed PDF in real-time.*

---

##📖 Executive SummaryThis project creates an automated "Change Order" agent designed to streamline scope change management in construction and architecture projects. Hosted locally on Docker (Windows/WSL2), the system intercepts change requests, retrieves project context, and uses AI (Gemini 1.5 Pro) to generate formal approval documentation.

##🚩 The ProblemIn the Architecture, Engineering, and Construction (AEC) industry, scope changes are frequently communicated verbally or via informal messaging. This lack of formal documentation leads to:

* 
**Financial Discrepancies:** Unrecorded costs for material and labor.


* 
**Timeline Conflicts:** Unapproved extensions to deadlines.


* 
**Payment Disputes:** Lack of signed evidence for completed variations.



##💡 The SolutionThis agent automates the creation of "Change Orders" to ensure all modifications are financially quantified, documented, and approved before execution.

###System ArchitectureThe system utilizes a containerized architecture for stability and data privacy.

**Workflow Overview:**

1. 
**Input:** Client submits a request via a Mobile Web Form.


2. 
**Data Acquisition:** The agent fetches project context and material rates from a structured Google Sheets database.


3. 
**Processing (AI Core):** Data is merged and sent to **Gemini 1.5 Pro**, which generates the natural language justification and cost breakdown.


4. 
**Document Generation:** A formal document is created in Google Docs, populated with AI text, and converted to PDF.


5. 
**Delivery & Logging:** The PDF is emailed to the client/manager, and the transaction is logged in the database.



---

##⚙️ Technical Stack* 
**Host Environment:** Windows Subsystem for Linux 2 (WSL2) on AMD Ryzen Hardware.


* 
**Containerization:** Docker Desktop for Windows.


* 
**Automation Engine:** n8n (Self-Hosted).


* 
**LLM/AI:** Google Gemini 1.5 Pro.


* 
**Database:** Google Sheets.


* 
**Authentication:** Google Cloud IAM (Service Account).



---

##🗄️ Database Schema (Google Sheets)The system relies on a "Ground Truth" Google Sheet with three distinct tabs:

1. 
**`Project_Tasks`**: Baseline data (Task ID, Description, Original Cost, Duration).


2. 
**`Rates`**: Lookup table for unit costs (e.g., cost per sq. meter, labor rates).


3. 
**`Change_Logs`**: Output register where the AI records generated orders and approval statuses.



---

##🚀 Installation & Deployment###1. Prerequisites* 
**BIOS:** Enable Virtualization (SVM Mode).


* 
**WSL2:** Install Windows Subsystem for Linux 2 to provide a native Linux kernel.


* **Docker:** Install Docker Desktop for Windows.

###2. Google Cloud Setup1. Create a project in Google Cloud Console (`n8n-construction-agent`).


2. Enable the **Google Sheets API** and **Google Drive API**.


3. Create an **IAM Service Account** (`n8n-bot`) to act as the "Robot Identity".


4. Generate and download a JSON private key.


5. 
**Important:** Share your Google Sheet with the Service Account email as an "Editor".



###3. Docker DeploymentDeploy the n8n instance using the following configuration to ensure persistence:

```bash
docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 -e N8N_HOST="127.0.0.1" \
 --restart unless-stopped \
 n8nio/n8n

```

* 
**Port Mapping:** `5678:5678` for localhost access.


* 
**Volume:** `n8n_data` ensures workflow/credential data persists across reboots.



###4. Workflow Configuration1. Import the workflow JSON file (provided in this repo).
2. Configure the **Google Sheets** and **Google Drive** nodes using the Service Account JSON key.
3. Configure the **Gemini Chat Model** node with your API key.

---

##🧩 Workflow LogicThe n8n workflow follows this logic path:

1. **On Form Submission:** Triggers the flow.
2. **Get Row(s):** Parallel execution fetches `Project Tasks` and `Material Rates`.
3. **Merge:** Combines task context with pricing data.
4. **Basic LLM Chain:** Sends the merged context to Gemini to draft the Change Order content.
5. **Append Row:** Logs the request in `Change_Logs`.
6. **Create/Update Document:** Uses a Google Doc template to create the formal letter.
7. **Download & Send:** Converts to PDF and emails the stakeholder.

---

##🛡️ Security* 
**Server-to-Server Auth:** Uses Google Cloud IAM Service Accounts to avoid manual user logins.


* 
**Principle of Least Privilege:** The robot identity only has access to the specific files shared with it, not the entire Drive.


* 
**Local Hosting:** Data is processed locally on the host machine before interacting with Cloud APIs.



---

##🔮 Future Improvements* 
**Output Generation:** Refine the write-back process to update the database with final signed PDF links.


* **Deep Integration:** Connect with BIM (Building Information Modeling) software for 3D context.

---

**Author:** Yashwanth


**Date:** November 20, 2025
