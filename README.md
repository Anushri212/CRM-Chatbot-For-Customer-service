# 📡 AI-Powered Telecom CRM Automation (n8n + LLM)

An intelligent **Telecom CRM Automation System** built using **n8n workflows, LLM agents, Redis memory, and JSON extraction logic**.

This system classifies user intent and dynamically routes requests to specialized subflows including:

* SIM related issues & troubleshooting
* Best offer recommendation
* Bill dispute management
* Bill summary & comparison (Multi-language)
* Contract renewal & extension
* Plan change / migration
* Order status tracking
* Ticket creation & ticket status

---

# 🏗️ Architecture Overview

```
User Input
    ↓
Main Intent Classifier
    ↓
Intent-based Routing
    ↓
Specialized Sub-Workflow
    ↓
CRM / Data Fetch / RAG / Ticket Creation
    ↓
Structured JSON Output
```

The solution uses:

* 🔹 Intent Classification Layer
* 🔹 JSON Extraction Agents
* 🔹 Telecom Validation Rules
* 🔹 Session Memory (Redis + Window Memory)
* 🔹 Multi-language Support
* 🔹 Strict Output Control (Production-Safe JSON)

---

# 🧠 1️⃣ Main Intent Classifier

📂 File: `crm_bill_contract_bestoffer_planChange_orderStatus.json` 

This is the **entry point workflow**.

It classifies user input into one of the following intents:

* `bill_dispute`
* `bill_summary`
* `contract_renewal`
* `best_offer`
* `change_plan`
* `order_status`
* `ticket_status`
* `general`

### 🔐 Strict Rules

* Returns pure JSON only
* No markdown blocks
* Single intent classification
* Strict separation between bill dispute vs bill summary

---

# 📶 2️⃣ SIM Related Issues + Offer + RAG + Ticket Flow

📂 File: `CRM_SIM_related_issues_development_V1_2.json` 

Handles:

* SIM blocked
* DSL issues
* Network problems
* Broadband issues
* Offer inquiries
* Troubleshooting steps (step-by-step)
* Auto ticket creation if unresolved

### 🔎 Key Features

✅ Mandatory 8–15 digit phone number validation
✅ Memory of previous phone number
✅ Strict JSON output with:

```json
{
  "input": "exact_user_message",
  "phone_number": "extracted_number"
}
```

✅ DSL Special Handling:

* If resolved → End flow
* If not resolved → Continue next troubleshooting step
* If still unresolved → Create support ticket

---

# 💰 3️⃣ Bill Dispute Flow

📂 File: `bill_dispute_crm_namratha.json` 

Handles:

* High bill complaints
* Overcharge issues
* Billing disputes
* Dispute ticket creation

### 🔄 Conversation Intelligence

Tracks:

* Account ID (6D format)
* Service ID (8–14 digits)
* Selected billing option
* Dispute amount

### 📦 Output Format

```json
{
  "account_id": "6DXXXX",
  "service_id": "XXXXXXXX",
  "ai_message": "response message",
  "option": "selected option",
  "level": "service_level | account_level | ticket_level"
}
```

---

# 📄 4️⃣ Bill Summary & Comparison (Multi-Language)

📂 File: `Bill_Summary_OBC.json` 

Supports:

* Invoice summary
* Bill comparison
* Month-to-month variation
* Due amount & payment history
* VAT breakdown
* Active connections summary
* Itemized bill

### 🌍 Multi-language Support

* English
* Arabic

Language detection handled via:

* Conversation memory
* Language selection prompt
* Redis chat memory

### 🧠 Advanced Capabilities

* Month comparison logic
* Bill difference explanation
* Language-based response switching
* Session-based storage

---

# 🔄 5️⃣ Contract Renewal & Extension

📂 File: `contract_renewal_integration_with_crm_somia.json` 

Handles:

* Auto renewal
* Extend contract
* Purchase new contract
* SA-based contract selection (12Months_SA, 24Months_SA, 36Months_SA)

### 🔐 Validation Rules

* Service ID: 8–14 digits
* Account ID: Must start with `6D`

### 🗓️ Extension Handling

* 1 week → 7 days
* 1 month → 30 days
* 1 year → 365 days
* OR exact date if provided

Structured output format:

```
cust_service: X | cust_AccountId: Y | type:Extend the contract | extension_day: Z
```

---

# 🔁 6️⃣ Change Plan Workflow

📂 File: `CRM_change_plan.json` 

Handles:

* Plan upgrade
* Plan migration
* Switching plans
* Customer-requested plan changes

### 🚨 Strict JSON Extraction Mode

* Requires valid 8–15 digit phone number
* No intent field allowed
* No extra fields allowed
* Pure JSON only

---

# 📦 7️⃣ Order Status & Ticket Status

Handled via main intent classifier routing.

Supports:

* Order stuck checks
* Order tracking
* Ticket status
* Ticket summary
* Ticket detail retrieval

---

# 🧠 Memory Architecture

System uses:

* Redis Chat Memory
* Window Buffer Memory
* Session-based tracking
* LATEST_NUMBER storage
* Conversation context retention

Ensures:

* No repeated ID requests
* No revalidation of valid IDs
* Clean multi-step conversational flows

---

# 🔐 Production Safety Controls

✔ Strict JSON-only responses
✔ No markdown JSON blocks
✔ No placeholder values
✔ No empty phone numbers
✔ No intent leakage in subflows
✔ Strict validation enforcement
✔ DSL resolution control logic

---

# 🛠 Tech Stack

* n8n
* OpenAI (gpt-4o-mini)
* Redis
* JSON extraction agents
* CRM API integrations
* RAG-based troubleshooting logic

---

# 🚀 Key Capabilities

| Feature                     | Supported |
| --------------------------- | --------- |
| Intent Classification       | ✅         |
| Telecom Issue Handling      | ✅         |
| Automated Troubleshooting   | ✅         |
| Auto Ticket Creation        | ✅         |
| Bill Comparison             | ✅         |
| Multi-Language Support      | ✅         |
| Contract Renewal Automation | ✅         |
| Plan Migration              | ✅         |
| Order Tracking              | ✅         |
| Strict JSON Production Mode | ✅         |

---

# 📌 Why This Project is Strong

* Fully modular architecture
* Strict production-grade JSON control
* Telecom domain-optimized prompts
* Conversation memory awareness
* Multi-language intelligent switching
* End-to-end automation

---

# 📎 Future Enhancements

* Analytics dashboard
* SLA tracking
* Voice bot integration
* WhatsApp / IVR integration
* Model fine-tuning for telecom intents

---
