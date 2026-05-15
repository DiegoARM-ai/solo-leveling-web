# Review — Brief [NUMBER] — [Feature Name]

Reviewed: [date]
Brief: _product/BRIEFS/[NUMBER]-[slug].md
Reviewer: Codex / Claude Code

---

## Acceptance criteria check

| Criteria | Status | Notes |
|---|---|---|
| [paste from brief] | ✅ met / ❌ not met / ⚠️ partial | [notes] |
| [paste from brief] | ✅ met / ❌ not met / ⚠️ partial | [notes] |

---

## Bugs found

[List any bugs discovered during review. Include file and line reference where possible.]

- [ ] [Bug description] — `src/[file]:[line]`

---

## Quality issues

[Code quality, maintainability, patterns that diverge from the codebase.]

- [ ] [Issue]

---

## PWA / mobile issues

[Offline behavior, installability, responsive problems.]

- [ ] [Issue]

---

## Security / reliability flags

[Anything that could break in production or expose a vulnerability.]

- [ ] [Issue]

---

## Risk remaining

[What wasn't fixed in this review and why. What to watch after deploy.]

---

## Verdict

- [ ] Ready to ship
- [ ] Needs fixes before shipping — see issues above
- [ ] Needs significant rework — do not ship

---

## Codex prompt used

```
Read _product/BRIEFS/[NUMBER]-[slug].md for context.
Review the implementation against the acceptance criteria.
Write your findings to _product/REVIEWS/[NUMBER]-[slug]-review.md.
Include: criteria met, bugs found, risks remaining, suggested fixes.
```
