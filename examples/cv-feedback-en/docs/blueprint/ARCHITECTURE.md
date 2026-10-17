# Architecture

## System overview

```mermaid
flowchart TB
  subgraph Vercel[Vercel Pro]
    Landing[Landing page and waitlist form]
    Tool[CV upload and feedback pages]
    API[Route Handlers and Server Actions]
    Gateway[AI Gateway]
  end
  Visitor[Visitor] --> Landing
  User[Invited user] --> Tool
  Landing --> API
  Tool --> API
  API --> Auth[Better Auth]
  Auth --> Google[Google OAuth]
  API --> DB[(Neon Postgres via Drizzle)]
  Auth --> DB
  API --> Gateway
  Gateway --> Model[Model chosen in T203]
  API -.-> Email[Email provider chosen in T205]
```

One Next.js application on Vercel serves the public landing page and the
signed-in tool. The database holds four kinds of rows: waitlist entries,
users and sessions (Better Auth), the invite allowlist, and review results.
The uploaded CV exists only in memory during the request: it is passed to
the model through the AI Gateway and then discarded. Email is added at the
end of week 2 for invites.

## Core flow

```mermaid
sequenceDiagram
  actor U as User
  participant A as Next.js app
  participant B as Better Auth
  participant D as Neon
  participant G as AI Gateway
  U->>A: Sign in with Google
  A->>B: OAuth callback
  B->>D: Is the email on the invite allowlist?
  D-->>B: yes
  B-->>U: Session cookie
  U->>A: Upload PDF up to 4 MB
  A->>A: Check session, type, size, daily cap
  A->>G: CV and feedback prompt, zero data retention
  G-->>A: Streamed structured feedback
  A-->>U: Feedback appears as it streams
  A->>D: Store feedback only, never the file
```

## Development workflow

```mermaid
flowchart LR
  Task[Task in QUEUE.md] --> Branch[Branch txxx-name]
  Branch --> Agent[Agent implements and tests]
  Agent --> PR[Pull request]
  PR --> CI[GitHub Actions CI]
  CI --> Preview[Vercel preview]
  Preview --> Review[Owner reviews and merges]
  Review --> Prod[Production on Vercel]
```

## Environments

| Environment | Purpose | Where it runs | Data |
|-------------|---------|---------------|------|
| Local | development | `next dev` on the builder's machine | Neon development branch, synthetic CVs only |
| Preview | per pull request | Vercel preview deployment | Neon development branch, synthetic CVs only |
| Production | real users | Vercel production | Neon main branch |
