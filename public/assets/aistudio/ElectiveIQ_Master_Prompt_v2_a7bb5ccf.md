# ElectiveIQ – Master Build Prompt (Cumulative, v2 – with Design Phase)

**Project:** ElectiveIQ – Intelligent Elective Recommendation & Academic Planning System
**Tagline:** Choose Smarter. Learn Better. Build Your Future.

This file merges (1) the original build prompt, (2) the nine enhancement blocks (A–I) derived from the *Elective Course Advisor – Project Guide*, and (3) a new design layer (blocks J–L) based on the attached dashboard screenshot, into one prompt. Nothing from the earlier versions has been removed; the engine requirements are simply deferred behind a phase switch. Where an enhancement block overlaps the base text, the enhancement block is the stricter, more detailed version and takes precedence.

## How to use

1. **Phase 1 (design only, active now):** attach the dashboard reference screenshot to your message. The prompt also describes the design in words, so it still works if the image is dropped. No CSV files are needed yet.
2. Copy everything between `PROMPT START` and `PROMPT END` and paste it as your message. The model should return `index.html` and `style.css` only, plus an integration map.
3. **Phase 2 (engine, later):** change the single line `ACTIVE_PHASE = 1` to `ACTIVE_PHASE = 2`, attach the real `Electives.csv` and `Students.csv` (the Project Guide PDF does not contain the data), and send the prompt together with the Phase 1 `index.html` and `style.css` so the engine is built against the existing markup and hooks.
4. Run the result by opening `index.html`, or serve it with a local server such as VS Code Live Server so `fetch()` can auto-load the CSVs (Phase 2).

## Contents and injection map

| Part | Source | Placed after / replaces |
|---|---|---|
| ROLE, TECH STACK, Sections 1–8, OUTPUT | Original prompt | Base text (kept in full) |
| Block A – Operating rules | Enhancement | After ROLE |
| Block B – Step 0 gate and audit format | Enhancement | Extends/overrides STEP 0 |
| Block C – Data contract | Enhancement | After Step 0 |
| Block D – Constraint registry | Enhancement | Inside Section 3, Step 1 |
| Block E – Scoring spec | Enhancement | Inside Section 3, Step 2 |
| Block F – Search and ranking spec | Enhancement | Inside Section 3, Step 3 |
| Block G – Explanation and unmet-needs specs | Enhancement | Extends/overrides Sections 4 and 5 |
| Block H – Testing oracle | Enhancement | Inside Section 6 |
| Optional extras build order | Enhancement | Inside Section 7 |
| Block I – Output contract and self-check | Enhancement | Extends/overrides OUTPUT (Phase 2) |
| Block J – Phase control | Design layer | Right after the PRECEDENCE NOTE; decides what is built now |
| Block K – Design system (from screenshot) | Design layer | After the operating rules |
| Block L – Backend-ready markup and Phase 1 output contract | Design layer | After the design system |

---

<!-- ===================== PROMPT START ===================== -->

## ROLE

You are a senior front-end developer. Build "ElectiveIQ – Intelligent Elective Recommendation & Academic Planning System" (tagline: "Choose Smarter. Learn Better. Build Your Future.") as a browser-only website. No backend, no login, no paid API keys.

PRECEDENCE NOTE: This prompt contains base requirements plus detailed specification blocks (tagged in angle brackets, e.g. <operating_rules>). Follow both. Where a tagged block is stricter or more specific than the base text, the tagged block wins. <phase_control> overrides everything else on what is built in this response.

<phase_control>
ACTIVE_PHASE = 1

PHASE 1 – DESIGN ONLY (active now). Build only the visual layer described in <design_system> and <backend_ready_markup>. Deliver exactly two files: index.html and style.css.
- Write NO JavaScript logic: no CSV parsing, no scoring, no combination generation, no fetch calls, no backend or API code, and no script.js.
- All on-screen content is static, clearly marked sample content so the design can be judged visually (see the placeholder policy in <backend_ready_markup>).
- The markup must be ready for a backend/engine file to be merged in later WITHOUT restructuring the HTML or renaming CSS classes.

PHASE 2 – ENGINE (deferred, do not implement while ACTIVE_PHASE = 1). Everything else in this prompt about the data audit (Step 0), data contract, constraints, scoring, search/ranking, explanations, unmet needs, testing, script.js and the self-check stays valid and must not be deleted or weakened. It is simply postponed.

Scope overrides while ACTIVE_PHASE = 1:
- TECH STACK deliverables: two files (index.html, style.css), not three.
- STEP 0 and the OUTPUT section: deferred. Do not claim any real column names, courses, students or results; use generic labels only.
- Section 1 (structure), Section 2 (design), and the design-related parts of Sections 4, 5, 6 and 7 still apply, but only as layout and visual states, not as working logic.
- If <design_system> conflicts with the sticky TOP navigation in Section 1, <design_system> wins: use the left sidebar navigation (with a sticky top bar on mobile).

When ACTIVE_PHASE = 2: implement script.js against the existing markup and hooks, and change index.html or style.css only where a hook is genuinely missing.
</phase_control>


<operating_rules>
Priority order when rules conflict: (1) correctness of constraint logic, (2) fidelity to the real CSV data, (3) transparency of every decision, (4) usability, (5) visual polish.
- Never assume the schema. Every column name, separator and category must come from the files.
- Never relax a hard constraint silently. If the data is ambiguous, flag it, apply the stricter reading, and say so in the UI.
- Never invent courses, slots, prerequisites, tags or student data.
- Never state that you ran or tested the code unless you actually executed it. Otherwise say "not executed" and list what the user should check.
- Treat weights, synonym maps, and difficulty mappings as ASSUMPTIONS: keep them in one CONFIG object and label them as tunable in the UI.
</operating_rules>

<design_system>
REFERENCE: the attached dashboard screenshot (a light, card-based learning dashboard shown on a laptop mockup). Recreate its visual language for ElectiveIQ.
- Do NOT reproduce its brand name, logo or copy (no "Miyagi Labs", no "Guang", no SAT wording). Use ElectiveIQ branding and an original simple logo mark drawn as inline SVG.
- The laptop frame and the orange gradient backdrop are presentation scenery, not part of the app. Do not build them.
- Colour values below are estimates taken from a screenshot. Put every one in CSS variables so they can be tuned in one place.

1. OVERALL LOOK AND FEEL
Calm, premium, high-contrast layout: a cool light-grey canvas holding large rounded white cards, a warm near-black rounded sidebar, one hot red-orange accent used sparingly for the primary action, active states and key icons, and soft green only for progress and "valid". Very generous white space, big confident bold headings, small muted labels, large bold numbers. Soft shadows, no hard borders, no clutter.

2. DESIGN TOKENS (all variables prefixed --eiq- and defined on :root of the .eiq-app wrapper)
- Canvas: --eiq-bg #EEF0F0 | Card surface: --eiq-surface #FFFFFF | Soft surface: --eiq-surface-soft #F7F7F6
- Sidebar: --eiq-side-bg #17100E | --eiq-side-text #F5F1EE | --eiq-side-muted #A39A95 | --eiq-side-active-bg rgba(255,255,255,.08)
- Text: --eiq-ink #141110 (headings, numbers) | --eiq-ink-muted #6F6A66 (labels; must stay readable on white)
- Accent: --eiq-accent #E2412B | --eiq-accent-grad linear-gradient(135deg, #E8502F, #C8352A)
- Status: --eiq-ok #1FA765 (valid/progress) | --eiq-warn #E39A12 (warning/trade-off) | --eiq-bad #B3261E (unmet/invalid, a deeper crimson than the accent so the two are not confused)
- Soft tints for stat tiles (subtle top-to-bottom gradients on white): blue #EAF3FF, green #EAF8EF, peach #FFF4E6
- Radius: --eiq-r-xl 28px (outer panels, sidebar) | --eiq-r-lg 22px (cards) | --eiq-r-md 14px (tiles, inputs) | --eiq-r-pill 999px (buttons, chips, mode pill)
- Shadow: --eiq-shadow 0 8px 30px rgba(20,17,16,.06) plus a 1px border rgba(20,17,16,.05)
- Spacing scale: 4, 8, 12, 16, 24, 32, 48
- Typography: heading/UI font stack "Manrope", "Plus Jakarta Sans", "Inter", system-ui, -apple-system, "Segoe UI", sans-serif (loading one font from Google Fonts is optional; the fallback stack must look good on its own). Greeting: clamp(2rem, 4.5vw, 3.25rem), weight 800, letter-spacing -0.03em, tight line-height. Big metric numbers: 2rem to 2.5rem, weight 700, tabular numerals. Card titles 1rem to 1.125rem, weight 700. Muted labels 0.8125rem (13px minimum). Body text 16px or larger.

3. APP SHELL AND LAYOUT
- Wrapper: a single .eiq-app root. Inside: a rounded dark sidebar on the left (inset from the canvas edge by about 12px, full-height, 240px wide) and the main area on the right.
- Sidebar, top to bottom: logo row (inline SVG mark + "ElectiveIQ"); a "Hide sidebar" control with a left-arrow icon (CSS-only toggle using a visually hidden checkbox and a label); primary nav: Dashboard, Student Profile, Recommendations, Comparison, Testing; a small secondary group: Data Status, Help. Each nav item is a 44px-tall row with a 20px line icon and a label. The active item has a faint lighter pill background and an accent-coloured icon. Inline SVG line icons only (stroke 1.75, rounded caps), no icon libraries.
- Main area top bar: left, a breadcrumb pill (home icon + current section name) on a soft surface; right, a "mode" pill with a small accent dot and a dropdown chevron (use it for the current goal or semester label slot) followed by a circular avatar. On mobile this bar is sticky and also holds the menu button.
- Below the top bar: the large greeting heading, then a 12-column card grid with 16px to 24px gaps. Cards do not touch the canvas edge; the main area has 24px to 32px padding on desktop.
- Single page, five anchored sections (ids: dashboard, student-profile, recommendations, comparison, testing). The sidebar links are in-page anchors.

4. SECTION DESIGNS
DASHBOARD (matches the reference screenshot most closely)
- Greeting heading: "Good <time-of-day>, <Student name>!" built as three separate slots (greeting word, name) so a backend can fill them; static sample text for now.
- Row A, two cards. Left, wide "Your Best Fit" card (equivalent of the reference "Today's Focus"): a tiny accent-coloured label with a round icon chip at the top; a segmented progress bar made of many thin vertical ticks (filled ticks in green, remaining ticks light grey) with a percentage figure and a small green change badge; the top combination title in bold; a muted one-line description; and a gradient pill button "Get Recommendations" at the bottom-right of the card. Right card, "Data and Test Snapshot" (equivalent of "Today's Progress"): a title row with a small bar-chart icon and three equal soft-gradient stat tiles (blue, green, peach tint): Students, Electives, Valid combinations. Label small and muted at the top-left of each tile, large bold number at the bottom-right.
- Row B: four metric cards plus one gradient call-to-action card. Metric cards (equivalents of Daily Activity, Accuracy, Exam Countdown, Consistency): "Electives required", "Average fit score", "Weekly hours", "Test pass rate". Metric card anatomy: a 36px rounded-square icon chip (soft grey background, accent icon) with the title beside it at the top-left; the large number at the bottom-right; a muted sub-label right-aligned under the number. The call-to-action card uses the red-orange gradient, white text, a short headline, and an outlined white pill button "Run test for all students". Keep it visually smaller in importance than the main "Get Recommendations" button.
- Data-quality warnings sit in a slim amber-tinted strip card under Row B, collapsed to a count badge with an expandable list (CSS-only details/summary).
STUDENT PROFILE
- A labelled, searchable student selector styled as a large soft pill input with a search icon (visual only for now). Below it, a profile card: initials avatar, goal as an accent-outlined chip, CGPA in a stat tile, cleared subjects as green-tinted chips, weak subjects as amber-tinted chips, workload preference as a 3-option segmented control (Light, Moderate, Heavy; display only), and "required electives per category" as a row of small tiles, one per category slot, each with a category name and a number.
RECOMMENDATIONS
- Up to 3 cards in a responsive grid (1 column on phone, 2 on tablet, 3 on desktop). Card anatomy, top to bottom: rank chip ("#1 Best fit", "#2", "#3"); the segmented score bar with a plain-English label (Excellent / Strong / Moderate / Weak fit); a list of course rows (title, category badge, slot, weekly hours); a validity checklist with ✔ / ✘ rows for prerequisites, timetable, category counts, duplicates; an amber-tinted "Trade-offs" panel; and a footer with total weekly hours. Card #1 is emphasised with a thin accent top border and the gradient primary button; #2 and #3 are plain.
- Include an "unmet needs" variant of the card area: a red-outlined (crimson, icon plus text) message card that lists reasons, with a muted "What would unlock this?" line.
COMPARISON
- One rounded card containing a table: the first column holds row labels (sticky on horizontal scroll), then one column per combination. Header cells show rank chips. Light row dividers, row hover tint, and the best value in each row gently highlighted in green with a ✔ icon (not colour alone). On narrow screens the table scrolls horizontally inside its own container with soft edge shadows; the page body never scrolls sideways.
TESTING
- A toolbar card with the primary gradient button "Run test for all students" and a secondary outline pill button "Download results as CSV". Beneath it, four summary tiles in the stat-tile style (Total tested, Full, Partial or None, Validation failures). Then a results table in a scrollable card with a sticky header, columns: student, combinations found, status badge, reason. Include a progress-style loading bar for the running state.

5. COMPONENT LIBRARY (build each once, reuse everywhere)
- Buttons: primary (gradient pill, white bold text), secondary (outlined pill), ghost (text with icon). Minimum height 44px. Hover lifts 2px with a slightly stronger shadow; pressed state; visible focus ring.
- Badges: valid (green tint, ✔), warning (amber tint, !), invalid (crimson tint, ✘). Every badge carries an icon AND a text label; colour is never the only signal.
- Chips: neutral, green-tinted (cleared), amber-tinted (weak), accent-outlined (goal).
- Segmented progress bar: driven by ONE custom property, for example style="--eiq-pct:56", so a backend only has to set a number. Build the ticks with CSS gradients, not with dozens of elements.
- Stat tile, metric card, section card, table, segmented control, soft pill input, breadcrumb pill, mode pill, avatar, tooltip.
- Tooltips: CSS-only, shown on hover AND keyboard focus, linked with aria-describedby, for terms such as "score" and "trade-off".
- State blocks (see backend_ready_markup): skeleton loader (shimmering grey shapes), friendly empty state (simple inline SVG illustration plus one sentence plus one action), error banner (crimson, plain-language message, retry action).

6. RESPONSIVE BEHAVIOUR (mobile-first)
- Below 768px: single column; sidebar becomes an off-canvas drawer opened from a menu button in the sticky top bar (CSS-only toggle); stat tiles go two per row; tables scroll inside their cards; touch targets at least 44px.
- 768px to 1199px: sidebar collapses to an icon-only rail or a drawer; two-column card grid.
- 1200px and above: full 240px sidebar and the 12-column grid as described.
- Test at 360px, 768px and 1280px widths. No horizontal page scroll at any width.

7. MOTION
Subtle only: 150ms to 200ms transitions on hover, focus and the sidebar toggle; a soft fade-in for cards. No autoplay animation. Respect prefers-reduced-motion by removing transitions and the skeleton shimmer.

8. ACCESSIBILITY AND PRINT
- Landmarks: aside with nav (labelled), main, each section with aria-labelledby, a "Skip to content" link as the first focusable element.
- White text on the accent gradient must keep at least 4.5:1 contrast. If a lighter gradient stop fails, darken it.
- Visible 2px focus rings with offset, including on the dark sidebar (use a light ring there).
- Labels on every input; real table markup with scope attributes and captions; aria-live="polite" on the regions that will later receive results.
- Print stylesheet: hide the sidebar, top bar and all buttons; white background; cards flat with no shadow; avoid page breaks inside cards; keep the recommendation cards and comparison table.
</design_system>

<backend_ready_markup>
Goal: a backend or script.js file can be attached later without touching the layout. Build the markup and CSS with these rules.

FILES AND STRUCTURE
- index.html contains markup only. style.css is organised in commented sections in this order: tokens, base, layout shell, components, section styles, states, responsive, print.
- Do not load any script in Phase 1. At the end of the body leave a comment: <!-- PHASE 2: <script src="script.js" defer></script> -->
- No frameworks, no build step, no external requests (an optional single font link is the only exception), no images (icons are inline SVG).
- Scope everything under one .eiq-app root; prefix all CSS variables with --eiq-; name classes with BEM (.eiq-card, .eiq-card__title, .eiq-card--accent). Use IDs only for in-page anchors and the hooks listed below, never for styling. This prevents collisions when merged with other files.
- The only inline styles allowed are the single custom-property values for progress bars (style="--eiq-pct:56").

HOOK ATTRIBUTES (the integration contract)
- data-region="<name>" on every container a script will fill. Required regions: stats, data-status, warnings, student-select, student-profile, recommendations, compare-table, test-summary, test-results.
- data-state="empty | loading | ready | error" on every region. Include all four state blocks (class .eiq-state--empty, --loading, --ready, --error) inside each region and let CSS show exactly one, for example [data-state="loading"] .eiq-state--loading { display: block }. The default in Phase 1 is "ready" so the sample design is visible, and the other three states must be viewable by changing only that attribute.
- data-bind="<path>" on every text node a script will replace, using flat dotted paths, for example: student.name, student.goal, student.cgpa, student.workload, stats.studentCount, stats.electiveCount, stats.validCount, rec.score, rec.scoreLabel, rec.totalHours, test.total, test.full, test.partialNone, test.failures.
- <template data-template="<name>"> for every repeatable element, with data-bind slots inside: rec-card, course-row, chip-cleared, chip-weak, category-tile, validity-row, tradeoff-item, warning-item, compare-column, compare-row, test-row. Also render one filled sample instance of each outside the template so the design is visible.
- data-status="valid | warn | invalid" on every badge; CSS derives colour and icon from this attribute alone, so a script only sets the attribute and the label text.
- data-action="load-data | get-recs | run-tests | download-csv | print-report | toggle-sidebar" on every button or control that will trigger behaviour. In Phase 1 these buttons are present and styled but do nothing.
- Progress bars: the --eiq-pct custom property, plus data-score on the element for scripts and assistive text.

PLACEHOLDER POLICY (no fabricated data)
- Every piece of sample content carries data-sample="true". Use generic labels such as "Elective title", "Category name", "Student name", "Goal", and plain sample numbers. Never use real-looking course names, real subject names or real student names.
- Show a small "Sample preview" pill in the top bar. A single attribute, data-sample-mode="off" on .eiq-app, must hide the pill and every [data-sample] element, so the design can be switched from sample to live content later.

INTEGRATION MAP (deliver it with the code)
After the code, output a table listing every region with: region name | states provided | data-bind paths | templates | data-action controls. This is the hand-off sheet for the later backend or script.js file.
</backend_ready_markup>

<phase1_output_contract>
While ACTIVE_PHASE = 1, respond in this order:
1. Design summary in 10 lines or fewer: tokens chosen, layout decisions, and any deviation from the reference with the reason.
2. Complete index.html in one fenced block. No "..." elisions, no TODOs.
3. Complete style.css in one fenced block.
4. The integration map table.
5. How to preview: open index.html in any browser; change data-state or data-sample-mode to view the other states.
6. Known gaps: anything that could not be reproduced faithfully.

Before replying, silently confirm and then state "Design self-check passed" or list failures:
[ ] no JavaScript and no script.js; no backend or fetch code
[ ] all five sections present with the specified anchors, in the sidebar layout (sticky top bar on mobile)
[ ] every token is a --eiq- variable; the file contains no hard-coded colours outside :root
[ ] all four states exist for every data-region, and the other three can be viewed by changing one attribute
[ ] every repeatable element has a <template> plus one rendered sample
[ ] all sample content is generic and marked data-sample="true"; the Sample preview pill works
[ ] valid, warning and invalid badges use icon plus text, never colour alone
[ ] contrast of white text on the accent gradient is at least 4.5:1
[ ] layout works at 360px, 768px and 1280px with no horizontal page scroll
[ ] focus rings visible everywhere (including the dark sidebar); skip link present
[ ] print stylesheet hides the sidebar, top bar and buttons
[ ] no brand names, logos or copy from the reference image
</phase1_output_contract>


## TECH STACK (strict)

- HTML5 → page structure only
- CSS3 → all design and responsiveness (no frameworks required; a small CDN like PapaParse is allowed for CSV parsing)
- Vanilla JavaScript → CSV handling, constraint checks, scoring, combination generation, explanations
- Deliver 3 files: index.html, style.css, script.js (keep the recommendation engine in clearly separated functions)

## STEP 0 – INSPECT THE DATA FIRST (do not assume anything)

Before coding, read the attached Electives.csv and Students.csv and tell me:
- exact column names, data types, missing values
- how prerequisites, career tags and skill tags are written (separator, case, synonyms)
- the elective categories and how many electives each has
- the timetable slots, and whether any explicit workload limit exists
- how "required electives per category" is stored for each student

Then build the app around what is actually in the files. Flag missing or ambiguous data instead of treating it as valid.

<step0_data_audit>
GATE: If Electives.csv or Students.csv content is not actually provided, STOP and ask for it. Do not generate placeholder data or continue with imagined columns.

Report the audit in this exact structure BEFORE any code:
1. File summary table: file | rows | columns
2. Column dictionary table: column | inferred type | example values | % missing | notes
3. List encoding: for prerequisites, career tags, skill tags, cleared subjects, weak subjects, state the separator, case style, spacing, and the token used for "none" (e.g. blank, "None", "-", "NA").
4. Categories table: category | number of electives | slots used.
5. Timetable: distinct slot values, whether a slot looks like a single time block or a multi-day pattern, and whether any overlap rule is stated. If none is stated, treat the same slot as a clash and record this as ASSUMPTION A-TT.
6. Workload: is there an explicit weekly-hours cap anywhere? Quote the column or rule. If not, treat workload as soft (ASSUMPTION A-WL).
7. Required-electives encoding: how the per-category counts are stored for each student (one column per category, or a packed string), and whether any student requires 0 for some category.
8. Data-quality findings: list each problem as a coded warning: code | file | row/ID | field | issue | handling.
   Codes: DQ-MISSING, DQ-DUPLICATE-ID, DQ-UNKNOWN-PREREQ (prerequisite not found among known subjects), DQ-CASE-SPACE (normalised), DQ-SYNONYM, DQ-NONNUMERIC, DQ-RANGE (e.g. CGPA out of range), DQ-UNKNOWN-CATEGORY, DQ-AMBIGUOUS.
9. Assumptions list, numbered A-1, A-2 ..., each with a one-line justification.
</step0_data_audit>

<data_contract>
Normalisation (apply identically to both files):
- trim whitespace; collapse internal multiple spaces; compare case-insensitively; keep the original string for display.
- split list fields on the separator found in Step 0 (tolerate , ; | and " / " only if the data uses them); drop empty tokens; de-duplicate.
- null tokens (blank, "none", "n/a", "-") map to an empty list, never to a literal subject.
- subject identity key = lowercase, trimmed, punctuation-stripped name. If a course has both an ID and a name, resolve prerequisites by ID first, then by normalised name.
- numeric fields: parse with Number(); non-numeric becomes null plus a DQ-NONNUMERIC warning.
- CGPA scale: detect it from the data (max <= 4 means 4.0 scale, max <= 10 means 10.0 scale); expose it in CONFIG.cgpaMax and show it as an assumption.
- Synonyms (e.g. "AI" = "artificial intelligence", "ML" = "machine learning", "infosec" = "cybersecurity"): keep a SYNONYMS map in CONFIG, built ONLY from variants actually observed in the files plus clearly standard abbreviations. Show the applied map on the Dashboard.
Data state per record: VALID, VALID_WITH_WARNING, or EXCLUDED (the reason is shown, and a course with missing slot, category or prerequisite-parse failure is never eligible).
</data_contract>

## 1. WEBSITE STRUCTURE (HTML)

Single page with a sticky top navigation and 5 sections:
1. Dashboard – number of students and electives, data-load status, data-quality warnings, overall test statistics
2. Student Profile – student dropdown (searchable) showing goal, CGPA, cleared subjects, weak subjects, workload preference, required electives per category
3. Recommendations – up to 3 recommendation cards
4. Comparison – side-by-side table of the 3 combinations
5. Testing – "Run test for all students" button and results table

Use semantic tags (header, nav, main, section, table), labels on every input, and ARIA attributes for accessibility.

## 2. USER-FRIENDLY DESIGN (CSS)

- Goal: very simple. A new user should get recommendations in 3 clicks: Load data → Pick student → Click "Get Recommendations".
- Load the CSVs automatically if possible; otherwise show two big file-upload boxes with clear instructions (browsers block local file fetch, so uploads are the fallback).
- Clean, modern, card-based layout; one primary accent colour, lots of white space, readable font (16px+), rounded cards, soft shadows.
- Mobile-first and responsive (works on phone, tablet, desktop).
- Colour-coded badges: green = valid, amber = warning/trade-off, red = unmet/invalid.
- Score shown as a progress bar plus a plain-English label (e.g. "Excellent fit").
- Loading state, friendly empty states, and helpful error messages (never a blank screen).
- Tooltips/short help text explaining terms like "score" and "trade-off".
- Print-friendly styles for the report.

## 3. RECOMMENDATION ALGORITHM (JavaScript)

Principle: FILTER FIRST, SCORE SECOND. Rank COMBINATIONS, not individual courses.

### Step 1 – Hard constraints (mandatory, never relaxed silently):
- prerequisites must be in the student's cleared subjects
- no timetable clashes (same slot conflicts only if the data establishes overlap)
- exact number of electives per category as required by the student
- no duplicate courses in one combination
- any explicit hard workload limit found in the data

<hard_constraints>
Implement each rule as a separate pure function returning {ok: boolean, code: string, detail: string}. checkHardConstraints() calls all of them. Nothing outside this registry may reject a combination.
HC1 PREREQ: every prerequisite of every selected course is in the student's cleared set (normalised). Fail code: PREREQ_UNMET, detail lists the missing subjects.
HC2 SLOT: no two selected courses share a conflicting slot (per Step 0 assumption A-TT). Fail code: SLOT_CLASH, detail names both courses and the slot.
HC3 CATEGORY_COUNT: for each category, selected count equals the student's required count exactly (including 0). Fail code: CATEGORY_COUNT, detail gives required vs selected.
HC4 NO_DUPLICATE: no course ID appears twice. Fail code: DUPLICATE.
HC5 AVAILABILITY: the course exists in the file and has no exclusion flag from the data contract. Fail code: UNAVAILABLE.
HC6 HARD_WORKLOAD: applies ONLY if the data defines an explicit maximum. Otherwise this rule is disabled and the UI says "No hard workload limit found in data".
Also: a student with no eligible courses in a category that requires N > 0 is infeasible for that category, so report the exact shortfall (see the unmet-needs block).
</hard_constraints>

### Step 2 – Score each eligible course (normalise every component to 0–100):

Score = 0.40 × Career + 0.25 × Skills + 0.20 × Academic suitability + 0.15 × Workload fit
- Career: match between student goal and course career tags (normalised tags/keywords, documented synonym handling)
- Skills: match with skill tags
- Academic suitability: CGPA vs difficulty (map Easy=1, Medium=2, Hard=3 if applicable) and weak subjects; treat CGPA as one indicator only
- Workload fit: weekly hours vs the student's workload preference (soft preference unless a strict maximum exists)

Keep weights in one config object at the top of script.js so they are easy to tune.

<scoring_spec>
Every component returns an integer 0-100 and a short evidence object listing the exact tags or values that produced it (used by the explanation module).
- Career(c, s): tokenise the student's goal (after synonyms); score = 100 x (matched goal tokens / total goal tokens), plus an exact-phrase match bonus capped so the total never exceeds 100. Evidence = matched tags.
- Skills(c, s): overlap between course skill tags and the student's goal and skill terms, as the matched fraction x 100. If the students file has no skill column, derive terms from the goal only and say so.
- Academic(c, s): map difficulty to numbers using CONFIG.difficultyMap. Compute readiness = student CGPA normalised to 0-1 by cgpaMax. Fit = 100 minus a penalty proportional to how far difficulty exceeds readiness. Subtract a fixed CONFIG.weakSubjectPenalty if a prerequisite or the course topic is in the student's weak subjects. Clamp 0-100. CGPA is ONE indicator and must not dominate: its influence is limited by this component's 20% weight.
- Workload(c, s): map the student's preference (e.g. Light/Moderate/Heavy) to a target weekly-hour band derived from the course-hours distribution in the data (quantiles, not hard-coded numbers). Score 100 inside the band, decreasing linearly with distance outside it.
- Missing inputs: if a component cannot be computed, use CONFIG.neutralScore (50), set a visible "estimated" badge on that component, and record a DQ warning. Never set it to 100.
- Course score = weights.career x Career + weights.skills x Skills + weights.academic x Academic + weights.workload x Workload. Weights live in CONFIG and must sum to 1; assert this at load time.
- Provide a small "Weights" panel with sliders that re-run scoring live, plus a note stating that the default weights are assumptions, not facts from the data.
</scoring_spec>

### Step 3 – Build and rank combinations:
- Generate combinations that meet the exact category counts (per-category selection, then cartesian product, with pruning for efficiency)
- Validate every combination against all hard constraints
- Combination score = average of its course scores (state the formula in the UI)
- Sort descending, return up to 3 DISTINCT valid combinations, with a deterministic tie-break (lower workload, then alphabetical)
- If fewer than 3 exist, show only those; never create invalid alternatives

<search_spec>
1. Per category: filter eligible courses (HC1, HC5), sort by score, and generate k-combinations where k is the required count (k = 0 yields one empty selection).
2. Cartesian product across categories with pruning: abandon a partial selection immediately on a HC2 clash or HC4 duplicate; if HC6 is active, prune when running hours exceed the cap.
3. Safety valve: if the estimated number of raw combinations exceeds CONFIG.maxCombos (e.g. 200000), use branch-and-bound with an optimistic upper bound (current sum plus best-possible remaining scores). State in the UI when this mode is used. Never drop valid combos silently otherwise.
4. Combination score = arithmetic mean of its course scores. Display the formula and the per-course values.
5. Sort: score descending, then total weekly hours ascending, then the alphabetical join of course names. The result must be deterministic for identical input.
6. Distinct = different SET of course IDs (order-insensitive). Return up to 3. If fewer exist, return fewer and never pad.
7. Re-run every returned combination through checkHardConstraints() once more before display (defence in depth).
</search_spec>

## 4. EXPLANATION FOR EACH RECOMMENDATION CARD

Show: (1) selected electives and their categories, (2) overall score and what it means, (3) why it matches the student's goal, (4) skill and career benefits, (5) total weekly hours, (6) prerequisite and timetable validity (✔ / ✘), (7) trade-offs: relevant courses left out and what skills/exposure they would have given.

<explanation_spec>
Every sentence must be generated from data fields or evidence objects. No generic filler or claims about courses that the tags do not support.
Per card, in this order:
1. Courses: name | category | slot | weekly hours | difficulty.
2. Score: N/100 with a bar and a label (>=85 Excellent fit, 70-84 Strong fit, 55-69 Moderate fit, <55 Weak fit). Show the 4 component averages.
3. Why it fits: "Matches your goal "<goal>" through: <matched career tags per course>."
4. Skill and career benefits: union of matched skill tags and career tags, grouped by course.
5. Total weekly hours: sum, plus a comparison with the student's workload preference band.
6. Validity checklist: one row per HC1-HC6 showing a green tick or red cross with its code and the evidence.
7. Trade-offs: take the top 3 eligible courses NOT selected, ranked by score. For each: name, score, and the career/skill tags it carries that the chosen set does NOT cover, plus the reason it was left out (lower score, slot clash with X, or category quota full). If nothing is lost, say "No relevant course was sacrificed."
Skill-gap note (optional, only if the data supports it): required career skills minus skills covered by cleared subjects and the chosen set.
</explanation_spec>

## 5. UNMET NEEDS AND IMPOSSIBLE CASES

If a requirement cannot be met, state exactly why using the data (unmet prerequisites, timetable clashes, not enough courses in a category). Example: "1 Management elective missing: only 1 eligible course; the other 3 require prerequisites you haven't cleared." Never invent courses or bend rules.

<unmet_needs_spec>
When Full (3 valid) is not reached, show a status banner:
- FULL: 3 valid combinations.
- PARTIAL: 1-2 valid combinations. Explain why a third does not exist.
- NONE: zero valid combinations. Give the exact blocking reason.
Reason codes and message template (always give numbers from the data):
- SUPPLY_SHORTAGE: "<Category>: <required> required, <eligible> eligible out of <total> offered."
- PREREQ_BLOCK: "<n> of <total> <Category> courses need subjects you have not cleared: <course> needs <subject>, ..."
- SLOT_BLOCK: "All eligible <Category> options clash with <course> in slot <slot>."
- WORKLOAD_BLOCK: only if a hard cap exists.
- DATA_BLOCK: "Cannot evaluate <course>: <field> missing."
Also show a "What would unlock this?" line listing the single smallest change (e.g. clearing subject X unlocks Y courses), but ONLY as information; never include those courses in results.
</unmet_needs_spec>

## 6. TESTING MODULE

"Run test for all students" must, for every row in Students.csv:
- run the engine
- re-verify prerequisites, timetable, category counts, workload limits, and score/ranking consistency
- show a results table (student, combinations found, status: Full / Partial / None / Validation failure, reason)
- show a summary: total tested, with valid combinations, partial/no-solution, validation failures
- offer "Download results as CSV"

<testing_spec>
Independence rule: the verifier must re-implement checks from raw normalised data, NOT call the same function the engine used, so a shared bug cannot pass itself.
Per student, verify and record pass/fail for: prerequisites; slot clashes; per-category counts; duplicates; hard workload (if any); each combo's score equals a fresh recomputation (tolerance 0.01); ranking order is non-increasing with the tie-break applied; distinctness; at most 3 results; and that a Partial/None status carries a non-empty reason code.
Status logic: Full (3 valid, all checks pass), Partial (1-2 valid, all checks pass, reason present), None (0 valid, reason present), Validation failure (any check fails or the reason is missing).
Results table columns: student ID | name | combos found | status | reason code | failed checks.
Summary: total tested, Full, Partial, None, Validation failures, plus the % of students with at least one valid combination.
CSV export columns match the table. Build a Blob download with correct escaping of commas and quotes.
Add a built-in "self-test" button with 4 synthetic fixtures (clearly labelled as synthetic and never mixed into real stats): prerequisite failure, same-slot clash, category shortage, and 0-required category. State the expected outcome of each.
</testing_spec>

## 7. OPTIONAL EXTRAS (only after the core works)

- What-if: change workload preference and instantly recalculate
- Download/print recommendation report
- Chart.js bar chart of weekly hours and scores

Build in this order: what-if workload toggle, then the weights panel, then print/download report, then Chart.js. Do not start an extra until all of the core checklist passes.

## 8. CODE QUALITY

- Comment each function; separate functions: parseData(), validateData(), checkHardConstraints(), scoreCourse(), generateCombinations(), rankCombinations(), explainCombination(), runAllTests(), renderUI()
- Handle edge cases: missing values, case/spacing differences in subject names, students needing 0 electives in a category
- Prioritise correctness of the recommendation logic over visual flair

## OUTPUT

First give the Step 0 data findings and any assumptions, then the full code for index.html, style.css and script.js, then short instructions for running it (open index.html, or use a local server such as VS Code Live Server).

<output_contract>
Order of the response:
1. Step 0 audit (per the step0_data_audit block), including the numbered assumptions.
2. Short design notes: CONFIG contents, the synonym map, and the chosen slot-clash rule.
3. Complete code in three separate fenced blocks: index.html, style.css, script.js. No "..." elisions, no TODOs, no placeholder logic.
4. Run instructions: open index.html directly (upload fallback) or serve with a local server (e.g. VS Code Live Server) so fetch() can auto-load the CSVs.
5. Known limitations and what you could not verify.

Before replying, silently confirm and then state "Self-check passed" or list the failures:
[ ] every function named in section 8 exists and has a comment
[ ] weights are in CONFIG and sum to 1
[ ] all HC rules are in one registry and used by both the engine and the UI validity list
[ ] no result can contain a course that fails a prerequisite
[ ] fewer than 3 results is handled without padding
[ ] 0-required categories handled
[ ] empty, loading and error states exist; the app never shows a blank screen
[ ] labels, aria-live regions for results, keyboard focus and colour-plus-icon (not colour alone) badges are present
[ ] print stylesheet hides nav and buttons
[ ] CSV parsing tolerates BOM, quoted commas and CRLF
</output_contract>

<!-- ====================== PROMPT END ====================== -->

---

## Appendix – Notes from the Project Guide (reference only, not part of the prompt)

- The weights (0.40 / 0.25 / 0.20 / 0.15) are illustrative starting assumptions, not facts from the problem statement; justify and tune them using the actual data.
- CGPA is one indicator of academic preparedness, not a complete measure of ability.
- Same-slot courses conflict only if the timetable data or rules establish that they overlap.
- Workload preference is a soft preference unless a strict maximum is explicitly supplied.
- If fewer than three valid combinations exist, show fewer; do not create invalid alternatives.
- Missing or ambiguous data should be flagged rather than silently treated as valid.
- Optional enhancement from the guide: skill-gap analysis, only where the data supports it.
- Final checklist from the guide: both real CSVs analysed first; browser-only app; top three results are valid combinations (not isolated course rankings); all mandatory constraints enforced; every recommendation explains benefits and trade-offs; unmet and impossible requirements clearly reported; algorithm tested against every student.
- Design reference (Phase 1): a light, card-based dashboard with a warm near-black rounded sidebar, large bold greeting, soft-gradient stat tiles, a segmented tick progress bar, and a red-orange gradient primary action. Colour values in the prompt are estimates from a screenshot and live in --eiq- CSS variables so they can be tuned.
