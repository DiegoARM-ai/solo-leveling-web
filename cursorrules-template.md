# .cursorrules — Diego's Default

Copy this file into the root of any project as `.cursorrules`.
Adjust the stack section to match the specific project before building.

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
- **Language:** TypeScript (but keep types simple — avoid complex generics)

If the project uses a different stack, there will be a note at the top of this file overriding these defaults.

---

## How to work with me

**Before building:**
- If the brief or request is ambiguous, incomplete, or missing key details — ask me questions before writing any code.
- Ask one focused question at a time, not a list of five.
- If you see a better approach than what I described, say so before implementing. I want your judgment, not just execution.

**While building:**
- Follow the existing patterns in this codebase exactly. Do not introduce new patterns without asking.
- Keep diffs small and focused. One thing at a time.
- Never touch files outside the scope of the current task.
- Never refactor unrelated code — even if it looks messy.
- Never add a new dependency without asking me first and explaining why it's necessary.

**After building:**
- Always explain what you changed and why in plain English.
- Flag anything you're uncertain about.
- Tell me if there's something I should test or check manually.
- If the change has any edge cases or risks I should know about, say so.

---

## Code quality rules

- Prefer simple, readable code over clever or abstract code.
- Avoid unnecessary abstractions, utility files, or helper layers unless the project clearly needs them.
- Write comments for anything non-obvious — I need to understand what the code is doing.
- If something could break for a real user (empty state, network failure, bad input), handle it.
- Mobile-first. Everything should work well on a phone.

---

## What I care about in outputs

1. Does it work for a real user on a real device?
2. Is it simple enough that I can explain what it does?
3. Does it follow the existing codebase patterns?
4. Is the diff focused — no unrelated changes?

If the answer to any of these is no, fix it before showing me.

---

## Mistakes to avoid

- Do not silently change things I didn't ask about.
- Do not assume I understand an error message — explain it.
- Do not add complexity to solve a problem that doesn't exist yet.
- Do not use local storage for anything that needs to persist or sync — use Supabase.
- Do not deploy or push anything without telling me what you're about to do.

---

## Project-specific overrides

[Add any project-specific notes here — current stack if different from default, known quirks, files to avoid, etc.]
