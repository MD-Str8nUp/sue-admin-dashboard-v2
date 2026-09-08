# Fable Review — Sue's Admin Dashboard vs Reference Mockup

Scope: visual/layout review only. No edits to `index.html`, `styles.css`, or `app.js` were made. All suggestions must preserve phase 1 safety — static, browser-local, no client data, no live integrations.

## Reference summary

A single laptop card showing a solid navy header ("Sue's Admin Dashboard" centered, subtitle "Personal planning — approved integrations only") over a 2×2 body grid:

- Left column: **Today** (timeline rail with blue dots) above **Quick capture**.
- Middle column: **Action queue** (checkbox rows with pill status badges) full-height.
- Right column: **Upcoming deadlines** (calendar icons + right-aligned dates) above **Approved sources** (green check rows).
- Thin footer strip: "Email agent identifies approved admin reminders; it does not store clinical content."

## Three biggest mismatches

1. **Header composition.** Reference is a centered title + one-line subtitle on a flat navy bar with no controls. Current header carries a kicker ("Phase 1 personal admin"), left-aligned title, a "Today" pill on the right, and two action buttons (Load demo / Empty reset). This makes the current top feel like an admin console; the reference feels like a personal planner.

2. **Body grid shape.** Reference is a compact 2×2 (Today + Quick capture stacked left; Deadlines + Approved sources stacked right; Action queue spanning full column height in the middle). Current is three stacked strips: `notice → glance stats → full-width Quick Capture → three-column workspace → two-column secondary`. Quick Capture is a full-width row instead of tucked under Today, and there is no right-column stack pairing.

3. **Item styling in Today and Action queue.** Reference "Today" is a vertical timeline rail with blue dot markers and time+label pairs; "Action queue" rows have a leading checkbox and a right-aligned pill badge (To review / Pending / To do). Current lists render as generic bordered rows with no rail, no dot markers, and no status pills — the visual hierarchy of "when" vs "what next" is lost.

## Five precise, implementable UI changes

Each change is phase-1 safe: presentational only, no data leaves the browser, no integrations wired.

1. **Simplify the header to match the reference band.**
   In `.topbar__inner`, centre-align the brand block, drop the kicker, drop `.topbar__today`, and move `#load-demo` / `#reset-empty` into an overflow menu (e.g. a `<details>` labelled "Dashboard tools") rendered below the header on the left of the notice panel. Add a subtitle element under `.topbar__title` reading "Personal planning — approved integrations only". Keep the current navy gradient.

2. **Reflow the workspace into the 2×2 grid.**
   Replace `.workspace { grid-template-columns: repeat(3, 1fr) }` with a two-column layout at ≥900px: left column stacks `#busy-list` panel over `.panel--capture`; right column stacks the deadlines panel over a new Approved-sources panel; centre column holds the Action Queue and spans both rows (`grid-row: 1 / span 2`). Move `.panel--capture` inside `.workspace` (currently a sibling above it). Remove the standalone `Quick Capture` heading row above the workspace.

3. **Add a timeline rail to Today Workload.**
   In `#busy-list li`, prepend a decorative dot (`::before` circle) and draw a 2px vertical rail via a `::after` on the `<ul>` positioned behind the dots. Time (`item__meta`) sits above the label (`item__title`) in a compact two-line stack. No data changes — pure CSS on the existing `<ul id="busy-list">` items. Rail colour: `--navy-soft`; dot: solid `--navy`.

4. **Add pill status badges to Action Queue rows.**
   Style `#task-list .item` to render the existing `Open / Waiting / Done` status as a small right-aligned rounded pill (`padding: 2px 10px; border-radius: 999px; font-size: 0.72rem`) with three tinted variants: Open→neutral grey, Waiting→amber tint (reuse `--warn-tint`/`--warn-line`), Done→green tint (reuse `--success`). Also render a leading unchecked square glyph via `::before` on non-Done rows to match the reference's checkbox affordance. Behaviour unchanged — clicking still routes through the existing Done/Delete mini-buttons.

5. **Add a static "Approved sources" panel — presentational only.**
   New `<section class="panel panel--sources">` under the deadlines panel, listing three read-only rows: "Google Calendar", "Practice calendar", "Work email". Each row shows a green check glyph and a plain-text status suffix such as "not connected — phase 1" (do **not** copy the reference's "connected / busy-only / reminder scan only" wording — that would misrepresent phase 1 boundaries). Include a one-line caption: "Integrations shown for planning only; nothing is connected in phase 1." This satisfies the reference layout without breaching the Privacy Check ("No calendar, inbox or practice system connection").

## Notes on things intentionally kept

- The orange notice panel, "At a glance" stats, Privacy Check, and Information for Xena panels are not in the reference but are load-bearing for phase 1 safety and Xena handoff. Recommend keeping them below the reworked 2×2 workspace rather than removing them.
- The reference footer strip mentions an "email agent". Phase 1 has no email agent, so do not reproduce that line verbatim. If a footer strip is desired, reuse existing privacy copy instead.
