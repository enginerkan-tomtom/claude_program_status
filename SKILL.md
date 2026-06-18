---
name: cariad-quality-boost-daily-status
description: Generate the Cariad Quality Boost status dashboard each weekday 08:00 Amsterdam; prepare Confluence draft for manual review (not auto-published).
---

---
name: cariad-quality-boost-daily-status
description: Generate the Cariad Test Drives Quality Boost status dashboard each weekday at 08:00 Amsterdam and prepare it as a Confluence DRAFT for Engin to review and publish manually.
---

Generate the daily status dashboard for the **Cariad Test Drives Quality Boosting** program (master ticket CM-15489) and **prepare it as a Confluence draft sub-page for manual review** — Engin (engin.erkan@tomtom.com) wants to review the content and publish it himself.

# Why this task exists
Engin wants a fresh status report DRAFTED every weekday morning until the W23.4 final delivery to Cariad on Thu 4 Jun 2026. Each run produces (a) an HTML dashboard saved to the outputs folder, (b) a dated Confluence **draft** sub-page under the parent page "W23 S-Map Status Updates" in space AMF (NOT auto-published — Engin reviews and publishes manually), and (c) the dated HTML attached to that draft.

# Atlassian configuration (verified)
- **cloudId:** `1cca41ed-de92-4455-812e-a4a463fc61a9`
- **Confluence space:** AMF (Maps Guidance) · **spaceId:** `130547712`
- **Parent page:** "W23 S-Map Status Updates" · **parentId:** `2001011532`
- **Atlassian base:** `https://tomtom.atlassian.net/wiki`

# 🔒 LOCKED HTML FORMAT — STRICT, NO DEVIATIONS

**The dashboard HTML format is LOCKED to the 28 May 2026 reference + 29 May refinements (compact alert-strip + Progress by Priority Area section).** Every future weekday run MUST mirror it precisely.

**Canonical reference file (read first, every run, no exceptions):**
- `/sessions/<latest-session>/mnt/outputs/cariad_quality_boost_status_LOCKED_FORMAT_REFERENCE.html`

That file IS the spec — copy its `<style>` block verbatim (including the compact-spacing overrides and the compact alert-strip block at the bottom), replicate every section in the exact order below, update only the data.

## ⚡ Above-the-fold rule (compact alert + timeline)

The **Milestone alert + Delivery Timeline must fit together on one viewport (~750–800px tall after browser chrome)** so Engin sees "what matters today" without scrolling. The locked CSS handles this:

```css
.alert-strip{padding:6px 12px;margin:-14px -18px 8px;font-size:11.5px;line-height:1.5;border-left-width:4px}
.alert-strip strong{display:inline;font-size:11.5px;margin-bottom:0;margin-right:6px}
.timeline-h{padding-top:8px;padding-bottom:0}
.timeline-h::before{top:22px}
.th-event::before{margin:0 auto 4px;width:11px;height:11px;border-width:3px}
.th-event .when{font-size:11px}
.th-event .what{font-size:10px;line-height:1.2}
```

**Content rules — keep the alert ≤ 2 wrapped lines:**
- Single sentence ≤ 200 chars. Use `·` separators rather than additional sentences.
- Start with inline `<strong>Milestone:</strong>` (NOT `Milestone Alert` followed by a paragraph break).
- Pack: headline gate · most-relevant readiness signal · ≤ 3 named residual risks separated by `·`. No owner @mentions inside the alert (owners go in workstream/risk panels).

## Locked section order — exact, no additions, no reorderings, no omissions

1. **`<header>`** — flex layout with `.titles` (h1 + .sub + .date) on the left and `.overall-rag` gradient badge on the right (`.overall-rag.red` / `.amber` / `.green`).

2. **`<div class="kpis">`** — exactly **5 `.kpi` tiles** (`grid-template-columns:repeat(5,1fr)`): W22.4 NDS.Live → Cariad · W23.4 FINAL · W23.4 content lock (VS) · Overall feature readiness · Open S-Map IMs.

3. **`<div class="ws-kpis">`** — exactly **6 `.ws-kpi` compact tiles** with `border-left` colored by RAG: Number of Lanes (PF0) · Right of Way (PF0) · Speeds (PF0) · Curvature (PF0) · Traffic Signs (PF1) · Traffic Lights (PF1).

4. **`<section>` Key Updates — last 24h** — compact section with `<h2 style="font-size:14px;margin-bottom:8px;padding-bottom:6px">` heading and `<ul class="updates" style="font-size:12px">` of 5–8 `<li style="padding:5px 0">` rows.

5. **`<section>` containing `.alert-strip` + Delivery Timeline** — single-line alert + 5-event horizontal timeline.

6. **`<section>` Geographical & Feature Scope** — `<section style="padding:14px 22px">` with `.two-col`: Cities · Priorities on the left, Feature dimensions on the right.

7. **`<section>` Overall RAG rationale + Workstream Summary** — Single section with two `<h2>`:
   - "Overall RAG rationale (v3)" + paragraph explaining the RAG judgement (mechanical rule vs override).
   - "Workstream Summary — by feature dimension" + `.ws-cards` with **exactly 7 numbered cards** (1 · Lanes, 2 · RoW, 3 · Speeds, 4 · Curvature, 5 · Signs, 6 · Lights, 7 · IM Fixes with red left border).

8. **🆕 `<section>` Progress by Priority Area (PA0 / PA1 / PA2)** — **MANDATORY**. Heading `<h2>Progress by Priority Area (PA0 / PA1 / PA2)</h2>` + intro `<p>` explaining the source mapping (CM-16077 Speeds comments = explicit PA0/PA1/PA2 split; HDPROD subtasks = Lights/Signs samples covering demo + buffer; weekly ODRV cuts = Lanes/RoW/Curvature across all PAs). Then `.ws-cards` with **exactly 3 `.ws-card` blocks** (mirroring the workstream-card pattern with `border-left:4px solid var(--green|amber|red)`):
   - **PA0 — Demo / KPI core** (hard "0 regression" requirement; red polygons on city maps).
   - **PA1 — Buffer area** (orange polygons; same production-quality bar as PA0).
   - **PA2 — Wider urban (best effort)** (blue polygons; per CM-15489 acceptance: "Best effort is given to boost quality in Prio 2 geo-scope (PA2)").

   Each card body MUST cite the per-feature evidence by PA:
   - **Lanes / RoW / Curvature:** state which weekly cut delivered the content and whether it covers the PA (typically yes for PA0/PA1, "no PA2-specific boost" for PA2).
   - **Speeds:** quote the explicit PA0/PA1/PA2 status from the latest CM-16077 comment (the Smagur weekly comments mark BUA + Speed Limit completion per PA, and explicitly flag PA2 risk).
   - **Traffic Signs/Lights:** cite the HDPROD subtask completion for RC1.5 samples (cover PA0 demo scope) vs Top3 RC samples (cover PA1 broader scope). Top3 partial overlap with PA2 should be noted as "no PA2-specific sample" so the reader doesn't over-count.

   **Readiness % per PA:** compute as a weighted average of the per-feature evidence within that PA. PA0/PA1 typically track ~85–90% when comments are positive; PA2 hangs in the 50–70% range until Smagur's PA2 ETAs land. End each card description with `<span class="rag green/amber/red">RAG</span>` and an `.owner` line of `.mention` chips for the PA's primary drivers (Smagur is the recurring driver for Speeds PA work; Stefanie owns the cuts; Monika/Oliwer/Błoński own the HDPROD work).

9. **`<section>` RAG by Feature × Delivery Stream** — `<table class="rag-table">` with 5 data columns (Weekly cuts · Spec/VS · PF1 samples · Validation/IM · Open IMs) + dimension column. 7 rows.

10. **`<section>` Risks & Blockers** — 5–6 `.risk-panel` blocks. R2 Speeds is typically the only High (red panel); others Medium (amber panel). Always include a Speeds PA2 risk when CM-16077 flags PA2 at risk.

11. **`<section>` Full Ticket Hierarchy** — grouped with `<tr class="group-hdr"><td colspan="6">Group name</td></tr>`: Master · Parent/Feasibility · 1 · Number of Lanes (PF0) · 3 · Speeds (PF0) · 5 · Traffic Signs (PF1) · 6 · Traffic Lights (PF1) · 7 · IM Fixes · Supporting.

12. **`<footer>`** — "Cariad Test Drives Quality Boost — Status · Auto-generated from Jira on {Day} {DD Mon YYYY} (W{NN}) · Source: <a href='https://tomtom.atlassian.net/browse/CM-15489'>CM-15489</a> · IM filter: <a href='https://tomtom.atlassian.net/issues/?filter=109323'>filter 109323</a>"

**CSS rules:**
- Use the exact `<style>` block from `cariad_quality_boost_status_LOCKED_FORMAT_REFERENCE.html`, including the compact-spacing overrides and the compact alert-strip block at the bottom.
- @mentions use **`<span class="mention">@Name</span>`** (CSS class — blue chip).
- Simon Hughes stays plain text (no `.mention` chip) in workstream 1, R3 risk panel, ODG-396 hierarchy row, action items. Include inline "(plain text — no @mention)" reminder.

**Verification gate before saving:** list the section sequence of `LOCKED_FORMAT_REFERENCE.html` and the file you're about to write. Must match: header → kpis → ws-kpis → Key Updates → alert+Timeline → Geographical&Feature Scope → Overall RAG rationale+Workstream Summary → **Progress by Priority Area** → RAG by Feature × Delivery Stream → Risks & Blockers → Full Ticket Hierarchy → footer. Visual scan: alert ≤ 2 wrapped lines, PA section present with 3 cards.

# Program tickets and workstreams

**Master:** CM-15489 — Cariad Road Model (S-Map) W20–W23 — Quality boost. Owner: Nivard van Gerrevink.
**Parent/feasibility:** CM-15443 (cloned-from).

**6 feature workstreams + 1 IM workstream (7 cards total):**
1. Number of Lanes (PF0) — weekly cuts. Gate: ODG-396.
2. Right of Way (PF0) — weekly cuts.
3. Speeds (PF0) — Speed Limits + ICL + Derived. CM-16077 (Approved CR). **Explicit PA0/PA1/PA2 split posted weekly by Michał Smagur — the primary source for the Progress by Priority Area section.**
4. Curvature (PF0) — V4 delivered W20.4.
5. Traffic Signs (PF1) — HDPROD-37351, -37338, -34210, -34214.
6. Traffic Lights (PF1) — HDPROD-37416, -37418, -34151, -34142.
7. IM Fixes (S-Map open) — JQL filter 109323.

**Priority Areas (PA0/PA1/PA2) — geographic scope per city, NOT standalone Jira tickets:**
- **PA0** = red polygon = demo routes + KPI core (Berlin demo routes, highest priority — hard "0 regression").
- **PA1** = orange polygon = buffer around KPI routes (production-quality bar).
- **PA2** = blue polygon = wider urban (best-effort per acceptance criteria).
- Sources: CM-15489 description + CM-16077 description (geo scope priorities per city) + recurring Smagur weekly comments on CM-16077 (PA0/PA1/PA2 status).

**Supporting:** ODRV-11477, ODRV-11574, ODRV-11511, ODRV-11525, ODRV-11539, NDSIMP-8677.

# Action-owner mention map (`<span class="mention">@Name</span>`)

**Program owners:** Nivard van Gerrevink (master + IMs) · Stefanie Gysels (ODRV cuts) · Oliwer Urban (RC1.5 Lights) · Michał Błoński (Top3 Lights) · Monika Kwak (Signs) · Debashree Sarkar (CM-16077 owner) · Michał Smagur (Speeds driver — also primary PA0/PA1/PA2 source) · Raquel Rosa (Feasibility) · Shivaramlinga Gade (NDSIMP).

**IM owners (filter 109323):** Kyumars Sheykh Esmaili (Curvature) · Szymon Dedek (Speed Limit advisory) · Abdeljalil Karam (Lanes) · Hristo Dimitrov (Signs / RoW / Lights) · Stijn Coenen (RoW) · Tim Bekaert (Lanes / regression).

**🚫 Plain text only (no `.mention` chip):**
- **Simon Hughes** (ODG-396 ODbL approver) — keep name where currently mentioned but as plain text with inline "(plain text — no @mention)" reminder.

When new IM owners appear in filter 109323, render with `.mention` if known; plain text otherwise.

# Run procedure

## Step 1 — Refresh program data from Jira (READ EVERY COMMENT, EVERY SUBTASK, EVERY LINKED ISSUE)

**⚠️ Never trust the ticket *status field alone*.** HDPROD parents stay "In Progress" while subtasks roll up to 80–93% complete. Readiness % is driven by latest comments and subtask states.

### 1a. Headline fields
`searchJiraIssuesUsingJql` with the 18-key list:
`key in (CM-15489, CM-15443, NDSIMP-8677, ODRV-11477, ODRV-11511, ODRV-11525, ODRV-11539, ODRV-11574, HDPROD-37351, HDPROD-37338, HDPROD-37416, HDPROD-37418, HDPROD-34142, HDPROD-34210, HDPROD-34214, HDPROD-34151, CM-16077, ODG-396)`,
fields `["summary","status","assignee","duedate","priority","updated","subtasks","issuelinks"]`.

### 1b. Full comment stream per ticket
For each of the 18 tickets, call `getJiraIssue` with `fields: ["summary","status","comment","subtasks","issuelinks"]`, `responseContentFormat: "markdown"`. Capture latest 3 comments + subtask key/status list + issuelink key/type/status list.

### 1c. Recurse into subtasks for the 8 PF1 samples + CM-16077 + ODRV-11539 + ODRV-11525 + CM-15489
For each HDPROD sample walk the subtask array, capture status + latest comment. Compute Done/total per parent.

### 1d. Per-workstream + per-PA evidence pack
Build TWO evidence packs:
- **Per-workstream** (6 feature workstreams + IM Fixes): latest 1–2 root comments + subtask ratio + latest comments on most-recently-touched subtasks.
- **Per-PA (PA0 / PA1 / PA2):** parse the latest CM-16077 comment for explicit PA0/PA1/PA2 mentions. The Smagur weekly comments follow the pattern:
  - "BUA area improvements in Prio 0 and Prio 1 — {status}"
  - "Speed limit improvements in Prio 0 and Prio 1 — {status}"
  - "Quality control within Prio 0 and Prio 1 — {status}"
  - "Prio 2 area — {status / ETA / at risk}"
  Extract each line per PA. Cross-reference with HDPROD subtask counts (RC1.5 = PA0-leaning; Top3 = PA1-leaning, with partial PA2 overlap). Note explicitly when there is no PA2-specific evidence vs when PA2 is flagged at risk.

**Every workstream card, every PA card, every risk panel MUST cite specific evidence.** If a card description lacks a citation, redo Step 1.

## Step 1b — Refresh S-Map IMs from filter 109323
JQL `filter = 109323`, fields `["summary","status","assignee","priority","updated"]`, `maxResults: 100`. Fetch latest 2 comments per IM. Categorise by feature substring match. Flag IMs open > 14 days. Diff vs yesterday's dated HTML for new/changed IMs.

## Step 2 — Compute metrics

**Days remaining** (use `date '+%Y-%m-%d'`):
- W22.4 NDS.Live: Thu 28 May 2026
- W23.4 Orbis VS cut-off: Sun 31 May 2026 EOD
- W23.4 OPC cut-off: Tue 2 Jun 2026
- W23.4 NDS.Live FINAL: Thu 4 Jun 2026

**Per-feature readiness % — DRIVEN BY EVIDENCE PACK.**
- PF1 samples: `subtasks_done / total` averaged across the 4 parents.
- PF0 features: scale baselines from comments.
- Baselines (replace, don't anchor): Lanes ~88% · RoW ~85% · Speeds ~75–82% · Curvature ~85% (cap Amber if curvature IM > 14d) · Signs/Lights subtask-driven.

**Per-PA readiness % — DRIVEN BY THE PER-PA EVIDENCE PACK:**
- PA0 (Demo / KPI core): weighted average of Lanes/RoW/Curvature (delivered via cuts) + Speeds PA0 (per CM-16077) + Signs/Lights RC1.5 subtask completion. Typically 85–90% when comments are positive.
- PA1 (Buffer): similar mix but using Top3 RC samples for Signs/Lights + Speeds PA1 from CM-16077.
- PA2 (Wider urban, best-effort): driven mainly by Speeds PA2 status from CM-16077 (most days the only PA2-specific signal). 50–70% range until Smagur confirms PA2 ETAs landed; Amber until then; Red if PA2 explicitly slips past 30 May ETA.

**Overall RAG (drives header badge + alert-strip class):**
- Default mechanical rule: Red if > 8 open IMs.
- Override: if overall ≥ 75% AND no feature < 50% AND ≤ 1 IM genuinely stuck > 14d AND no other High risks beyond Speeds → Amber by judgement.
- Otherwise Green if ≥ 75% with no High risks; Red if mechanical conditions fire without override.
- Always include "Overall RAG rationale (v3)" paragraph reflecting the call.

## Step 3 — Generate the HTML dashboard

Mirror the LOCKED format. Read `cariad_quality_boost_status_LOCKED_FORMAT_REFERENCE.html` before writing. **Apply the compact alert-strip rule** AND **always include the Progress by Priority Area section** (3 cards, between Workstream Summary and RAG by Feature × Delivery Stream).

**Save to:**
- `/sessions/<current-session>/mnt/outputs/cariad_quality_boost_status.html`
- `/sessions/<current-session>/mnt/outputs/cariad_quality_boost_status_YYYY-MM-DD.html`

After writing, `ls -lh` both files and assert non-zero size.

**Never overwrite `cariad_quality_boost_status_LOCKED_FORMAT_REFERENCE.html`.**

## Step 4 — Prepare Confluence DRAFT (do NOT publish)

`createConfluencePage` with `status: "draft"`, parentId `2001011532`, spaceId `130547712`. Title `Cariad Test Drives Quality Boost — Status Report — DD MMM YYYY`. Body mirrors the HTML sections in the same order, including **a "Progress by Priority Area" `<h2>` block** with 3 `<h3>` sub-sections (PA0/PA1/PA2) — each citing the per-PA evidence.

Verify `"status": "draft"`.

## Step 5 — Attach the HTML to the Confluence DRAFT (MANDATORY)

Credential discovery in order: env vars (`ATLASSIAN_API_TOKEN`, `ATLASSIAN_TOKEN`, `CONFLUENCE_API_TOKEN`, `ATLASSIAN_OAUTH_TOKEN`) → files (`~/.atlassian/credentials`, `~/.atlassian/token`, `~/.config/atlassian/token`, `/sessions/*/mnt/.claude/atlassian_token`, `~/.netrc`) → Engin's home paths. Email defaults to `engin.erkan@tomtom.com`. POST multipart to `/wiki/rest/api/content/{PAGE_ID}/child/attachment`, `X-Atlassian-Token: no-check`, one retry on 5xx.

## Step 6 — Report back

- Headline: "📝 Draft prepared for review — open in Confluence to review and publish."
- Attachment status.
- Draft edit URL / view URL.
- `computer://` links to canonical + dated HTML.
- 3-bullet headline: overall RAG + readiness %, biggest risk, days to next deadline.

# Important guardrails

- **🔒 HTML FORMAT IS LOCKED to the 28 May reference + 29 May compact alert-strip + Progress by Priority Area refinements.** Read the LOCKED reference first every run; mirror structure exactly.
- **⚡ Alert-strip + Delivery Timeline must fit above the fold.** Single-line alert ≤ 200 chars.
- **🆕 Progress by Priority Area section is MANDATORY.** 3 cards (PA0/PA1/PA2) between Workstream Summary and RAG by Feature × Delivery Stream. Each card cites per-feature evidence by PA, with the CM-16077 Smagur weekly comment as the primary source for PA0/PA1/PA2 Speeds status. Note "no PA2-specific sample" for Signs/Lights when applicable.
- **Comments and subtask completion override the status field** for readiness %.
- **Every workstream card, every PA card, every risk panel must cite a specific dated comment, ticket key, or subtask state.**
- **`.mention` CSS class** for @mentions in HTML (not `<span data-type="mention">`). Simon Hughes stays plain text.
- **DO NOT publish.** `status: "draft"` is mandatory.
- **Attachment is required** on the draft.
- **Always read freshest comments + subtasks + linked issues** for every ticket.
- **Always re-run filter 109323.**
- **Sub-page title MUST include the date** in `DD MMM YYYY` form.
- **Save HTML output before creating the Confluence draft.**
- **Never delete previous dated HTML files** — historical archive.
- **Never overwrite `cariad_quality_boost_status_LOCKED_FORMAT_REFERENCE.html`.**