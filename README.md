# 📄 DocuParse Agent (Legal eDiscovery Prototype)

> **Status: 🚧 Active Development (Expected Completion: [16/04/2026])**

## 🎯 Overview
DocuParse is a lightweight, agentic document review pipeline designed to simulate legal eDiscovery workflows. Rather than relying on zero-shot prompting, this platform utilizes a multi-step orchestration approach powered by **Claude 3.5 Sonnet** to extract, classify, and flag high-risk entities within unstructured legal text.

## 🏗️ Tech Stack
* **Frontend:** Next.js, Tailwind CSS (User upload & results dashboard)
* **Backend:** Python (FastAPI)
* **LLM Engine:** Anthropic API (Claude 3.5 Sonnet)
* **Agentic Framework:** Custom Python orchestration (multi-pass inference)

## 🧠 The Agentic Workflow
To ensure reliability and prevent hallucinations, the system does not ask Claude to "read the document and summarize." Instead, it uses a deterministic, multi-step pipeline:

1. **Ingestion & Parsing:** The user uploads a `.txt` file (simulating a contract or legal brief). The backend chunks the text if necessary.
2. **Phase 1: Entity Extraction (Agent 1):** Claude is prompted with strict instructions to output a structured JSON payload extracting specific entities: *Counterparties, Effective Dates, and Governing Law*.
3. **Phase 2: Risk Flagging (Agent 2):** A secondary agentic pass takes the extracted clauses and evaluates them against a predefined set of "High-Risk Rules" (e.g., *Is the liability uncapped? Is the governing law outside the US/UK?*).
4. **Validation & Rendering:** The backend validates the JSON output and serves the structured data back to the Next.js frontend for review.

## Reliability & Guardrails
* **Strict Typing:** Prompts are engineered to force structured JSON outputs, validated via Pydantic models in FastAPI before reaching the client.
* **Separation of Concerns:** Splitting extraction and evaluation into two distinct LLM calls drastically reduces context-confusion and improves accuracy on dense legal text.

## 🚀 Local Setup (Coming Soon)
```bash
# 1. Clone the repository
git clone [https://github.com/yourusername/docuparse-agent.git](https://github.com/yourusername/docuparse-agent.git)

# 2. Setup Backend (FastAPI)
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. Setup Frontend (Next.js)
cd frontend
npm install
npm run dev

# 4. Environment Variables
# Add your Anthropic API key to backend/.env
ANTHROPIC_API_KEY=sk-ant-api03-...
