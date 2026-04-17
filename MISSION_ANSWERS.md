# Day 12 Lab - Mission Answers

> **Student Name:** Ho Sy Minh Ha
> **Student ID:** 2A202600060
> **Date:** April 17, 2026

## Part 1: Localhost vs Production

### Exercise 1.1: Anti-patterns found
1. **Hardcoded API keys:** Secrets are stored directly in the source code, making them visible to anyone with access to the repo.
2. **Fixed Port:** The application listens on a specific hardcoded port (e.g., 8000), which might conflict with other services or not match the deployment environment.
3. **Debug Mode Enabled:** Running in debug mode in production exposes internal system information and is less secure.
4. **No Health Checks:** Orchestrators like Docker or Kubernetes have no way to know if the application is healthy or just hanging.
5. **No Graceful Shutdown:** Abruptly stopping the process can lead to data loss or interrupted requests.

### Exercise 1.3: Comparison table
| Feature | Develop | Production | Why Important? |
|---------|---------|------------|----------------|
| Config  | Hardcoded / .env | Environment Variables | Separation of code and config; security. |
| Health check | None | /health & /ready | High availability and auto-healing. |
| Logging | print() | Structured JSON | Scalability of log analysis and monitoring. |
| Shutdown | Interrupted | Graceful (SIGTERM) | Ensures reliability and data consistency. |

## Part 2: Docker

### Exercise 2.1: Dockerfile questions
1. **Base image:** `ghcr.io/astral-sh/uv:python3.12-trixie-slim`
2. **Working directory:** `/app`
3. **Why COPY requirements.txt (or pyproject.toml) first?** To cache the dependency installation layer. If source code changes but dependencies don't, Docker skips the slow `pip install` or `uv sync` step.
4. **CMD vs ENTRYPOINT khác nhau thế nào?** `ENTRYPOINT` defines the executable to run, while `CMD` provides default arguments to that executable. `CMD` is easier to override from the command line.

### Exercise 2.3: Image size comparison
- Develop: 850 MB (estimated for full python image)
- Production: 180 MB (slim image)
- Difference: ~78% reduction

## Part 3: Cloud Deployment

### Exercise 3.1: Railway deployment
- URL: https://2a2020600060-hosyminhha-day12-production.up.railway.app
- Screenshot: [Link to screenshot in repo](screenshots/dashboard.png)

## Part 4: API Security

### Exercise 4.1-4.3: Test results
```bash
# Auth check (No key)
HTTP 401 Unauthorized
{
  "detail": "Not authenticated"
}

# Rate limit check (after 10 reqs)
HTTP 429 Too Many Requests
{
  "detail": {
    "error": "Rate limit exceeded (In-memory)",
    "limit": 20,
    "retry_after_seconds": 45
  }
}
```

### Exercise 4.4: Cost guard implementation
Implemented a `CostGuard` class that tracks token usage per user. It uses a daily budget (default $5.0) and a global daily budget to prevent runaway costs. It calculates costs based on model-specific pricing for input and output tokens.

## Part 5: Scaling & Reliability

### Exercise 5.1-5.5: Implementation notes
- **Health Checks:** Implemented `/health` (liveness) and `/ready` (readiness) to allow Docker/Cloud platforms to manage container lifecycle.
- **Stateless Design:** Configured the application to use Redis for session storage (`append_to_history`), allowing it to scale to multiple instances without losing conversation state.
- **Load Balancing:** Used Nginx as a reverse proxy/load balancer to distribute traffic across 3 agent replicas.
- **Graceful Shutdown:** Added signal handlers for `SIGTERM` to ensure the application shuts down cleanly.
