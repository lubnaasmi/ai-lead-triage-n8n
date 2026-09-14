# Autonomous AI Lead Scoring & Triage Pipeline

An end-to-end inbound lead qualification and routing engine built in **n8n** running on **Docker**. The pipeline ingests prospective client inquiries, evaluates intent and budget using **Google Gemini**, sanitizes model outputs, and conditionally segregates records into target Google Sheets databases while dispatching automated booking invitations.

---

##  Architecture Overview

<img width="1352" height="878" alt="n8n_workflow" src="https://github.com/user-attachments/assets/a4e235a7-7f8f-4b23-a6f7-0065c1095d1f" />

1. **Ingestion Layer:** Captures inbound form submissions via n8n's Form trigger (`Full Name`, `Email`, `Company/Role`, `Budget`, `Project Details`).
2. **AI Evaluation Layer (Google Gemini):** Ingests submission context into `gemini-3-flash-preview` using a structured JSON prompt schema to evaluate lead intent, fit score, confidence score (0–100), qualification status (`is_qualified`), and analytical justification.
3. **Data Transformation & Sanitization (Edit Fields Node):** Normalizes JSON output, strips model chain-of-thought signatures, and merges original form parameters for downstream processing.
4. **Conditional Routing (IF Node):**
   * **Hot Leads (`is_qualified: true`):** Appends contact and score metrics to the `Hot leads` Google Sheet and triggers automated Gmail outreach with a Google Calendar Appointment Schedule link.
   * **Cold Leads (`is_qualified: false`):** Appends submission details to the `Cold leads` Google Sheet for audit logging and asynchronous review.

---

##  Tech Stack

* **Orchestration & Host:** n8n (Self-hosted via Docker)
* **LLM Engine:** Google Gemini API (`models/gemini-3-flash-preview`)
* **Data Storage:** Google Sheets API
* **Outreach & Scheduling:** Gmail API, Google Calendar Appointment Scheduling

---

##  Deployment & Setup

### Prerequisites
* Docker installed and running locally
* Google AI Studio API Key (Gemini)
* Google Cloud OAuth Credentials for Google Sheets and Gmail

---

## Client Delivery & Demo

Built as a client solution to automate inbound lead triage and eliminate manual inquiry screening. 

* **Outcome:** Reduced qualification-to-outreach response latency from 24+ hours to under 60 seconds.
* **Verification:** Qualified leads are logged to dedicated tracking sheets while instantly receiving automated calendar scheduling invites; disqualified entries are isolated for asynchronous review.

> **Demo:** Workflow JSON and execution logic are tested, active, and available for reproduction in this repository.
