# ATLAS - Agentic Cinematic Production 🎬

> A fully autonomous AI agent designed for cinematic production, powered by **Gemini Enterprise Agent Platform** and **Replit** for the *Agentic Cinema: The Blockbuster Hackathon*.

![ATLAS Demo Placeholder](screenshots/demo-placeholder.png)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Problem Statement
Creating high-quality cinematic content requires executing complex rendering scripts and data processing pipelines. Setting up these execution environments manually creates friction for enterprise media studios scaling their AI pipelines.

## 🚀 Solution Overview
**ATLAS** is an autonomous AI agent that orchestrates the entire cinematic production pipeline. Built exclusively on **Google Cloud Agent Builder** and powered by **Gemini**, ATLAS acts as the autonomous "Studio Head". It orchestrates story generation and scene breakdowns, and securely executes video rendering scripts in isolated, dynamic sandboxes using our partner platform, **Replit**.

## ✨ Key Features & Hackathon Criteria
- **Powered by Gemini:** Uses Google's Gemini models for deterministic, multi-step agent reasoning to solve enterprise friction in media pipelines.
- **Replit Partner Integration:** The Gemini agent dynamically provisions Replit deployments and sandboxes to execute video processing and rendering scripts, demonstrating secure and scalable code execution.
- **Script to Screen Pipeline:** Automated breakdown of scripts into discrete scenes using Google Cloud Agent Builder.
- **Open Source:** Fully public repository with an MIT open-source license.

## 🏗️ Architecture

![Architecture Placeholder](screenshots/architecture-placeholder.png)
*For a detailed breakdown, please see our [Architecture Documentation](docs/architecture.md).*

## 💻 Technology Stack
- **AI/Agent Framework:** Gemini Enterprise Agent Platform (Google Cloud)
- **Partner Integration:** Replit (Dynamic sandbox execution environment)
- **Backend:** Python (FastAPI)
- **Frontend:** React / Next.js
- **Deployment:** Google Cloud Run

## 🛠️ Installation & Setup

### Prerequisites
- Node.js >= 18.x
- Python >= 3.10
- Google Cloud CLI (`gcloud`)
- Replit Account (with API access)

### 1. Clone the Repository
```bash
git clone https://github.com/benoualiabdelkader/ATLAS.git
cd ATLAS
```

### 2. Environment Variables Setup
Copy the example environment file and configure it with your API keys:
```bash
cp .env.example .env
```
*Note: Make sure never to commit your `.env` file containing real API keys.*

### 3. Authenticate with Google Cloud
Ensure your local environment is authenticated to call Google Cloud APIs:
```bash
gcloud auth application-default login
gcloud config set project YOUR_GOOGLE_CLOUD_PROJECT
```

### 4. Running Locally
Using Docker Compose:
```bash
docker-compose up --build
```
Alternatively, follow the detailed setup in [docs/setup.md](docs/setup.md).

## 🎥 The 3-Minute Trailer (Demo)
*Watch our 3-Minute Demo Video on YouTube: [Link Placeholder]*

## 🤝 Contribution Guidelines
We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details. By participating in this project, you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md).

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. This license is critical for our Devpost submission.
