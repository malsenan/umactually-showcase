# umactually.ai – Multi-LLM Debate Assistant

**Ask one question. Get three LLMs to argue about the answer, then synthesize the best response.**

umactually.ai is a web app that runs a structured “debate” between multiple LLMs (OpenAI, Anthropic, Gemini, etc.) and gives you a final, synthesized answer *plus* the full back-and-forth if you want to inspect the reasoning.

This repo is **public-facing documentation only** (no source code) so I can share the concept, architecture, and live app with others.

---

## 🔍 Why this exists

Most LLM apps do this:

> User asks question → one model answers → user hopes it’s correct.

That breaks down when:
- You care about **factual accuracy**, not just eloquence  
- You want **different models’ perspectives** without copy-pasting between tools  
- You’d like **transparent reasoning** instead of a mysterious final answer  

umactually.ai tries to fix this by:
- Running **multiple models in parallel** on the same prompt  
- Having them **critique each other’s answers** over several rounds  
- Producing a **final synthesized answer** that integrates the strongest parts and fixes errors  
- Letting you **toggle on/off** the internal “LLM conversation” if you want to see the debate

---

## 🧠 How it works (high level)

1. **You ask a question** in the chat UI.
2. The backend sends your prompt to **three different LLMs**.
3. Each model produces an initial answer.
4. In a few rounds, each model:
   - Reads the others’ answers  
   - Points out mistakes, missing context, or weak reasoning  
   - Revises its own answer based on those critiques  
5. The app then produces a **final answer** that:
   - Resolves disagreements where possible  
   - Surfaces important caveats/uncertainties  
   - Uses the strongest reasoning found during the debate  
6. You can:
   - Just read the **final answer**, or  
   - **Expand** the LLM conversation to see the entire debate per round.

---

## 🌐 How to access the app

👉 **Live app:** https://umactually.ai  

- If the app is open:  
  Just visit the URL above and start asking questions.
- If the app is currently gated / in beta:  
  You might see a password screen or access notice. In that case:
  - For access during the beta, email me at **malsenan.dev@gmail.com**.

---

## ✨ Key “user-facing” features

- **Single prompt, multi-model answers**  
  One input → multiple LLMs coordinate behind the scenes.

- **Configurable debate depth**  
  Choose how many critique rounds (e.g., 1–4) for deeper or faster answers.

- **Final synthesis**  
  A single, readable answer that integrates the best reasoning.

- **Optional transparency**  
  Toggle to reveal the full LLM debate if you want to inspect the reasoning.

- **Persistent chats & conversations**  
  Chats store multiple “conversation rounds,” so you can revisit how an answer evolved.

---

## 🏗️ Under the hood (architecture overview)

> This is intentionally high-level; the implementation is in a private repo.

- **Frontend**
  - React + TypeScript
  - Modern chat UI with:
    - Streaming-friendly layout
    - Per-conversation toggles for showing/hiding LLM debates
    - Sidebar of chats & conversations
    - Support for future features like message attachments and FAQ modals

- **Backend**
  - FastAPI (Python), async
  - Orchestrates calls to:
    - OpenAI (ChatGPT family)
    - Anthropic (Claude)
    - Google (Gemini)
  - Runs a **multi-round evaluation loop** where each model:
    - Reads peer responses
    - Produces critiques
    - Updates its own answer
  - Persists all chats / conversations / model responses in a relational DB

- **Data & Infra**
  - Postgres (via SQLModel / SQLAlchemy)
  - Deployed to a cloud platform with connection pooling
  - SSE / streaming-ready architecture for sending incremental updates to the frontend

---

## 🎯 What problems this solves (for real users)

- **Reduces hallucination risk**  
  Models catch each other’s mistakes more often than a single model catching its own.

- **Surfaces edge cases & caveats**  
  Contradictions between models often highlight “gotchas” you’d miss with one answer.

- **Saves time hopping between tools**  
  Instead of manually asking 3 separate models, the orchestration is automatic.

- **Great for learning & research**  
  Seeing the debate can help you understand *why* an answer might or might not be trustworthy.

---

## 📁 What this repo contains (and doesn’t)

**This public repo _does_ include:**
- This README explaining:
  - The concept and motivation
  - The architecture at a high level
  - How to access and use the app
- TODO: Screenshots / diagrams in a `/docs` or `/assets` folder  

**This public repo _does not_ include:**
- Backend or frontend source code
- Infrastructure configs
- Secrets or API keys

The actual implementation lives in a **private repo** for now (security, cleanliness, and WIP reasons).

---

## 👋 For hiring managers / collaborators

If you’re here because you saw this project on my resume or LinkedIn:

- This app is a demonstration of:
  - Full-stack ownership (frontend, backend, infra, and product design)
  - Working with multiple LLM providers and APIs
  - Designing structured prompts and multi-agent coordination
  - Building a production-ready experience around LLMs (not just a toy chat)

If you’d like:
- A deeper technical walkthrough  
- Screenshots / short demo video  
- Or to discuss potential collaboration / roles  

Feel free to reach out:  
**\[Muhannad Alsenan\] – [malsenan.dev@gmail.com]**

---

## License

This documentation is provided under the MIT License.  
The underlying application code is proprietary and not included in this repository.
