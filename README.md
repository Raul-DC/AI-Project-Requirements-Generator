# AI Project Requirements Generator

## 🚀 Overview

This project demonstrates a **proof-of-concept workflow** that transforms a **project brief in natural language** into a **structured technical requirements document**.  
The workflow uses:

- **N8N** (local workflow automation)
- **Ollama LLM** (local AI agent)
- **Webhook integration** for brief submission
- **Markdown output** ready for GitHub repositories

> **Goal:** Show technical ability to orchestrate AI Agents, automate workflows, and structure output for tech delivery — without relying on paid APIs.

---

## 🛠️ Stack

- **N8N** (local workflow editor)
- **Ollama** (local LLM)
- **Model:** `phi` (lightweight, fast, local)
- **PowerShell / Invoke-WebRequest** (to send briefs)
- **Markdown** (for output)

---

## 🏗 Repository Structure

```text
AI-Project-Requirements-Generator/
├── README.md
├── workflows/
│   └── AI_Project_Requirements_Generator.json
├── scripts/
│   └── test_request.ps1
├── screenshots/
│   ├── n8n_workflow.png
│   ├── webhook_config.png
│   ├── http_request_node.png
│   └── sample_output.png
└── LICENSE
````

---

## ⚙️ Workflow Description

### 1️⃣ Webhook Node

Receives the **project brief** as POST request.

* **HTTP Method:** POST
* **Path:** `project-brief`
* **Authentication:** None

![webhook\_config](screenshots/webhook_config.png)

> *Captura sugerida:* muestra la configuración del nodo Webhook en N8N (HTTP Method y Path).

---

### 2️⃣ HTTP Request Node

Sends the brief to **Ollama** local LLM.

* **URL:** `http://127.0.0.1:11434/api/generate`
* **Method:** POST
* **Headers:** `Content-Type: application/json`
* **Body:** JSON containing `model` and `prompt`

![http\_request\_node](screenshots/http_request_node.png)

> *Captura sugerida:* nodo HTTP Request con body y headers configurados.

---

### 3️⃣ Respond to Webhook Node

Returns the **Markdown output** from Ollama.

* **Response Type:** Text
* **Response Body:** `{{ $json.response }}`

![n8n\_workflow](screenshots/n8n_workflow.png)

> *Captura sugerida:* todo el workflow en el canvas de N8N mostrando la conexión Webhook → HTTP Request → Respond to Webhook.

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

![sample\_output](screenshots/sample_output.png)

> *Captura sugerida:* resultado Markdown generado por el AI Agent.

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
