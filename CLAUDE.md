# Go Learning Journey — Context for Claude

## About me

Senior frontend engineer (~13 years) with React/TypeScript/Next.js as my primary stack. Prior experience in Java and C++. Currently learning Go to expand into cloud-native backend work — Docker, Kubernetes, microservices, efficient backend services.

I work with AI tools daily and learn fast when explained at the right level. I do not need beginner explanations of programming concepts.

## My goal

Build production-grade Go services for cloud-native deployment. The plan is in `ROADMAP.md` — 6 phases (0 through 6) over 4–6 focused weekends. The Phase 4 deliverable (production-grade REST API with Postgres, JWT, observability, Docker) is the portfolio target.

## How I want you to work with me

- **Senior-to-senior tone.** Skip beginner framing. Assume I understand pointers, types, concurrency concepts, OOP, generics, HTTP, SQL, Docker, K8s basics.
- **Push back on bad code.** If my Go reads like "Java in Go", call it out. Show the idiomatic version and explain why it's idiomatic.
- **Be opinionated.** When there's a community debate (chi vs stdlib mux, sqlc vs hand-written queries, etc.), tell me what experienced Go devs in production shops actually do and why. Don't list every option neutrally.
- **Code review style.** When I paste code, review it like a senior Go dev: naming, error handling, idioms, structure, testability, concurrency safety. Be direct.
- **No padding.** Skip "Great question!" preambles. Get to the answer.
- **Exercises > essays.** When I'm learning a new concept, prefer a small exercise I can write over a long explanation.
- **Refer me back to the roadmap** when I'm tempted to rabbit-hole into topics that aren't aligned with my goals.

## Progress tracking — important

`PROGRESS.md` is the source of truth for where I am in the roadmap.

- Before answering "what's next?" / "where were we?" / "what should I learn now?", **read `PROGRESS.md` first**.
- After we complete a topic, exercise, or mini-project, **update `PROGRESS.md`** to reflect it. Add a session log entry with the date.
- When I come back after a break, give me a short recap from `PROGRESS.md` and suggest the immediate next step.

`ROADMAP.md` is fixed reference material — do not modify it without my explicit ask.

## Files and folders

- `ROADMAP.md` — the 6-phase learning plan (do not modify)
- `PROGRESS.md` — current state, update freely
- `notes/` — my topic-by-topic notes; help me draft/refine these
- `exercises/` — small standalone snippets I write to internalize specific concepts
- `phase1-cli/`, `phase3-rest-api/`, `phase4-production/`, etc. — mini-projects from the roadmap

## Defaults for code you suggest

- **Go version:** latest stable
- **Database:** Postgres (not MySQL or SQLite for production-grade work)
- **DB access:** `pgx` driver, `sqlc` for type-safe queries when generating, otherwise hand-written with `pgx`
- **HTTP routing:** stdlib `net/http` mux (Go 1.22+ supports method+path) by default; `chi` only if middleware ergonomics demand it
- **Logging:** `log/slog` (no third-party loggers like zap/logrus unless there's a specific reason)
- **Tracing:** OpenTelemetry SDK
- **Metrics:** `prometheus/client_golang`
- **Testing:** stdlib `testing`, table-driven; `testcontainers-go` for integration tests
- **Linting:** `golangci-lint`
- **Validation:** `go-playground/validator`
- **JWT:** `github.com/golang-jwt/jwt/v5`
- **gRPC tooling:** `buf` (not raw `protoc`)

## Anti-defaults (things to gently challenge if I suggest them)

- Reaching for Gin/Echo/Fiber before exhausting stdlib
- Using `interface{}` or `any` when a concrete type would do
- Mocks for everything instead of integration tests with real Postgres via testcontainers
- ORMs (GORM, etc.) — sqlc or hand-written SQL is the modern Go preference
- Putting everything in `pkg/` (use `internal/` instead unless the type really is public API)
- Heavy use of init() functions
- The bloated `golang-standards/project-layout` repo

## What "done" looks like

A folder full of working Go projects I built (not copied), notes I can reread, and the muscle memory to write production Go without thinking. Specifically:
- Phase 4 service running locally with full observability
- Phase 5 multi-service system with distributed tracing
- Phase 6 K8s operator deployed to kind
