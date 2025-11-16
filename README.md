##**Automated Social Media Agent**
## 🚀 **Overview**

This project is a fully automated **AI-powered social media agent** designed to handle:

* 🔹 Incoming DMs & comments
* 🔹 Smart auto-replies using LLM agents
* 🔹 Trend analysis across platforms
* 🔹 Automated content generation (captions, ideas, variants)
* 🔹 Content scheduling & auto-posting
* 🔹 Human escalation system
* 🔹 Continuous learning via RAG + feedback loops

Built using:

* **n8n** for workflow automation
* **OpenAI/Gemini** for LLM capabilities
* **Instagram/Facebook Graph API** for posting
* **Google Sheets** for logging + analytics
* **HTTP/Webhooks** for universal platform support
* **Vector DB** (optional, future) for RAG-based reply generation

This project is your **Capstone-ready** submission for Kaggle’s “5-Day AI Agents Intensive”.

---

## 🧠 **Core Features**

### ✔ 1. Incoming Message Automation

Handles all inbound messages using:

* Webhook trigger
* Intent classification (greeting, inquiry, complaint, lead, spam, etc.)
* Smart reply via LLM
* Spam filtering
* Multi-agent reply system
* Logs all interactions to Google Sheets
* Safe rules for sensitive queries

Supports:
Instagram, Facebook, Twitter, Telegram, WhatsApp (Business API).

---

### ✔ 2. Content Generation Agent

Automatically produces:

* Post ideas
* Caption variants
* Reels ideas
* Brand-specific tone
* Hashtags
* Carousel content outlines

LLM prompt templates ensure *consistent brand voice*.

---

### ✔ 3. Trend Analysis Agent

Every 24 hours, the agent:

* Pulls trending topics from Google Trends, X/Twitter, Reddit
* Analyzes patterns
* Creates actionable content ideas
* Stores results in Google Sheets
* Suggests tomorrow’s content direction

---

### ✔ 4. Content Scheduler

Automatically:

* Fetches scheduled posts
* Prepares Instagram/Facebook API payload
* Publishes at optimal times
* Logs everything in Google Sheets

Works similarly to Buffer/Hootsuite but fully automated and customizable.

---

### ✔ 5. Human Escalation System

For complex or sensitive messages:

* Sends notification to Slack/Email
* Waits for human input
* Uses a “Wait for Webhook” node
* Human-approved message is sent back to the user
* Logged for audit

---

## 🏗 **Architecture**

```
                   ┌────────────────────────┐
                   │     Social Platforms    │
                   │ IG / FB / X / TG / WA   │
                   └───────┬────────────────┘
                           ↓ Webhooks
              ┌────────────────────────────────┐
              │         n8n Automation         │
              │                                │
              │  1. Incoming Message Workflow  │
              │  2. Trend Analysis Workflow    │
              │  3. Content Generator          │
              │  4. Content Scheduler          │
              │  5. Human Escalation           │
              └───────┬──────────────┬────────┘
                      ↓              ↓
            ┌────────────────┐   ┌─────────────────┐
            │ LLM (GPT/Gemini)│   │   Google Sheets │
            └────────────────┘   └─────────────────┘
                      ↓
            ┌──────────────────────┐
            │  Platform API Sender  │
            └──────────────────────┘
                      ↓
            ┌──────────────────────┐
            │ Social Media Posting │
            └──────────────────────┘
```

---

## 📦 **Project Structure**

```
/
├── workflows/
│   ├── incoming_message_workflow.json
│   ├── content_generation_workflow.json
│   ├── trend_analysis_workflow.json
│   ├── scheduling_workflow.json
│   └── escalations_workflow.json
│
├── docs/
│   ├── architecture_diagram.png
│   ├── api_setup_guide.md
│   ├── llm_prompts.md
│   ├── platform_credentials.md
│   └── troubleshooting.md
│
├── README.md  ← (you are here)
│
└── environment/
    ├── .env.example
    └── secrets_notes.md
```

---

## 🛠 **Technologies Used**

### **Automation**

* n8n (core workflow engine)
* Webhooks
* Cron triggers
* HTTP Request nodes

### **AI Models**

* OpenAI GPT-4o / GPT-4o-mini
* Google Gemini 1.5 Flash / 1.5 Pro
* Optional: Claude, Llama-based models

### **APIs**

* Meta Graph API (Instagram/Facebook)
* Twitter/X API
* Google Sheets API
* Google Trends (via proxy)
* Telegram API (optional)

### **Storage**

* Google Sheets (logs, content ideas, trends)
* Optional: Pinecone / Chroma for RAG

---

## 🔧 **Setup Instructions**

### **1. Install n8n**

Use Docker (recommended):

```sh
docker run -it --rm -p 5678:5678 n8nio/n8n
```

Or manually install:

```sh
npm install n8n -g
n8n start
```

---

### **2. Import Workflows**

Go to:

**n8n → Workflows → Import from File → Select JSON**

Import all:

* Incoming Message Automation
* Content Generation
* Trend Analysis
* Scheduler
* Escalation

Or import the **full combined workflow**.

---

### **3. Add Credentials**

Inside n8n → Credentials:

* OpenAI / Gemini
* Google Sheets
* HTTP Request (for platform APIs)
* Slack / Gmail (for escalation)

---

### **4. Configure API Endpoints**

Update:

* Instagram page token
* Facebook page ID
* Google Sheet ID
* API URLs in HTTP nodes
* Trend proxy endpoint

---

### **5. Set Up Platform Webhooks**

Point your social media webhook URLs to:

```
https://your-n8n-domain/webhook/incoming-message
```

---

## 🧪 **Testing the System**

### **Test 1 — Incoming Message**

Send:

```json
POST /webhook/incoming-message
{
  "platform": "instagram",
  "user": "user123",
  "message": "What is your pricing?"
}
```

Result:

* Intent classification
* LLM reply
* Auto reply sent
* Logged to Google Sheets

---

### **Test 2 — Trend Agent**

Trigger the **Trend Cron** manually.

Result:

* Trends pulled
* LLM analysis
* Ideas stored

---

### **Test 3 — Content Generation**

Trigger Content Cron.

---

### **Test 4 — Scheduler**

Feed a scheduled post and wait.

---

### **Test 5 — Human Escalation**

Send a message like:

> “Your product is a scam”

Slack receives alert → human approves → reply sent.

---

## 📈 **Performance & Safety**

Built-in:

* Rate limiting
* Spam detection
* Human fallback
* API retry logic (platform-side)
* Brand-safe prompt templates

---

## 🧩 **Multi-Agent Breakdown**

### **Agents in the System:**

| Agent           | Role                          |
| --------------- | ----------------------------- |
| Reply Agent     | Auto-replies to messages      |
| Sentiment Agent | Determines safe tone          |
| Order Agent     | Handles order-related queries |
| Complaint Agent | Escalates sensitive issues    |
| Content Agent   | Generates content & captions  |
| Trend Agent     | Pulls & analyzes trends       |
| Scheduler Agent | Publishes posts               |
| RAG Agent       | Learns from past replies      |

---

## 🧠 **LLM Prompts**

These are stored in `/docs/llm_prompts.md`.

Includes:

* Intent classifier prompt
* Brand tone system prompt
* Content generator prompt
* Complaint-handling prompt
* RAG-enhanced instruction prompt

---

## 🚀 **Future Improvements**

* Add Pinecone for advanced reply memory
* Add reinforcement learning from engagement
* Add multi-language reply support
* Add analytics dashboard (React.js)
* Add niche-specific models
* Add drift detection in customer behavior

---

## 🏁 **Conclusion**

This project is a **complete AI Automation suite** for social media teams:
auto-replies, trend analysis, content generation, and scheduling — all in one intelligent agent.

---
