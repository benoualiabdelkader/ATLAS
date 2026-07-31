# ATLAS - Work Breakdown Structure (WBS) & AI Execution Plan

> **CRITICAL INSTRUCTIONS FOR REPLIT AI:**
> This plan is structured as a formal Work Breakdown Structure (WBS). 
> You must execute the **Leaf Node** tasks sequentially. Do not begin any task until its defined **Predecessors** are completed. 
> Your progress must follow the 0:100 earning rule (a task is only considered complete when it is 100% functional).
> **CRITICAL CONTEXT RULE:** Do NOT read entire files. You must ONLY read the specific target section mentioned in the "Read Target" of each Leaf Node. This prevents context dilution.

---

## 1. Project Root: ATLAS Cinematic Agent

### 1.1 Summary Task: Project Initialization & Infrastructure
- **1.1.1 Leaf Node: Backend Repository Setup**
  - **Predecessors:** None
  - **Read Target:** `docs/blueprint/17_File_Structure_(Root_Repository).txt` -> Read ONLY the hierarchy under `backend/`.
  - **Action:** Create the `/backend` directory. Initialize Python environment, `requirements.txt` (FastAPI, SQLAlchemy, Pydantic, Celery), and `main.py` with a basic Hello World route.
  - **Definition of Done (DoD):** FastAPI runs and returns 200 OK on `/`.

- **1.1.2 Leaf Node: Frontend Repository Setup**
  - **Predecessors:** None
  - **Read Target:** `docs/blueprint/17_File_Structure_(Root_Repository).txt` -> Read ONLY the hierarchy under `frontend/`.
  - **Action:** Create the `/frontend` directory. Initialize Next.js 14 App Router with Tailwind CSS and TypeScript. Remove the default Next.js boilerplate.
  - **DoD:** Next.js dev server starts without errors.

- **1.1.3 Leaf Node: Environment Configuration**
  - **Predecessors:** 1.1.1, 1.1.2
  - **Read Target:** `docs/blueprint/14_DevOps.txt` -> Read ONLY the "Deployment" section.
  - **Action:** Set up `.env` files for both frontend and backend using the provided `.env.example`.

### 1.2 Summary Task: Database Architecture & Models
- **1.2.1 Leaf Node: Database Connection Engine**
  - **Predecessors:** 1.1.1
  - **Read Target:** `docs/blueprint/10_Backend_Architecture_(FastAPI).txt` -> Read ONLY the "Folder Structure" for `/db`.
  - **Action:** Create `backend/db/session.py` to establish the SQLAlchemy engine and `SessionLocal` using the `DATABASE_URL` environment variable.

- **1.2.2 Leaf Node: Define User Model**
  - **Predecessors:** 1.2.1
  - **Read Target:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt` -> Read ONLY the bullet points under "Table: users".
  - **Action:** Create `backend/db/models.py`. Implement the `User` class mapping exactly to the UUID, email, and password_hash fields specified.

- **1.2.3 Leaf Node: Define Project and Scene Models**
  - **Predecessors:** 1.2.2
  - **Read Target:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt` -> Read ONLY the bullet points under "Table: projects" and "Table: scenes".
  - **Action:** Append `Project` and `Scene` classes to `backend/db/models.py`. Implement the `ON DELETE CASCADE` foreign key relationships exactly as specified.

- **1.2.4 Leaf Node: Define ReplitJob Model**
  - **Predecessors:** 1.2.3
  - **Read Target:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt` -> Read ONLY the bullet points under "Table: replit_jobs".
  - **Action:** Append the `ReplitJob` class. Ensure all models validate and tables can be generated.

### 1.3 Summary Task: Security & Authentication
- **1.3.1 Leaf Node: Password Hashing Utils**
  - **Predecessors:** 1.1.1
  - **Read Target:** `docs/blueprint/12_Security.txt` -> Read ONLY the "Secrets Management" section.
  - **Action:** Implement bcrypt hashing functions in `backend/core/security.py`.

- **1.3.2 Leaf Node: JWT Generation**
  - **Predecessors:** 1.3.1
  - **Read Target:** `docs/blueprint/8_Authentication.txt` -> Read ONLY the "Lifecycle" section.
  - **Action:** Implement JWT creation functions (15-min access token) in `backend/core/security.py`.

- **1.3.3 Leaf Node: Auth Endpoints**
  - **Predecessors:** 1.2.2, 1.3.2
  - **Read Target:** `docs/blueprint/8_Authentication.txt` -> Read ONLY the "Mechanism" section regarding HttpOnly cookies.
  - **Action:** Build `/api/v1/auth/register` and `/api/v1/auth/login`. 
  - **DoD:** Login endpoint sets an `HttpOnly` secure cookie containing the JWT. Do not return the JWT in the JSON body.

### 1.4 Summary Task: Core Business API
- **1.4.1 Leaf Node: Auth Dependency Injection**
  - **Predecessors:** 1.3.3
  - **Read Target:** `docs/blueprint/8_Authentication.txt` -> Read ONLY the "Permissions" section.
  - **Action:** Build the `get_current_user` FastAPI dependency to read the HttpOnly cookie and validate the JWT.

- **1.4.2 Leaf Node: Project API Endpoints**
  - **Predecessors:** 1.4.1
  - **Read Target:** `docs/blueprint/7_API_Specification.txt` -> Read ONLY the "Endpoint: Create Project" section.
  - **Action:** Build POST and GET `/api/v1/projects`. Enforce user isolation so users only retrieve their own projects.

- **1.4.3 Leaf Node: Scene API Endpoints**
  - **Predecessors:** 1.4.2
  - **Read Target:** `docs/blueprint/2_Functional_Requirements.txt` -> Read ONLY "Feature 2" Inputs/Outputs.
  - **Action:** Build POST and GET `/api/v1/scenes` linked to specific projects.

### 1.5 Summary Task: AI Orchestration & External Integrations
- **1.5.1 Leaf Node: Gemini Service Orchestrator**
  - **Predecessors:** 1.4.3
  - **Read Target:** `docs/blueprint/9_AI_Components.txt` -> Read ONLY the "Prompt Template (Screenwriter)" section.
  - **Action:** Create `backend/services/llm_service.py`. Implement `AgentOrchestrator` to call Gemini 1.5 Pro. Use the exact prompt template specified to enforce JSON structure.
  
- **1.5.2 Leaf Node: Background Worker Setup**
  - **Predecessors:** 1.1.1
  - **Read Target:** `docs/blueprint/10_Backend_Architecture_(FastAPI).txt` -> Read ONLY the "Background Jobs" section.
  - **Action:** Initialize Celery worker in `backend/worker/tasks.py`.

- **1.5.3 Leaf Node: Replit Sandbox Task**
  - **Predecessors:** 1.5.2
  - **Read Target:** `docs/blueprint/2_Functional_Requirements.txt` -> Read ONLY "Feature 3: Replit Sandbox Execution".
  - **Action:** Build a Celery task that takes Python code, sends it to the Replit API, and polls for logs. Build the POST `/api/v1/scenes/{id}/render` endpoint to trigger it.

### 1.6 Summary Task: Frontend Foundation & UI System
- **1.6.1 Leaf Node: Theme Configuration**
  - **Predecessors:** 1.1.2
  - **Read Target:** `docs/blueprint/4_UI_UX_Specification.txt` -> Read ONLY the "Colors" and "Typography" sections.
  - **Action:** Configure `tailwind.config.ts` with the exact hex codes provided (`#0A0A0A`, `#1A1A1A`, `#E50914`).

- **1.6.2 Leaf Node: Atomic UI Components**
  - **Predecessors:** 1.6.1
  - **Read Target:** `docs/blueprint/4_UI_UX_Specification.txt` -> Read ONLY the "Components" section.
  - **Action:** Build `PrimaryButton` (with the `#B80710` hover state) and `ExecutionLogViewer`.

### 1.7 Summary Task: Frontend Pages & State Management
- **1.7.1 Leaf Node: Global State and Axios Config**
  - **Predecessors:** 1.6.2
  - **Read Target:** `docs/blueprint/11_Frontend_Architecture_(Nextjs_14_App_Router).txt` -> Read ONLY the "State Management" section.
  - **Action:** Configure Axios to send credentials (`withCredentials: true`). Set up Zustand store.

- **1.7.2 Leaf Node: Auth Pages**
  - **Predecessors:** 1.7.1, 1.3.3
  - **Read Target:** `docs/blueprint/5_Navigation.txt` -> Read ONLY the "/login & /register" redirect rules.
  - **Action:** Build `/login` and `/register`. On successful login, route to `/dashboard`.

- **1.7.3 Leaf Node: Dashboard & Workspace**
  - **Predecessors:** 1.7.2, 1.4.2
  - **Read Target:** `docs/blueprint/4_UI_UX_Specification.txt` -> Read ONLY the "Layout (Dashboard)" section.
  - **Action:** Build `/dashboard` (project grid) and `/projects/[id]` (split pane for Script and Logs). Wire up the "Generate Script" button to the API.

### 1.8 Summary Task: Polish & Validation
- **1.8.1 Leaf Node: Error Boundaries**
  - **Predecessors:** 1.7.3
  - **Read Target:** `docs/blueprint/16_Error_Handling.txt` -> Read ONLY the "User-Facing" section.
  - **Action:** Implement global error boundaries and toast notifications.

- **1.8.2 Leaf Node: Final Acceptance**
  - **Predecessors:** All previous leaf nodes.
  - **Read Target:** `docs/blueprint/19_Acceptance_Criteria.txt` -> Read ALL.
  - **Action:** Verify all criteria. Ensure the end-to-end sandbox execution works and logs are streamed back correctly.
