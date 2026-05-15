# _product

Lightweight product-thinking layer for this repo.
Captures ideas, turns them into agent-ready briefs, tracks reviews.
Everything lives here so Cursor and Codex can read it directly — no copy-pasting from Notion.

---

## Folder structure

```
_product/
├── README.md           ← this file — workflow instructions for you
├── AGENTS.md           ← instructions for AI agents — read this first
├── CONTEXT.md          ← what this app is, target stack, current goals
├── BACKLOG.md          ← raw capture inbox — ideas, bugs, UX thoughts
├── BRIEFS/
│   ├── 000-template.md ← copy this for every new brief
│   └── 001-[slug].md   ← ready-to-build specs
└── REVIEWS/
    ├── 000-template-review.md ← copy this for every new review
    └── 001-[slug]-review.md   ← Codex output after each build
```

**When setting up a new repo:** fill in `CONTEXT.md` first, before running any agent. It's the file that tells every agent what the app is, where it's going, and what done looks like. Without it, agents are guessing.

---

## The workflow

```
IDEA → BACKLOG → BRIEF → BUILD → REVIEW → SHIP
```

**Step 1 — Capture**
Any idea, bug, or thought → add to BACKLOG.md in 30 seconds. No formatting required.

**Step 2 — Brief**
When ready to build, open Claude and run:
```
Turn this backlog entry into a complete implementation brief using my brief template.
Entry: [paste entry]
```
Save output to BRIEFS/[number]-[slug].md. Mark backlog entry #ready.

**Step 3 — Build**
Open Cursor and paste:
```
Read _product/BRIEFS/[number]-[slug].md and implement it.
Follow existing patterns. Keep the diff small. Avoid unrelated refactors.
Ask before adding dependencies. Run narrowest useful build check after.
```

**Step 4 — Review**
Run Codex after building:
```
Read _product/BRIEFS/[number]-[slug].md for context.
Review the implementation against the acceptance criteria.
Write findings to _product/REVIEWS/[number]-[slug]-review.md.
Include: criteria met, bugs found, risks remaining, suggested fixes.
```

**Step 5 — Ship**
If review verdict is "ready to ship" → deploy.
Mark brief status as `done`.

---

## Numbering

Use three-digit numbers: 001, 002, 003...
Brief and review share the same number and slug.
Example: `BRIEFS/001-user-auth.md` → `REVIEWS/001-user-auth-review.md`

---

## Types (use in BACKLOG.md)

- `feature` — new capability
- `bug` — something broken
- `ux` — user experience improvement
- `debt` — technical debt or cleanup
- `experiment` — test an idea before committing
