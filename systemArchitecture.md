# Circle — System Architecture (updated)

## Summary — current repo state
This repository contains a frontend-only React + Vite SPA demo that uses local mock data. There is no backend, no persistent database, and no production notification or authentication integration in this workspace.

---

## Implemented (what's actually available)
- Frontend: React + Vite (scripts in Circle_Frontend/package.json)
- Dependencies present: react, react-dom, axios (present but used against mock data), vite, @vitejs/plugin-react
- UI features implemented:
  - Home page (friend list)
  - FriendCard components showing name, lastContact and notes
  - UI actions: "Remind me", "Message" (client-side only handlers)
- Files: src/, index.html, Circle_Frontend/package.json, Circle_Frontend/README.md
- Data: in-memory mock arrays in components (no persistence)
- No auth, no background worker, no external providers

---

## Proposed backend & API (precise, actionable design)
This section describes the minimal backend to integrate with the current frontend and how it should behave.

**Key API endpoints**
POST /api/v1/auth/register
POST /api/v1/auth/login          -> returns access_token, refresh_token
POST /api/v1/auth/token/refresh

GET  /api/v1/users/me
GET  /api/v1/users/:id

GET  /api/v1/friends             -> list friend records, supports pagination/filter
POST /api/v1/friends             -> add friend
GET  /api/v1/friends/:id
PUT  /api/v1/friends/:id
DELETE /api/v1/friends/:id

POST /api/v1/reminders           -> schedule one-off or recurring reminder
GET  /api/v1/reminders?friendId=
PUT  /api/v1/reminders/:id
DELETE /api/v1/reminders/:id

POST /api/v1/notifications/send  -> internal: send notification (used by worker)
GET  /api/v1/health

**API contracts / shapes **

Friend:
Json
{
  "id":"uuid",
  "userId":"uuid",
  "name":"string",
  "contactInfo":{"email":"", "phone":""},
  "lastContactedAt":"ISO8601",
  "notes":"string",
  "tags":["school","work"]
}

Reminder:

{
  "id":"uuid",
  "userId":"uuid",
  "friendId":"uuid",
  "type":"email|push|in-app",
  "schedule":{"type":"once|daily|weekly","cron":""},
  "message":"string",
  "status":"scheduled|sent|failed"
}

API endpoints (JSON REST)
- GET /api/friends
  - Query: ?limit=&page=&q=
  - Response 200: [{ id, name, notes, lastContact (ISO), createdAt, updatedAt }]
- GET /api/friends/:id
  - Response 200: { id, name, notes, lastContact, createdAt, updatedAt }
  - 404 if not found
- POST /api/friends
  - Body: { name: string, notes?: string, lastContact?: ISODate }
  - Validation: name required (min length 1)
  - Response 201: created friend object
- PUT /api/friends/:id
  - Body: partial friend object
  - Response 200: updated friend
  - 404 if not found
- DELETE /api/friends/:id
  - Response 204
- POST /api/reminders
  - Body: { friendId: number, remindAt: ISODate, channel: "email"|"push" }
  - Response 201: { id, friendId, remindAt, channel, status: "scheduled" }
- GET /api/reminders?status=&limit=
  - Response 200: list of reminders (for worker UI)
- POST /api/auth/login (future)
  - Body: { username, password }
  - Response 200: { accessToken } + httpOnly refresh cookie (design)

Error shape
- 4xx/5xx responses use: { error: "Human message", code: "ERR_CODE" }

**Justification for tech stack choices**

Frontend: React + Vite (current)
Why: Fast dev server, minimal config, modern build optimizations. React provides component model and huge ecosystem for UI/state management.
Feasible for: SPA, prototyping, and production when paired with static hosting/CDN.
Backend: Node + Express OR Python + FastAPI
Node + Express
Why: JavaScript/TypeScript stack parity with frontend, wide ecosystem, many developers comfortable with it.
When to pick: If the team prefers JS/TS end-to-end and wants lightweight middleware control; good for real-time and event-driven integrations.
Python + FastAPI
Why: Fast development, excellent typing, automatic OpenAPI docs, high performance for Python frameworks.
When to pick: If team prefers Python, wants automatic interactive docs, or will use Python for workers/data processing.
Database: PostgreSQL
Why: ACID, relational model suits friend/relationship data, mature migrations (Flyway/Prisma/Alembic), good for joins and analytics.
Cache/Queue: Redis & RabbitMQ / AWS SQS
Redis: session caching, rate-limiting, hot lookups (last-contact pages).
Queue (RabbitMQ/SQS): reliable background job handling (send reminder emails/push).
Worker: Celery / BullMQ / Sidekiq-like
Why: Offload scheduled tasks, retry logic, scale independently, avoid blocking API requests.
Hosting & infra
Static frontend: S3 + CloudFront / Vercel / Netlify.
API: containerized in ECS/EKS / GCP Cloud Run / Heroku.
Database: RDS / Cloud SQL.
Monitoring: Prometheus + Grafana, or hosted solutions (Datadog).

Server-side rules
- Input validation via Joi (Express) or Pydantic (FastAPI)
- Strict CORS allowing frontend origin in dev
- Rate limiting (IP-based) for public endpoints

Backend components
- API server (Express or FastAPI)
  - Responsible for validation, business logic, and scheduling reminders
- Database: PostgreSQL
  - Tables: users, friends, reminders, jobs (basic schema below)
- Cache / queue: Redis
  - Use for caching friend lists and as job queue (Bull/BullMQ or RQ)
- Worker: separate process to dispatch reminders (Node worker or Celery)
- Notification providers: SendGrid/SES for email; Web Push for browser push

Minimal DB schema (MVP)
- friends: id (PK), user_id (FK), name, notes, last_contact (date), created_at, updated_at
- reminders: id (PK), friend_id (FK), remind_at (timestamp), channel, status, attempts, payload, created_at, updated_at
- users (future): id, email, password_hash, created_at

Why these endpoints/components
- They map directly to UI actions and support a stepwise migration from mock -> real data.
- Splitting API, DB, and Worker keeps services simple and horizontally scalable.

---

## Justification for tech stack choices
- Frontend: React + Vite
  - Fast dev cycles, HMR, small bundle for SPA demos.
- Backend choices:
  - Node.js + Express: same language as frontend, large ecosystem, quick prototyping.
  - FastAPI (alternative): stronger built-in validation, async-first, automatic OpenAPI docs.
  - Choose by team familiarity; both are production-feasible.
- Database: PostgreSQL
  - ACID guarantees, easy joins for friend/reminder relationships, mature tooling.
- Redis:
  - Fast cache for read-heavy friend lists, sorted sets or queues for scheduling reminders.
- Worker: Bull/BullMQ (Node) or Celery (Python)
  - Reliable job processing, retries, backoff strategies.
- Notification providers: SendGrid / AWS SES for email; Web Push for in-browser.

Technical tradeoffs
- Express = faster JS-only start; requires separate validation library.
- FastAPI = built-in validation and docs; better type-safety if team uses Python.
- Redis adds operational complexity but is necessary for reliable scheduling at scale.

---

## Architecture diagram
(visualize the components and flows)

<img width="1855" height="772" alt="image" src="https://github.com/user-attachments/assets/e5be8e56-2075-4ab9-ba81-b987f526db65" />


```mermaid
flowchart LR
  Browser[Browser (React + Vite)] -->|HTTPS JSON| API[Backend API (Express / FastAPI)]
  API --> DB[(PostgreSQL)]
  API --> Cache[(Redis cache)]
  API -->|enqueue job| Queue[(Redis / Bull)]
  Queue --> Worker[Worker (send reminders)]
  Worker --> External[Email / Push Provider]
  style Browser fill:#f3f4f6,stroke:#111
  style API fill:#e6f7ff,stroke:#111
  style DB fill:#fff1f0,stroke:#111
  style Cache fill:#fff7e6,stroke:#111
  style Worker fill:#eafff0,stroke:#111
  style External fill:#f0f4ff,stroke:#111
```

---

## Detailed data flow (step-by-step)

1. Fetch friend list (read)
   - Browser: GET /api/friends
   - API: check Redis cache key "friends:user:{id}:page:...".
     - If cache hit -> return JSON from cache.
     - If miss -> query Postgres, transform results, populate Redis (TTL), return JSON.
   - Browser renders FriendCard components.

2. Add friend (create)
   - Browser: POST /api/friends { name, notes }
   - API: validate -> insert into Postgres -> invalidate related cache keys -> return 201 with created record.
   - Browser: optimistic update or re-fetch GET /api/friends.

3. Schedule a reminder
   - Browser: POST /api/reminders { friendId, remindAt, channel }
   - API: validate -> insert reminder in Postgres (status = scheduled) -> push job to Redis queue with timestamp metadata -> return 201.
   - Worker: Polls / uses delayed job mechanism -> when due pops job -> marks reminder status = sending -> attempts delivery via provider -> on success mark reminder as sent and record sent_at; on failure increment attempts and requeue/backoff.
   - Browser: user can query GET /api/reminders to show status.

4. Send notification (worker)
   - Worker fetches job -> prepares payload -> calls external provider (SendGrid / Web Push) -> updates reminders table with delivery result and logs.

5. Authentication (future)
   - Browser logs in -> receives short-lived access token + httpOnly refresh cookie -> Browser includes access token in Authorization header for API calls -> API validates token, enforces user scoping on queries.

---

## Feasibility, scaling and operational notes

MVP feasibility
- Implement API server with in-memory store for fast integration tests with frontend, then switch to Postgres.
- Start with single API instance, single Postgres, single worker.

Scaling roadmap
- Stateless API nodes behind load balancer.
- Use Postgres read-replicas for heavy reads; use Redis for caching frequent queries.
- Increase worker concurrency based on queue size; autoscale workers.
- Implement monitoring (Prometheus/Grafana), structured logs, Sentry for errors.
- Use feature flags for progressive rollout of notifications.

Resiliency considerations
- Retries and backoff for external provider failures.
- Idempotent job processing (use job IDs).
- Back pressure on queue when provider rate-limits.

---

## Local integration steps (quick)
1. Scaffold minimal backend (Express or FastAPI) implementing the endpoints above.
2. Run backend locally (example: http://localhost:5000)
3. In frontend, set axios baseURL to http://localhost:5000/api and replace mockFriends with GET /friends.
4. Use Postman or curl to test endpoints before wiring UI.

---

## Next immediate tasks (practical)
1. Create a backend repo with the REST endpoints (start with in-memory store).
2. Wire frontend axios calls to the local API and test flows.
3. Add Postgres and Redis, move persistence from memory to DB.
4. Implement a simple worker to dequeue and log reminders, then integrate a real provider.
