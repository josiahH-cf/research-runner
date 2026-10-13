# Report generation (HTML)

The separate final phase of a Research Run. It turns a frozen findings snapshot into one HTML report. Change this file to change how reports look; the research method in `SKILL.md` and `principles.md` does not change with it.

## Boundaries

- **Input**: `publication-source-vN.json`, plus `frame.json` and `state.json` for the clarification and execution records. Nothing else.
- **Output**: `report-vN.html` in the run folder, where N matches the snapshot version. Never a Markdown report, Markdown renamed to `.html`, or Markdown with an optional HTML export.
- **Presentation only**: do not search, open new sources, re-review, change any finding, verdict, confidence or qualification, or attach a source to a statement the snapshot does not support. Nothing in the report may be stronger than the snapshot.
- **Outside the review loop**: the report is not sent through research review, and writing it never restarts research.
- **Coverage**: every information-to-explain item appears, either as a finding in the report or noted briefly as open, with detail in the supporting area.
- **Problems noticed while writing** (an unclear finding, a gap, an apparent inconsistency) are presented as the snapshot states them and listed under "worth verifying next" and in the Continue area. They are not fixed here.
- **Budget**: this phase uses the report reserve. If the reserve runs short, shorten the supporting detail, never the front report, and record the shortening in `state.json`.
- **Final pass before saving**: reread each report statement against the snapshot and correct any wording that overstates it; confirm that every citation link has a matching source anchor and every contents link a matching section.

## The file

- One self-contained `.html` file that reads correctly when opened directly from disk: inline CSS and inline SVG, with no external scripts, fonts, images or network requests.
- Reading requires no JavaScript. All text is real HTML text (headings, paragraphs, lists, tables), never text inside images, and nothing appears only on hover.
- Three areas, in this order: **Report** (shown first), **Supporting documentation** and **Continue**, reached from a tab bar. Use the CSS-only tabs in the skeleton below; in browsers without that CSS support, and when printing, the three areas show one after another.
- Title the page with the question's plain title, not the folder name.

## Report area (front)

It answers: what did the research find, and what should I understand? Write in clear, natural language for the approved audience. Keep it high level and easy to navigate without dropping qualifications that change a finding's meaning. Leave out process vocabulary (stages, rounds, reviewers, claim IDs, budgets), except for one plain sentence when research stopped at the budget before something important was settled.

In this order:

1. **Title** and a one-line statement of the question.
2. **🧭 TL;DR**: a short paragraph that states the central findings, stands on its own and keeps its essential qualifications.
3. **✅ Here's what you should know**: readable bullets, one idea each, most important first.
4. **📑 Contents**: links to the sections that follow.
5. **The report**: sections organized around what the reader needs to understand, not around how the research ran. Each section explains a finding, comparison or implication: what is known, how confident the evidence allows, and why it matters for the intended outcome, with any material limitation beside the finding it qualifies. Include these when the snapshot has them:
   - separate verdicts, one per dimension, never blended;
   - what testing changed, in plain terms: ✅ what held up, ⚠️ what was narrowed, ⛔ what did not survive;
   - decisions or next steps, each with the condition that would change it;
   - every metric or comparison the brief named, with explicit gaps where evidence is missing, and cohort or sample boundaries beside statistics;
   - for a later version, a short note on what changed since the previous report and why.

   Use relevant emojis in headings where they help scanning, not on every line.
6. **⚠️ Gotchas, ❓ open questions and 🔎 worth verifying next**: a few consequential caveats, open-ended questions that invite further exploration, and checks worth doing next. Say plainly that these are invitations, not work already under way.

Citations and qualifications:

- Mark supported statements with short linked references to the source list, such as `<a href="#src-S3">[S3]</a>`. Number sources `S1`, `S2` and so on in the report, and show each one's ledger ID in the supporting area.
- State confidence in plain words, such as "strong evidence", "early signals" or "in the sources checked, as of March 2026".
- A gotcha is a brief, meaningful caveat. The exhaustive record of uncertainty belongs in the supporting area.
- When the evidence supports no recommendation, or fewer findings than hoped, say so plainly rather than padding. Assumed thresholds and discovery plans are proposals, never findings.

## Diagrams

Include at least one diagram that explains the central finding, comparison or relationship (for example a comparison, a causal chain, how parts relate, a trade-off, or what held up versus what was narrowed), and add others where they help the reader follow a finding. Only when the snapshot holds no supported relationship or comparison at all, omit diagrams rather than invent one.

- Draw it as inline `<svg>` inside a `<figure>`, with a `<title>` and a `<figcaption>` stating in words what it shows, so the copied text still makes sense without the picture.
- Draw only supported findings and relationships. Chart quantities only when the snapshot has comparable observations in a common unit, each tied to a claim whose excerpt contains the value, and prefer simple bars. Otherwise use a table or a qualitative diagram.
- Never invent scores or measurements, chart process counts (searches, rounds, tokens) as findings, add decorative visuals, or hide uncertainty to make a picture cleaner.
- Use `currentColor` and the page's CSS colors so diagrams stay legible in light and dark themes. Keep labels short and never overlapping, use a `viewBox` no wider than 640 units with text of at least 16 units so it stays readable on a phone, and stack multi-step flows vertically.

## Supporting documentation area

Everything needed to inspect the work, kept here so it does not interrupt the report. Nothing useful is dropped to simplify the front.

1. **Clarification record**: the request verbatim; the approved question, intended outcome, scope and exclusions; audience; evidence expectations; clarifications (user, inferred or assumed); token cap and plan; the approval.
2. **Sources**: `S#` entries with anchors (`id="src-S3"`) giving title, publisher, date, link or file, accessed date, source class, whether supplied or found, whether opened, and any access notes.
3. **Findings and their evidence**: each finding with its claims, excerpts, locators, standing and confidence, labeled fact, estimate, interpretation or assumption.
4. **What testing changed**: attacks and freshness checks, with what held up, what was narrowed (old and new wording) and what was contradicted.
5. **Rejected alternatives and contradicted claims**, with the evidence against them.
6. **Contested evidence**: both sides and their sources.
7. **Detailed unknowns**: why each is open, its practical significance and what would resolve it, separating further research from input only the user or another party can give.
8. **Review history**: who reviewed and how independently, what was checked, the dispositions and repairs, and unresolved disagreements.
9. **Budget**: the cap, the plan, usage (measured and estimated figures labeled), the reserve, what was deferred or cut for budget, any overrun and its cause, and the budgets of earlier versions and continuations.
10. **Execution record**: rounds in order, returns to earlier work and why, handoffs, limitations and failures.
11. **Changes since the previous version** (later versions only): findings added, corrected, narrowed or retired, and why.

## Continue area

Material for the next iteration, separate from the report. Nothing here starts work.

1. **Where things stand**: two or three sentences on whether research is complete or stopped at the budget, and what remains open.
2. **Questions for you**: only the questions that matter for the intended outcome. For each, give the question, why it matters, what happens if it stays unanswered, and a `Your answer:` line.
3. **Possible next directions**: what further research could settle, each with a rough token estimate labeled as an estimate.
4. **Budget so far**: the cap and usage, with measured and estimated figures labeled. The budget for a continuation is the user's choice.
5. **How to continue**: a copyable plain-text block (`<pre>`) that starts with `$research-run <run folder name>` and lists the questions with blank answer lines, plus one sentence explaining that the run resumes from its records and reuses its sources and findings, and that `$` can be swapped for the app's own trigger (`@` in the ChatGPT app, `/` in Claude).

## Skeleton

Keep the area IDs `report`, `supporting` and `continue` and the `area` class so the tabs work. Headings, section content and styling beyond this skeleton may change freely.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Plain title of the question</title>
<style>
:root { --bg: #fbfaf7; --fg: #1f2328; --muted: #5b6470; --line: #d9dde3; --accent: #2f5f9e; --panel: #ffffff; }
@media (prefers-color-scheme: dark) { :root { --bg: #15181c; --fg: #e6e8eb; --muted: #a3abb5; --line: #343a42; --accent: #8fb3e8; --panel: #1c2026; } }
body { margin: 0; background: var(--bg); color: var(--fg); font: 16px/1.6 system-ui, -apple-system, "Segoe UI", sans-serif; }
.wrap { max-width: 860px; margin: 0 auto; padding: 0 16px 48px; }
nav.tabs { display: flex; flex-wrap: wrap; gap: 8px; padding: 12px 0; border-bottom: 1px solid var(--line); }
nav.tabs a { padding: 6px 12px; border: 1px solid var(--line); border-radius: 6px; color: var(--fg); text-decoration: none; }
a { color: var(--accent); }
table { border-collapse: collapse; width: 100%; }
th, td { border: 1px solid var(--line); padding: 6px 8px; text-align: left; vertical-align: top; }
figure { margin: 24px 0; padding: 12px; background: var(--panel); border: 1px solid var(--line); border-radius: 8px; }
figure svg { width: 100%; height: auto; color: var(--fg); }
figcaption { color: var(--muted); font-size: 14px; }
pre { white-space: pre-wrap; background: var(--panel); border: 1px solid var(--line); border-radius: 6px; padding: 12px; }
@supports selector(:has(*)) {
  .area { display: none; }
  .area:target, .area:has(:target) { display: block; }
  main:not(:has(.area:target, .area :target)) #report { display: block; }
}
@media print { .area { display: block !important; } nav.tabs { display: none; } }
</style>
</head>
<body>
<div class="wrap">
<header>
  <h1>Plain title of the question</h1>
  <p>The question, in one line.</p>
</header>
<nav class="tabs">
  <a href="#report">📖 Report</a>
  <a href="#supporting">🗂️ Supporting documentation</a>
  <a href="#continue">➡️ Continue</a>
</nav>
<main>
<section id="report" class="area">
  <h2 id="tldr">🧭 TL;DR</h2>
  <p>Central findings with their essential qualifications. <a href="#src-S1">[S1]</a></p>
  <h2 id="know">✅ Here's what you should know</h2>
  <ul><li>One idea per bullet.</li></ul>
  <h2 id="contents">📑 Contents</h2>
  <ol><li><a href="#section-1">First section</a></li><li><a href="#closing">Gotchas, open questions and what to verify next</a></li></ol>
  <h2 id="section-1">First section</h2>
  <p>Explanation of a finding.</p>
  <figure>
    <svg viewBox="0 0 640 200" role="img" aria-labelledby="fig1-title"><title id="fig1-title">What the diagram shows</title><rect x="20" y="60" width="180" height="80" rx="8" fill="none" stroke="currentColor"/><text x="110" y="105" text-anchor="middle" fill="currentColor" font-size="16">Supported finding</text></svg>
    <figcaption>In words: what the diagram shows and the evidence behind it.</figcaption>
  </figure>
  <h2 id="closing">⚠️ Gotchas, ❓ open questions and 🔎 what to verify next</h2>
  <ul><li>A consequential caveat.</li></ul>
</section>
<section id="supporting" class="area">
  <h2>🗂️ Supporting documentation</h2>
  <h3>Sources</h3>
  <ol><li id="src-S1">Title, publisher, date, link, accessed date, source class, supplied or found, opened (ledger ID)</li></ol>
</section>
<section id="continue" class="area">
  <h2>➡️ Continue</h2>
  <pre>$research-run run-folder-name
B1. Question?
Your answer:</pre>
</section>
</main>
</div>
</body>
</html>
```
