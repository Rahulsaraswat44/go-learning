# Go Learning — Revisions Queue

> Spaced-repetition queue for the ~20% of Go material that doesn't get reinforced through daily project work.

## How this works (in brief)

- **Selective by design.** Only gotchas, rarely-used APIs, architectural criteria, quotable idioms, and toolchain incantations live here. See `CLAUDE.md` → Revision system for full criteria.
- **Two modes:** code-exercise (default) and flashcard.
- **SM-2-lite scheduling:**
  - New items start at 1-day interval
  - **Good** → interval × 2.5
  - **Hard** → interval × 1.5
  - **Again** → reset to 1 day
  - Max interval: 180 days (mastered)
- **Daily cap:** 15 minutes of revision before new material.

## Ease ladder

- **new** — just added, never revised
- **learning** — interval 1–7 days
- **mastering** — interval 7–30 days
- **mastered** — interval 30+ days; near 180 means essentially permanent

---

## Due today

_Claude: populate this section at the start of each revision session by scanning All items for those with `next-due ≤ today`. List IDs only; full content is in All items._

(none — queue is empty)

---

## All items

_Newest first. Empty until first items are added._

(none yet)

---

## Item template

When adding an item, use the next sequential 4-digit ID (REV-0001, REV-0002, ...).

---

### REV-XXXX — \<short topic name\>

- **Type:** code-exercise | flashcard
- **Phase:** \<0–6\>
- **First learned:** YYYY-MM-DD
- **Last revised:** never
- **Interval:** 1 day
- **Next due:** YYYY-MM-DD  _(= first-learned + 1)_
- **Ease:** new

**Prompt:**

\<For flashcards: the question.\>
\<For code-exercises: the problem statement, no scaffolding given.\>

**Expected answer / acceptance criteria:**

\<For flashcards: the answer.\>
\<For code-exercises: what idiomatic Go looks like, and the key checks (correctness, idiom, naming, error handling, concurrency safety).\>

**Revision log:**

- _(none yet)_

---

## Revision log entry format

When updating an item after a revision, append one line per attempt to its **Revision log** section:

`- YYYY-MM-DD — Good|Hard|Again — interval: <prev>d → <new>d — <short note>`

Examples (illustrative, not real items):

- `2025-06-02 — Good — interval: 1d → 3d — recalled without hesitation`
- `2025-06-05 — Hard — interval: 3d → 5d — got it but forgot the type-vs-value nuance`
- `2025-06-10 — Again — interval: 5d → 1d — confused with another concept; needs notes review`

---

## Maintenance notes

- If an item has been **Again** three times in a row, flag it for a notes session — the underlying understanding isn't there yet and revision alone won't fix it.
- If an item reaches 180-day interval and gets another **Good**, archive it (move to an "Archived" section at the bottom of this file). It's mastered.
- Keep total active items under ~40. If the queue grows beyond that, prune items that are conceptually similar — consolidate, don't accumulate.
