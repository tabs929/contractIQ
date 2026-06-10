# ContractIQ – Demo Presentation Script

Use this script to record an engaging demo that **shows** (not just states) modularity, scalability, and security. Adjust timing to your video length.

---

## 1. Opening (30–45 sec)

**Say:**  
*"This is ContractIQ — an AI-powered platform for contract and vendor evaluation: RFP scoring,. I’ll show what we’ve built since the first demo, then walk through how we engineered it for modularity, scalability, and security."*

**Do:**  
Show the welcome screen and mode navigation (Ask, Score, Vendor, ). Briefly show workspace creation and selection.

---

## 2. What’s New Since the First Demo (1–2 min)

**Say (with live demo):**

- **"Workspace and documents."** We had basic workspace; now we have workspace list, selection, upload (PDF/DOCX), and **Google Drive integration** — connect a workspace to Drive and pull documents from there. *[Show creating a workspace, uploading or picking from Drive.]*

- **"Ask a question."** We had simple Q&A; now we have **response size and format** (short/long, bullet/paragraph), **optional web search and specific URL**, and **quick prompts** so common questions are one click. *[Show Ask mode with a prompt, optional web/URL, and response.]*

- **"Score contracts."** We added **async scoring** so long runs don’t time out — the UI gets a job ID and polls until done. We also support **compare OpenRouter vs ChatGPT**, **edit score matrix and recalculate** without re-calling AI, **admin comment and close scoring**, and **combined evaluation** (multiple criteria in one run). *[Show starting a score, then show matrix and optional edit/recalc.]*

- **"Vendor flows."** Since the first demo we added **Vendor Recommendations** (requirements → shortlist), **Vendor Comparison** (side-by-side), and **Vendor Research** (in-depth report). All of these use the same async job pattern. *[Show one of these flows and that it returns when the job completes.]*

- **"Audit and legal."** We have **contract audit** (risks and recommendations) and **legal analysis** (clauses and insights), both async. *[Trigger one and show result.]*

- **"Reporting."** When scoring is submitted we **save score JSON** and **trigger an Airflow DAG** that generates CSV/DOCX and can email reports — so the pipeline is decoupled from the API. *[Mention or show DAG trigger / report output.]*

- **"UX."** We added **progress steps** for long operations, **theme toggle** (dark/light), **error handling** so failures are visible, and **Feature Request** from the welcome screen.

**Do:**  
Keep it to 2–3 concrete flows; you can say “same pattern” for the rest.

---

## 3. Modularity — Show, Don’t Just State (1–1.5 min)

**Say:**  
*"We didn’t just say we’re modular — we built it that way. Here’s how."*

**Show (code or architecture diagram):**

1. **Layered / component-based design**
   - **Frontend:** One React app, but **each mode is its own component**: `AskQuestionMode`, `ScoreContractsMode`, `VendorRecommendationMode`, `VendorComparisonMode`, `VendorResearchMode`, `AuditMode`, `LegalMode`, `ResumeScoringMode`, `CombinedEvaluationMode`, `FeatureRequestMode`. Shared pieces like `WorkspaceSelector`, `FileUpload`, `PromptManager`, `ThemeToggle` are reused. *[Optional: open `frontend/src/components` and scroll the list.]*
   - **Backend:** FastAPI in `main.py` for routes, but **business logic lives in services**: `rag_service`, `audit_service`, `legal_service`, `vendor_recommendation_service`, `vendor_comparison_service`, `vendor_research_service`, `resume_scoring_service`, `combined_evaluation_service`, `parser_service`, `embedder_service`, `google_drive_service`. Routes call these; they don’t embed scoring or RAG logic. *[Optional: open `backend/services/` and name 2–3.]*

2. **Separation of concerns**
   - **Data layer:** Qdrant for embeddings and vector search; persistent storage under `data/{workspace}` for documents and results. The diagram shows User → Frontend → Backend → Data/AI/External APIs — clear layers. *[Show diagram4.mermaid or README layout.]*
   - **External APIs:** OpenRouter, Perplexity, SMTP, Google Drive are used by the backend; the UI doesn’t talk to them directly. Adding a new AI provider or email backend is a backend change.

**Say:**  
*"So modularity is real: modes as components, services for logic, and a clear split between UI, API, data, and external services."*

---

## 4. Scalability — Show, Don’t Just State (1–1.5 min)

**Say:**  
*"Scalability is built in through a few concrete choices."*

**Show:**

1. **Async jobs and no request timeouts**
   - Long operations (score, audit, legal, vendor comparison, vendor research, etc.) can run for minutes. We **don’t hold the HTTP request open**. We use a **Redis job queue**: the API enqueues a job, returns a **job_id** immediately, and a **background worker** processes it. The frontend **polls** `GET /jobs/{job_id}` until status is SUCCESS or FAILURE. *[Optional: show `pollJobStatus` in `api.js` or a mode that uses it.]*
   - That avoids gateway timeouts (e.g. Cloudflare 524) and lets many users start jobs without blocking the server.

2. **Extensibility**
   - New “modes” or workflows = new frontend component + new backend endpoint (and often a new service). The job queue is generic: `POST /jobs/{job_type}` with a payload, so new long-running flows plug into the same pattern.
   - Reporting is **offloaded to Airflow**: score submission writes JSON and triggers a DAG. Heavier reporting (CSV, DOCX, email) runs in the workflow engine, not in the API process.

3. **Configuration and growth**
   - Hosts and ports (Qdrant, Redis, Airflow, frontend URL) come from **environment variables** (e.g. `.env`), so we can point to different backends or scale components independently. *[Mention README: REDIS_URL, QDRANT_HOST, AIRFLOW_*, etc.]*

**Say:**  
*"So scalability comes from async jobs, a generic job pipeline, and moving reporting to Airflow, with config-driven deployment."*

---

## 5. Security — Show, Don’t Just State (1–1.5 min)

**Say:**  
*"Security is addressed in several places; here’s what’s actually in the system."*

**Show:**

1. **Authentication and access control**
   - We have an access check used on sensitive operations (e.g. Google Drive auth). *[Optional: point to `check_access()` and the 403 on “Access denied” in main.py.]*
   - **Google Drive** is OAuth-based: we don’t store user passwords; we exchange a code for tokens per workspace. *[Mention when showing Drive integration.]*
   - **Airflow** is configured with its own auth (e.g. `simple_auth_manager` in config); the API triggers the DAG with credentials from env (no secrets in code).

2. **Input validation**
   - **API request bodies** are validated with **Pydantic** models: `QuestionRequest`, `ScoreContractsRequest`, `VendorRecommendationRequest`, etc. Invalid or missing required fields return **422** before any business logic runs. *[Optional: show one `class XRequest(BaseModel)` in main.py.]*
   - We use **typed request models** for score, audit, legal, vendor, and contact/feature requests so we don’t trust raw JSON.

3. **Secure configuration**
   - **Secrets** (API keys, SMTP, Airflow credentials) are in **`.env`**, not in the repo. README states `.env` is required and not committed. *[Show README “.env File” section.]*
   - **CORS** is explicit: only allowed origins (e.g. your frontend domains and localhost) are in the middleware list — not a wildcard. *[Point to `allow_origins` in main.py.]*

4. **Threat-aware design**
   - Licensing/validation code (e.g. `allyin_licensing/validation.py`) validates product/customer IDs and rejects path-unsafe characters and reserved names — that’s the kind of validation we want for workspace or user inputs in the future. *[Optional: briefly mention that we validate/sanitize inputs where it matters.]*

**Say:**  
*"So security is implemented via access checks, OAuth for Drive, Pydantic validation on the API, env-based secrets, and strict CORS."*

---

## 6. Engaging / Innovative Angle (30–45 sec)

**Pick one or two:**

- **“Ask with the web”:** Show asking a question with **web search** or a **specific URL** and getting an answer that combines workspace documents and live web content. *"That’s not just RAG — it’s RAG plus external context, controlled by the user."*

- **“One click to score”:** Show **quick prompts** in Ask mode and then **Score** with a single criterion and async completion. *"From question to scored contracts without leaving the flow."*

- **“Vendor pipeline”:** Show **Vendor Recommendations** → pick vendors → **Vendor Comparison** or **Vendor Research**. *"We close the loop from requirements to shortlist to comparison and research."*

- **“Transparency”:** Show **Compare OpenRouter vs ChatGPT** (Ask or Score) and briefly show both answers or score sets. *"We don’t hide the model — we let you compare."*

**Say:**  
*"That’s ContractIQ: built modular so we can extend it, built async so it scales, and built with validation and secure config so we take security seriously."*

---

## 7. Closing (15–30 sec)

**Say:**  
*"To recap: we showed what’s new since the first demo — workspaces, Ask with web/URL and prompts, async scoring and vendor flows, audit and legal, and reporting via Airflow. We showed modularity with component-based UI and a service layer in the backend, scalability with a Redis job queue and Airflow for reporting, and security with access checks, OAuth, Pydantic validation, and env-based secrets. Thanks for watching."*

---

## Quick reference – Evidence checklist

| Theme        | What to show |
|-------------|--------------|
| **Modularity** | `frontend/src/components/` (mode components), `backend/services/` (rag, audit, legal, vendor_*, etc.), diagram4.mermaid (layers). |
| **Scalability** | Redis `JobManager`, `async_mode=true` and `job_id`, `pollJobStatus` in api.js, Airflow DAG trigger, env vars in README. |
| **Security**   | `check_access()`, Pydantic request models, CORS `allow_origins`, `.env` in README, Google OAuth for Drive. |

Use this script as a narrative; shorten or expand sections to fit your video length and style.
