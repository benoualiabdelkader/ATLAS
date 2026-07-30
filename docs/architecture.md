# ATLAS Architecture: Google Cloud & Replit

The ATLAS system is designed as a distributed, multi-agent platform tailored for cinematic production, strictly adhering to the Agentic Cinema Hackathon requirements.

## High-Level Components

1. **Frontend Dashboard:** A React interface for Studio Executives to submit scripts and monitor production progress.
2. **Backend API (Google Cloud Run):** A Python server that acts as the secure boundary between the frontend and our agent networks.
3. **Gemini Orchestrator (Google Cloud Agent Builder):** The core autonomous engine. It interprets user input and coordinates the cinematic vision.
4. **Partner Integration (Replit):** The execution layer. When the Gemini agent generates python scripts to process video data or render scenes, it dynamically spins up a Replit Sandbox to execute the code securely without risking the main infrastructure.

## Data Flow
1. User submits a raw script concept via the Frontend.
2. The Backend passes the prompt to the Gemini Orchestrator.
3. The Gemini agent breaks down the script and writes the rendering code.
4. The agent pushes the code to a dynamically provisioned Replit Sandbox via the Replit API.
5. Replit executes the cinematic pipeline scripts and returns the output to Google Cloud.
6. The final orchestrated production plan and video assets are served back to the user.
