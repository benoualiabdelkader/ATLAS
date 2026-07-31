# Replit AI Execution Plan (Step-by-Step Orchestration)

> **INSTRUCTIONS FOR REPLIT AI:**
> Do NOT attempt to build the entire project at once. Building everything in one shot will lead to context loss and errors. 
> Follow this plan **step-by-step**. Before starting a phase, read the specifically referenced `.txt` blueprint files for that phase located in the `docs/blueprint/` directory.
> Only move to the next phase after completing and verifying the current one perfectly.

---

## Phase 1: Project Initialization & Infrastructure
- **Files to Read:** 
  - `docs/blueprint/17_File_Structure_(Root_Repository).txt`
  - `docs/blueprint/14_DevOps.txt`
- **Action:** Create the foundational folder structure for `/backend` and `/frontend`. Set up `package.json`, `requirements.txt`, `.env` loaders, and basic container configurations.

## Phase 2: Database & Models
- **Files to Read:** 
  - `docs/blueprint/6_Database_Design_(PostgreSQL).txt`
- **Action:** Inside the backend, set up your ORM (e.g., SQLAlchemy/Prisma). Define the `users`, `projects`, `scenes`, and `replit_jobs` tables with exact relationships, constraints, and UUID primary keys.

## Phase 3: Backend Core & Authentication
- **Files to Read:** 
  - `docs/blueprint/10_Backend_Architecture_(FastAPI).txt`
  - `docs/blueprint/8_Authentication.txt`
  - `docs/blueprint/12_Security.txt`
- **Action:** Build the FastAPI core, JWT HTTP-only cookie middleware, password hashing, and user registration/login endpoints. Ensure CORS and security headers are configured.

## Phase 4: Backend Business Logic & API
- **Files to Read:** 
  - `docs/blueprint/7_API_Specification.txt`
  - `docs/blueprint/2_Functional_Requirements.txt`
- **Action:** Implement the endpoints for CRUD operations on Projects and Scenes. Do not integrate the AI generation logic yet; focus strictly on data persistence and REST API contracts.

## Phase 5: AI Orchestration & Integrations
- **Files to Read:** 
  - `docs/blueprint/9_AI_Components.txt`
- **Action:** Implement the `AgentOrchestrator` service that calls the Google Gemini API (using the exact prompts specified). Implement the Celery/background worker logic that handles spawning dynamic sandboxes via the Replit API.

## Phase 6: Frontend Foundation & UI System
- **Files to Read:** 
  - `docs/blueprint/11_Frontend_Architecture_(Nextjs_14_App_Router).txt`
  - `docs/blueprint/4_UI_UX_Specification.txt`
- **Action:** Initialize Next.js App router. Set up Tailwind CSS with the exact colors (`#0A0A0A`, `#E50914`, etc.). Build the reusable UI components (Primary Button, Skeleton Loaders, Execution Log Viewer).

## Phase 7: Frontend Pages & Navigation
- **Files to Read:** 
  - `docs/blueprint/5_Navigation.txt`
  - `docs/blueprint/3_Complete_User_Journey.txt`
- **Action:** Build the actual pages (`/login`, `/dashboard`, `/projects/[id]`). Implement Zustand state management and React Query/Axios hooks to seamlessly connect to the FastAPI backend. Connect the UI to the authentication flow.

## Phase 8: Refinement, Error Handling & Testing
- **Files to Read:** 
  - `docs/blueprint/16_Error_Handling.txt`
  - `docs/blueprint/13_Performance.txt`
  - `docs/blueprint/19_Acceptance_Criteria.txt`
- **Action:** Review the entire codebase. Add optimistic UI updates, loading states, fallback error boundaries, and toast notifications. Ensure the "Replit Sandbox Execution" acceptance criteria are fully met.
