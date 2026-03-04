# 📡 AI-Powered Telecom CRM Automation Platform
### n8n + LLM + Redis Memory + WebSocket Event Streaming

This project implements an **AI-driven Telecom CRM automation system** that intelligently routes customer queries to specialized workflows such as billing disputes, SIM issues, contract renewal, plan migration, and bill analysis.

The system is designed for **production-grade conversational automation** with strict JSON responses, session memory, workflow orchestration, and resilient API handling.

---

# 🏗️ System Architecture

User → WebSocket Gateway → Intent Classifier → Workflow Router → Domain Subflows → CRM APIs / Data Sources → Structured Response

Core design principles:

• Deterministic JSON outputs  
• Conversation memory persistence  
• Strict validation rules  
• Multi-step conversational flows  
• Event-based responses via WebSocket  

---

# 🧠 Intelligent Intent Routing Layer

The entry workflow acts as a **central intent classifier**.

User queries are classified into:

- bill_dispute
- bill_summary
- contract_renewal
- best_offer
- change_plan
- order_status
- ticket_status
- general

### Key Characteristics

✔ Strict JSON-only responses  
✔ No markdown / formatting noise  
✔ Deterministic intent classification  
✔ Clear separation between billing dispute and bill summary  

This layer ensures **clean orchestration of downstream workflows**.

---

# 🧠 Conversation Memory Management

Memory is critical for telecom conversations where user information is collected across multiple turns.

The system uses **two types of memory layers**.

## 1️⃣ Redis Session Memory

Used for:

• storing session conversations  
• storing extracted IDs  
• storing selected language  
• maintaining conversation continuity  

Example keys:

user:{sessionId}:output

Redis allows:

✔ horizontal scaling  
✔ persistent conversation tracking  
✔ multi-channel session continuation  

---

## 2️⃣ Window Buffer Memory

Implemented using **LangChain memory buffer window**.

Tracks:

• previous conversation messages  
• previously collected parameters  
• user selected options  

Benefits:

• avoids asking same ID repeatedly  
• allows context-aware responses  
• supports multi-step workflows  

Example stored variables:

- AccountId
- ServiceId
- phone_number
- selected_option
- language

---

# 🌐 WebSocket Event Streaming

The system sends responses to the client through **WebSocket events** instead of simple HTTP replies.

Example event structure:

{
"type": "data_response",
"param0": "employee_records"
}

Additional payload types:

• table_data  
• dropdown_content  
• text_message  

### Advantages

✔ Real-time chat response  
✔ structured UI rendering  
✔ dynamic UI components  

For example:

- tables for bill data  
- dropdowns for plan selection  
- structured responses for CRM data

---

# 🔁 Workflow Orchestration

Each telecom use case is implemented as a **dedicated n8n workflow**.

Main workflows include:

### SIM Issue & Offer Support

Handles:

• SIM blocked  
• DSL issues  
• network connectivity issues  
• broadband problems  
• plan offers  

Includes **step-by-step troubleshooting flows**.

If issue remains unresolved:

→ system creates support ticket automatically.

---

### Billing Dispute Workflow

Handles:

• high bill complaints  
• overcharge issues  
• billing disputes  

Conversation flow:

1️⃣ identify billing issue  
2️⃣ collect Account ID  
3️⃣ collect Service ID  
4️⃣ collect dispute amount  
5️⃣ create ticket  

Conversation memory ensures **IDs are not requested repeatedly**.

---

### Bill Summary & Comparison

Supports:

• invoice summary  
• bill comparison  
• VAT breakdown  
• due date detection  
• month-to-month variation  

Also supports **bill comparison logic across months**.

---

### Contract Renewal Automation

Handles:

• auto renewal  
• contract extension  
• purchase new contracts  

Supported contract packages:

- 12Months_SA
- 24Months_SA
- 36Months_SA

Duration conversion logic:

1 week → 7 days  
1 month → 30 days  
1 year → 365 days  

---

### Plan Change Workflow

Handles:

• plan upgrade  
• plan migration  
• switching telecom plans  

Strict phone number validation:

Valid range:

8–15 digits

JSON output is created **only when number is present**.

---

# 🔐 Strict JSON Extraction Engine

All LLM outputs follow **strict deterministic JSON rules**.

Example output:

{
"input": "SIM blocked",
"phone_number": "96834561234"
}

Rules enforced:

✔ No markdown blocks  
✔ No additional keys  
✔ No empty values  
✔ No placeholder numbers  

This ensures **safe integration with downstream APIs**.

---

# 🔄 API Integration & Failure Handling

CRM APIs are invoked for:

• billing data  
• service status  
• account information  
• contract data  
• plan details  

## Failure Handling Strategy

If API call fails:

1️⃣ retry mechanism triggered  
2️⃣ workflow logs error state  
3️⃣ fallback message generated  

Example fallback:

"We are currently unable to retrieve your data. Please try again shortly."

If failure persists:

→ support ticket suggestion.

---

# 🔔 Follow-up Notification Handling

When an issue cannot be resolved automatically:

The system triggers:

✔ ticket creation workflow  
✔ follow-up notification  

Possible follow-ups include:

• ticket confirmation message  
• escalation notification  
• agent follow-up ticket  

Example:

create support ticket for DSL issue

Follow-up events can be sent via:

• CRM notification  
• WebSocket push event  
• ticket tracking response

---

# 🌍 Multi-Language Support

Currently supported languages:

- English
- Arabic

Language detection logic:

1️⃣ ask user preferred language  
2️⃣ store language in memory  
3️⃣ route response through language branch  

Responses automatically switch language based on stored preference.

---

# 🛠 Technology Stack

Core orchestration

• n8n

AI layer

• OpenAI GPT models

Memory layer

• Redis
• LangChain Memory Buffer

Integration layer

• CRM APIs
• WebSocket gateway

---

# 🚀 Key System Capabilities

• AI intent classification  
• telecom troubleshooting automation  
• bill analysis and comparison  
• ticket automation  
• contract lifecycle automation  
• plan recommendation & switching  
• real-time WebSocket responses  
• persistent conversation memory  

---

# 📈 Why This System Is Production Ready

✔ deterministic JSON outputs  
✔ scalable Redis memory  
✔ real-time WebSocket delivery  
✔ strict validation rules  
✔ workflow modularity  
✔ resilient API integration  

---

# 📌 Future Enhancements

Potential improvements:

• WhatsApp bot integration  
• Voice bot / IVR integration  
• Telecom analytics dashboard  
• proactive churn alerts  
• predictive issue detection  

---

© Telecom AI CRM Automation Platform
