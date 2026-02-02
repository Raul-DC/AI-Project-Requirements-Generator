# AI Project Requirements Generator

<img width="1024" height="1536" alt="projectimage" src="https://github.com/user-attachments/assets/cefaf09f-cabc-462c-87b6-0ed20d56b228" />


## 🚀 Overview

This project demonstrates a **proof-of-concept workflow** that transforms a **project brief in natural language** into a **structured technical requirements document**.  

The workflow uses:

- **N8N** (local workflow automation)
- **Ollama LLM** (local AI agent)
- **Webhook integration** for brief submission
- **Markdown output** ready for GitHub repositories

> **Goal:** Show technical ability to orchestrate AI Agents, automate workflows, and structure output for tech delivery — without relying on paid APIs.

![N8NWorkflow](https://github.com/user-attachments/assets/0c1cf036-a589-4571-af77-3ca6fbb3c404)

---

## 🛠️ Stack

- **N8N** (local workflow editor)
- **Ollama** (local LLM)
- **Model:** `phi` (lightweight, fast, local)
- **PowerShell / Invoke-WebRequest** (to send briefs)
- **Markdown** (for output)

---

## ⚙️ Workflow Description

### 1️⃣ Webhook Node

Receives the **project brief** as POST request.

* **HTTP Method:** POST
* **Path:** `project-brief`
* **Authentication:** None

![WebhookNodeConfig](https://github.com/user-attachments/assets/3d545e2c-7909-4215-b33e-01a1b3fac98d)

---

### 2️⃣ HTTP Request Node

Sends the brief to **Ollama** local LLM.

* **URL:** `http://127.0.0.1:11434/api/generate`
* **Method:** POST
* **Headers:** `Content-Type: application/json`
* **Body:** JSON containing `model` and `prompt`

![HttpRequestNodeConfig](https://github.com/user-attachments/assets/7f08797a-c982-42ee-9339-9be96c980e64)


---

### 3️⃣ Respond to Webhook Node

Returns the **Markdown output** from Ollama.

* **Response Type:** Text
* **Response Body:** `{{ $json.response }}`

![RespondToWebhookNodeConfig](https://github.com/user-attachments/assets/a7f61adb-2fee-48ff-a4ca-64b36c6810de)

---

## 🧩 Example Execution

Send a test brief using PowerShell:

```powershell
Invoke-WebRequest `
  -Uri http://localhost:5678/webhook-test/project-brief `
  -Method POST `
  -ContentType "text/plain" `
  -Body "We want a web platform where clients can submit support tickets and track their status."
```

Output example:

![InvokeRequestPowerShell](https://github.com/user-attachments/assets/5c2e91c3-92a6-4394-993f-d652396bc948)

---

## 🤖 AI Agent Details

* The **AI Agent** is the `phi` LLM running locally via Ollama.
* The **prompt** defines how to transform natural language into structured technical requirements.

```markdown
# Project Title
## Executive Summary
## Assumptions
## User Stories
## Proposed API Endpoints
## Suggested Project Structure
```

> The workflow itself is model-agnostic — you can swap `phi` for a larger model like LLaMA 3 or Mistral for more accurate outputs.

---

## ⚠️ Known Limitations

* Output quality depends on the selected LLM.
* Lightweight local models may misinterpret nuanced requirements.
* Human review is recommended before delivery.

---

## 📝 References / Notes

* LLM runs locally using an open-source model to avoid external dependencies.
* Workflow demonstrates professional handling of AI, automation, and technical documentation.

---
