# Go Learning Progress

> Source of truth for where I am in the roadmap. Update after every meaningful session.

## Current state

- **Phase:** 0 — Ecosystem & Tooling (complete ✅)
- **Started:** 2026-05-26
- **Last session:** 2026-05-27
- **Hours invested:** ~2

## Completed

- Go installed and working
- `go env` skimmed, GOPATH/GOROOT mental model in place
- Core toolchain understood: `go build`, `run`, `test`, `fmt`, `vet`, `mod`
- `goimports` and `golangci-lint` installed
- Phase 0 mini-exercise done:
  - `go mod init`, added `github.com/google/uuid` dependency
  - `go run .` — ran the program, printed a UUID
  - `go build -o hello .` — produced a 2.7 MB executable
  - `file` + `ls -lh` — inspected the binary
  - `GOOS=linux GOARCH=arm64 go build` — cross-compiled to ARM64, confirmed statically linked
- Understood the difference between dynamically linked (default native build) vs statically linked (`CGO_ENABLED=0`)

## In progress

Moving to Phase 1 — Language Fundamentals.

## Next up

**Phase 1 — Language Fundamentals.** Start with the syntax primer in ROADMAP.md, then deeper topics. Mini-project: CLI tool that processes JSON.

Before starting Phase 1, revisit the items listed below (Go module infrastructure deep-dive).

## Open questions / things to revisit

- **Go module system deep-dive** — `go.mod`, `go.sum`, `go get`, `go mod tidy`, module cache, versioning. How this compares to Maven/Gradle (Java) and the fact that C/C++ had no standard package manager for decades (CMake, vcpkg, conan are all ecosystem-level, not language-level). Want a proper mental model, not surface-level.
- **`CGO_ENABLED=0`** — forces a fully static binary on native builds. Needed when building Go binaries for minimal Docker images (`FROM scratch`). Revisit in Phase 4 when we write Dockerfiles.
- **Dynamic vs static linking explained properly** — touched on it during Phase 0 (cross-compiled binary was statically linked, native was not). Want the full picture before Phase 4.
- **`go env` output** — skimmed but not deeply understood. Worth one proper read-through.

## Session log

### 2026-05-26 — Setup + Phase 0 theory
- Created learning folder, dropped in CLAUDE.md, ROADMAP.md, PROGRESS.md
- Reviewed the full roadmap end-to-end
- Installed Go, understood go modules, go.mod/go.sum, GOPATH/GOROOT, core toolchain

### 2026-05-27 — Phase 0 mini-exercise + tooling
- Installed `goimports` and `golangci-lint`
- Ran full Phase 0 mini-exercise: uuid dep, go run, go build, cross-compile to ARM64
- Learned dynamic vs static linking difference (native vs cross-compiled)
- Established explanation style: scaffold systems/tooling topics, normal pace for syntax
- **Next session:** revisit Go module system deeply, then start Phase 1

---

## Roadmap quick reference

| Phase | Focus | Mini-project | Status |
|---|---|---|---|
| 0 | Ecosystem & Tooling | Hello world + cross-compile | ✅ Complete |
| 1 | Language Fundamentals | CLI JSON processor | Not started |
| 2 | Concurrency | Concurrent web scraper / port scanner | Not started |
| 3 | Stdlib Mastery | REST API (in-memory) | Not started |
| 4 | Production Backend | REST API + Postgres + auth + observability + Docker | Not started |
| 5 | Microservices | 2–3 service system with gRPC + NATS | Not started |
| 6 | Cloud-native / K8s | K8s operator on kind | Not started |
