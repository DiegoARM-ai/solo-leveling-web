# .cursorrules — Template

Copy this file into the root of any project and rename it `.cursorrules`.
Fill in the stack and project-specific overrides before running any agent.

---

## Who I am

I am a non-technical founder building software to help me achieve personal and professional goals.
I work alone. I do not have a team or a technical co-founder.
I rely on you to make good engineering decisions and explain them to me in plain English.
Treat me like a smart, capable person who does not know code — not a developer who forgot the syntax.

---

## Default stack

Unless the project specifies otherwise, assume:

- **Framework:** Next.js (App Router)
- **Styling:** Tailwind CSS
- **Backend / Database / Auth:** Supabase
- **Deployment:** Vercel
- **Language:** TypeScript (keep types simple — avoid complex generics)

If this project uses a different stack, it is noted in the Project overrides section below.

---

## How to work with me

**Before building:**
- Read `_product/AGENTS.md` and `_product/CONTEXT.md` before doing anything.
- If a brief exists for the current task, read it completely before touching any code.
- If no brief exists, say so and ask for one before proceeding.
- If the brief or request is ambiguous or missing key details, ask one focused question before building. Do not guess.
- If you see a better approach than what I described, say so before implementing.

**While building:**
- Follow the existing patterns in this codebase exactly.
- Keep diffs small and focused. One thing at a time.
- Never touch files outside the scope of the current task.
- Never refactor unrelated code — even if it looks messy.
- Never add a new dependency without asking me first and explaining why it's needed.

**After building:**
- Always explain what you changed and why in plain English.
- Flag anything you're uncertain about.
- Tell me if there's something I should test or check manually.
- Note any edge cases or risks I should know about.

---

## Code quality rules

- Prefer simple, readable code over clever or abstract code.
- Avoid unnecessary abstractions unless the project clearly needs them.
- Write comments for anything non-obvious.
- Handle failure states — empty inputs, network errors, unexpected user behavior.
- Mobile-first. Everything should work well on a phone.

---

## What good output looks like

1. It works for a real user on a real device
2. It follows existing codebase patterns
3. The diff is focused — no unrelated changes
4. I can understand what changed from your plain English explanation

---

## What to never do

- Silently change things I didn't ask about
- Assume I understand an error message — explain it
- Add complexity to solve a problem that doesn't exist yet
- Use local storage for anything that needs to persist or sync — use Supabase
- Deploy or push anything without telling me what you're about to do
- Push or commit directly — I always review and commit via GitHub Desktop

---

## Git and commits

I use GitHub Desktop to review and push all changes. Never push or commit automatically.

After every build or meaningful change, provide a ready-to-paste commit summary following this format:

**Commit title** (50 chars max, imperative tense):
```
Add [feature] / Fix [bug] / Update [thing] / Remove [thing]
```

**Commit description** (what and why, not how):
```
- What changed and why it matters to the user
- Any edge cases or tradeoffs made
- Brief number this implements (e.g. "Implements _product/BRIEFS/001-feature.md")
```

**Branch naming** (when starting new work):
- Feature: `feature/[short-slug]`
- Bug fix: `fix/[short-slug]`
- Migration: `migration/[short-slug]`

Always remind me to create a branch before building anything non-trivial. Never work directly on main.

---

## Project overrides

[Fill in anything specific to this project — stack differences, files to avoid, known quirks, architectural decisions already made.]

Project name:
Stack (if different from default):
Notes:
