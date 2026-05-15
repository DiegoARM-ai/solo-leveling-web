# Project Context

Last updated: 2026-05-15

## Project summary

Solo Leveling Web is a personal optimization PWA for Diego. It turns daily routines, weekly practices, and long-term goals into a gamified "THE SYSTEM" interface with XP, levels, rank, streaks, and module-specific protocols.

This is not a generic habit tracker. The app is highly personal, copy-heavy, and intentionally themed around a Solo Leveling / Shadow Monarch system. Preserve the voice, module names, and tight mobile-first experience unless a brief explicitly asks to change them.

## Product purpose

The app helps Diego execute and track a five-stat life protocol:

- Physique / Adonis: grooming, skincare, supplementation, weight logging, training, nutrition, hair styling, and body composition goals.
- Vitality / Somnus: sleep protocol, ramp targets, evening wind-down, sleep debt warnings, and a bridge to the separate Somnus tracker.
- Connection: combined stat made from Apollo and Hestia.
- Apollo: romantic pursuit, dating pipeline habits, date planning, field strategy, and weekly social/dating XP.
- Hestia: friendship and belonging, embodied social hours, friend list, maintenance moves, deepening conversations.
- Craft / Prometheus: deep work, RevOps mastery, AI and automation, builder portfolio, consultancy path.
- Purpose / Seneca: morning ignition, meditation, reading, evening close, weekly review, quarterly capacity practices.

The home screen is the daily command center. The modules screen shows stat cards, levels, rings, pending tasks, and bottleneck warnings. Settings handles schedule, sleep metric overrides, Apollo Notion URL, level management, export, and reset.

## Current architecture

This is a static single-page app.

- Main app: `index.html`
- PWA manifest: `manifest.json`
- Service worker: `sw.js`
- Product docs and workflow: `_product/`

There is no package manager, build step, framework, bundler, or test runner in the repo today. The entire app lives in one HTML file with embedded CSS and JavaScript.

## Stack

- HTML, CSS, and vanilla JavaScript
- PWA support through `manifest.json` and `sw.js`
- Local persistence through `localStorage`
- External font: Google Fonts Rajdhani
- External weather fetch in Apollo date planner: Open-Meteo API
- External links to Somnus, Notion, Eventbrite, Secret NYC, Time Out, Fever, and other resources

Default template assumptions like Next.js, Tailwind, Supabase, TypeScript, and Vercel do not apply unless the project is intentionally migrated later.

## Runtime and deployment

The app is designed for GitHub Pages under:

- App scope/start URL: `/solo-leveling-web/`
- Somnus tracker link: `https://diegoarm-ai.github.io/sleep-tracker-web/`

Because it is a static app, a local browser can open `index.html` directly for basic checks. Use a small local static server when validating service worker, PWA scope, fetch behavior, or mobile browser behavior.

## Important files

- `index.html`: all UI, style, routing, state, protocol definitions, module renderers, and event handlers.
- `manifest.json`: PWA metadata, scope, start URL, theme colors, and inline SVG icon.
- `sw.js`: cache-first service worker with network refresh fallback. Cache version must be bumped when shipping changes that should reliably invalidate old app shells.
- `_product/README.md`: product workflow for backlog, briefs, builds, reviews, and shipping.
- `_product/AGENTS.md`: agent workflow rules, including brief lifecycle and review ownership.
- `_product/BACKLOG.md`: raw inbox for ideas, bugs, UX notes, debt, and experiments.
- `_product/BRIEFS/000-template.md`: implementation brief template.
- `_product/REVIEWS/000-template-review.md`: review template.
- `cursorrules-template.md`: generic project rules template. This project still needs a filled `.cursorrules` or equivalent agent instruction file if desired.

## State model

Primary app state is stored in `localStorage` under:

- `sl_v1`: main app state object
- `sl_sessions`: Craft/deep work session log
- `sl_commitments`: Seneca morning/evening commitment log
- `somnus_state_v1`: separate Somnus tracker state, read and partly written by this app when both apps share the same GitHub Pages origin

Main state is created by `defaultState()` and upgraded by `migrateState()`. Current schema fields include:

- `schemaVersion`
- `cfg`
- `streaks`
- `log`
- `mfFresh` — MacroFactor freshness checkpoints per day (`dateKey → [bool, bool, bool]` for first food, midday, final log)
- `sleep`
- `weight`
- `preDate`
- `sessionLog`
- `hestia`
- `senecaCapacity`
- `xp`
- `levels`

Be careful with state changes. Any new persisted shape should be added to `defaultState()` and protected in `migrateState()` so existing users do not lose data.

## Core app flow

The app creates screen containers during `init()`:

- `home`
- `modules`
- `physique`
- `vitality`
- `presence`
- `apollo`
- `hestia`
- `connection`
- `craft`
- `purpose`
- `settings`

Navigation is handled by `go(s)`, which toggles `.screen.active`, updates the bottom nav, and calls `render(s)`.

Rendering is mostly direct `innerHTML` from functions like:

- `rHome()`
- `rModules()`
- `rPhysique()`
- `rVitality()`
- `rPresence()` for Apollo
- `rConnection()`
- `rHestia()`
- `rCraft()`
- `rPurpose()`
- `rSettings()`

Shared rendering helpers include `proto()`, `ring()`, `lvBar()`, `bottleneckBanner()`, `sleepFocusBanner()`, `renderTrainingCard()`, and `toggleRef()`.

## Protocol and XP system

The protocol definitions live near the top of the script in `const P`.

Task objects generally include:

- `id`
- `xp`
- `tod`
- `mod`
- `name`
- `note`
- optional `requiresInput`

Task lookup maps are generated into:

- `TASK_MOD`
- `TASK_XP`
- `TASK_INPUT`

Daily completion is stored by date key in `S.log`. Toggling a task updates completion, weekly XP, streaks, localStorage, and sometimes the current UI.

Weekly XP uses `weekKey()`, `initXP()`, `getWeeklyXP()`, and `addXP()`. Level gates are in `XP_TARGETS`. Level-up requires both the XP gate and a real-world outcome gate, but the app currently confirms this through prompts rather than enforcing outcome data.

## Rank and bottlenecks

Rank is calculated from five effective stats:

- `physique`
- `vitality`
- `connection`
- `craft`
- `purpose`

Connection is special. It is derived from Apollo and Hestia using `connectionLevel()`. During the Hestia grace period, Connection equals Apollo. After the grace period, Connection is the lower of Apollo and Hestia.

`calcRank()` returns E, D, C, B, A, or S based on level thresholds. `getBottleneck()` and `bottleneckBanner()` identify lagging stats and surface them on the home/modules UI.

## Somnus integration

Vitality reads live sleep state from `somnus_state_v1`, which is created by the separate Somnus tracker. Shared-origin localStorage on GitHub Pages is a core assumption.

Important bridge helpers include:

- `getSS()`
- `writeSS()`
- `ssFinalTarget()`
- `ssIsEmergency()`
- `ssRampTarget()`
- `ss7dayAdherence()`
- `ss7dayValves()`
- `ssDebtAvg7()`
- `ssEffAvg7()`
- `ssConsistPct7()`
- `patchSomnusTimes()`

The app writes last-night protocol status back into Somnus via `logNight(status)`. Avoid changing this bridge without checking compatibility with the separate sleep tracker.

## Module notes

### Home

Home is the daily dashboard. It shows date/time, block, overall completion ring, streak, rank, awakening state, sleep warning, bottleneck warning, MacroFactor freshness bar, deep work bar, and time-grouped tasks.

The greeting currently hardcodes Diego's name.

### Physique / Adonis

Includes morning, afternoon, PM, and weekly dermaroll protocols; current cut stats; daily weight logging; a training card with day-specific lifting/run/rest plans; and a hair styling reference.

Dermaroll day replaces the normal PM routine and warns to skip tretinoin.

### Vitality / Somnus

Includes morning sleep anchors, evening wind-down, a live Somnus summary, emergency sleep mode, 28-day adherence heatmap, and links to the full Somnus tracker.

The wind-down schedule intentionally mixes fixed Tier 1 cues with dynamic in-bed target calculations.

### Connection

Connection is a landing page for Apollo and Hestia. It explains that one stat is backed by two modules and links into each branch.

### Apollo

Apollo has tabs for Today, Plan, Log, and Ref.

- Today: daily dating pipeline protocol and Notion pipeline link from settings.
- Plan: weather-aware NYC date planner using Open-Meteo and hardcoded itinerary data.
- Log: weekly field XP for dance class, run club, spike events, approaches, numbers, and dates.
- Ref: principles, target profile, pre-date checklist, signal vocabulary, texting rules, and field strategy.

Weather fetch can fail silently, so UI should remain usable without forecast data.

### Hestia

Hestia has Today, Friends, and Log tabs.

- Today: daily protocol, weekly embodied social hours, maintenance move, and unoptimized relationship prompt.
- Friends: lightweight friend list with last contact dates.
- Log: quarterly deepening conversations and maintenance history.

This module intentionally avoids becoming a full CRM.

### Craft / Prometheus

Craft handles deep work, learning, build logs, weekly actions, pillar breakdown, and session history.

Deep work sessions are stored separately in `sl_sessions`. First and second sprints are 90 minutes; third and later sprints are 45 minutes. XP is awarded through custom Craft logging functions rather than ordinary checkbox toggles.

The North Star is location-independent RevOps plus AI consultancy.

### Purpose / Seneca

Purpose handles morning ignition, coherence breathing plus meditation, intentional reading, evening close, commitments, weekly review, reading list, evidence base, and capacity track.

Commitments are stored in `sl_commitments` and shown on the home screen as today's commitment. The capacity track logs hard conversations, therapy/introspection, and selection reviews.

### Settings

Settings edits wake time, lights-out target, dermaroll day, Somnus override metrics, Apollo Notion URL, level management, data export, and destructive reset.

Reset clears only `sl_v1`; separate stores like `sl_sessions`, `sl_commitments`, and `somnus_state_v1` are not cleared by the current reset flow.

## UX and design conventions

- Mobile-first, app-like, portrait PWA.
- Dark "System" visual language with purple/void base and stat-specific accents.
- Bottom nav has Daily, Stats, and System.
- Text is intentionally intense, directive, and thematic.
- Cards, rings, progress bars, glowing states, and collapsible reference sections are core visual patterns.
- UI is optimized for a narrow mobile viewport around 430px.

When changing UI, verify small-screen behavior. Avoid desktop-only assumptions.

## Engineering conventions

- Keep diffs small and localized.
- Prefer editing existing functions and patterns over adding new abstractions.
- Do not add dependencies without asking first.
- Do not introduce a framework or build step unless a brief explicitly calls for a migration.
- Keep state migrations backward-compatible.
- When adding a task, update `P`; the lookup maps derive from it.
- When adding persistent app state, update both `defaultState()` and `migrateState()`.
- When changing app shell assets or service worker behavior, consider bumping `CACHE_VERSION` in `sw.js`.
- Use careful manual testing because there is no automated test suite.

## Known quirks and risks

- `index.html` is very large and mixes CSS, data, state, rendering, and handlers in one file.
- Some route names are legacy, especially `presence` versus `apollo`.
- Craft XP targets are currently zeroed in `XP_TARGETS`, so Craft level-up behavior differs from other modules.
- `dwBarHTML()` labels sprint 2 as `+25` while `_dwSprintXP(2)` awards 30 XP. Confirm desired value before changing either.
- Reset only clears `sl_v1`, not all app-related localStorage keys.
- Service worker cache version is manual.
- PWA behavior and localStorage sharing can differ between direct file opens, localhost, and GitHub Pages.
- Apollo date planner depends on a network call to Open-Meteo.
- There is personal and sensitive content in copy, logs, dating strategy, health routines, and goals. Treat the repo as private by default.

## Product workflow

Use `_product/` for product thinking:

1. Capture raw ideas in `_product/BACKLOG.md`.
2. Promote ready work into `_product/BRIEFS/[number]-[slug].md`.
3. Implement against the brief.
4. Review against acceptance criteria and write `_product/REVIEWS/[number]-[slug]-review.md`.
5. Mark the brief done when ready to ship.

Follow `_product/AGENTS.md` for the exact brief lifecycle. Cursor implements and updates Implementation notes; Codex reviews and updates the Review section/status.

## Suggested manual checks

For most UI changes:

- Open `index.html` or serve the directory locally.
- Check home, modules, changed module page, and settings.
- Test a mobile-sized viewport around 390px by 844px.
- Toggle at least one task and confirm XP/completion state updates.
- Reload and confirm state persists.

For PWA/service worker changes:

- Serve from the expected `/solo-leveling-web/` path if possible.
- Check installability/scope.
- Confirm cache invalidation by bumping `CACHE_VERSION`.

For Somnus changes:

- Test with no `somnus_state_v1`.
- Test with a representative `somnus_state_v1`.
- Confirm the external Somnus tracker can still own its state.

## Open questions for Diego

- Should the canonical context file live only at `_product/CONTEXT.md`, or do you also want a full root-level `context.md`?
- Do you want a filled `.cursorrules` created from `cursorrules-template.md` for this project?
- Where is this deployed today: GitHub Pages only, or also another host?
- Is the Somnus shared-localStorage bridge still the intended long-term integration, or should this eventually move to a shared backend?
- Should `Reset All Data` clear only main progress, as it does today, or all related stores including sessions, commitments, and Somnus data?
- Should Craft sprint 2 display `+25` or award `+30`? The UI and logic currently disagree.
- Are the current Q3 goals, personal stats, and module copy meant to stay hardcoded in the app, or should they eventually become editable content?
