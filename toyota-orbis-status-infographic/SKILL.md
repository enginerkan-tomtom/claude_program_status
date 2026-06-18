---
name: toyota-orbis-status-infographic
description: Weekdays 7 AM CET — Toyota status. Compact one-page top (NO Milestone Alert) + 5-event timeline (RAG-colored filled round circles, display:block, on a connector bar anchored circle-center to circle-center above the labels, borderless heading) incl. 26 Jun release-notes deadline + Geo Scope fits first screen + recursive child/sub-task comment fetch + comments-aware completion % + baseline-end-date schedule blend.
---

## Toyota Sample Delivery — Daily Status Report

Re-fetch live Jira data (including comments on umbrellas AND children AND sub-tasks), regenerate the HTML dashboard with the SAME compact one-page format as the previous run, and prepare a Confluence sub-page **as a DRAFT** with @mentions. The user reviews the draft in Confluence and publishes it manually — DO NOT publish.

---

### CRITICAL CONSTRAINTS — DO NOT VIOLATE

1. **There is NO NDS.live delivery for Toyota.** Never mention NDS.live in any Toyota context.
2. **DO NOT publish the Confluence page.** Create it as a DRAFT only — `status: "draft"`. The user reviews and publishes manually.
3. **DO NOT change the HTML format, CSS, section order, KPI grid, color palette, or component structure unless the user explicitly asks for a redesign in chat.** The canonical format is whatever the most recent dated HTML in the outputs folder looks like — read it, mirror its `<style>` block and section structure verbatim, only swap in fresh data values. **LOCKED FORMAT (as of 12 Jun 2026):** the canonical dated HTML has **NO Milestone Alert section**; a **compact 5-event Delivery Timeline** (Apr 30 baseline · Jun 9 TL@J response passed · Jun 21 VS cut-off · **Jun 26 release-notes availability deadline** · Jul 3 delivery) rendered as **RAG-colored filled round circle markers** (the marker is a `display:block` span so width/height/RAG-fill actually apply) **sitting on a thin horizontal connector bar anchored from the first circle centre to the last (left:10%/right:10%) that runs above the milestone labels**, under a **borderless timeline heading** (no underline under the "Delivery Timeline" title); and a **compacted Geographical Scope** section that fits on the first screen. Preserve all of this on every run.
4. **COMPACT ONE-PAGE TOP SECTION.** The Header + KPI grid + Key Updates + Delivery Timeline + **Geographical Scope** MUST fit on a single ~900–1000px viewport without scrolling. Achieve this by:
   - body padding 12px; section padding 10px 14px; section margin-bottom 10px
   - header padding 14px 20px, h1 19px, sub 12px, date 11px
   - kpi padding 8px 11px, val 20px, note 10px, ws-pill 8.5px
   - Key Updates list **2-column grid** (`grid-template-columns:1fr 1fr; column-gap:18px`) with 10.5px font, 4px vertical padding, line-height 1.35
   - **Compact horizontal timeline (locked):** timeline section uses `class="timeline-sec"` with `section.timeline-sec{padding:8px 14px 4px;margin-bottom:6px}` and `section.timeline-sec h2{margin-bottom:2px;border-bottom:none;padding-bottom:0}` (heading has NO underline). `.htimeline{display:flex;justify-content:space-between;align-items:flex-start;position:relative;padding:6px 6px 0;margin-top:0}`. **Connector bar runs above the labels, anchored circle-centre to circle-centre, behind the circles:** `.htimeline::before{content:'';position:absolute;left:10%;right:10%;top:14px;height:3px;background:var(--border);border-radius:2px;z-index:0}`. Markers are **RAG-colored filled round circles overlaying the bar** — the marker span MUST be `display:block` (otherwise an inline span ignores width/height and the colour/shape won't render): `.hevent{position:relative;text-align:center;flex:1;z-index:1}` and `.hevent .hd{display:block;width:18px;height:18px;border-radius:50%;background:var(--blue);border:2px solid var(--card);box-shadow:0 0 0 1px var(--border);margin:0 auto 5px}` with color variants `.hevent.done .hd{background:var(--green)}` · `.hevent.next .hd{background:var(--amber)}` · `.hevent.crit .hd{background:var(--red)}` (default blue). `.when` 10.5px line-height 1.2 / `.what` 9.5px line-height 1.2 sit BELOW each circle. Per-event classes drive colour: Apr 30 = done(green); Jun 9 &amp; Jun 26 = next(amber); Jun 21 &amp; Jul 3 = crit(red).
   - **Compact Geographical Scope (locked):** geo section uses `class="geo-sec"` with `section.geo-sec{padding:8px 14px}`, `h2{margin-bottom:4px;padding-bottom:5px}`, `h3{margin:5px 0 3px}`, `.geo-chips{gap:5px;margin-top:4px}`, `.chip{padding:3px 9px;font-size:10.5px}`, `p{font-size:10.5px;line-height:1.35;margin-top:5px}` so it clears the fold.
   - There is **NO Milestone Alert panel** — do not reintroduce it.
   - This compactness is the **canonical format** — preserve it on every run.
5. **COMMENT FETCHING IS RECURSIVE AND MANDATORY.** For every workstream, you MUST fetch the latest comments on the umbrella AND every linked child ticket AND any sub-tasks of those children — across ALL dimensions (Feature Quality, CC Rule ID, CC Rule Dev, CC Run & Fix, Product Audit). PMs post weekly "W##" updates on individual feature tickets per the agreed WoW (one comment per CM per week); the umbrella usually does NOT contain these. **Skipping child-ticket comments is the single most common defect of this report. Do not ship a report without them.** Use bulk JQL fetch (`searchJiraIssuesUsingJql` with `fields: ["summary","status","assignee","comment","issuelinks","subtasks","duedate","customfield_10158"]` and `key in (...)`). Then resolve any `subtasks` and fetch their comments too. (Comment payloads exceed the inline token limit and are written to disk — parse the saved file with jq via the workspace bash sandbox; the /var/folders tool-result path maps to `/sessions/<session>/mnt/.claude/projects/.../tool-results/`.)
6. **COMPLETION % IS COMMENTS-AWARE, NOT COUNT-ONLY.** A bare Done/active count drastically understates progress because PMs report progress in W## comments long before a ticket flips to Done. For every active child ticket, score completion 0–100 from the latest PM comment, then average across the workstream's active children. Status-only fallback only when no comment exists.
   - Comment-aware scoring rubric:
     - Status = Done or comment explicitly says "Quality reached" / "completed" → **100**
     - Comment says "ready to close" or "0 violations / nothing to fix" or "~100% violations solved" → **95**
     - Comment says "ON TRACK" with concrete W## evidence (rules requested, RCA done, targeted updates, high % of violations solved) → **65–75**
     - Comment says "In Progress" / weekly W## update without explicit RAG (e.g. ~50% violations) → **40–55**
     - Comment flags "AT-RISK" / blocker / new dependency / "no clarity what to fix" → **25–35**
     - Status = Open and no W## comment → **0**
     - Status = Cancelled → exclude from denominator
   - Cite which comment drove each score in the workstream card description and in the Latest comment column of the Full Ticket Hierarchy.
   - Apply the same rubric to all 5 workstreams. Per-workstream completion % shown in KPI tiles and Workstream Summary cards must use this method, not the bare count.
7. **BLEND THE JIRA BASELINE END DATE TO IDENTIFY OFF-TRACK / DELAYED ITEMS — but never as the SOLE signal.** Every CM ticket carries a "Baseline end date" in `customfield_10158`. Fetch it for every umbrella and child, and combine it with the comment-aware status from Constraint 6 to flag schedule slippage. Baseline is one input, blended with comments/status — not the only determinant.
   - **DATA-FRESHNESS GUARDRAIL (mandatory):** ALWAYS use the baseline end date fetched in THIS run from `customfield_10158`. NEVER carry forward, infer, or reuse a baseline value from a previous report — baselines are frequently corrected between runs. Before flagging ANY item as off-track, re-confirm its CURRENT baseline from this run's fetch. If an item's baseline is within the window (on/before the VS cut-off) and its comment is on-track, it is NOT off-track — do not list it.
   - Compute, for each item, the relationship of its CURRENT baseline end date to **today**, the **VS cut-off (Jun 21, 2026 EOD)**, and **delivery (Jul 3, 2026)**:
     - Baseline end < today AND status not Done/Closed/Cancelled → **OVERDUE** (quote the gap, e.g. "~7 weeks overdue").
     - Baseline end > Jul 3 delivery date → **POST-DELIVERY** (at-risk for Jul-3).
     - Baseline end == Jul 3 → **ZERO BUFFER**.
     - Baseline end in the future but item still Open / not started with little runway → schedule-risk (state days remaining).
     - Baseline end on/before the cut-off AND comment on-track / status healthy → **on-time, NOT flagged.**
   - **Blend rules:** a recent on-track comment does NOT clear a genuinely OVERDUE (past, this-run) baseline — flag DELAYED and nudge the score down ~10; but if the baseline is NOT actually past, do not flag. An item baselined POST-DELIVERY is at-risk even if its comment is positive — flag it. An item Done/Closed before/around its baseline is on-time → no flag. Umbrella baselines that have passed → note "re-baseline to Jun 21" but do not double-count children.
   - Reflect the blended verdict in: (a) the workstream-card "⚠ Baseline" note, (b) the dedicated **"Schedule vs Baseline — Off-track Items"** section, (c) risk panel **R8 — Schedule slippage vs Jira baseline end dates**, and (d) a `Baseline end` column in the Full Ticket Hierarchy. List in the off-track table ONLY items genuinely OVERDUE / POST-DELIVERY / zero-buffer per THIS run's data; add an "on-time vs baseline (no flag)" line for the healthy ones.

---

### Step 1 — Load yesterday's report for comparison AND format reference

Read the most recent dated HTML file from any session's outputs folder. This file is BOTH the previous report (for delta comparison) AND the canonical format (mirror its structure verbatim — the compact CSS in Constraint 4, the **5-event compact timeline with RAG-colored filled round circles (display:block) on a connector bar anchored circle-centre to circle-centre above the labels, borderless heading, 26 Jun release-notes deadline, NO Milestone Alert**, the **compact geo-sec**, and the Schedule-vs-Baseline section + R8). Use it ONLY for format and delta comparison — NEVER copy its baseline dates or scores forward; every data value (especially baseline end dates) must come from this run's fresh Jira fetch:
```python
import glob
files = sorted(glob.glob("/sessions/*/mnt/outputs/toyota_orbis_status_*.html"))
yesterday_file = files[-1] if files else None
```
Parse it to extract per-workstream Done/IP/Open/Cancelled counts, comments-aware %, baseline/off-track flags, CC Rule Dev QADEV breakdown numbers, RAG per dimension, per-workstream RAG colors and percentages, open risk count. Use as baseline for Key Updates deltas only.

---

### Step 2 — Fetch fresh Jira data WITH comments AND baseline end dates (umbrellas + children + sub-tasks)

Use `mcp__08f7421b-ce66-4e47-b03b-40a1fb19413c__getJiraIssue` and/or `searchJiraIssuesUsingJql` with cloudId `1cca41ed-de92-4455-812e-a4a463fc61a9`. Fields: `["summary","status","assignee","comment","description","issuelinks","subtasks","duedate","customfield_10158"]`. `customfield_10158` = "Baseline end date". Always pull fresh. NOTE: `getJiraIssue` with `comment`+`description` can time out — prefer fetching umbrella structure via `linkedIssues("<umbrella>")` JQL with light fields `["summary","status","assignee","customfield_10158"]`, then a separate `key in (...)` JQL that includes `comment` for the active children (parse the disk-saved payload with jq).

**Pass A — Umbrellas (light fields, no comment):**
- **Master:** CM-14773 (carries the **release-notes availability deadline = 26 Jun 2026** plus Jul 3 delivery / Jun 21 cut-off in its description — re-read each run; PMs also post overall-status comments here, e.g. "CC rule identification complete except TL@Junction")
- **Feature Quality umbrella:** CM-16120
- **CC parent (groups the 3 CC umbrellas):** CM-16187
- **CC Rule ID umbrella:** CM-16186
- **CC Rule Development umbrella:** CM-15370 (+ CM-14649, CM-15072, CM-15012, CM-14957)
- **CC Run & Fix umbrella:** CM-15373
- **Product Audit umbrella:** CM-15375
- **Risk tickets:** CM-16117, CM-16116

From each umbrella's links collect every child key.

**Pass B — Children + sub-tasks (bulk JQL WITH comment):** `key in (child1, child2, ...)` with the full fields list (written to disk; parse with jq). Resolve `subtasks` and fetch their comments too. If a comment contradicts the formal status, trust the comment.

---

### Step 3 — Parse comments + baselines and compute deltas

- Sort comments by `created` descending; find weekly "W##" updates.
- For every active child, score 0–100 per Constraint 6 rubric — record the score and a 1-sentence reason citing the comment.
- For every umbrella and child, read `customfield_10158` FROM THIS RUN and derive its Constraint 7 verdict; blend with the comment-aware score.
- Compute deltas vs yesterday's report: per-workstream % delta; per-feature score deltas (e.g. RoW 45→30 because Seema raised a risk; Oneway 50→95 at ~99.8% solved); new tickets moved to Done/IP/Cancelled/On Hold/Planned; new W## comments; newly off-track / newly-cleared vs baseline (incl. corrected baselines).
- Flag any active child whose latest comment is >7 days old and a W## update is overdue — surface in the cadence-gap risk panel.

---

### Step 4 — Compute metrics

- Days to VS cut-off (Jun 21, 2026 EOD) and Jul 3 from today's date.
- Per-workstream comments-aware completion % (Constraint 6), blended with baseline (Constraint 7).
- Overall completion = simple mean of the 5 workstream comments-aware %.
- RAG per dimension informed by status AND comments AND baseline.
- Off-track/delayed list per Constraint 7 (genuinely off-track items only).

---

### Step 5 — Generate HTML dashboard — PRESERVE LOCKED COMPACT FORMAT

Read yesterday's HTML and replicate its structure exactly. Update only data values.

**Sections in order (do NOT reorder, do NOT add/remove — there is NO Milestone Alert):**
1. **Header** with the Overall RAG badge top-right (14px 20px padding, 19px h1)
2. **KPI grid** — 2 rows on a 20-column grid (8px 11px padding, 20px val): Row 1 (4×5 cols) Days to VS cut-off · Days to delivery · Overall completion · Open risks; Row 2 (5×4 cols) Feature Quality · CC Rule ID · CC Rule Dev (QADEVs) · CC Run & Fix · Product Audit — each comments-aware % + tight breakdown
3. **Key Updates — Since Previous Report** — 2-column grid, color-coded dot + status pill, terse (12–14 items), incl. baseline-driven deltas and corrections
4. **Delivery Timeline** — `class="timeline-sec"`, RAG-colored filled round circle markers (display:block) on a thin connector bar anchored circle-centre to circle-centre above the labels, borderless heading (locked CSS in Constraint 4). **5 events:** Apr 30 baseline · Jun 9 TL@J response (passed) · Jun 21 EOD VS cut-off · **Jun 26 Release-notes availability deadline (per CM-14773)** · Jul 3 delivery.
5. **Geographical Scope** — `class="geo-sec"` two-column section, compacted (must clear the first viewport)
6. **Workstream Summary** — 5 cards in 1 row; each cites per-feature comment-aware scores + a "⚠ Baseline" note for genuinely overdue/post-delivery items
7. **RAG by Dimension** — 6-column table including Product Audit
8. **Schedule vs Baseline — Off-track Items (blended)** — table of ONLY genuinely-flagged items: Item · Workstream · Baseline end · Status · Comment-aware signal · Blended verdict · **Action owner(s) — please act** (@mentions). One-line note that baseline is blended with comments; closing "On-time vs baseline (no flag)" line. Tag owner(s) on every off-track row.
9. **Risks & Blockers** — R1–R8, where **R8 = Schedule slippage vs Jira baseline end dates** (High)
10. **Full Ticket Hierarchy** — `Baseline end` column (this run) + `Latest comment` column from Pass B; mark only genuinely overdue/post-delivery baseline cells red
11. **Footer** — `Auto-generated from Jira on {today} (W##) · comments-aware, blended with Jira baseline end dates · TomTom Maps · Source: CM-14773`

Sections 1–5 (Header through Geographical Scope) must fit in one ~900–1000px viewport. Sections 6+ continue below the fold.

Save to current session's outputs folder: `toyota_orbis_status.html` (overwrite) and `toyota_orbis_status_{YYYY-MM-DD}.html` (dated copy).

TomTom CSS variables:
```css
:root {
  --navy: #0d1f35; --navy2: #132944; --blue: #1a73e8; --teal: #0097a7;
  --red: #d32f2f; --amber: #f57c00; --green: #2e7d32;
  --grey: #546e7a; --bg: #f0f4f8; --card: #ffffff;
  --border: #dde3ea; --text: #1a2a3a; --subtext: #607080;
}
```

---

### Step 6 — Ownership & RAG rules

#### Feature Quality (CM-16120)
- **PM owners:** Tomasz Ciniewski, Kateryna Yushko, Asena Sahin, Raquel Rosa, Mick van der Spoel, Akshay Borawake, Delyan Peyankov, Abhishek Srivastava, Abdeljalil Karam

#### CC Rule ID (CM-16186) — Owner: MapEx team · E2E CC Oversight: Yannis Koutavas
#### CC Rule Dev (CM-15370) — Owner: Hans's team (Hans Verheyden) · E2E CC Oversight: Yannis Koutavas
#### CC Run & Fix (CM-15373) — Owner: Seema Nayyar / MSO team · E2E CC Oversight: Yannis Koutavas
- CC Run is **partially active** — existing rules run, violations can be fixed. Do NOT describe as fully blocked.

#### Product Audit (CM-15375 umbrella, 16 child audits)
- **Owners:** Seema Nayyar + Piotr Klusek (**Olga Simakova must NOT appear as owner**)
- CM-16075 is one of 16 children — never call it the umbrella

#### Yannis Koutavas — E2E CC Oversight across all 3 CC dimensions. Never primary action owner. Label as "E2E CC Oversight: Yannis Koutavas" in all three CC workstream rows.

**Off-track owner mapping (tag the responsible owner per item, only when genuinely off-track this run):** Toll POIs → Delyan Peyankov + Liesbeth De Groote; Lanes → Abdeljalil Karam + Seema Nayyar; Right of Way → Raquel Rosa / Seema Nayyar; CC Rule-Dev rules (CM-14649 / CM-15012) → Hans Verheyden + oversight Yannis Koutavas; Rule ID / Run & Fix dimension tickets → MapEx / Seema Nayyar + oversight Yannis Koutavas (+ the feature PM, e.g. Mick van der Spoel for TS/TL@J); open Feature-Quality features → their PM owner; umbrellas (re-baseline) → Yannis Koutavas + Seema Nayyar.

---

### Step 7 — Prepare Confluence DRAFT (do NOT publish)

If a Confluence page already exists for the same date, use `updateConfluencePage` (not create) so we don't accumulate drafts; if the only existing draft is from a prior date and was never published, update it in place and retitle to today's date. Otherwise use `createConfluencePage` with `status: "draft"`. (Large create/update responses are written to disk; parse the saved file with jq for the page id/links — the call still succeeded.) Current working draft page id: **2066580868**.

Parameters: **cloudId** `1cca41ed-de92-4455-812e-a4a463fc61a9` · **spaceId** `130547712` · **parentId** `1990722423` · **title** `Toyota Sample Delivery — Status Report — {DD Mon YYYY}` · **contentFormat** `html` · **status** `draft` (CRITICAL: prevents publication).

**Confluence body** mirrors the HTML dashboard sections (including Schedule vs Baseline with the Action-owner @mention column, and R8), using Confluence syntax: `<span data-type="status" data-color="red|yellow|green|neutral|purple">` for RAG/status cells and `<span data-type="mention" data-user-id="ACCOUNT_ID">@Name</span>` for owners. Latest-comment column must reflect actual child-ticket comments from Pass B, with a `Baseline end` column. Workstream Summary % must be the comments-aware figure with the score breakdown inline. (The Confluence body has no Delivery Timeline / Milestone Alert section — those are HTML-dashboard-only.)

**Belt-and-braces backup** — save the prepared Confluence body HTML to `/sessions/{current-session}/mnt/outputs/toyota_confluence_draft_{YYYY-MM-DD}.html`.

#### Account IDs (hardcoded):
| Name | Account ID |
|---|---|
| Tomasz Ciniewski | `712020:8def95d3-8500-4794-ae5a-3f60c00b252a` |
| Kateryna Yushko | `712020:838ef6a8-8dff-4748-8640-1ebdf6ba0b61` |
| Asena Sahin | `712020:e1868773-21e3-4104-94fd-4c5658d2c991` |
| Raquel Rosa | `712020:ec6c42b1-0ac0-4ede-af16-8057f732b59d` |
| Mick van der Spoel | `712020:75d5e26c-bffd-4156-b634-9dcf44a49c03` |
| Akshay Borawake | `712020:92875e84-367e-4194-b20c-e2f0cacb4980` |
| Delyan Peyankov | `712020:8562dc00-cea4-4840-b51a-e4b5feea8a47` |
| Abhishek Srivastava | `712020:8b0a3454-4446-4056-a5aa-d9437bb49bff` |
| Abdeljalil Karam | `61e68403fd5a690068bc0fe6` |
| Yannis Koutavas | `712020:1c943532-ce29-4617-8ffb-01a452db6861` |
| Seema Nayyar | `70121:4db8d304-e6f6-498e-b359-fd7263c16fd0` |
| Piotr Klusek | `712020:c76827ea-e749-4de2-923a-02d462764154` |
| Hans Verheyden | `6232f36c6a68240069715afa` |
| Liesbeth De Groote | `712020:8c880aab-eb8f-46f7-b79c-241698df0550` |

---

### Step 8 — Confirm completion

Report back with:
- ✅ HTML dashboard saved (computer:// link) — locked compact top format preserved (NO Milestone Alert; 5-event timeline with RAG-colored filled round circles on a connector bar anchored circle-centre to circle-centre above the labels, borderless heading, 26 Jun release notes; compact Geo Scope on first screen)
- ✅ Confluence DRAFT created/updated (URL — note it is a draft, NOT published)
- ✅ Confluence body backup saved (computer:// link)
- ✅ @mentions applied (incl. every off-track row in Schedule vs Baseline)
- ✅ Comments read and incorporated — explicitly confirm child-ticket and sub-task comments were fetched (Pass B), not just umbrellas
- ✅ Per-workstream % computed comments-aware (Constraint 6), not count-only
- ✅ Baseline end dates (customfield_10158) fetched THIS RUN and blended per Constraint 7 — list items flagged OVERDUE / POST-DELIVERY / zero-buffer; confirm no within-window+on-track item was wrongly flagged
- 📝 Manual step: user reviews the Confluence draft and clicks Publish when ready
- Top headline: Overall RAG · days to cut-off · biggest red workstream · open risks count · key new delta since yesterday · biggest schedule slip vs baseline