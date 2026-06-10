# ContractIQ – Agile Backlog & Sprint Planning

Product: **ContractIQ** – AI-powered contract and vendor evaluation (RFP/RFQ, scoring, audit, legal analysis, resume screening).

---

## 1. Product Backlog (Epics & User Stories)

### Epic 1: Workspace & Document Management
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| W-1 | Story | Create workspace | As a **buyer**, I want to **create a named workspace** so that I can **organize documents and runs by project/RFP**. | Create workspace; name required; appears in sidebar; can switch between workspaces. | Must |
| W-2 | Story | Upload documents to workspace | As a **buyer**, I want to **upload PDF/DOCX files into a workspace** so that **Ask, Score, and other modes can use them**. | Upload via UI; support PDF, DOCX; show list; handle errors (size, type). | Must |
| W-3 | Story | Delete workspace | As a **buyer**, I want to **delete a workspace** so that I can **remove old or test projects**. | Confirm before delete; remove workspace and its data from UI/backend. | Should |
| W-4 | Story | Workspace list and selection | As a **buyer**, I want to **see all my workspaces and select one** so that **I work in the right project**. | List workspaces; select one; selection persists during session; clear empty state. | Must |

---

### Epic 2: Ask a Question (Q&A over documents)
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| A-1 | Story | Ask question in workspace | As a **buyer**, I want to **ask a natural-language question** about my workspace documents so that **I get an AI answer with sources**. | Enter question; submit; receive answer; see source refs (e.g. PDF/page); async job supported. | Must |
| A-2 | Story | Compare ChatGPT vs OpenRouter response | As a **buyer**, I want to **optionally compare answers from two AI providers** so that I can **check consistency or choose the better one**. | Toggle to enable comparison; show both answers and sources; clear labeling. | Should |
| A-3 | Story | Use quick prompts in Ask mode | As a **buyer**, I want to **use saved quick prompts** in Ask mode so that **I don’t retype common questions**. | Prompt manager visible when workspace + Ask mode; select prompt populates question; can submit. | Should |
| A-4 | Story | Control response size/type in Ask | As a **buyer**, I want to **choose response length and format** (e.g. short/long, bullet/paragraph) so that **answers match my need**. | Options for size and type; selection sent to backend; response respects choice. | Could |
| A-5 | Story | Web search / specific URL in Ask | As a **buyer**, I want to **optionally use web search or a specific URL** with my question so that **answers can include external context**. | Toggle for web search; optional URL; backend uses it; result reflects external content where used. | Could |

---

### Epic 3: Vendor Recommendations & Research
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| V-1 | Story | Get vendor recommendations from requirements | As a **buyer**, I want to **describe my project requirements** and get **AI shortlisted vendors** so that I can **draft RFPs for the right vendors**. | Input requirements; call vendor-recommendations API (async); show list with rationale; clear loading/error states. | Must |
| V-2 | Story | Toggle sponsored vendors panel | As a **buyer**, I want to **show or hide a sponsored vendors panel** in Vendor Recommendations so that **I can compare organic vs sponsored results**. | Toggle in UI; panel shows/hides; state persists for session. | Should |
| V-3 | Story | Compare vendors side-by-side | As a **buyer**, I want to **compare multiple vendors** in a structured view so that **I can evaluate fit and trade-offs**. | Use Vendor Comparison mode; input selection; get comparison (e.g. table); async supported. | Must |
| V-4 | Story | Research a vendor in depth | As a **buyer**, I want to **run vendor research** for a selected vendor so that **I get a summarized research report**. | Use Vendor Research mode; select vendor/context; get report; async; show in dedicated UI. | Should |

---

### Epic 4: Score Contracts & Combined Evaluation
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| S-1 | Story | Score contracts by criterion | As a **buyer**, I want to **score vendor contracts against a criterion** (e.g. “project understanding”) so that **I get numeric scores and justification**. | Select/enter criterion; set max score; run scoring; see scores per vendor; optional matrix edit; async job. | Must |
| S-2 | Story | Compare OpenRouter vs ChatGPT scores | As a **buyer**, I want to **optionally see scores from both AI providers** so that I can **compare and validate**. | Toggle; backend returns both; UI shows both score sets and comparison. | Should |
| S-3 | Story | Edit score matrix and recalculate | As a **buyer**, I want to **edit the score matrix** (e.g. override a cell) and **recalculate totals** so that **I can adjust and re-run without re-calling AI**. | Edit matrix in UI; recalc on change or button; final scores update; no re-submit to AI for edits. | Should |
| S-4 | Story | Submit admin comment and close scoring | As a **buyer**, I want to **add an admin comment and submit** so that **scoring is finalized and the reporting workflow can run**. | Admin comment field; submit calls backend; success/error feedback; ties to workspace/mode. | Must |
| S-5 | Story | Combined evaluation of contracts | As a **buyer**, I want to **run a combined evaluation** (multiple criteria or dimensions) so that **I get one consolidated view**. | Combined mode; upload/select docs; run evaluation; see combined results; async. | Must |
| S-6 | Story | Resume scoring from last run | As a **buyer**, I want to **resume scoring** from the last saved state so that **I don’t lose progress**. | Resume mode loads last state; can edit and re-submit; same UX as Score mode where applicable. | Should |

---

### Epic 5: Contract Audit & Legal Analysis
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| L-1 | Story | Run contract audit | As a **buyer**, I want to **run an audit** on workspace contracts so that **I get risks and recommendations**. | Audit mode; trigger run; async; show audit report; edit mode to adjust if supported. | Must |
| L-2 | Story | Run legal analysis | As a **buyer**, I want to **run legal analysis** on workspace contracts so that **I get legal insights and clauses summary**. | Legal mode; trigger run; async; show legal report; edit mode to adjust if supported. | Must |
| L-3 | Story | Edit audit/legal report and re-export | As a **buyer**, I want to **edit the generated audit/legal text** and **re-export or save** so that **I can refine before sharing**. | Enter edit mode; change text; save/export; state persists in session. | Should |

---

### Epic 6: Resume Scoring (HR)
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| R-1 | Story | Score resumes in workspace | As an **HR user**, I want to **score resumes** in a workspace so that **I get rankings and fit scores**. | Upload/select resumes; run score-resumes; async; see results; clear UI. | Must |
| R-2 | Story | Edit resume scores and re-run | As an **HR user**, I want to **edit score matrix** and **recalculate** so that **I can tune without re-running AI**. | Same pattern as contract score matrix edit; recalc; persist. | Should |

---

### Epic 7: Feature Request & Feedback
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| F-1 | Story | Submit feature request | As a **user**, I want to **submit a feature request** (no workspace required) so that **product can improve**. | Form (title, description, etc.); submit to backend; success/error; no workspace needed. | Should |
| F-2 | Story | Request Feature from welcome screen | As a **user**, I want to **open Feature Request** from the welcome screen so that **I can give feedback before choosing a workspace**. | Button on welcome page; opens feature-request mode; form works. | Could |

---

### Epic 8: Reporting & Backend Workflows
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| B-1 | Story | Generate score report (JSON → DAG) | As a **system**, when scoring is submitted, **save score JSON and trigger DAG** so that **reports (CSV, DOCX) are generated**. | /score saves to data/{workspace}/last_score.json; trigger Airflow DAG; DAG runs. | Must |
| B-2 | Story | Email delivery of reports | As a **buyer**, I want **generated reports to be emailed** so that **stakeholders get them without logging in**. | DAG or backend sends email (SMTP); configurable in .env; delivery status visible or logged. | Should |
| B-3 | Story | Poll async job status | As a **user**, I want **long-running jobs to be async** so that **the UI doesn’t time out**. | Endpoints return job_id when async; frontend polls until done; show progress or spinner. | Must |

---

### Epic 9: UX & Quality
| ID | Type | Title | User Story | Acceptance Criteria | Priority |
|----|------|--------|------------|---------------------|----------|
| U-1 | Story | Dark/Light theme toggle | As a **user**, I want to **switch between dark and light theme** so that **I can reduce eye strain**. | Toggle in UI; theme persists for session (or stored); all main screens respect it. | Should |
| U-2 | Story | Progress steps for long operations | As a **user**, I want to **see progress steps** during long runs (e.g. Ask, Score) so that **I know the system is working**. | Steps shown (e.g. “Embedding…”, “Scoring…”); update as job progresses; clear on completion. | Should |
| U-3 | Story | Error handling and messages | As a **user**, I want **clear error messages** when something fails so that **I can retry or fix input**. | API errors surfaced in UI; validation errors on forms; no silent failures. | Must |
| U-4 | Story | Welcome page and mode navigation | As a **user**, I want a **welcome page and clear mode navigation** so that **I can find Ask, Score, Vendor, Audit, Legal, Resume, Feature Request**. | Welcome when no workspace or mode; sidebar or buttons for each mode; sections (Procurement, RFP, HR, Feedback). | Must |

---

## 2. Sample Sprint Plan (2-week sprint)

**Sprint goal:** Stable workspace + Ask + Score flows and one vendor flow.

### Sprint backlog (tickets)

| Ticket | Title | Story Points | Owner | Notes |
|--------|--------|--------------|--------|--------|
| W-1 | Create workspace | 3 | Dev | API + UI |
| W-2 | Upload documents to workspace | 5 | Dev | File types, size limits, list |
| W-4 | Workspace list and selection | 3 | Dev | Sidebar, selection state |
| A-1 | Ask question in workspace | 5 | Dev | Async ask, sources |
| A-2 | Compare ChatGPT vs OpenRouter (Ask) | 3 | Dev | Toggle + dual response UI |
| S-1 | Score contracts by criterion | 5 | Dev | Criterion, max score, async |
| S-4 | Submit admin comment and close scoring | 2 | Dev | API + success/error |
| B-3 | Poll async job status (Ask + Score) | 5 | Dev | job_id, polling, timeout |
| U-3 | Error handling and messages | 3 | Dev | Global and per-mode |
| U-4 | Welcome page and mode navigation | 3 | Dev | Layout, links to all modes |

**Sprint capacity:** ~37 points (adjust to your team’s velocity.)

**Stretch:** V-1 (Vendor recommendations) if capacity allows.

---

## 3. Backlog Summary (for board columns)

Use these as **backlog items** you can paste into Jira/Linear/Notion/Excel:

- **Epic 1 – Workspace:** W-1, W-2, W-3, W-4  
- **Epic 2 – Ask:** A-1, A-2, A-3, A-4, A-5  
- **Epic 3 – Vendor:** V-1, V-2, V-3, V-4  
- **Epic 4 – Score & Combined:** S-1, S-2, S-3, S-4, S-5, S-6  
- **Epic 5 – Audit & Legal:** L-1, L-2, L-3  
- **Epic 6 – Resume:** R-1, R-2  
- **Epic 7 – Feature Request:** F-1, F-2  
- **Epic 8 – Backend/Reporting:** B-1, B-2, B-3  
- **Epic 9 – UX:** U-1, U-2, U-3, U-4  

---

## 4. Definition of Done (suggested)

- Code merged to main (or sprint branch) and reviewed.  
- Acceptance criteria for the story met.  
- No critical/blocking bugs for the story.  
- Async flows (where applicable) use job_id and polling.  
- Errors shown in UI; no silent failures for user actions.  
- README or .env comments updated if new config is added.  

---

*You can copy tables into your tool (e.g. Jira import, CSV, or Notion) and adjust IDs, points, and owners to match your team.*
