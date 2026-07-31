# ATLAS: Agentic Cinematic Production - Master Software Blueprint

**Version:** 1.0.0
**Target Environment:** Google Cloud (Gemini), Replit, Next.js, FastAPI, PostgreSQL

---

## 1. Product Vision
- **Complete Purpose:** ATLAS is an autonomous AI agent orchestration platform that automates the cinematic production lifecycle. It translates rough concepts into structured screenplays, breaks them down into scenes, writes rendering/generation code for those scenes, and securely executes that code via Replit sandboxes to generate final video assets.
- **Target Users:** Independent filmmakers, enterprise media studio executives, content creators, and AI researchers.
- **User Personas:**
  - *The Studio Executive:* Wants a high-level overview of project status, costs, and final asset review.
  - *The Director:* Inputs creative vision, reviews script drafts, and approves storyboard generations.
  - *The Technical Producer:* Manages API keys, monitors Replit sandbox execution logs, and handles integrations.
- **Main Problems Solved:** The fragmentation of generative AI tools. Currently, creators manually move text from LLMs to image generators to video generators. ATLAS automates the entire pipeline through multi-agent orchestration.
- **Success Metrics:** Time from prompt to final render (target < 15 mins for a 3-min trailer), 99.9% sandbox execution success rate, zero leaked API credentials.
- **Long-term Vision:** Become the industry standard "Autonomous Studio" operating system, replacing manual post-production pipelines.

---

## 2. Functional Requirements

### Feature 1: Project & Concept Ingestion
- **Purpose:** Allow users to create a new movie project by providing a raw text concept.
- **Trigger:** User clicks "New Project" and submits a form.
- **Inputs:** `title` (string, max 100 chars), `concept` (text, min 50 chars, max 2000 chars), `genre` (enum).
- **Outputs:** Creates a `Project` database record. Transitions UI to the "Script Generation" dashboard.
- **Validation:** `concept` must pass basic content safety filters (no highly illegal content).
- **Edge Cases:** Network failure during submission -> save draft locally in `localStorage`.
- **Error Handling:** If database insert fails, return 500 with "Unable to initialize project. Please try again."
- **Dependencies:** PostgreSQL database.
- **User Permissions:** Authenticated users only.
- **Expected Behavior:** Returns `projectId` and redirects to `/projects/{projectId}`.

### Feature 2: Autonomous Script Generation
- **Purpose:** Gemini Agent acts as a screenwriter to expand the concept into a scene-by-scene script.
- **Trigger:** Automatic upon project creation, or manual trigger "Generate Script".
- **Inputs:** `projectId`, `concept`.
- **Outputs:** A structured JSON object containing an array of `Scene` objects (scene number, location, action, dialogue).
- **Validation:** JSON schema validation of the LLM output. Must contain at least 1 scene.
- **Error Handling:** If Gemini returns malformed JSON, retry up to 3 times with a strict JSON format prompt.
- **Dependencies:** Google Cloud Vertex AI (Gemini 1.5 Pro).

### Feature 3: Replit Sandbox Execution (The Editor Agent)
- **Purpose:** Execute python scripts (e.g., calling FFmpeg, video generation APIs) to render a specific scene.
- **Trigger:** User clicks "Render Scene" or auto-triggered by pipeline.
- **Inputs:** `sceneId`, `generated_python_code` (from Gemini).
- **Outputs:** A URL to the rendered `.mp4` asset.
- **Business Rules:** Code MUST execute in an isolated Replit Sandbox. Execution timeout is exactly 300 seconds.
- **Error Handling:** If Sandbox crashes, capture `stderr` logs, feed them back to Gemini to "fix the code", and retry once.
- **Dependencies:** Replit API.

---

## 3. Complete User Journey
- **First-Time Experience:** User lands on `/`. Sees landing page. Clicks "Start Creating". Redirected to `/login`. Completes registration. Redirected to onboarding: a 3-step wizard to input their Replit API token and Google Cloud Project ID.
- **Happy Path:** User navigates to `/dashboard`. Clicks "Create Movie". Enters "A cyberpunk detective solves a crime". System generates script (loads for 5-10s with skeleton loaders). User approves script. System begins rendering scenes sequentially. User watches real-time execution logs from Replit. Final video appears.
- **Error Path (API Limit Reached):** During rendering, Replit API returns 429 Too Many Requests. System halts pipeline, updates UI to "Rate Limited", displays countdown timer for retry, and sends an in-app notification.
- **Recovery Path:** Once timer expires, system automatically resumes rendering from the exact failed `sceneId`.
- **Empty States:** On `/dashboard`, if 0 projects, display a stylized clapperboard icon with text "Your studio is empty. Create your first cinematic masterpiece." and a primary CTA button.

---

## 4. UI/UX Specification
- **Theme:** Dark mode only. Represents a premium cinematic editing software (like DaVinci Resolve or Premiere Pro).
- **Colors:**
  - Background: `#0A0A0A` (Deep Black)
  - Surface: `#1A1A1A` (Dark Gray)
  - Primary Accent: `#E50914` (Cinematic Red)
  - Text Primary: `#FFFFFF`
  - Text Secondary: `#A3A3A3`
- **Typography:** 'Inter' for UI elements, 'Courier Prime' for script/screenplay displays.
- **Layout (Dashboard):** 
  - Left Sidebar (Fixed, 250px): Navigation links (Projects, Agents, Settings), User Profile at bottom.
  - Topbar (Height 60px): Breadcrumbs, Global Search, Notification Bell.
  - Main Content Area: Responsive grid.
- **Components:**
  - **Primary Button:** Background `#E50914`, Text `#FFF`, Border Radius `4px`, padding `10px 24px`. Hover state: `#B80710` with a 200ms ease-in-out transition.
  - **Execution Log Viewer:** A terminal-like window. Background `#000`, Text `#00FF00` (Monospace). Auto-scrolls to bottom on new log line.
- **Skeleton Loaders:** Shimmering gray (`#2A2A2A` to `#3A3A3A`) placeholders matching the exact dimensions of the content they replace.

---

## 5. Navigation
- `/` - Public landing page.
- `/login` & `/register` - Authentication routes.
- `/dashboard` - Protected. Grid of user's projects.
- `/projects/[id]` - Protected. The main workspace for a specific movie.
- `/projects/[id]/script` - Protected. Screenplay editor view.
- `/projects/[id]/render` - Protected. Pipeline execution and Replit logs view.
- `/settings` - Protected. API Key management.
- **Redirects:** Unauthenticated users accessing `/dashboard` are strictly 302 redirected to `/login?next=/dashboard`.

---

## 6. Database Design (PostgreSQL)

**ER Diagram (Text):**
`User (1) --- (N) Project (1) --- (N) Scene (1) --- (N) ReplitJob`

**Tables & Fields:**
- **Table: `users`**
  - `id` (UUID, PK)
  - `email` (VARCHAR 255, Unique, Not Null)
  - `password_hash` (VARCHAR 255, Not Null)
  - `created_at` (TIMESTAMP, Default NOW())
- **Table: `projects`**
  - `id` (UUID, PK)
  - `user_id` (UUID, FK -> `users.id`, ON DELETE CASCADE)
  - `title` (VARCHAR 100, Not Null)
  - `concept` (TEXT, Not Null)
  - `status` (VARCHAR 20, Default 'DRAFT')
- **Table: `scenes`**
  - `id` (UUID, PK)
  - `project_id` (UUID, FK -> `projects.id`, ON DELETE CASCADE)
  - `scene_number` (INT, Not Null)
  - `content` (TEXT, Not Null)
  - `render_status` (VARCHAR 20, Default 'PENDING')
- **Table: `replit_jobs`**
  - `id` (UUID, PK)
  - `scene_id` (UUID, FK -> `scenes.id`, ON DELETE CASCADE)
  - `repl_id` (VARCHAR 255, Not Null)
  - `code_payload` (TEXT, Not Null)
  - `logs` (TEXT)
  - `asset_url` (VARCHAR 500)
  - `status` (VARCHAR 20, Default 'QUEUED')
- **Indexes:** Index on `projects.user_id`, `scenes.project_id`.

---

## 7. API Specification

**Endpoint: Create Project**
- **URL:** `/api/v1/projects`
- **Method:** `POST`
- **Headers:** `Authorization: Bearer <JWT>`
- **Request Schema:** `{"title": "string", "concept": "string"}`
- **Response Schema (201):** `{"id": "uuid", "title": "string", "status": "DRAFT"}`
- **Error Responses:** 400 Bad Request if fields missing. 401 Unauthorized if JWT invalid.

**Endpoint: Trigger Replit Render**
- **URL:** `/api/v1/scenes/{scene_id}/render`
- **Method:** `POST`
- **Headers:** `Authorization: Bearer <JWT>`
- **Request Schema:** `{}`
- **Response Schema (202):** `{"job_id": "uuid", "status": "QUEUED"}`
- **Expected Behavior:** Enqueues a Celery background task to interact with Replit. Returns 202 Accepted immediately.

---

## 8. Authentication
- **Mechanism:** JWT (JSON Web Tokens) stored in HTTP-only, secure cookies to prevent XSS.
- **Lifecycle:** Access token expires in 15 minutes. Refresh token expires in 7 days.
- **Password Reset:** Standard email flow with expiring magic link (JWT with 1-hour expiration).
- **Permissions:** A user can only read/write `Projects` and `Scenes` where `project.user_id == jwt.user_id`. Verified via middleware on every request.

---

## 9. AI Components
- **Model:** Google `gemini-1.5-pro` via Vertex AI.
- **Prompt Template (Screenwriter):**
  `"You are an expert Hollywood screenwriter. Given the concept: {concept}, break it down into exactly {num_scenes} scenes. Return strictly valid JSON matching this schema: {schema}."`
- **Context Injection:** When generating python rendering code, the prompt is injected with the Replit environment constraints (e.g., "Assume FFmpeg is installed. Write the output to output.mp4").
- **Safety Mechanisms:** Strict JSON parsing with Pydantic. If parsing fails, trigger auto-correction prompt loop up to 3 times.

---

## 10. Backend Architecture (FastAPI)
- **Folder Structure:**
  - `/api/routes`: Endpoints grouped by resource (`projects.py`, `scenes.py`).
  - `/core`: `config.py`, `security.py`.
  - `/db`: `models.py`, `session.py`.
  - `/services`: Business logic (`llm_service.py`, `replit_service.py`).
  - `/worker`: Celery tasks (`tasks.py`).
- **Dependency Injection:** Database sessions and external API clients are injected into routes using FastAPI's `Depends()`.
- **Background Jobs:** Celery + Redis broker. Used strictly for polling Replit sandbox status and executing long LLM chains to prevent blocking HTTP workers.

---

## 11. Frontend Architecture (Next.js 14 App Router)
- **Folder Structure:**
  - `/app`: Page routes and layouts.
  - `/components/ui`: Reusable dumb components (buttons, inputs) using shadcn/ui.
  - `/components/features`: Complex components tied to business logic (e.g., `SceneEditor.tsx`).
  - `/lib`: Utility functions, API clients (Axios/fetch wrappers).
  - `/store`: Zustand state management for UI state (sidebar toggles, active project).
- **State Management:** React Query for server state (caching API responses, invalidating on mutations). Zustand for local client state.
- **Routing:** App router with `loading.tsx` and `error.tsx` boundaries for every nested route.

---

## 12. Security
- **Secrets Management:** Environment variables strictly. Replit tokens and Google Cloud credentials NEVER exposed to the frontend.
- **CORS:** Backend configured to accept requests ONLY from the frontend origin.
- **CSRF:** Avoided by using SameSite=Strict HTTP-only cookies for auth.
- **SQL Injection:** Prevented by using SQLAlchemy ORM; no raw SQL queries allowed.
- **Input Validation:** All incoming API payloads validated strictly using Pydantic schemas.

---

## 13. Performance
- **API Optimization:** Pagination applied to `GET /api/projects` (limit 20 per page).
- **Frontend Optimization:** Next.js Image component used for all assets. Code splitting via App Router. 
- **Database Optimization:** Foreign keys indexed.
- **UX Performance:** Optimistic UI updates. When a user changes a scene's text, update UI instantly, save to backend in background.

---

## 14. DevOps
- **Deployment:** 
  - Frontend: Vercel or Google Cloud Run via Docker.
  - Backend: Google Cloud Run.
  - Database: Google Cloud SQL (PostgreSQL).
- **CI/CD:** GitHub Actions. On `push` to `main`: run lint, run pytest, build Docker images, deploy to Google Cloud Run.
- **Logging:** Python `logging` module configured to output JSON logs. Ingested by Google Cloud Logging.

---

## 15. Testing
- **Unit Tests:** `pytest` for backend. Test all services independently (mocking Gemini and Replit APIs). Minimum 80% coverage.
- **Integration Tests:** Test the database repository layer with a test Postgres container.
- **E2E Tests:** Cypress or Playwright to test the happy path user journey (Login -> Create Project -> View Script).

---

## 16. Error Handling
- **User-Facing:** "Something went wrong, but our team is on it." Toast notifications for transient errors.
- **Internal:** All exceptions caught in FastAPI global exception handler, logged with stack traces, and mapped to standard HTTP status codes.
- **Fallback:** If Gemini 1.5 Pro is down, fallback to `gemini-1.5-flash`.

---

## 17. File Structure (Root Repository)
```
/
├── .github/workflows/   # CI/CD pipelines
├── backend/             # FastAPI application
│   ├── app/             # Application source code
│   ├── tests/           # Pytest suite
│   ├── Dockerfile       # Backend container definition
│   └── requirements.txt # Python dependencies
├── frontend/            # Next.js application
│   ├── app/             # App router pages
│   ├── components/      # React components
│   ├── Dockerfile       # Frontend container definition
│   └── package.json     # Node dependencies
├── docs/                # Project documentation
│   └── SOFTWARE_BLUEPRINT.md # THIS FILE
├── .env.example         # Template for environment variables
├── .gitignore
├── docker-compose.yml   # Local development orchestration
└── README.md            # Hackathon summary
```

---

## 18. Implementation Roadmap
- **Phase 1: Foundation (Days 1-2)**
  - Setup repo, Docker compose, database schemas, and FastAPI/Next.js scaffolding.
  - Implement Auth.
- **Phase 2: Core Orchestration (Days 3-5)**
  - Integrate Gemini API. Implement script generation workflow.
  - Build UI for viewing and editing the script.
- **Phase 3: Partner Integration (Days 6-8)**
  - Integrate Replit API. Create the background worker system to dispatch and monitor sandbox code execution.
  - Build real-time log viewer in frontend.
- **Phase 4: Polish & Hackathon Delivery (Days 9-10)**
  - Record 3-minute trailer. Finalize README. Fix edge case bugs.

---

## 19. Acceptance Criteria
**Feature: Replit Execution**
- **Must Happen:** Python code generated by Gemini MUST be executed in a remote Replit sandbox, returning `exit_code` and `logs`.
- **Must Never Happen:** The python code executes on the host backend server.
- **Success Condition:** The Replit API returns a 200 OK with the execution results within 5 minutes.

---

## 20. Developer Notes
- **Important Assumptions:** We assume the Replit API allows arbitrary python script execution via their deployments/sandbox API.
- **Technical Debt:** For the MVP, we are using long-polling on the frontend to check Replit job status. Post-hackathon, this must be refactored to WebSockets for true real-time performance.
- **Extension Points:** The `AgentOrchestrator` service is designed via an interface so we can easily swap out Gemini for another LLM if required in the future.
