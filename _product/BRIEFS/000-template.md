# Brief [NUMBER] — [Feature Name]

Status: `draft` / `ready` / `in-progress` / `done`
Created: [date]
Backlog entry: [link or paste original idea]

---

## What and why

[What this feature does and why it matters to the user. 2-4 sentences.]

---

## User behavior

[Step-by-step: what the user does, what they see, what happens.]

1. User does X
2. App responds with Y
3. User sees Z

---

## Acceptance criteria

- [ ] [Specific, testable condition — if this is true, the feature is done]
- [ ] [Another condition]
- [ ] [Another condition]

---

## Edge cases and failure states

- [What happens if the input is empty?]
- [What happens if the network is offline?]
- [What happens if the user does something unexpected?]

---

## Files likely involved

[Best guess at which files, components, or areas of the codebase this touches.]

- `src/[component]` — [why]
- `src/[file]` — [why]

---

## Mobile / PWA considerations

- [Responsive behavior on small screens]
- [Offline or cached state behavior]
- [Installability implications if any]

---

## Out of scope

[Explicitly what this brief does NOT include. This prevents scope creep.]

- [Not doing X in this brief]
- [Not doing Y — separate brief]

---

## Open questions

[Anything unresolved that would materially change the implementation. Answer before handing to Cursor.]

- [ ] [Question]
- [ ] [Question]

---

## Agent instructions

Paste this into Cursor to start the build:

```
Read _product/BRIEFS/[NUMBER]-[slug].md and implement it.
Follow existing code patterns and file structure.
Keep the diff small and focused on this brief only.
Avoid unrelated refactors.
Ask before adding new dependencies.
After editing, run the narrowest useful build check.
```

---

## Review

After Codex review, paste summary here or link to _product/REVIEWS/[NUMBER]-[slug]-review.md

- Review status: `pending` / `complete`
- Issues found: [summary]
- Ship status: `ready to ship` / `needs fixes`
