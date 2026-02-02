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

![AboutTheProject](https://github.com/user-attachments/assets/25eff973-860b-4893-ba39-4be266906515)

---

## 🛠️ Stack

- **N8N** (local workflow editor)
- **Ollama** (local LLM)
- **Model:** `phi` (lightweight, fast, local)
- **PowerShell / Invoke-WebRequest** (to send briefs)
- **Markdown** (for output)

---

## ⚙️ Workflow Description

 Let's view the workflow and explain each node:

![N8NWorkflow](https://github.com/user-attachments/assets/6c2150df-56e7-491b-816d-f7968d12b92f)


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

## 🖥️ Running the Workflow

To run the project locally, open **two PowerShell windows**:

1. **PowerShell 1:** Start N8N editor
```powershell
npx n8n
```

![NPXN8NPowerShell](https://github.com/user-attachments/assets/02e0b424-d213-4926-8eed-572f9011db33)

> Open your browser at [http://localhost:5678](http://localhost:5678) to access the workflow editor.

![N8NPersonalTab](https://github.com/user-attachments/assets/54297129-2c27-4ef2-8da7-f9079e05f62a)

2. **PowerShell 2:** Start the AI Agent (`phi` LLM)

```powershell
ollama run phi
```

![ollamaRunPowerShell](https://github.com/user-attachments/assets/c7e57b15-2b37-47fd-bcfa-054d0b46c611)

> Keep both windows open while testing the workflow.

---

## 🧩 Example Execution

Press the "Execute Buttom" on the workflow:

![N8NExecuteWorkflowButtom](https://github.com/user-attachments/assets/73d6b5d9-a34e-4a8a-b510-da9cc79d98d0)

The N8N will look like this:

![N8NWaitingTriggerEvent](https://github.com/user-attachments/assets/25aa2072-59dc-4afc-b36c-01d3902b09d9)

Send a test brief using PowerShell (You must open a 3rd PowerShell window):

```powershell
Invoke-WebRequest `
  -Uri http://localhost:5678/webhook-test/project-brief `
  -Method POST `
  -ContentType "text/plain" `
  -Body "We want a web platform where clients can submit support tickets y track their status." `
  | Select-Object -ExpandProperty Content `
  | Tee-Object -FilePath "C:\Users\User\Desktop\RHLoop notes\Proyectos\Proyecto 10 - N8N AI Agent\output.md"
```

Input example:

![InvokeRequestPowerShell](https://github.com/user-attachments/assets/d59c229a-adc8-45a7-a576-89cbddaa0cfb)

N8N workflow executing a brief, showing the HTTP Request node processing the AI Agent response:

![N8NExecutingWorkflow](https://github.com/user-attachments/assets/9e97f74d-0e5e-4adb-8b04-791d7bb00ac9)

Final state of workflow execution, with all nodes marked as successfully executed:

![N8NExecutedWorkflow](https://github.com/user-attachments/assets/09011b11-ce55-4c82-9575-edff1d094f19)

Output example:

![InvokeRequestResultPowerShell](https://github.com/user-attachments/assets/30d20ce9-d3bf-4188-8832-1a1aabcdb160)

![mdFile](https://github.com/user-attachments/assets/9daf53f9-0c75-43c8-9389-6de0cb83d646)

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
