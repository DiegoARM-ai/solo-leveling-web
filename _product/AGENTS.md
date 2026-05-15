# Agent Instructions

Read this before doing anything in this repo.

---

## How this repo works

All feature work flows through `_product/`. There are no exceptions.

```
IDEA → BACKLOG → BRIEF → BUILD → REVIEW → SHIP
```

## Brief lifecycle

Every brief moves through these statuses in order:

```
draft → ready → in-progress → built/pending review → done
```

If fixes are needed after review: `done` → back to `in-progress` → `built/pending review` → `done`

**Never rewrite or delete content above the Implementation notes section.** The brief above that line is the permanent record of intent. Only add to it, never change it.

---

## Your rules

**Before building anything (Cursor):**
- Look for a brief in `_product/BRIEFS/` first.
- Read `_product/CONTEXT.md` to understand what this app is, what stack it uses, and what the current goals are.
- Read `.cursorrules` at the root of this repo for code style, stack, and communication preferences.
- If you were given a brief number or slug, read that brief completely before touching any code.
- If no brief exists for what you've been asked to build, say so and ask for one before proceeding.
- If the brief is ambiguous or missing key details, ask one focused question before building. Do not guess.
- Update brief status from `ready` → `in-progress` before writing any code.

**While building (Cursor):**
- Stay strictly within the scope of the brief. Do not touch files outside it.
- Follow existing patterns in the codebase. Do not introduce new ones without asking.
- Keep diffs small and reviewable. Build in stages if the brief is large.
- Never add a dependency without asking first.

**After building (Cursor):**
- Update the **Implementation notes** section of the brief — what was built, any deviations, anything left out, branch and commit reference.
- Update brief status from `in-progress` → `built/pending review`.
- Generate a commit message in the format specified in `.cursorrules`.
- Do NOT write the review — that is Codex's job.

**After building (Codex — reviewing):**
- Read the brief including the Implementation notes section.
- Write a full review to `_product/REVIEWS/[number]-[slug]-review.md`.
- Check every acceptance criterion — met, partial, or not met.
- List bugs found, risks remaining, and your ship verdict.
- Update the Review section of the brief with the summary and verdict.
- Update brief status: `built/pending review` → `done` (if clean) or back to `in-progress` (if fixes needed).
- Summarize everything in plain English. The owner is non-technical.

---

## Fix loop (when review finds issues)

When Codex returns a verdict of `needs fixes`, the loop is:

```
Codex flags issues → Cursor fixes → Codex re-reviews (targeted) → done or repeat
```

**Cursor fix prompt:**
```text
Read _product/BRIEFS/[number]-[slug].md and
_product/REVIEWS/[number]-[slug]-review.md.

Fix only the issues marked as blocking in the review.
Do not touch anything outside the scope of those fixes.
Do not re-implement anything Codex already marked as passing.
Update the Implementation notes section of the brief when done.
Generate a commit message.
```

**Codex targeted re-review prompt:**
```text
Read _product/BRIEFS/[number]-[slug].md and
_product/REVIEWS/[number]-[slug]-review.md.

Re-review only the specific issues that were marked as needing fixes.
Do not re-check items already marked as passing — those are done.
Confirm whether each fix is resolved.
Update the review file with your findings.
Update brief status to done if all issues are resolved,
or back to in-progress if any remain.
```

**Rules for the fix loop:**
- Cursor only fixes what Codex flagged — nothing else
- Codex only re-checks what was fixed — nothing else
- Same branch throughout — fixes are part of the same feature
- Each pass gets its own commit with a clear message referencing the fix
- Loop repeats until Codex verdict is `ready to ship`

---

## File map

| File | What it is | Read it? |
|---|---|---|
| `_product/CONTEXT.md` | What this app is, target stack, current goals | Always, first |
| `.cursorrules` | Code style, stack, communication rules | Always |
| `_product/BACKLOG.md` | Raw ideas not yet ready to build | Only if asked |
| `_product/BRIEFS/000-template.md` | Brief format reference | Only if writing a brief |
| `_product/BRIEFS/[number]-[slug].md` | The brief for the current task | Always before building |
| `_product/REVIEWS/` | Past review outputs | Only if asked |

---

## Writing a brief (when asked)

If asked to review a codebase and write a migration or feature brief:

1. Read `_product/CONTEXT.md` for the target state
2. Read `_product/BRIEFS/000-template.md` for the format
3. Inspect the relevant files in the codebase
4. Write the brief to `_product/BRIEFS/[next-number]-[slug].md`
5. Be specific — vague briefs produce bad builds
6. List every acceptance criterion as a testable checkbox
7. Note anything ambiguous in the Open Questions section — do not assume

---

## What you should never do

- Build from a freehand prompt without a brief
- Touch files outside the scope of the current brief
- Add dependencies without asking
- Silently change things not mentioned in the brief
- Skip writing the review after building
- Assume the owner understands error messages or technical jargon — explain everything in plain English
