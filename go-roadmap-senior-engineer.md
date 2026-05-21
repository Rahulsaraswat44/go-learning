# Go Learning Roadmap for a Senior Backend/Cloud Engineer

**Target:** Production-grade Go for cloud-native backend (microservices, Docker, K8s) in 4–6 focused weekends.

**Assumptions:** You're a senior dev with Java/C++ background. You understand pointers, types, concurrency concepts, OOP, generics. This roadmap skips beginner content.

**Philosophy:** Learn by building. AI is your tutor. Each phase has a deliverable. Don't move forward until the deliverable is done.

---

## Phase 0 — Ecosystem & Tooling (a few hours, before you write any Go)

### Goal
Understand how Go projects are structured, built, and managed. No virtualenvs, no dependency hell.

### Topics

**1. Installing Go and version management**
- Install via official installer (go.dev), Homebrew, `apt`, or `asdf`. One installation is fine for most work — Go has strong backward compatibility (the "Go 1 promise").
- Since Go 1.21, the `go` command can auto-download and use the toolchain version specified in `go.mod`. Read about the `GOTOOLCHAIN` env var.
- For multiple versions side-by-side: `go install golang.org/dl/go1.21.5@latest && go1.21.5 download`.
- *Compare to Python:* no `pyenv` / `venv` story needed. Modules give you per-project isolation.

**2. The Go module system (the biggest thing to internalize)**
- A *module* is a unit of code with its own dependencies, defined by a `go.mod` file at the root.
- `go mod init github.com/you/project` creates it.
- `go.mod` lists deps and the Go version; `go.sum` has checksums. Commit both.
- No virtualenvs — dependencies live in a shared cache (`$GOPATH/pkg/mod`), but each module pins its own versions.
- `go get` adds/updates deps; `go mod tidy` cleans up unused ones.
- Closer to npm's `package.json` / `package-lock.json` than to Python's requirements.txt.

**3. GOPATH and GOROOT (legacy, briefly)**
- `GOROOT` — where Go itself lives. You don't touch this.
- `GOPATH` (default `~/go`) — where binaries installed via `go install` land, and where the module cache lives. You no longer put your project source here (that was pre-1.11).
- TL;DR: install Go, add `$GOPATH/bin` to your PATH, forget the rest.

**4. The toolchain (what comes free with `go`)**
- `go build` — compile
- `go run` — compile + run
- `go test` — run tests (and `go test -race`)
- `go fmt` / `gofmt` — format. **There is no style debate in Go.** Wonderful coming from JS/TS.
- `go vet` — built-in static analysis
- `go install` — install a binary to `$GOPATH/bin`
- `go doc` — read package docs from the terminal
- `go mod ...` — module commands

**5. Things you'll install separately**
- `goimports` — format + auto-manage imports. Use instead of `gofmt`.
- `golangci-lint` — the de facto linter aggregator. Add to every project.
- `gopls` — the Go language server used by every IDE. Auto-installed by the VS Code Go extension.

**6. Compilation and binary characteristics (the cloud-native angle)**
- Go compiles to **statically linked native binaries**. No runtime, no JVM, no interpreter needed on the target machine.
- Cross-compile trivially: `GOOS=linux GOARCH=arm64 go build`.
- This is *why* Go dominates cloud-native — a Docker image with a Go binary is often 10–20 MB, vs hundreds of MB for JVM/Python.
- CGO (calling C code) breaks the static-binary story. Avoid it unless needed.

**7. Workspaces (Go 1.18+) — multi-module dev**
- `go work init` and `go.work` files. Like pnpm/yarn workspaces, for monorepos with multiple modules. Skip for now.

**8. IDE setup**
- VS Code + Go extension (free, excellent, uses gopls) or GoLand (JetBrains, paid, also excellent). Either works — the Go ecosystem isn't IDE-fragmented like Java's was.

**9. Project structure conventions**
- `cmd/<name>/main.go` for each binary entry point
- `internal/` for private packages — **the compiler enforces** that nothing outside the module can import these. Unique to Go, very useful for clean APIs.
- Don't follow the bloated `golang-standards/project-layout` repo. It's community-made and considered overkill by many core team members.

**10. Go's release cadence**
- New minor version every ~6 months (Feb and Aug).
- Backward compatibility is taken very seriously. Code from 2014 likely still compiles.
- Two most recent versions are officially supported. Use the latest stable.

### Comparison cheat sheet (Python / Java → Go)

| Concept | Python | Java | Go |
|---|---|---|---|
| Dependency manifest | requirements.txt / pyproject.toml | pom.xml / build.gradle | `go.mod` |
| Lockfile | poetry.lock / pip freeze | maven/gradle lockfile | `go.sum` |
| Env isolation | venv / virtualenv | (classpath-based) | modules (built-in, per-project) |
| Build output | .pyc / source | .jar / .class | static native binary |
| Runtime on target | Python interpreter | JVM | none |
| Format | black, ruff | (debated) | `gofmt` (no debate) |
| Lint | flake8, pylint, ruff | checkstyle, SpotBugs | `golangci-lint` |
| Package registry | PyPI | Maven Central | proxy.golang.org (defaults to GitHub) |
| Test framework | pytest, unittest | JUnit | stdlib `testing` |

### Mini-exercise
- Install Go (official installer or asdf)
- Run `go env` and read the output — understand what each var means
- Hello world: `mkdir hello && cd hello && go mod init example.com/hello`, write `main.go`, `go run .`
- Add a dep: `go get github.com/google/uuid`, use it in `main.go`, then `go mod tidy`
- Build and inspect: `go build && file ./hello && ls -lh ./hello` — note the static binary size
- Cross-compile: `GOOS=linux GOARCH=arm64 go build` (build for a Pi from your laptop)

### Checkpoint
- You understand `go.mod` is your module manifest and `go.sum` is the lockfile
- You can explain why Go doesn't need virtualenvs
- You can cross-compile to another OS/arch
- `$GOPATH/bin` is in your PATH and `golangci-lint` is installed

---

## Phase 1 — Language Fundamentals (Weekend 1, ~10–12 hours)

### Goal
Read and write idiomatic Go for non-concurrent code. Stop "writing Java in Go".

### Syntax primer (start here)

Just enough to read and write Go. Refer back as needed; use AI to expand on anything unclear.

**Variables and types**
```go
var x int = 5            // explicit
y := 10                  // short form (inside functions only); type inferred
const pi = 3.14159       // const — basic types only

// Built-ins: int, int8/16/32/64, uint*, float32/64, bool, string
// byte = uint8, rune = int32 (a Unicode code point)
// NO implicit conversions:
//   var a int32 = 5; var b int64 = a   // won't compile
//   var b int64 = int64(a)             // explicit cast required
```

**Control flow**
```go
if x > 0 { ... } else if x < 0 { ... } else { ... }
if v, err := compute(); err == nil { use(v) }   // init statement before condition

// for is the ONLY loop — three forms
for i := 0; i < 10; i++ { ... }      // classic
for x < limit { ... }                 // while-style
for { ... }                           // infinite
for i, v := range slice { ... }       // iteration
for k, v := range myMap { ... }
for v := range channel { ... }

// switch — no fallthrough by default (opposite of Java/C++)
switch x {
case 1, 2: ...                        // multiple values per case
case 3: ...
default: ...
}

// type switch
switch v := x.(type) {
case int:    ...
case string: ...
}

// No ternary operator. Period.
```

**Functions**
```go
func add(a, b int) int { return a + b }
func divmod(a, b int) (int, int) { return a / b, a % b }    // multiple returns
func split(s string) (head, tail string) {                   // named returns
    head, tail = s[:1], s[1:]
    return
}
func sum(nums ...int) int { ... }     // variadic; call: sum(1,2,3) or sum(slice...)

// First-class functions and closures
counter := func() func() int {
    n := 0
    return func() int { n++; return n }
}()
```

**Composite types**
```go
// Slice (the workhorse — dynamic array)
nums := []int{1, 2, 3}
nums = append(nums, 4)

// Map
m := map[string]int{"a": 1, "b": 2}
v, ok := m["c"]                       // ok is false if key missing

// Struct
type Point struct {
    X, Y int
}
p := Point{X: 1, Y: 2}                // named fields (preferred)
```

**Pointers**
```go
x := 5
p := &x          // take address
*p = 10          // dereference; now x == 10
// No pointer arithmetic. nil is the zero value.
```

**Methods**
```go
type Point struct{ X, Y int }

func (p Point) Distance() float64 { ... }     // value receiver (gets a copy)
func (p *Point) Scale(f int) {                // pointer receiver (mutates)
    p.X *= f
}
```

**Interfaces (the Java dev's first surprise)**
```go
type Stringer interface {
    String() string
}

// Any type with a String() string method satisfies Stringer.
// No "implements" keyword — structural typing, the compiler figures it out.

var x any = 42       // 'any' is the Go 1.18+ alias for interface{}
```

**Error handling**
```go
f, err := os.Open("file.txt")
if err != nil {
    return fmt.Errorf("opening config: %w", err)   // wrap with %w
}
defer f.Close()
```

**Visibility (no keywords)**
```go
type point struct { ... }     // lowercase = unexported (package-private)
type Point struct { ... }     // uppercase = exported (public)
func helper() { ... }         // unexported
func Process() { ... }        // exported
```

**Defer**
```go
// Runs when the surrounding function returns. LIFO order across multiple defers.
f, _ := os.Open("...")
defer f.Close()              // idiomatic: cleanup right next to acquisition
```

That's enough to read most Go code. The topics below go deeper into the things experienced devs from Java/C++ get wrong.

### Topics to internalize (in order)
1. **Modules and packages** — `go mod init`, imports, visibility via capitalization, `internal/` directory.
2. **Pointers** — like C++, but no pointer arithmetic. When to use them.
3. **Structs and methods** — value vs pointer receivers. This is where most Java devs trip. Rule of thumb: pointer receivers if the method mutates, the struct is large, or other methods of the type use pointer receivers (consistency wins).
4. **Interfaces** — structural typing. No `implements` keyword. **Accept interfaces, return structs.** The empty interface `any` and type assertions.
5. **Error handling** — multiple return values, `errors.Is`, `errors.As`, `fmt.Errorf("...: %w", err)`. No exceptions, no try/catch.
6. **Zero values** — every type has one. `var x T` is always valid. Lean on this; don't write defensive nil checks like in Java.
7. **Slices** — biggest footgun in Go. Understand the underlying array, capacity vs length, `append` behavior, sub-slicing gotchas.
8. **Maps** — no order, no concurrent writes (will panic).
9. **Strings and runes** — strings are UTF-8 bytes. Iterate with `range` to get runes.
10. **Generics** (Go 1.18+) — type parameters. Use sparingly; Go ecosystem still leans concrete.

### Read in parallel
- [Effective Go](https://go.dev/doc/effective_go) — one evening read
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments) — bookmark, reference often
- [Uber Go Style Guide](https://github.com/uber-go/guide) — opinionated but mostly community-accepted

### Mini-project: CLI tool
Build a CLI that processes JSON. E.g., reads a JSON file of GitHub issues and outputs filtered/grouped CSV. Use `flag`, `encoding/json`, file I/O. ~150–250 lines.

### Checkpoint — you're ready to move on when you:
- Can explain value vs pointer receivers without thinking
- Don't reach for `*string` when you mean "optional string"
- Write `if err != nil { return err }` without flinching
- Use `errors.Is` and `%w` correctly
- Understand why nil interface ≠ nil pointer in interface

---

## Phase 2 — Concurrency (Weekend 2, ~8–10 hours)

### Goal
Use goroutines and channels confidently. Pass `context` everywhere.

### Topics
1. **Goroutines** — cheap, but not free. When to spawn.
2. **Channels** — unbuffered (synchronization) vs buffered (queue). Directional channels in function signatures (`chan<-`, `<-chan`).
3. **`select`** — non-blocking communication, timeouts, multiplexing.
4. **`sync` package** — `Mutex`, `RWMutex`, `WaitGroup`, `Once`. When mutex beats channel ("share memory by communicating" is a guideline, not a law).
5. **`context.Context`** — pass it as first arg to every function that does I/O or blocks. Cancellation, deadlines, request-scoped values.
6. **The race detector** — `go test -race`. Run it always.
7. **Common patterns** — worker pool, fan-in/fan-out, pipeline, semaphore via buffered channel.
8. **`errgroup`** — `golang.org/x/sync/errgroup` for concurrent ops with error propagation.

### Read alongside
- "Go Concurrency Patterns" by Rob Pike (30-min talk on YouTube — exception to the no-tutorial rule, it's seminal)
- Concurrency chapter of "100 Go Mistakes and How to Avoid Them"

### Mini-project: Concurrent web scraper OR port scanner
Required features: configurable worker pool size, context-based cancellation (Ctrl+C cleans up), bounded concurrency, error aggregation via errgroup. Run with `-race` and make sure it's clean.

### Checkpoint:
- You know when a channel should be buffered
- You understand why `context` is always the first argument
- You can implement a worker pool from memory
- Race detector passes on your code

---

## Phase 3 — Standard Library Mastery (Weekend 3, ~8 hours)

### Goal
Build HTTP services and DB-backed apps without third-party frameworks. Go's stdlib is exceptional — most "web frameworks" aren't needed.

### Topics
1. **`net/http`** — server, handlers, middleware via handler-wrapping. Since Go 1.22, stdlib router supports method-and-path routing — you may not need chi/gin.
2. **`encoding/json`** — marshaling, struct tags, custom marshalers.
3. **`database/sql`** — drivers (use `pgx` for Postgres), prepared statements, transactions, context usage.
4. **`testing`** — **table-driven tests** (the idiomatic pattern), `t.Run` for subtests, `httptest` for HTTP handler tests.
5. **`log/slog`** (Go 1.21+) — structured logging, the modern way. Forget `log`.
6. **`io` and `bufio`** — reader/writer interfaces are everywhere.
7. **`os` and `flag`** — env vars, args, file ops.
8. **`time`** — duration handling, timers, tickers.

### Libraries worth knowing (don't reach for them too eagerly)
- **`chi`** — if you want middleware ergonomics beyond stdlib mux
- **`pgx`** — modern Postgres driver
- **`sqlc`** — generates type-safe Go from SQL queries (very popular in modern Go shops)
- **`testify`** — assertions, but stdlib `testing` is often enough
- **`godotenv`** or **`viper`** — config

### Mini-project: REST API
JSON REST API with: 4–5 endpoints, in-memory store (no DB yet), middleware (logging, recovery, request ID), table-driven tests, graceful shutdown, structured logging with slog. ~400–600 lines.

### Checkpoint:
- You stand up an HTTP server with graceful shutdown without looking it up
- Your tests are table-driven
- You log with structured fields, not printf-style

---

## Phase 4 — Production Backend (Weekend 4, ~10–12 hours)

### Goal
Build something you'd ship.

### Topics
1. **Project layout** — no official one. `cmd/` for binaries, `internal/` for private code, flat otherwise. Ignore the bloated `golang-standards/project-layout` repo; it's overkill. Look at how HashiCorp structures small projects.
2. **Configuration** — env vars via `os.Getenv` or a small lib. 12-factor.
3. **Postgres** — `pgx` + `sqlc` (or hand-written queries). Migrations via `goose` or `golang-migrate`.
4. **Auth** — JWT via `github.com/golang-jwt/jwt/v5`, middleware to validate, refresh tokens.
5. **Validation** — `go-playground/validator` is standard.
6. **Observability** — slog for logs, `prometheus/client_golang` for metrics, OpenTelemetry SDK for traces. Wire all three.
7. **Health and readiness endpoints** — for K8s probes.
8. **Graceful shutdown** — signal handling, drain in-flight requests, close DB.
9. **Dockerization** — multi-stage builds. Final image `FROM scratch` or `gcr.io/distroless/static`. Target: ~15MB.
10. **Testing strategy** — unit tests + integration tests via `testcontainers-go` (real Postgres in Docker for tests).

### Mini-project: Production-grade service
Harden the Phase 3 REST API: Postgres-backed, JWT auth, request validation, structured logs, Prometheus metrics, OpenTelemetry tracing, health endpoints, graceful shutdown, Dockerfile, docker-compose for local dev with Postgres + Jaeger + Prometheus, integration tests with testcontainers. **This is your portfolio piece.**

### Checkpoint:
- `docker build` produces an image under 30MB
- `go test -race ./...` passes
- `curl /metrics` returns Prometheus data
- SIGTERM lets in-flight requests finish

---

## Phase 5 — Microservices (Weekend 5, ~10 hours)

### Goal
Multiple services talking to each other with proper inter-service patterns.

### Topics
1. **gRPC + Protobuf** — define `.proto`, generate Go code, server, client. Dominant inter-service RPC in Go shops.
2. **Buf** (`buf.build`) — modern protobuf tooling; replaces `protoc` for most projects.
3. **Inter-service patterns** — synchronous (gRPC) vs async (NATS, Kafka). When to choose which.
4. **NATS** basics — lightweight messaging, common in Go ecosystems. Or **Kafka** via `segmentio/kafka-go`.
5. **Resilience** — retries with backoff (`cenkalti/backoff`), circuit breakers (`sony/gobreaker`), timeouts everywhere.
6. **Distributed tracing** — OpenTelemetry across services, trace IDs propagated through context.
7. **API gateway pattern** — one public HTTP service fronting internal gRPC services.

### Mini-project: 2–3 service system
- **API gateway** (HTTP, public)
- **Users service** (gRPC, owns its Postgres)
- **Orders service** (gRPC, owns its Postgres)
- Gateway calls both. Async event from orders → users via NATS. Distributed tracing end-to-end. docker-compose to run it all locally.

### Checkpoint:
- A trace from gateway → gRPC → DB shows up in Jaeger
- You understand when to use gRPC vs HTTP vs message queue
- Each service owns its data — no shared DB

---

## Phase 6 — Cloud-native / Kubernetes (Weekend 6+, open-ended)

### Goal
Be the person who *builds* K8s-native software, not just deploys to K8s.

### Topics
1. **Kubernetes basics from a dev's POV** — Pods, Deployments, Services, ConfigMaps, Secrets, Ingress. (Probably already familiar.)
2. **kind or minikube** — local K8s.
3. **Manifests for your Phase 5 system** — write the YAML by hand once before reaching for Helm.
4. **Helm basics** — for packaging.
5. **`client-go`** — programmatic K8s API access. Useful for tooling.
6. **`controller-runtime` + Kubebuilder** — building operators. Define a CRD, write a reconciler. **This is what 90% of "Go for K8s" jobs actually mean.**
7. **Sample operator** — something trivial, like a `ScheduledScaler` CRD that scales Deployments on a cron schedule.

### Reference reading
- *Programming Kubernetes* (Schimanski / Hausenblas) — the book
- Kubebuilder Book — free official online
- KubeCon talks on YouTube for operator patterns

### Mini-project: K8s operator
Define a CRD, write a reconciler, deploy to `kind`, demonstrate that creating your custom resource causes something to happen in the cluster.

---

## Cross-cutting: Java/C++ → Go gotchas

Stuff that will bite you specifically:

- **No constructors.** Use factory functions: `NewThing(...)`.
- **No inheritance.** Composition via embedding. Interfaces are structural.
- **No exceptions.** Errors are values. Propagate explicitly. `panic` is for truly unrecoverable bugs only.
- **No generics-everywhere mindset.** Use them when you'd otherwise duplicate code across types — not as a default.
- **`nil` is nuanced.** A nil interface is not a nil pointer wrapped in an interface. This trips up everyone once.
- **Loop variable capture** — pre-Go 1.22, `for _, v := range xs { go f(v) }` captured the same `v`. Fixed in 1.22, but you'll see legacy code with the `v := v` shadow trick.
- **No setters/getters by default.** Capitalize the field. If you need invariants, make it unexported with methods.
- **`init()` functions** exist, but use sparingly. Explicit beats implicit.
- **No `final` / `const` for arbitrary types.** Only basic types can be const.
- **Slice aliasing** — `b := a[1:3]` shares backing array. `append` may or may not allocate. Copy explicitly when you need independence.

---

## Always-open references

- [pkg.go.dev](https://pkg.go.dev) — stdlib docs, search any package here first
- [Go by Example](https://gobyexample.com/) — quick syntax reference for any feature
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments)
- [Effective Go](https://go.dev/doc/effective_go)
- [Uber Go Style Guide](https://github.com/uber-go/guide)
- [Awesome Go](https://github.com/avelino/awesome-go) — finding libraries

---

## How to use AI effectively during this

- **Don't ask "write me X".** Ask "is this idiomatic? what would a senior Go dev change?"
- **Paste your code and ask "what's the bug here?"** — but only after you've tried for 10 minutes.
- **Ask for the "Go way" of doing a Java pattern.** "I'd use a Singleton in Java; what's the Go equivalent?" (Usually: a package-level var or `sync.Once`, but often you just... don't need a singleton.)
- **Get full files reviewed.** Paste 200 lines, ask for a senior-level review. Iterate.

---

## What "done with the roadmap" looks like

You can:
- Stand up a production-grade Go HTTP service from a blank `main.go` in under 2 hours
- Write gRPC service-to-service code without reaching for tutorials
- Build and explain a K8s operator
- Tell when a Go dev is writing "Java in Go" by reading their PR
- Say in an interview: "Yes, I'd build this backend in Go and here's why."
