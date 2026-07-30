# Setup and Installation Guide

## Local Development

### 1. Prerequisites
- Install [Docker](https://docs.docker.com/get-docker/) and Docker Compose.
- Ensure you have the required API keys (OpenAI, etc.).

### 2. Environment Configuration
Copy `.env.example` to `.env` and fill in your keys.
```bash
cp .env.example .env
```
**Important:** Never commit the `.env` file to version control.

### 3. Starting the Services
From the root directory, run:
```bash
docker-compose up --build
```
This will spin up the database, backend API, and frontend development server.

### 4. Running Tests
To run the automated test suite:
```bash
pytest tests/
```

## Deployment to Google Cloud (Hackathon Target)
*Instructions for deploying the Docker containers to Google Cloud Run will be added here.*
