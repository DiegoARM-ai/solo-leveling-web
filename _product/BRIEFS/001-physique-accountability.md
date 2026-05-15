# Brief 001 - Physique Accountability Prompts

Status: `done`
Created: 2026-05-15
Backlog entry: `_product/BACKLOG.md` - 2026-05-15 ux #ready

---

## What and why

Add a stronger Physique accountability surface to the Today screen so nutrition and movement stay visible before they drift. The current Today screen already makes deep work feel important with a top progress bar; MacroFactor logging needs a similar lightweight prompt because missed logs are causing body-fat and physique goals to slip. Also revisit the current "Lift session" task so days without gym access can still support the goal through a flexible "get a sweat" action.

This should help Diego recover consistency without turning the app into a full nutrition tracker. MacroFactor remains the source of truth for calories, macros, protein, expenditure, and body-weight trend. This app should track the upstream behavior: "Is MacroFactor current today?"

---

## User behavior

1. User opens the Today screen.
2. Near the existing deep work bar, user sees a Physique accountability bar or check-in card for MacroFactor freshness.
3. The card shows progress for the day with three flexible checkpoints: first food logged, midday check done, and final log complete.
4. User taps a checkpoint after updating MacroFactor.
5. The app records that checkpoint for today, updates progress, and visually reinforces completion.
6. If all expected checkpoints are complete, the card shows a completed/locked-in state.
7. On days when a gym lift is not possible, the user can still satisfy the movement intent through a daily sweat action.

---

## Acceptance criteria

- [ ] Today screen includes a compact, visually polished Physique accountability component near the existing deep work bar.
- [ ] The component tracks daily MacroFactor freshness checkpoints without trying to store calories, protein grams, macros, food items, or weight-loss calculations.
- [ ] The component persists completion state by date using existing localStorage patterns.
- [ ] The component makes incomplete MacroFactor logging obvious enough to catch attention without overwhelming the Today screen.
- [ ] Completed checkpoints can be undone or corrected the same day.
- [ ] The UI works cleanly on mobile viewports around 390px wide.
- [ ] Existing deep work bar, Today task list, completion rings, XP, streak, rank, and navigation continue to work.
- [ ] The current Physique workout task copy changes from `Lift session` to `Training / sweat session`.
- [ ] The movement task note makes the hierarchy clear: scheduled lift is the priority; a quick run, incline walk, circuit, or intentional sweat is the fallback when gym training is not possible.

---

## Edge cases and failure states

- If the user skips breakfast or combines meals, the UI should not shame or block the day; it should support the chosen logging cadence.
- If the user forgets to log until evening, they should be able to mark earlier checkpoints complete.
- If a checkpoint is tapped accidentally, the user should be able to undo it.
- If localStorage data is missing or malformed, the app should fall back to an empty daily MacroFactor freshness state.
- If the date changes, the component should reset for the new day while preserving previous dates.
- If MacroFactor itself was not used, this app should not fake macro data; it should only record that MacroFactor was updated.

---

## Files likely involved

- `index.html` - add MacroFactor freshness state helpers, Today-screen card rendering, event handlers, and any related CSS.
- `index.html` - adjust the Physique protocol task currently named `Lift session`.
- `_product/CONTEXT.md` - update if the final implementation adds new localStorage keys or changes the Physique module behavior.
- `_product/BRIEFS/001-physique-accountability.md` - Cursor should update only the Implementation notes/status after building.

---

## Mobile / PWA considerations

- The component should feel like the deep work bar: high-signal, tappable, compact, and visually motivating.
- It should not push the active time-block tasks too far down the Today screen.
- Tappable areas must be large enough for one-handed mobile use.
- Persisted state should work offline because the app is localStorage-first.
- If this ships, consider bumping `CACHE_VERSION` in `sw.js` so the updated app shell is reliably refreshed in the installed PWA.

---

## Out of scope

- Do not integrate with the MacroFactor API.
- Do not store detailed meal data, calories, macros, protein grams, food names, or body-fat calculations.
- Do not redesign the full Physique module.
- Do not add a backend, auth, notifications, or dependencies.
- Do not create a full meal planner.
- Do not add a protein counter or slider in v1. That is downstream of the logging behavior and can be revisited after logging consistency improves.
- Do not change training programming beyond the narrow task wording/behavior decision.

---

## Product recommendation

Use this app as the behavior trigger and MacroFactor as the data system. The most upstream fix is not tracking grams of protein inside this app; it is making sure MacroFactor gets updated before the day gets away.

V1 checkpoints:

- Morning: `First food logged`
- Afternoon: `Midday check done`
- Evening: `Final log complete`

This avoids requiring exactly three meals while still creating three daily checkpoints. The goal is not "eat three times" or "manually count protein here"; the goal is "keep MacroFactor current before the day gets away."

For movement, change `Lift session` to:

- `Training / sweat session`

The note should clarify: "Priority is scheduled lift. If gym is impossible, log a quick run, incline walk, circuit, or other intentional sweat. Do not use this to skip lifting when lifting is available."

---

## Implementation decisions

- Use the flexible checkpoint labels: `First food logged`, `Midday check done`, `Final log complete`.
- Keep MacroFactor freshness accountability-only in v1. Do not award extra XP for the card itself.
- Keep the existing `Hit protein target` task separate as the downstream nutrition outcome.
- Place the MacroFactor freshness card near the existing deep work bar, preferably above it so Physique drift is visible early without burying the task list.
- Change the workout task copy from `Lift session` to `Training / sweat session`.
- Keep the same XP value for now, but use the task note to make the hierarchy clear: scheduled lift is the priority; a quick run, incline walk, circuit, or other intentional sweat is an acceptable fallback when the gym is not possible.
- Do not add a protein counter, calorie tracker, macro tracker, or nutrition slider in this version.

---

## Open questions

- [ ] After v1 proves useful, is there any need for a protein counter, or is MacroFactor enough?

---

## Agent instructions

Paste this into Cursor to start the build:

```
Read _product/CONTEXT.md, _product/AGENTS.md, and _product/BRIEFS/001-physique-accountability.md.
Update the brief status from ready to in-progress before editing code.
Implement the brief in index.html following existing code patterns.
Keep the diff small and focused on the Today-screen Physique accountability component and the workout task copy/behavior.
Do not add dependencies.
Do not integrate with MacroFactor beyond user-confirmed freshness checkpoints.
Do not add protein grams, calorie tracking, macro tracking, or a nutrition slider in this version.
Persist any new state using the app's existing localStorage/date-key patterns.
After editing, run the narrowest useful local browser check or static smoke check available.
When finished, add short Implementation notes with files changed, checks run, and any deviations.
Update the brief status from in-progress to built/pending review.
Do not write the review and do not mark the brief done; leave review for Codex.
```

---

## Review

After Codex review, paste summary here or link to `_product/REVIEWS/001-physique-accountability-review.md`

- Review status: complete — `_product/REVIEWS/001-physique-accountability-review.md`
- Issues found: none blocking after re-review. The malformed `mfFresh` persistence issue was fixed and retested.
- Ship status: ready to ship

## Implementation notes

- **Files changed:** `index.html`, `sw.js` (CACHE_VERSION `sl-v60` → `sl-v61`), `_product/CONTEXT.md`
- **Built:** MacroFactor freshness card on Today (`mfBarHTML`) above deep work bar; three tappable checkpoints persisted in `sl_v1.mfFresh` by date; no XP on checkpoints. Task `f3` renamed to `Training / sweat session` with hierarchy note.
- **Checks run:** Node parse of embedded script (OK); string presence for helpers, task copy, and `mfBarHTML()` in `rHome`.
- **Deviations:** None.
- **Review fix:** Hardened `mfFresh` migration/runtime guards so malformed root state resets to `{}` before checkpoint reads or writes.
- **Fix checks run:** Embedded script parse (OK); targeted malformed `mfFresh` recovery smoke (OK).
