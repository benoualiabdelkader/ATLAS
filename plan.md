# Replit AI Execution Plan (Micro-Step Orchestration)

> **CRITICAL INSTRUCTIONS FOR REPLIT AI:**
> You must execute this plan using the **Batch Execution System**. 
> You will execute ONE Phase per session. You must ONLY read the specific target section mentioned in the "Read Target" of each Leaf Node. 
> At the end of every Phase, you MUST HALT execution and wait for the user to authorize the next Phase.

---

## Phase 1: Project Initialization & Infrastructure
- **1.1.1 Leaf Node: Backend Repository Setup**
  - **Predecessors:** None
  - **Read Target:** `docs/blueprint/17_File_Structure_(Root_Repository).txt` -> Read ONLY the hierarchy under `backend/`.
  - **Action:** Create the `/backend` directory. Initialize Python environment, `requirements.txt` (FastAPI, SQLAlchemy, Pydantic, Celery), and `main.py` with a basic Hello World route.

- **1.1.2 Leaf Node: Frontend Repository Setup**
  - **Predecessors:** None
  - **Read Target:** `docs/blueprint/17_File_Structure_(Root_Repository).txt` -> Read ONLY the hierarchy under `frontend/`.
  - **Action:** Create the `/frontend` directory. Initialize Next.js 14 App Router with Tailwind CSS and TypeScript. Remove the default Next.js boilerplate.

- **1.1.3 Leaf Node: Environment Configuration**
  - **Predecessors:** 1.1.1, 1.1.2
  - **Read Target:** `docs/blueprint/14_DevOps.txt` -> Read ONLY the "Deployment" section.
  - **Action:** Set up `.env` files for both frontend and backend using the provided `.env.example`.

**🛑 HALT EXECUTION:** Output exactly `[PHASE 1 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 2.]` Do not proceed.

---

## Phase 2: Database Architecture & Models
- **1.2.1 Leaf Node: Database Connection Engine**
  - **Predecessors:** Phase 1
  - **Read Target:** `docs/blueprint/10_Backend_Architecture_(FastAPI).txt` -> Read ONLY the "Folder Structure" for `/db`.
  - **Action:** Create `backend/db/session.py` to establish the SQLAlchemy engine.

- **1.2.2 Leaf Node: Define User Model**
  - **Predecessors:** 1.2.1
  - **Read Target:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt` -> Read ONLY the bullet points under "Table: users".
  - **Action:** Create `backend/db/models.py`. Implement the `User` class.

- **1.2.3 Leaf Node: Define Project and Scene Models**
  - **Predecessors:** 1.2.2
  - **Read Target:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt` -> Read ONLY the bullet points under "Table: projects" and "Table: scenes".
  - **Action:** Append `Project` and `Scene` classes to `backend/db/models.py`.

- **1.2.4 Leaf Node: Define ReplitJob Model**
  - **Predecessors:** 1.2.3
  - **Read Target:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt` -> Read ONLY the bullet points under "Table: replit_jobs".
  - **Action:** Append the `ReplitJob` class.

**🛑 HALT EXECUTION:** Output exactly `[PHASE 2 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 3.]` Do not proceed.

---

## Phase 3: Security & Authentication
- **1.3.1 Leaf Node: Password Hashing Utils**
  - **Predecessors:** Phase 2
  - **Read Target:** `docs/blueprint/12_Security.txt` -> Read ONLY the "Secrets Management" section.
  - **Action:** Implement bcrypt hashing functions in `backend/core/security.py`.

- **1.3.2 Leaf Node: JWT Generation**
  - **Predecessors:** 1.3.1
  - **Read Target:** `docs/blueprint/8_Authentication.txt` -> Read ONLY the "Lifecycle" section.
  - **Action:** Implement JWT creation functions in `backend/core/security.py`.

- **1.3.3 Leaf Node: Auth Endpoints**
  - **Predecessors:** 1.3.2
  - **Read Target:** `docs/blueprint/8_Authentication.txt` -> Read ONLY the "Mechanism" section.
  - **Action:** Build `/api/v1/auth/register` and `/api/v1/auth/login`. Set the JWT inside a secure `HttpOnly` cookie.

**🛑 HALT EXECUTION:** Output exactly `[PHASE 3 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 4.]` Do not proceed.

---

## Phase 4: Core Business API
- **1.4.1 Leaf Node: Auth Dependency Injection**
  - **Predecessors:** Phase 3
  - **Read Target:** `docs/blueprint/8_Authentication.txt` -> Read ONLY the "Permissions" section.
  - **Action:** Build the `get_current_user` FastAPI dependency.

- **1.4.2 Leaf Node: Project API Endpoints**
  - **Predecessors:** 1.4.1
  - **Read Target:** `docs/blueprint/7_API_Specification.txt` -> Read ONLY the "Endpoint: Create Project" section.
  - **Action:** Build POST and GET `/api/v1/projects`. 

- **1.4.3 Leaf Node: Scene API Endpoints**
  - **Predecessors:** 1.4.2
  - **Read Target:** `docs/blueprint/2_Functional_Requirements.txt` -> Read ONLY "Feature 2" Inputs/Outputs.
  - **Action:** Build POST and GET `/api/v1/scenes`.

**🛑 HALT EXECUTION:** Output exactly `[PHASE 4 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 5.]` Do not proceed.

---

## Phase 5: AI Orchestration & External Integrations
- **1.5.1 Leaf Node: Gemini Service Orchestrator**
  - **Predecessors:** Phase 4
  - **Read Target:** `docs/blueprint/9_AI_Components.txt` -> Read ONLY the "Prompt Template (Screenwriter)" section.
  - **Action:** Create `backend/services/llm_service.py`. Implement `AgentOrchestrator` to call Gemini 1.5 Pro.
  
- **1.5.2 Leaf Node: Background Worker Setup**
  - **Predecessors:** 1.5.1
  - **Read Target:** `docs/blueprint/10_Backend_Architecture_(FastAPI).txt` -> Read ONLY the "Background Jobs" section.
  - **Action:** Initialize Celery worker in `backend/worker/tasks.py`.

- **1.5.3 Leaf Node: Replit Sandbox Task**
  - **Predecessors:** 1.5.2
  - **Read Target:** `docs/blueprint/2_Functional_Requirements.txt` -> Read ONLY "Feature 3: Replit Sandbox Execution".
  - **Action:** Build Celery task to send python code to Replit API and poll for logs.

**🛑 HALT EXECUTION:** Output exactly `[PHASE 5 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 6.]` Do not proceed.

---

## Phase 6: Frontend Foundation & UI System
- **1.6.1 Leaf Node: Theme Configuration**
  - **Predecessors:** Phase 5
  - **Read Target:** `docs/blueprint/4_UI_UX_Specification.txt` -> Read ONLY the "Colors" and "Typography" sections.
  - **Action:** Configure `tailwind.config.ts`.

- **1.6.2 Leaf Node: Atomic UI Components**
  - **Predecessors:** 1.6.1
  - **Read Target:** `docs/blueprint/4_UI_UX_Specification.txt` -> Read ONLY the "Components" section.
  - **Action:** Build `PrimaryButton` and `ExecutionLogViewer`.

**🛑 HALT EXECUTION:** Output exactly `[PHASE 6 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 7.]` Do not proceed.

---

## Phase 7: Frontend Pages & State Management
- **1.7.1 Leaf Node: Global State and Axios Config**
  - **Predecessors:** Phase 6
  - **Read Target:** `docs/blueprint/11_Frontend_Architecture_(Nextjs_14_App_Router).txt` -> Read ONLY the "State Management" section.
  - **Action:** Configure Axios (`withCredentials: true`). Set up Zustand store.

- **1.7.2 Leaf Node: Auth Pages**
  - **Predecessors:** 1.7.1
  - **Read Target:** `docs/blueprint/5_Navigation.txt` -> Read ONLY the "/login & /register" rules.
  - **Action:** Build `/login` and `/register`.

- **1.7.3 Leaf Node: Dashboard & Workspace**
  - **Predecessors:** 1.7.2
  - **Read Target:** `docs/blueprint/4_UI_UX_Specification.txt` -> Read ONLY the "Layout (Dashboard)" section.
  - **Action:** Build `/dashboard` and `/projects/[id]`. 

**🛑 HALT EXECUTION:** Output exactly `[PHASE 7 COMPLETE. I AM CLEARING MY CONTEXT. WAITING FOR USER AUTHORIZATION TO BEGIN PHASE 8.]` Do not proceed.

---

## Phase 8: Polish & Validation
- **1.8.1 Leaf Node: Error Boundaries**
  - **Predecessors:** Phase 7
  - **Read Target:** `docs/blueprint/16_Error_Handling.txt` -> Read ONLY the "User-Facing" section.
  - **Action:** Implement global error boundaries and toast notifications.

- **1.8.2 Leaf Node: Final Acceptance**
  - **Predecessors:** 1.8.1
  - **Read Target:** `docs/blueprint/19_Acceptance_Criteria.txt` -> Read ALL.
  - **Action:** Verify all criteria. Ensure the end-to-end sandbox execution works.

**🛑 HALT EXECUTION:** Output exactly `[PROJECT COMPLETE. ALL 8 PHASES EXECUTED SUCCESSFULLY.]`
