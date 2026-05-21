# Go Learning Progress

> Source of truth for where I am in the roadmap. Update after every meaningful session.

## Current state

- **Phase:** 0 — Ecosystem & Tooling
- **Started:** _fill in when you begin_
- **Last session:** _update each time_
- **Hours invested:** 0

## Completed

_Nothing yet — journey just started._

## In progress

**Phase 0 — Ecosystem & Tooling**

Topics:
- [ ] Install Go (official installer or asdf)
- [ ] Understand modules vs virtualenvs (mental model)
- [ ] Skim `go env` output, understand GOPATH / GOROOT
- [ ] Toolchain familiarity: `go build`, `run`, `test`, `fmt`, `vet`, `mod`
- [ ] Install `goimports`, `golangci-lint`
- [ ] Set up IDE (VS Code + Go extension, or GoLand)
- [ ] Understand static binary compilation + cross-compile

Mini-exercise:
- [ ] `go mod init example.com/hello`
- [ ] Write `main.go`, `go run .`
- [ ] Add `github.com/google/uuid` dep, use it, `go mod tidy`
- [ ] `go build`, inspect binary with `file` and `ls -lh`
- [ ] Cross-compile: `GOOS=linux GOARCH=arm64 go build`

Checkpoint:
- [ ] Can explain why Go doesn't need virtualenvs
- [ ] Can cross-compile to another OS/arch
- [ ] `$GOPATH/bin` is in PATH, `golangci-lint` works

## Next up

Once Phase 0 is done → Phase 1 (Language Fundamentals). Start with the syntax primer in ROADMAP.md, then the deeper topics list. Mini-project: CLI tool that processes JSON.

## Open questions / things to revisit

_Capture anything you defer or want to come back to here. Examples:_
- _"Need to understand the nil interface vs nil pointer gotcha deeper"_
- _"Revisit when to use buffered vs unbuffered channels with a real example"_
- _"Explore sqlc when I get to Phase 4"_

## Session log

_Append a short entry per session. Format: date — what I did — what's next._

### YYYY-MM-DD — Setup
- Created learning folder, dropped in CLAUDE.md, ROADMAP.md, PROGRESS.md
- Reviewed the full roadmap end-to-end
- **Next session:** complete Phase 0 mini-exercise

---

## Roadmap quick reference

| Phase | Focus | Mini-project | Status |
|---|---|---|---|
| 0 | Ecosystem & Tooling | Hello world + cross-compile | In progress |
| 1 | Language Fundamentals | CLI JSON processor | Not started |
| 2 | Concurrency | Concurrent web scraper / port scanner | Not started |
| 3 | Stdlib Mastery | REST API (in-memory) | Not started |
| 4 | Production Backend | REST API + Postgres + auth + observability + Docker | Not started |
| 5 | Microservices | 2–3 service system with gRPC + NATS | Not started |
| 6 | Cloud-native / K8s | K8s operator on kind | Not started |
