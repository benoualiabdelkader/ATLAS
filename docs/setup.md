# Setup and Installation Guide

## Local Development

### 1. Prerequisites
- Install Google Cloud CLI.
- Ensure you have a Replit account and API Token.

### 2. Google Cloud Setup
You must enable the Gemini Enterprise Agent Platform APIs in your Google Cloud Console.
```bash
gcloud auth application-default login
```

### 3. Environment Configuration
Copy `.env.example` to `.env` and fill in your keys.
```bash
cp .env.example .env
```

### 4. Replit Setup
Retrieve your Replit API Token from your Replit account settings and place it in the `.env` file. This token allows the Gemini agent to spawn sandboxes.

### 5. Starting the Services
From the root directory, run:
```bash
docker-compose up --build
```
