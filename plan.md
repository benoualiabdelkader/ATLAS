# ATLAS - Work Breakdown Structure (WBS) & AI Execution Plan

> **CRITICAL INSTRUCTIONS FOR REPLIT AI:**
> This plan is structured as a formal Work Breakdown Structure (WBS). 
> You must execute the **Leaf Node** tasks sequentially. Do not begin any task until its defined **Predecessors** are completed. 
> Your progress must follow the 0:100 earning rule (a task is only considered complete when it is 100% functional).

---

## 1. Project Root: ATLAS Cinematic Agent

### 1.1 Summary Task: Project Initialization & Infrastructure
**Prerequisite Reading:** `docs/blueprint/17_File_Structure_(Root_Repository).txt`, `docs/blueprint/14_DevOps.txt`

- **1.1.1 Leaf Node: Backend Repository Setup**
  - **Predecessors:** None
  - **Effort Estimate:** Low
  - **Action:** Create `/backend`. Initialize Python environment, `requirements.txt` (FastAPI, SQLAlchemy, Pydantic, Celery), and `main.py`.
  - **Definition of Done (DoD):** FastAPI runs and returns 200 OK on `/`.

- **1.1.2 Leaf Node: Frontend Repository Setup**
  - **Predecessors:** None
  - **Effort Estimate:** Low
  - **Action:** Create `/frontend`. Initialize Next.js 14 App Router with Tailwind CSS and TypeScript.
  - **DoD:** Next.js dev server starts without errors.

- **1.1.3 Leaf Node: Environment Configuration**
  - **Predecessors:** 1.1.1, 1.1.2
  - **Effort Estimate:** Low
  - **Action:** Set up `.env` files for both frontend and backend based on `.env.example`.

### 1.2 Summary Task: Database Architecture & Models
**Prerequisite Reading:** `docs/blueprint/6_Database_Design_(PostgreSQL).txt`

- **1.2.1 Leaf Node: Database Connection**
  - **Predecessors:** 1.1.1
  - **Effort Estimate:** Low
  - **Action:** Create `backend/db/session.py` to establish SQLAlchemy engine.
  - **DoD:** Database connects successfully on app startup.

- **1.2.2 Leaf Node: Define ORM Models**
  - **Predecessors:** 1.2.1
  - **Effort Estimate:** Medium
  - **Action:** Create models for `User`, `Project`, `Scene`, and `ReplitJob` with UUIDs and exact Foreign Key constraints.
  - **DoD:** Models validate and tables are successfully generated in PostgreSQL.

### 1.3 Summary Task: Security & Authentication
**Prerequisite Reading:** `docs/blueprint/8_Authentication.txt`, `docs/blueprint/12_Security.txt`

- **1.3.1 Leaf Node: Password Hashing & JWT Utils**
  - **Predecessors:** 1.1.1
  - **Effort Estimate:** Low
  - **Action:** Implement bcrypt hashing and JWT generation in `backend/core/security.py`.

- **1.3.2 Leaf Node: Auth Endpoints**
  - **Predecessors:** 1.2.2, 1.3.1
  - **Effort Estimate:** Medium
  - **Action:** Build `/api/v1/auth/register` and `/api/v1/auth/login`. 
  - **DoD:** Login endpoint sets an `HttpOnly` secure cookie containing the JWT.

### 1.4 Summary Task: Core Business API
**Prerequisite Reading:** `docs/blueprint/7_API_Specification.txt`, `docs/blueprint/2_Functional_Requirements.txt`

- **1.4.1 Leaf Node: Auth Middleware**
  - **Predecessors:** 1.3.2
  - **Effort Estimate:** Low
  - **Action:** Build `get_current_user` dependency to protect routes.

- **1.4.2 Leaf Node: Project CRUD Endpoints**
  - **Predecessors:** 1.4.1
  - **Effort Estimate:** Medium
  - **Action:** Build `/api/v1/projects`. Enforce user isolation (return only user's projects).

- **1.4.3 Leaf Node: Scene CRUD Endpoints**
  - **Predecessors:** 1.4.2
  - **Effort Estimate:** Medium
  - **Action:** Build `/api/v1/scenes` linked to specific projects.

### 1.5 Summary Task: AI Orchestration & External Integrations
**Prerequisite Reading:** `docs/blueprint/9_AI_Components.txt`

- **1.5.1 Leaf Node: Gemini Service Orchestrator**
  - **Predecessors:** 1.4.3
  - **Effort Estimate:** High
  - **Action:** Implement `AgentOrchestrator` to interface with Gemini 1.5 Pro. Handle strict JSON prompt injection for scene breakdowns.
  
- **1.5.2 Leaf Node: Background Worker (Celery)**
  - **Predecessors:** 1.1.1
  - **Effort Estimate:** Medium
  - **Action:** Implement Celery worker for long-running tasks.

- **1.5.3 Leaf Node: Replit Sandbox Execution Task**
  - **Predecessors:** 1.5.2
  - **Effort Estimate:** High
  - **Action:** Build Celery task that takes Python code, sends it to Replit API, and polls for execution logs. Link to `/api/v1/scenes/{id}/render`.

### 1.6 Summary Task: Frontend Foundation & UI System
**Prerequisite Reading:** `docs/blueprint/11_Frontend_Architecture_(Nextjs_14_App_Router).txt`, `docs/blueprint/4_UI_UX_Specification.txt`

- **1.6.1 Leaf Node: Theme & Styling Setup**
  - **Predecessors:** 1.1.2
  - **Effort Estimate:** Low
  - **Action:** Configure Tailwind colors (`#0A0A0A`, `#1A1A1A`, `#E50914`). Install UI libraries (`lucide-react`).

- **1.6.2 Leaf Node: Reusable UI Components**
  - **Predecessors:** 1.6.1
  - **Effort Estimate:** Medium
  - **Action:** Build `PrimaryButton`, `SkeletonLoader`, and `ExecutionLogViewer`.

### 1.7 Summary Task: Frontend Pages & State Management
**Prerequisite Reading:** `docs/blueprint/5_Navigation.txt`, `docs/blueprint/3_Complete_User_Journey.txt`

- **1.7.1 Leaf Node: Auth Pages & Global State**
  - **Predecessors:** 1.6.2, 1.3.2
  - **Effort Estimate:** Medium
  - **Action:** Build `/login` and `/register`. Implement Axios interceptors and Zustand state.

- **1.7.2 Leaf Node: Dashboard Page**
  - **Predecessors:** 1.7.1, 1.4.2
  - **Effort Estimate:** Medium
  - **Action:** Build `/dashboard`. Fetch user projects with React Query. Implement empty states.

- **1.7.3 Leaf Node: Project Workspace Page**
  - **Predecessors:** 1.7.2, 1.4.3, 1.5.3
  - **Effort Estimate:** High
  - **Action:** Build `/projects/[id]`. Split pane UI for Script/Scenes and ExecutionLogViewer. Wire up the "Generate Script" and "Render" buttons to the AI backend.

### 1.8 Summary Task: Polish, Error Handling & Validation
**Prerequisite Reading:** `docs/blueprint/16_Error_Handling.txt`, `docs/blueprint/19_Acceptance_Criteria.txt`

- **1.8.1 Leaf Node: Global Error Boundaries**
  - **Predecessors:** 1.7.3
  - **Effort Estimate:** Low
  - **Action:** Implement error toasts (`react-hot-toast`) and Next.js `error.tsx` boundaries.

- **1.8.2 Leaf Node: E2E Acceptance Verification**
  - **Predecessors:** All previous leaf nodes.
  - **Effort Estimate:** High
  - **Action:** Perform end-to-end testing against the Acceptance Criteria. Ensure Replit Sandbox executes successfully and returns video URLs.
  - **DoD:** 100% Earned Value across the entire WBS.
