# Deployment Information

## Public URL
https://2a2020600060-hosyminhha-day12-production.up.railway.app

## Platform
Railway

## Test Commands

### Health Check
```bash
$ curl https://2a2020600060-hosyminhha-day12-production.up.railway.app/health

>> {
    "status": "ok",
    "version":"1.0.0",
    "environment": "production",
    "uptime_seconds":376.8, 
    "total_requests":5,
    "checks": {
        "llm": "gemini-3.1-flash-lite-preview"
    },
    "timestamp":"2026-04-17T13:10:45.671021+00:00",
    "storage":"redis",
    "instance_id":"instance-a8c0c9",
    "redis_connected":true
}
```

### API Test (with authentication)
```bash
$ curl -X POST \
    'https://2a2020600060-hosyminhha-day12-production.up.railway.app/ask' \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJzdHVkZW50Iiwicm9sZSI6InVzZXIiLCJpYXQiOjE3NzY0Mzg3NzgsImV4cCI6MTc3NjQ0MjM3OH0.OM3o9o21ZhDipreC5tUWcXkZG8_Ey5iTvW93eOe95N4" \
  -H "Content-Type: application/json" \
  -d '{"question": "Hello"}'

>> {
    "question":"Hello",
    "answer":"Hello! I am your ReAct agent. How can I help you today?",
    "timestamp":"2026-04-17T13:16:56.715001+00:00"
}
```

## Environment Variables Set
- PORT
- REDIS_URL
- AGENT_API_KEY
- LOG_LEVEL

## Screenshots
- [Deployment dashboard](screenshots/dashboard.png)
- [Service running](screenshots/running.png)
- [Test results](screenshots/test.png)

##  Pre-Submission Checklist

- [x] Repository is public (or instructor has access)
- [x] `MISSION_ANSWERS.md` completed with all exercises
- [x] `DEPLOYMENT.md` has working public URL
- [x] All source code in `app/` directory
- [x] `README.md` has clear setup instructions
- [x] No `.env` file committed (only `.env.example`)
- [x] No hardcoded secrets in code
- [x] Public URL is accessible and working
- [x] Screenshots included in `screenshots/` folder
- [x] Repository has clear commit history

---

##  Self-Test

Before submitting, verify your deployment:

```bash
# 1. Health check
curl https://your-app.railway.app/health

# 2. Authentication required
curl https://your-app.railway.app/ask
# Should return 401

# 3. With API key works
curl -H "X-API-Key: YOUR_KEY" https://your-app.railway.app/ask \
  -X POST -d '{"user_id":"test","question":"Hello"}'
# Should return 200

# 4. Rate limiting
for i in {1..15}; do 
  curl -H "X-API-Key: YOUR_KEY" https://your-app.railway.app/ask \
    -X POST -d '{"user_id":"test","question":"test"}'; 
done
# Should eventually return 429
```

---

##  Submission

**Submit your GitHub repository URL:**

```
https://github.com/cafesuada24/2A202600060-HoSyMinhHa-day12
```
