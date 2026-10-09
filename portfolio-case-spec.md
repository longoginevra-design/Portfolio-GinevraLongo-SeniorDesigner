# Portfolio case study spec

This file is the single source of truth for each case page's structure and copy.
Revisions get written into the relevant case's section here — never kept as a
parallel/competing document.

## Site-wide architecture (Aug 2026)

`index.html`'s CSS and JS are extracted into shared `styles.css` and `main.js`
at the repo root, loaded by all four pages (`index.html`, `case-01/02/03.html`)
via `<link rel="stylesheet" href="styles.css">` and
`<script src="main.js" defer></script>` — relative paths, no leading slash,
safe for a GitHub Pages project page. `case.css`/`case.js` were **not**
retired — they remain the second, case-page-specific stylesheet/script,
loaded after `styles.css`, and still carry real case-only infrastructure
(`.text`/`.wide` width system, section rhythm, `.inverted`, `.media-frame`,
the sticky-scroll stepper, toggle-group wiring, nav progress bar). Case pages
now effectively load three CSS sources in cascade order: `styles.css` →
`case.css` → the page's own remaining inline `<style>` (whatever's genuinely
unique to that page — case-01's hero-split and diagrams, case-02's
component-viz/docv system, case-03's price-list/walker/funnel/record-card).

Reconciliation done as part of this: case-01's inline `<style>` had grown to
duplicate ~150 lines of `index.html`'s own CSS (`.case-top`, `.case-title`,
`.case-lede`, `.case-body`, `.spec`/`.spec-row`, `.label-inline`,
`.phases`/`.phase`, `.decisions`/`.decision`, `.reveal`/`.visible`) from the
narrative-revision rounds before this file was shared — all removed, now
served by `styles.css`. `.rise`/`.is-visible` (the old case-only reveal
system) is fully retired from `case.css`/`case.js`; all three case pages now
use `.reveal`/`.visible` (see Motion, below). The old single-link `.next`
component is retired from `case.css`, replaced by `.pager` (from
`styles.css`) on all three case pages. `.nav.is-scrolled` was renamed to
`.nav.scrolled` and `case.js`'s own nav-border toggle removed — `main.js`'s
scroll listener (targeting `#nav`, now present on every page) does that job
alone.

**Scope not covered by this pass:** case-02.html and case-03.html were not
migrated to the 5-section `h2`-label/`h3`-pre-title structure Case 01 uses
below — they remain on the original 7-section skeleton (`.eyebrow`,
`.blocks`/`.block`, `.stats`, etc.), and their extensive custom visualisation
CSS (price-list, component-viz, docv, walker, funnel, decline/negotiation
SVGs) still uses `var(--mono)` throughout. Neither was in scope for the
cross-page brief that drove this architecture change (which named six
specific changes: nav mark, case-page titling, reveal migration, emphasis,
pager, widows) — flagged rather than silently left inconsistent with the
site's "two fonts only" rule, which currently holds only for `index.html`
and `case-01.html`.

*Note (Aug 2026, final-refinements round, below): case-02 and case-03 were
in fact both fully rebuilt onto the 5-section structure in later rounds this
session — this "scope not covered" note is historical, from the point in the
session when the site-wide architecture brief itself landed, and is left
here as-written rather than silently deleted, since the file's own rule is
that revisions get appended, not retconned.*

---

## Site-wide — final refinements round (Aug 2026)

Five fixes, all cross-cutting or `index.html`-only:

1. **Inverted-section text colour, case-01 and case-03**: consolidated into
   shared `case.css` rules (`.inverted .case-body`, `.inverted .case-body
   strong`, `.inverted .case-body em.key`, `section.inverted .sec-label`,
   all `color: var(--white)`), replacing every page-local inline
   `style="color:..."` hack. This is what actually fixed case-03's real bug
   — its inverted-section `<p>` tags had never had a colour override at
   all, silently inheriting the failing `--gray-700` default; case-01's
   paragraphs had an inline `--gray-300` override that was serviceable but
   redundant once the shared rule existed, so it was removed too. See the
   Case 01 and Case 03 sections below for the full contrast measurements.
2. **Case 01 grid diagram, real proportions**: rebuilt as two independent
   `<svg>` elements (not one shared viewBox) so each can reflow on its own
   below 640px — `align-items: flex-end` on the flex row does the
   bottom-alignment instead of shared absolute y-coordinates. See the Case
   01 section below.
3. **Nav mark, real circle**: `.name-dot` in the nav is now an empty
   `aria-hidden` `<span>` (`width/height: 5px`, `border-radius: 50%`,
   `background: var(--accent)`, `margin: 0 3px 0 4px`), replacing the old
   scaled text period, whose near-zero side-bearing let the `scale(2.1)`
   hover state grow into the preceding "o" in "Longo". Hover scale reduced
   to `1.8`. The hero `<h1>`'s dot is unrelated and was given its own class,
   `.hero-dot` (colour: accent only, no hover/scale) — it's a heading, not
   a control, so it never shared the nav's interactive behaviour and
   shouldn't share its class either now that the nav dot has real geometry.
   No `vertical-align` nudge was added beyond the browser default
   `baseline` — a real period's glyph also sits on the baseline, so a
   same-height circle should already read as sitting in the same place;
   this is reasoned from the box model, not confirmed in an actual browser
   (none is available in this environment), and is worth a visual spot
   check.
4. **Tooling copy, "How I work with engineers"**: the two Cursor mentions
   were factually wrong (Claude and Claude Code in VS Code are the actual
   tools) — fixed in the profile row; the footer's mention is retired
   entirely by fix 5, below.
5. **New "How this site was made" block**: added to `index.html` between
   "How I work" and the footer (after "Get in touch", immediately before
   `<footer>`), reusing only `.section-block`/`.container` (layout),
   `.label-inline` (heading — no `.section-head`/numeral, deliberately
   lighter than a numbered section) and `.prose-body` (copy). Zero new CSS.
   The footer's old opening line ("Hand-coded HTML, CSS and JavaScript" —
   false; self-contradicted two sentences later by "I directed Claude and
   Cursor to build it") is replaced with a one-line byline, "Designed by
   Ginevra Longo, built with Claude Code — no framework, no template,
   deployed from GitHub." **Flagged deviation**: the brief specified exact
   copy for the new block but only said to "fix" the footer sentence,
   without giving replacement text — given the new block now carries the
   full build narrative, repeating it in the footer would be redundant, so
   the footer was shortened to a byline rather than rewritten as a second
   full paragraph. Worth a look to confirm the tone lands right.
   `<span class="tbc">time tbc</span>` is gone (superseded by "Four days"
   in the new block, stated as fact); the `.tbc` CSS rule was confirmed
   unused across the four live pages and deleted from `styles.css`. A
   fifth, unrelated file — `portfolio.html`, an orphaned earlier draft with
   its own inline `<style>`, not linked from any live page, untouched since
   before this session — still uses a `.tbc` class of its own; since it
   doesn't consume the shared `styles.css` at all, this has no effect on
   it and no action was taken on that file.

---

## Site-wide, punctuation and navigation round (Aug 2026)

Seven changes. Items 1, 2 and 6 are cross-page; the rest touch `index.html`
only, except item 5 which reconciles the case pages against it.

**Supersedes, explicitly:** the `INSIDE THE PRODUCT — DESIGN DECISIONS` label
spelling wherever it appears above; `.case-nav`'s `position: fixed`;
`index.html`'s hero-intro copy; the `SECTORS I'VE WORKED IN` profile label.

### 1. No em dash anywhere on the site

`—` (U+2014) is banned in every page, every caption, every diagram label,
every `alt` and `title` string, and every `<title>`. The reason is not
typographic: it is the single most recognisable tell of machine-written copy,
and on a portfolio the writing is itself a craft judgement about her.

**Do not run a find-and-replace.** A blind swap to commas produces comma
splices and a blind swap to ` - ` produces something that looks cheaper than
what it replaced. Resolve each instance with one of four moves, chosen by
what the dash was actually doing:

| What the dash was doing | Move |
|---|---|
| Joining two independent statements | Full stop, two sentences |
| Introducing a consequence, a list or a restatement | Colon |
| Fencing a short aside inside a sentence | Comma pair |
| Fencing a genuinely parenthetical aside | Parentheses |
| Separating peer items in a label or meta row | `·` middot |

If a chosen move produces a comma splice or a run-on, split the sentence
instead. Never substitute ` - ` (spaced hyphen) or a semicolon: those are the
two marks that reveal the substitution happened mechanically.

**Survives:** `–` (U+2013) inside numeric and date ranges only, as in
`Jan–Dec 2025` and `30–50%`, never in prose. `·` middot separators in
`.spec`, `.meta-item`, kicker rows and section labels. Hyphens inside
compounds (`task-oriented`, `cross-product`).

**Enforcement gate:** `grep -rn '—' *.html` must return nothing. Run it as
the last step and paste the output. This gate is what makes the rule safe
even where copy is rewritten from scratch rather than corrected.

**About this file.** Every quoted copy block in this spec is now dash-free
and is authoritative for the text that ships. The narrative round-by-round
entries around them still contain em dashes; they are historical record and
nothing is ever copied out of them into a page. Anything newly written into
this file follows the rule.

**Report** the count removed per file, and any instance where the right move
was not obvious, rather than guessing silently.

### 2. Section label respelled

`INSIDE THE PRODUCT · DESIGN DECISIONS`, on all three case pages, in the
`h2`. Middot because it is already the site's peer separator in `.spec` rows,
`.meta-item` rows and the kicker line, so no new idiom is introduced. The
other four labels are unchanged.

### 3. `index.html` hero intro, shortened

The current intro runs 95 words and the feedback is that its length costs it
the read. Replacement copy, 70 words, same four content units in the same
order, three `em.key` emphases in the same three positions:

```html
<p>Product designer and UX/UI designer, 4+ years turning <em class="key">complex technical domains</em> into usable interfaces. In technical and industrial sectors I've taken products from problem framing through UX/UI design and design system architecture into development, next to the engineers building them.</p>

<p>Strongest where <em class="key">the problem is technical and the users are domain experts</em>, working between engineering, product and business. Now moving from consulting into a product company, to <em class="key">stay with one product past its first release</em>.</p>
```

Copy this verbatim. Do not improve it, do not re-mark the emphases, do not
add a fourth.

Withdrawn from the intro, deliberately, both flagged and accepted:
- *"with a track record of moving from an ambiguous brief to a concrete
  direction"* — the one claim in the intro no case demonstrates, and it
  contains `brief`, which places her in consulting.
- *"led a team of 4 designers"* — the count is still unverified (see Open
  items). If it is confirmed and she wants it back, the insertion point is
  the end of the second sentence: `..., next to the engineers building them,
  leading a team of 4.` Nowhere else.

### 4. Profile: sectors row states the role

Label changes from `SECTORS I'VE WORKED IN` to
`SECTORS I'VE WORKED IN AS PRODUCT DESIGNER`. Tags unchanged.

The label idiom is uppercase and letter-spaced on a fixed-width column, so
this longer string will wrap. Two lines is acceptable, three is not. **Check
it at 380px, 768px and 1440px before reporting done.** If it reaches three
lines, do not shrink the type and do not narrow the letter-spacing: revert
the label to `SECTORS I'VE WORKED IN` and open the row's content with
`Product designer in:` ahead of the tags instead. Report which of the two you
ended up with.

### 5. Role and date reconciliation: `index.html` wins

The role and date values in the case pages' `.spec` panels disagree with the
work-item entries on the home page. **`index.html` is the source of truth**;
the case pages align to it.

Procedure, in this order:

1. Read the role and date for each of the three work items in `index.html`.
2. Read `My role` and `Timeline` in each case page's `.spec`.
3. **Report the full diff before changing anything**: home value against case
   value, per field, per case.
4. Then apply, home value overwriting case value.

Two conditions:
- Where the home has a real value and the case page has a placeholder
  (`[to confirm]`, `.tbc`), the home value wins.
- Where both are placeholders, both stay placeholders. Nothing is invented to
  close a gap.

Watch case 01 specifically: the home may carry 2025 while the Red Dot page
linked in item 7 is a Design Concept 2024 entry. If those two disagree,
report it rather than resolving it.

### 6. Case-page nav: sticky, auto-hiding, present at the bottom

**The defect.** `.case-nav` is `position: fixed`, so it reserves no space in
the flow. `.case-top`'s padding absorbs it at desktop width, but at 380px the
nav wraps to two lines and sits over the `h1`, which makes the case title
unreadable at the exact moment the page is first seen.

**Part one, never overlap at rest.** `position: sticky` instead of `fixed`.
It stays in the flow, so the space is reserved by construction at every
breakpoint and no height needs measuring in JS.

```css
.case-nav {
  position: sticky;
  top: 0;
  z-index: 50;
  background: var(--white);
  border-bottom: 1px solid var(--gray-300);
  transition: transform 0.28s ease;
}
.case-nav.is-hidden { transform: translateY(-100%); }
@media (prefers-reduced-motion: reduce) {
  .case-nav { transition: none; }
}
html { scroll-padding-top: 72px; }
```

**Structural precondition, and the thing most likely to break this:**
`position: sticky` does not work inside an ancestor that clips. `.case-top`
carries `overflow: hidden` to contain the gradient's `right: -6vw`. So
`.case-nav` must be a **sibling of `.case-top`, placed before it**, not a
child. If it is currently a child, move it and confirm the gradient still
does not produce a horizontal scrollbar.

**Part two, hides on the way down, returns on the way up.** In `main.js`, not
per page:

```js
(function () {
  var nav = document.querySelector('.case-nav');
  if (!nav) return;
  var last = window.pageYOffset;
  window.addEventListener('scroll', function () {
    var y = window.pageYOffset;
    if (Math.abs(y - last) < 8) return;            // ignore jitter
    var atBottom = y + window.innerHeight >= document.documentElement.scrollHeight - 4;
    nav.classList.toggle('is-hidden', !atBottom && y > last && y > nav.offsetHeight);
    last = y;
  }, { passive: true });
})();
```

**Why `atBottom` is in there and is not optional.** The footer pager is gone
from the case pages, so this nav is now the only route from one case to the
next. Without the bottom condition, a reader who reaches the end of the page
by scrolling down has no navigation at all and has to scroll back up to find
it.

Nothing here is a new visual value: `--white`, `--gray-300` and the 1px
hairline are `index.html`'s. `z-index: 50` and the transform are behaviour,
not style.

### 7. Red Dot link

The award citation becomes a link:

```html
<a href="https://www.red-dot.org/project/ims-technologies-new-hmi-generation-72251"
   target="_blank" rel="noopener noreferrer">Red Dot Award: Design Concept 2024 · Best of the Best</a>
```

Link styling is inherited from `index.html`'s existing inline link treatment.
Do not author a new one, and do not add an external-link icon.

Two facts about the destination page, both verified by opening it, both
consequential and both accepted by her:

1. **Her name is not in the credits.** The listed NiEW designers are Andrea
   Zambonini, Ewoud Westerduin, Luca Tognazzi, Andrea Ceci, Andrea Violante
   and Daniele De Cia; team leads Teresa Alaniz and Clara Reali. So the
   citation must not be phrased as a personal award. `project by NiEW`, or
   equivalent studio attribution, stays adjacent to it.
2. **The page names the client**: IMS Technologies Spa, converting industry.
   Case 01 describes an OEM group of comparable size and character, so the
   two are joinable by a reader who follows the link. This does not change
   the NDA approach on the assets, which protects the screens rather than the
   client identity, and no case-page copy names a client anywhere.

### Regression checks for this round

1. `grep -rn '—' *.html` returns nothing. Paste the output.
2. `grep -rn ' - ' *.html` shows no instance standing in for a dash between
   clauses.
3. `grep -rn '–' *.html` shows en dashes only inside numeric or date ranges.
4. All three case pages read `INSIDE THE PRODUCT · DESIGN DECISIONS`.
5. Hero intro is exactly the two paragraphs above, with exactly three
   `em.key` spans.
6. Sectors label renders in two lines or fewer at 380px, 768px and 1440px.
7. Role and date diff reported before it was applied, and no placeholder was
   filled with an invented value.
8. At 380px on each case page: on load the `h1` and `.case-lede` are fully
   visible with nothing over them; scrolling down 200px hides the nav;
   scrolling up 100px returns it; at the page bottom the nav is present.
9. Keyboard: the nav's links are still reachable and focus is visible while
   the nav is in its returned state.
10. `prefers-reduced-motion: reduce`: the nav appears and disappears with no
    transition, everything else unchanged.
11. Red Dot link opens in a new tab and carries `rel="noopener noreferrer"`.
12. No horizontal scrollbar at 380px, 768px or 1440px after `.case-nav` was
    moved out of `.case-top`.
13. Zero console errors. No layout shift on load caused by the nav now
    occupying flow space.

---

## Case 01 — Designing to ship

*Oct 2026 — copy pass: body copy across the header and all five sections
replaced per `brief-case-01-copy.md`; structure, markup, classes, CSS,
diagrams, images, alt text and captions unchanged. Supersedes every copy
block quoted below (Header `.case-body`, 01–05).*

*Case 01 restructured — narrative revision, Aug 2026. Withdrawn: seven-section
skeleton, sticky sequence, interactive configuration switcher, meta strip,
"What this shows" tag row, WORKING WITH ENGINEERING as a standalone section.*

*Aug 2026 update: hero replaced with a light/dark draggable-split of two real
screens (was a single composite placeholder); gallery withdrawn outright
rather than left conditional on assets.*

*Aug 2026 update 2: hero mode labels moved outside the image as a legend row
(were overlaid on the image itself) with a visible accent knob added as the
drag affordance; header intro rewritten to two paragraphs with three accent
emphases; project-info panel restyled to hairline rows off the accent, not
grey; decision block 03 gained an inline grid diagram (previously text-only
by rule — that rule is superseded); site-wide reveal-on-scroll audited and
reused rather than duplicated, and given a page-scoped no-JS fallback it was
missing.*

*Aug 2026 fix round: update 2 shipped with four defects, all fixed — see the
next entry, since the fix that mattered (defects 3/4) was itself superseded
almost immediately.*

*Aug 2026 visual-system pass — supersedes the previous round's defect-3/4
fix. Root cause of both: this page had been styled as an invented, parallel
system instead of reusing `index.html`'s real one. Fixed properly this time
by pulling the actual page structure onto `index.html`'s existing case-page
classes:*
- *Header rebuilt as `.case-top` (`index.html`'s wrapper, gradient and all)
  containing `.backlink`, `.case-title`, `.case-lede`, `.case-body`, and the
  info panel as `.spec`/`.spec-row`/`.spec-row--key` — the hand-written
  `.project-info` is gone entirely.*
- *Monospace is gone, everywhere. `index.html` has no third font family;
  every label on this page now uses the DM Sans uppercase idiom
  (`.label-inline` values, or the closest matching label class) instead of
  `var(--mono)`.*
- *Emphasis in the intro flipped from bold-accent to italic-accent, matching
  `.hero-intro em`/`.case-title em` elsewhere on the site — there is no
  bold-accent idiom on this site.*
- *THE IMPACT's cards and DESIGN DECISIONS' blocks now reuse `.phases`/
  `.phase`/`.phase-number` and `.decisions`/`.decision` respectively,
  `index.html`'s own components, unchanged.*
- *Reveal-on-scroll switched from the shared `.rise`/`.is-visible` (case.css/
  case.js) to `index.html`'s own `.reveal`/`.visible`, copied onto this page
  verbatim including its observer — this page no longer shares a motion
  system with case-02/case-03, it shares one with the home page instead. See
  Motion below for why, and for the one addition (a safety timeout) on top.*
- *Diagram label text moved from mono to `.dgm-label`/`.dgm-sub` in DM Sans
  (9px), and the `8` count numeral moved to serif — matching `.phase-number`/
  `.section-head .num`'s use of serif for standalone numerals.*
- *The diagram-stretch fix (explicit `width`/`height` on every `<svg>`) and
  the withdrawal of the stroke-dashoffset draw-in both carry forward
  unchanged from the previous round.*

*Aug 2026 site-wide consolidation — see "Site-wide architecture" at the top
of this file for the styles.css/main.js extraction. On this page
specifically: the header gained `.work-kicker` ("Designing to ship") above
the `h1`, and the `h1` itself changed from the series name to the literal
work-item title, "HMI Redesign for an Industrial Group" (copied verbatim
from `index.html`'s `.work-title`) — the series name reads as `<title>`
alongside it now too. The ~150 lines of CSS this page had duplicated from
`index.html` (before styles.css existed to share it) were deleted from this
page's own `<style>`, now served by the shared file. Nav gained the
name-dot hover mark. `<strong>` added on "eight interfaces" (THE PROBLEM)
and "one foundation to design and maintain" (THE SOLUTION, with an inline
white-text override — `.case-body strong`'s default black would be
invisible on this section's black background) and on "56px" (DESIGN
DECISIONS block 02). The closing single-link footer is now the shared
`.pager`, both directions always present.*

### Structure

Five sections, in this order, preceded by a three-part header. No sticky
sequence. No toggles, switchers or interactive visuals anywhere on this page
except the hero's light/dark reveal, which has no state and changes nothing
else on the page.

```
HEADER          title · one-line · Why it's here · PROJECT INFO panel
HERO            light/dark split of one screen, draggable divider
01 THE PROBLEM
02 THE SOLUTION                       ← contains the page's only diagram; this is the inverted section
03 THE IMPACT
04 INSIDE THE PRODUCT · DESIGN DECISIONS   ← three numbered blocks, inline crops
05 WHAT I'D DO DIFFERENTLY
```

Section labels ("THE PROBLEM", "THE SOLUTION", "THE IMPACT", "INSIDE THE
PRODUCT · DESIGN DECISIONS", "WHAT I'D DO DIFFERENTLY") are the same on all
three case pages, rendered as `h2`. Under each label sits a pre-title (`h3`)
carrying the page-specific argument. Both are structural — never merge them.

### Header

Structure is `index.html`'s own `.case-top` (padding, `overflow:hidden`,
and the `.hero::after` radial-gradient accent copied onto it verbatim,
rescoped from `.hero` to `.case-top`) wrapping a `.container` (`index.html`'s
plain `max-width:820px;margin:0 auto`, added since the given header markup
had no width cap of its own — without it the header would run full viewport
width while every other section on the page is capped at 820/1140px).

- `.backlink`: "← Back", to `index.html`.
- `h1.case-title` (`index.html`'s real case-title sizing, not `.hero h1`
  which is the landing-page size): "Designing to ship"
- `.case-lede`: "Two brands, one machine, eight interfaces to build, and a
  development plan that couldn't hit its deadline."
- `.case-body` (two paragraphs), three `em.key` emphasis spans — italic
  accent, matching `.hero-intro em`/`.case-title em` elsewhere on the site
  (there is no bold-accent idiom anywhere on `index.html`; an earlier round
  of this page used bold-accent and that was wrong), each readable
  standalone: "This is the case I followed *from the first architectural
  decision through implementation and quality checks* with the development
  team. The design system wasn't in the request. *I proposed it*." / "Then I
  designed both products on top of it, screens and physical controls, and
  *stayed inside the build* until the final checks were done." A fourth
  emphasis would turn the block from scannable into highlighted — don't add
  one.
- `dl.spec` — `index.html`'s real info-panel component (`.spec`/`.spec-row`/
  `.spec-row--key`), not the page's own invented `.project-info`, which is
  gone. Two optional deviations applied, both existing tokens: panel
  background `--accent-light` instead of `--gray-100`, and the lead row's
  `dt` in `--accent` — kept for consistency with this page's own established
  "hairlines off the accent, not grey" direction from earlier rounds.
  - My role (`.spec-row--key`, serif `dd` + sans `<small>` sub-line — its
    native treatment): "Design Lead" / "Led the design of the screens for
    both machines, architected the design system, and ran the project:
    planning, client relationship, and oversight through development.
    Development by the client's IT team."
  - Sector: "Manufacturing · industrial OEM group (~€390M), built through
    European acquisitions"
  - Timeline: "Jan – Dec 2025"
  - Team: "3 designers · client front-end and back-end teams"
  - Product: "Embedded HMI · 2 machines · 2 screen sizes · 2 modes"

Only the lead row (`My role`) may run to three lines; if any other row wraps
past two, the copy is wrong — flag it, don't shrink the type to compensate.

**Contrast flag, not fixed:** `.spec-row dt` reuses `index.html`'s own value,
`--gray-500` on `--gray-100` (or `--accent-light` with the deviation above) —
measured at ~3.0–3.1:1, below the 4.5:1 small-text threshold. This is
`index.html`'s own value for this exact use (also present in its live,
currently-rendered `.hero-label`/`.work-date`/`.footer-note`, all `--gray-500`
on light backgrounds), not something introduced on this page, and the
standing instruction this round was to reuse home's tokens over any literal
value here — so it wasn't changed. Worth a site-wide look, not a case-01 fix.

### Hero — light/dark split

One screen, two modes, split down the middle: light on the left, dark on the
right. A draggable divider moves the split so either mode can be seen in
full. Two source assets, not a composite — the split is done in CSS/JS from
two real screenshots (`case01-hero-light.*` / `case01-hero-dark.*`), never a
pre-merged image.

Non-negotiable properties:
1. Works with JS disabled/failed — the split sits at 50% and the hero is a
   valid static image. Never hide the overlay behind a JS-set class.
2. The control is a real `<input type="range">`, transparent and stretched
   over the image — not a custom `mousedown`-drag handle. Keyboard-operable,
   screen-reader-announced, visible focus.
3. Both images must be the same screen at identical pixel dimensions —
   verified before wiring, not assumed from filenames. A mismatch is a stop
   condition, not a design decision.
4. `LIGHT MODE` / `DARK MODE` labels (DM Sans, uppercase, `.meta-item`-style
   treatment — no monospace font exists on this site) sit outside the image,
   above it, as a legend row — inside the frame they vanish against screen
   content. They're also the only thing preventing the split from reading
   as a before/after improvement slider, the wrong argument entirely.
5. A visible accent knob, centred on the seam and moving with it, is the
   drag affordance — `pointer-events: none` on the knob/seam so the
   full-bleed transparent range input underneath still takes the drag
   anywhere on the image, not just on the knob itself.
6. No auto-sweep, no "drag me" tooltip. The knob is the invitation; nothing
   moves on its own, so `prefers-reduced-motion` needs no special handling
   here.

### Gallery — withdrawn

No gallery on this page. The three extra screens in the asset folder cover
one brand and one mode only, and a browsable strip with no argument attached
invites a question it can't answer. Screens earn a place on this page only
where a caption can say what decision they show — i.e. the inline-crop slots
inside DESIGN DECISIONS below, never a standalone browsable section.

### 01 — The problem

**Pre-title:** Every product started from zero

> The group had grown by acquisition, and two of its brands shipped the same
> kind of machine with interfaces rebuilt from scratch each time. The plan
> was to do it again: two products, two screen sizes, light and dark mode —
> eight interfaces designed and maintained one by one, against deadlines
> already fixed.

### 02 — The solution

**Pre-title:** The system nobody asked for

The page's one inverted section. Contains the page's only diagram (see
below).

> The request was for screens. The problem underneath it was switching brand
> and mode without rebuilding, and that is an architecture question. So I
> proposed a design system built on variables: one foundation, every
> configuration derived from it.
>
> Navigation and interaction are the same across the group; the brand lives
> in the visual layer. An operator trained on one machine can run the other.

**Diagram** — inline SVG, technical-drawing register (hairlines, DM Sans
uppercase labels at 9px — no monospace font on this site, no
gradients/shadows/icons, `currentColor` throughout): one FOUNDATION block
resolving through brand/mode/device branches into eight labeled
configuration marks, plus a dashed "+ NEXT BRAND" slot showing further
brands enter by swapping the foundation. A dimension bracket marks the
eight, with the `8` itself set in serif — a standalone numeral takes the
same treatment as `.phase-number`/`.section-head .num` elsewhere on the
site. Below 768px, drop the eight text labels (the branching structure
alone still carries the argument) — never shrink SVG text below 9px to
compensate. Lives in the `.text` (820px) column, not `.wide` — `--max-width`
is 820px, which is why both diagram viewBoxes are 820 wide: at full column
width, 1 SVG unit equals 1 CSS pixel.

Uniform scaling only: every `<svg>` (this diagram and block 03's grid
diagram) carries explicit `width`/`height` attributes matching its
`viewBox`, so `width: 100%; height: auto` in CSS has a reliable intrinsic
ratio to scale from — an svg with only a `viewBox` and no size attributes is
not a safe bet for `height: auto` across engines, and was the actual cause
of both diagrams once rendering at roughly double their correct height.
No `preserveAspectRatio` override, no forced height, no `min-height` on the
figure or its ancestors.

Diagrams do not animate their own strokes. `stroke-dasharray` /
`stroke-dashoffset` are never animated anywhere on this site — a
stroke-dashoffset draw-in was tried once, shipped broken (both diagrams
rendered blank, since a reveal that never completes leaves every dash at
zero length), and is withdrawn for good. The only permitted use of
`stroke-dasharray` is a static dashed line, like the "+ NEXT BRAND" slot.
Diagrams reveal with the same `.reveal` fade as every other figure on the
page, or don't animate at all.

Verification: a reader of the diagram alone must be able to say "one thing
on the left produces eight on the right."

### 03 — The impact

**Pre-title:** One foundation, eight configurations, deadlines met

Rendered as cards, reusing `index.html`'s `.phases`/`.phase`/`.phase-number`
component unchanged — same class names, same values, including its numeral
slot (`01`/`02`/`03`) and its hover lift (kept as-is even though its own
timing doesn't match this page's `.reveal` transition — "reuse unchanged"
wins over internal consistency here). Bold lead clause of each point becomes
the card's `h4`; the remainder is the card's `p`. `.reveal` sits on the
`<ol class="phases">` itself, once, not on each card individually — the
three IMPACT bullets are one revealed unit, unlike DESIGN DECISIONS below
where each block reveals on its own. Three lead-bolded paragraphs, mapped
straight onto that structure:

> **8 configurations from 1 foundation.** 2 products × 2 screen sizes × 2
> modes, designed and maintained as one system. A new brand enters by
> swapping the foundation.
>
> **Deadlines met on a 12-month build,** at a fraction of the projected
> build cost, on a plan that had started as eight separate products.
>
> **Tokens adopted in the client's codebase.** The development team built on
> the same variable architecture, so a change of brand or mode is made once —
> in design and in code — instead of twice.

### 04 · Inside the product · design decisions

**Pre-title:** Three decisions, and what each one cost

Rendered as `index.html`'s `.decisions`/`.decision` component (28px numeral
column + content column, hairline top per block), reused as-is except for
one new rule: the numeral is `.decision-num`, not `.decision`'s own native
`.decision-mark` — set in serif/accent/1rem to match `.phase-number`/
`.section-head .num`'s numeral treatment, rather than `.decision-mark`'s
smaller dimmed-opacity look. Heading and body sit in a `.case-body` wrapper
inside each `.decision`. Each block's closing sentence is its cost/payoff
clause, marked up as a plain `<em>` inside `.case-body` — italic, quiet,
inheriting `.case-body`'s own grey, not accent-coloured. (This supersedes
the earlier `.tradeoff`-class treatment from prior rounds.) Never cut for
length.

> **01 · Physical controls and screen designed as one product**
> Part of the machine is operated by hand. I decided which functions stay
> physical, what their limits are, and how the screen shows what the
> operator's hands are doing. Slower than designing the screen alone.
>
> **02 · Touch targets at 56px, and less on screen**
> Operators set the machine up with gloves on, in noise and changing light,
> where a wrong parameter ruins a part. The client asked for the standard
> 48px minimum; I set targets at 56px and limited each screen to what the
> current task needs, with secondary parameters one tab away. Fewer rows of
> data on a project that wanted as much data as possible. The client tested
> the larger target with its operators and kept it.
>
> **03 · A grid the development team could scale alone**
> Two resolutions could have meant two sets of screens. I set up a grid
> engineering could adapt themselves, rebuilt the parameter list with the
> front-end and back-end teams in one session, and stayed through the build.
> More of my time went into their build cost than into new screens. That's
> why the deadlines became reachable.

Inline crops: at most one per block, only for blocks 01 and 02. Sourced from
whichever of the three extra screenshots actually shows the argument — opened
and matched by content, never assumed from filename:
- Block 01 (physical controls + screen together) — no asset in the folder
  shows physical controls in frame (all three extras are pure software
  screens), so this block stays text-only.
- Block 02 (gloves / bad light → task-first screens, secondary params one
  tab away) — `case01-designedforglovesandbadlight.png`, a Manual Function
  screen with a Functions/Parameters tab split, matches the argument.

Crops sit full column width below their block, caption at most eight words
in the DM Sans uppercase label idiom (no monospace font on this site).
Missing crop → block renders text-only, no placeholder frame inside a
decision block (unlike the Hero rule).

Block 03 gets an inline SVG grid diagram instead of a screenshot — the two
column grids (12-column / 8-column) side by side, same technical-drawing
register as the derivation diagram, reusing its `.dgm-label` rule rather
than duplicating it. Implemented as two independent `<svg>` panels in a flex
row rather than one combined viewBox, specifically so they can reflow to a
stacked column below 640px — a single monolithic SVG scales as one unit and
can't do that. Each panel carries its own `title`/`desc`; the shared
`<figcaption>` ties the pair together for sighted readers.

### 05 — What I'd do differently

**Pre-title:** The validation happened without me

> The client ran the operator sessions without me, and the card sort was
> done with the people building the product, not the people using it. I'd
> negotiate that access at kickoff. What I know of the operators' reaction
> is one result: they confirmed the 56px target.

Closing note, small, italic, quiet (`index.html`'s `.footnote` text
treatment — grey italic, no monospace) below the section: *Operator
validation run by the client.* This is the only role/credits-style note left
on the page — the old repeated role sentence at the page foot is withdrawn.

### Motion

This page now reuses `index.html`'s own reveal mechanism — `.reveal` /
`.reveal.visible`, not case.css/case.js's `.rise`/`.is-visible`, which the
other two case pages still use. That's a deliberate fork, not an oversight:
this round's instruction was to reuse the home page's real classes
throughout, and its class names are `.reveal`/`.visible`. The observer is
copied verbatim from `index.html`'s own inline script (same threshold
`0.08`, same lack of an explicit `unobserve` — repeatedly adding an
already-present class is a no-op, so it still only visibly animates once).
`Case.init()` (`case.js`) still runs for nav/scroll-chrome; its own
`Case.initRise()` call is now a harmless no-op on this page since no `.rise`
elements exist here anymore.

Applied to: each section's `h2` label + `h3` pre-title as one wrapped unit,
the `.phases` list as a single unit, each `.decision` individually, and
every figure (hero, both diagrams, the block-02 crop). Not applied to: the
header (`.case-top`, above the fold, must be present on load — no reveal
class at all, exactly like the landing hero), and not to individual body
paragraphs inside a section or a block — those render statically; only the
containing block/figure/heading-unit reveals.

**Two gaps found and fixed, both because case-01 no longer shares
infrastructure with case-02/case-03:**
1. `index.html`'s own reduced-motion block forces `opacity:1!important`
   universally, which is what protects `.reveal` there — but this page
   loads `case.css`'s reduced-motion block instead, which only special-cases
   `.rise`. Added a page-scoped `@media (prefers-reduced-motion: reduce) {
   .reveal { opacity: 1 !important; transform: none !important; } }` so
   `.reveal` content isn't stuck invisible under reduced motion here too.
2. No-JS fallback: same early inline `document.documentElement.classList.
   add('js')` in `<head>` as before, now paired with `html:not(.js) .reveal
   { opacity: 1 !important; transform: none !important; }` (was `.rise`).

**Safety timeout, as instructed:** a 2-second `setTimeout` after the
observer is wired up adds `.visible` to any remaining `.reveal:not(.visible)`
— the direct fix for a repeat of the blank-diagram failure, in case the
observer never fires for an element.

**Diagrams do not draw in.** A `stroke-dashoffset` draw-in was tried once
and withdrawn — it shipped broken, since a reveal that never completes
leaves every stroke at zero length, rendering both diagrams blank. Neither
`stroke-dasharray` nor `stroke-dashoffset` is animated anywhere on this
site now. Both diagrams use the same `.reveal` fade as any other figure.

### Withdrawn from this page

- The seven-section skeleton.
- The gallery / browsable screenshot strip.
- The sticky sequence (Case 01 has none).
- The interactive configuration switcher and any brand/mode/device toggle —
  the hero's light/dark divider is not a switcher: it reveals one
  pre-rendered pair of images, has no state, and changes nothing else on the
  page.
- The meta strip under the title — superseded by the `.spec` panel.
- The `What this shows:` tag row.
- `WORKING WITH ENGINEERING` as a standalone section.
- The role sentence repeated at the foot of the page (operator-validation
  note only survives, under section 05).
- Every use of a monospace font family on this page. There is no third font
  on this site; every former mono-styled label now uses the DM Sans
  uppercase idiom instead.
- The hand-written `.project-info` component, replaced by `index.html`'s
  real `.spec`/`.spec-row`/`.spec-row--key`.
- The `.tradeoff` class as used for DESIGN DECISIONS' closing sentences,
  replaced by a plain `<em>` inside `.case-body`.
- This page's use of the shared `.rise`/`.is-visible` reveal, replaced by
  `index.html`'s own `.reveal`/`.visible` (see Motion).

### Regression checklist

1. `h2` labels + `h3` pre-titles, read top to bottom, must form a coherent
   story on their own.
2. Exactly one hero block + two inline diagrams (derivation, grid) + at most
   two inline crops. No gallery, no carousel, no toggles.
3. With JS blocked, the hero still renders as a 50/50 split of two real
   images.
4. No asset uses `object-fit`, a fixed `height`, or a forced aspect ratio;
   every real `img` is `width: 100%; height: auto` (or the shared
   `.media`/`.media-frame` pattern, see `assets/README.md`).
5. Every image has non-empty, specific alt text; both diagrams expose
   `title` + `desc`.
6. Keyboard: tab reaches the hero range input, arrow keys move the split,
   focus visible, no focus trap.
7. Contrast: `.spec-row dd`/lead-row `dt` both clear 4.5:1+; the non-lead
   `dt` labels sit ~3.0–3.1:1 (see the flag under Header) — inherited from
   `index.html`'s own value, not fixed on this page.
8. `prefers-reduced-motion: reduce`: nothing animates — reveals resolve
   instantly (page-scoped `.reveal` override, since case.css's own
   reduced-motion rule doesn't cover this page's class); both diagrams
   render in their final state regardless, since they no longer animate at
   all under any setting.
9. Zero console errors, no layout shift after fonts load.
10. With JS blocked (and separately, scrolled straight to the page bottom on
    load): all copy visible, hero at a static 50/50 split, both diagrams
    fully drawn — nothing hidden waiting for a class. The 2s safety timeout
    is the backstop for the scrolled-to-bottom case; the `html.js` gate is
    the backstop for JS being blocked entirely.
11. Only one reveal mechanism on this page (`.reveal`/`.visible`, copied
    from `index.html` — nothing new invented). No
    `stroke-dasharray`/`stroke-dashoffset` animation anywhere.
12. Mobile at 380px: `.spec` panel stacks, the grid diagram's two panels
    stack, the derivation diagram drops its labels, no horizontal scroll
    anywhere.
13. Both diagrams render at their correct height — a 820×300 viewBox at
    820 wide renders 300 tall, not 600. Every rect/rail/bracket visible,
    nothing missing.
14. Grep this page for `IBM Plex`, `monospace`, `@font-face`, or any
    `fonts.googleapis` request beyond the existing DM Serif Display + DM
    Sans one — zero hits, confirmed. Grep the page's own `<style>` block for
    `font-size`/`font-weight`/`line-height` — every hit should trace to a
    value copied from `index.html` (`.case-title`, `.case-lede`,
    `.case-body`, `.spec`/`.spec-row`, `.phases`/`.phase`/`.phase-number`,
    `.decisions`/`.decision`), an SVG label rule, or `.decision-num`/
    `.closing-note` (the two small additions this round made, both
    justified in-line). None trace to an invented body-copy size.
15. Open the home page and the case page side by side at the same zoom —
    title, lede, body copy, label sizes and the gradient should be
    indistinguishable for every element reused from `index.html`.
16. No horizontal scrollbar at 380px, 768px or 1440px — `.case-top::after`'s
    `right: -6vw` is the thing to watch, and `.case-top` now carries
    `overflow: hidden` to contain it.
17. Intro emphasis (`em.key`) is italic accent, not bold — exactly three
    instances. The DESIGN DECISIONS closing-sentence `<em>` is plain italic,
    no accent — exactly three instances, one per block.

*Aug 2026, final-refinements round: the grid diagram previously drew both
displays at equal height, which read as "same device, different width" —
wrong, since display 1 is a real 17″ panel and display 2 is 10″. Rebuilt
with the actual diagonal ratio (10÷17 ≈ 0.588) applied to both width and
height of a 4:3 frame: 476×357 (17″) vs 280×210 (10″) — both dimensions
verified exactly 0.5882, within the 0.588±0.01 the brief asked for. Kept as
two independent `<svg>` elements (own local viewBox each, re-originated to
(0,0)) rather than one combined viewBox, specifically so each can rescale
independently below 640px — `.grid-diagram__row { align-items: flex-end }`
does the bottom-alignment, and the mobile media query re-asserts
`max-width: 58.8%` on the display-2 panel so it stays visibly smaller than
display 1 even stacked, rather than both being stretched to the same
width. Labels now read "DISPLAY 1 · 17″ · 12 COLUMNS" / "DISPLAY 2 · 10″ ·
8 COLUMNS" with a real U+2033 double-prime character, not a straight
quote. The 4:3 aspect assumption for both panels (needed to turn a single
diagonal ratio into both a width and a height) was not independently
confirmed against real hardware specs — implemented as given, flagged per
the brief's own fallback instruction ("if the panels aren't actually both
4:3, report rather than stretch").

---

## Case 02 — Designing to last

*Oct 2026 — copy pass: body copy across the header and all five sections
replaced per `brief-case-02-copy.md`; structure, markup, classes, CSS,
gallery, documentation-pair images, alt text and captions unchanged. Every
em dash on the page was also removed this round (not only within the
sections touched by the brief) and replaced with a comma, colon or full
stop. Supersedes the copy blocks below.*

*Aug 2026, header retrofit round: `.case-top`/`.container`/`.backlink`,
`.work-kicker` "Designing to last", `.case-title` h1 changed from the old
poetic line to the literal work-item title. `.rise` → `.reveal` throughout.
Old single-link footer replaced with `.pager`.*

*Aug 2026, narrative revision v2 (supersedes v1 and the header-only round
above): full migration to Case 01's structure — three-part header, five
`h2`-labelled/`h3`-pre-titled sections, `.phases` for THE IMPACT,
`.decisions` for DESIGN DECISIONS. `.sec-label`/`.sec-pretitle`/
`.closing-note` moved out of case-01's page-local `<style>` into the shared
`case.css`, since this page needed the identical pattern — the exact
duplication the site-wide consolidation was meant to prevent. Every
`var(--mono)` reference is gone from this page as a direct consequence of
the rewrite (not a deliberate mono-purge pass — the old component-viz/docv
system that used it was replaced wholesale). Visual budget: exactly two
blocks, a gallery and a documentation-image pair — no diagram, no inline
crops in the decision blocks.*

*Aug 2026, v2 refinement (same v2, revised after real documentation-pair
assets landed): Why it's here cut from two paragraphs/three emphases to one
paragraph/two — resolves cleanly, no restructuring needed. PROJECT INFO
dates resolved ("Jan 2025 – present", `.tbc` placeholder gone) and the
Sector row's revenue figure dropped. THE IMPACT's three lead sentences
rewritten — the 70% figure moved from the lead into the body of card 1,
replaced by "Renewed for a fourth consecutive year"; cards 2/3 leads now
state the numbers directly ("Adopted by 3 new teams, 6 in total" / "3 junior
designers trained"). Documentation-pair images replaced outright — see
Assets.*

*Aug 2026, timeline-consistency pass: the underlying facts are now stated
once, explicitly, and every year/count reference in the copy resolves
against them — project starts 2022, Ginevra takes over Jan 2025 (three years
in, its fourth year), 70% change-request decline by that fourth year,
renews anyway into 2026 (a fourth *consecutive renewal* — a different four
from "fourth year," deliberately never placed in the same sentence as it),
ongoing mid-2026. Touched: THE PROBLEM ("since 2022," was "for three
years"), THE SOLUTION ("Three years in," was "After three years"), THE
IMPACT's `h3` pre-title ("Renewed in the year it looked finished," was
"...in year four...") and card 1's body (rewritten around the same facts,
was framed as "by year four"/"from the outside"), and WHAT I'D DO
DIFFERENTLY's second paragraph ("a project already three years old"/"in the
first weeks"/"my first month," was "a three-year-old project"/"from day
zero"/"the first month instead of the fourth year" — the last phrase
specifically violated the new "never both ordinals in one sentence" rule).*

### Structure

```
HEADER          kicker "Designing to last" · h1 (work-item title) · lede · Why it's here (1 paragraph) · PROJECT INFO (.spec)
GALLERY         6 images, one at a time, prev/next — discovered from assets/cases/02/, not hardcoded
01 THE PROBLEM
02 THE SOLUTION      ← documentation pair (component doc, then pattern doc, stacked full-width — never side by side)
03 THE IMPACT        ← .phases, 3 cards
04 INSIDE THE PRODUCT · DESIGN DECISIONS   ← .decisions, 3 blocks, no images
05 WHAT I'D DO DIFFERENTLY
```

### Header

`.case-lede`: "A mature design system, widely used and quietly stalling,
inherited from someone the client had trusted for years."

`.case-body` (one paragraph), two `em.key` emphasis spans: "The case about
what happens after a system ships. I didn't build it: I took it over three
years in, adopted and stalling, and worked on *the part that keeps a
product alive*: adoption, arbitration between teams, and *documentation
people actually open*." No emphasis elsewhere in this paragraph.

`.spec` panel: unchanged. The spacing between `.case-lede` and `.case-body`
is widened by a page-scoped `.case-why { margin-top: 32px }`, matching the
same fix applied to Case 01.

### 01 — The problem

**Pre-title:** The components worked. The seams didn't.

> The libraries existed and worked: a foundation library, around 56 core
> components and an icon system, in use across the group's products since
> 2022. The friction was between products. Each team applied the system its
> own way, change requests pulled in opposite directions, and nobody
> arbitrated. And I was replacing, with no notice, the person the client had
> worked with since the start.

One paragraph. `<strong>` on "56 core components."

### 02 — The solution

**Pre-title:** I changed how we worked, not what we shipped

> Handing over components became a standing conversation with the product
> teams, and the documentation moved up a level, from components to
> patterns, starting with dashboards: component docs explain how a control
> works, pattern docs explain how to build the screen a team has been asked
> for, which is what teams actually ask. That also answered a problem
> nobody had briefed me on. Three years in, a mature system stops
> generating requests, and from the client's side that looks like a project
> that is finished.

One paragraph. `<strong>` on "from components to patterns." Documentation
pair, `figcaption`s and alt text: unchanged.

### 03 — The impact

**Pre-title:** Renewed in the year it looked finished

> **Renewed for a fourth consecutive year.** After change requests had
> fallen by 70%.
>
> **6 teams using the system, 3 of them new.** The weekly meeting went from
> 1 client contact to 5.
>
> **3 junior designers trained on it.** Now one of the most formative paths
> in the firm.

`<strong>` on "70%" in card 01 only.

### 04 · Inside the product · design decisions

**Pre-title:** Three decisions, and what each one cost

> **01 · Negotiation in the open rather than central decisions**
> When two products wanted opposite things I put both teams in the room and
> ran it as a negotiation. The hardest case was sizing and typography,
> where every product had a reason to be the exception.
> *Some requests took longer to close. The decisions held, because nobody
> had them imposed.*
>
> **02 · Documentation moved to where people work**
> Thorough, and barely read: it lived in Figma, and the people who needed
> it didn't. We moved it to Confluence, where the teams already documented,
> and the specs into Figma's handoff tools, where the developers were.
> *A full migration on a live system, maintained from now on outside the
> design tool.*
>
> **03 · Variables at the foundation**
> Brand, mode and product variation resolve once, at the foundation,
> instead of component by component.
> *A migration on a library in production, for a result nobody sees:
> duplication that doesn't happen.*

One plain `<em>` closing sentence per block, three in total.

### 05 — What I'd do differently

**Pre-title:** I read the continuity risk late

> Fewer change requests looks like success from inside; I'd treat request
> volume as a health metric from day one. And on a project already three
> years old I'd put its roadmap on the table in the first weeks, so the
> value of the work was visible before I had to prove it.

One paragraph. `.closing-note` (unchanged): Component library built by the
team before and during my tenure.

### Assets

**Gallery**: 6 images in `assets/cases/02/`, all 1920×1641 — opened and
matched by content, not filename (originals were named generically, e.g.
"Image 4 - built to meet market, technology, and user needs.png"). Renamed
to `case02-gallery-01`–`06` (discovery order: cover image first, then
numeric). Because every gallery image shares identical native dimensions,
`.gallery__frame` needs no height-pinning logic — nothing to jump between
items, unlike a gallery built from heterogeneous screenshots.

**Documentation pair**: `case02-solution-component-level.png` (2468×3410)
and `case02-solution-pattern-level.png` (2468×3490) — tall portrait
document-page screenshots, opened and confirmed a clean match to their
captions before wiring (component page: introduction/anatomy/variants/
behaviour sections on a "Pie Chart" component; pattern page: context-of-use
guidance and do/don't best-practice pairs on dashboard design). Rendered
`.media-frame` stacked full-width, never side by side — at half column
width these portrait pages' own body text becomes illegible. This pair
*replaced* an earlier, weaker pairing (two generic system-overview slides
repurposed from the gallery pool, one of which didn't actually match its
own caption) once the real documentation-page assets landed — the earlier
files were deleted rather than left as orphaned dead weight.

### Open items (carried from the brief, not resolved here)

- **Team size discrepancy**: `index.html`'s landing copy says "a team of 4
  designers"; this page's PROJECT INFO says "3 designers." Implemented
  exactly as given in each source — not reconciled, since the brief itself
  flagged this as unresolved and didn't say which number is correct.
- **The "three teams newly adopting" figure** (IMPACT card 2, DESIGN
  DECISIONS block 01) — implemented as given; the brief asked to confirm
  it's safe to state publicly before publishing, still open.
- **THE IMPACT card 2's two team-counts** ("6 in total" vs. "three product
  teams," in the same card) count different things — teams adopting the
  system vs. teams in the weekly forum. Implemented exactly as given; the
  brief flagged this itself as worth a second read before publishing, not
  as something to change.
- **Team-of-4 count** (withdrawn from `index.html`'s hero intro in the
  punctuation/navigation round) is still unverified. The reinsertion point is
  specified in that section; nothing goes back in until the number is
  confirmed. This is the same discrepancy as the row above, seen from the home
  page instead of the case page.
- **`~56 components`** (THE PROBLEM) is now the only place that figure
  appears, since PROJECT INFO lists the four libraries by name instead of a
  count. Implemented as given; confirm the figure is still accurate.

## Case 03 — Designing to sell

*Oct 2026 — copy pass: body copy across the header and all five sections
replaced per `brief-case-03-copy.md`; structure, markup, classes, CSS,
video, the three scenario screenshots, their labels, alt text and captions
unchanged, with one bounded exception: THE IMPACT's card 4 ("The method
outlived the project") was removed, and with only three cards left the
page-local `.phases` 2-column override was removed too, so the shared
3-column default now applies, matching case-01 and case-02. Every em dash
on the page was also removed this round, replaced with a comma, colon or
full stop. Supersedes the copy blocks below.*

*Aug 2026, header retrofit round: kicker "Designing to sell", `.case-title`
h1 changed to "Sales Tool for a Connected Product Ecosystem", `.case-lede`
renamed from `.lede`. `.rise` → `.reveal` throughout. `.pager` replaces the
old single `.next` link.*

*Aug 2026, narrative revision v1 (supersedes the header-only round above):
full migration to the case-01/case-02 structure. Superseded almost
immediately by v2 below — kept here only as a record of what changed, not
as a description of the current page.*

*Aug 2026, narrative revision v2 (supersedes v1): visual budget flipped back
from "diagram + gallery" to "video + diagram" — the autoplaying hero video
is back (the same `case03-hero.mp4`/`case03-hero-poster.jpg` from before,
reused rather than reshot), and the gallery is gone. THE SOLUTION is now
this page's one inverted section, matching case-01's pattern, containing a
new hybrid diagram: the three real downtime-scenario screenshots
(`case03-scenario01`–`03`) laid out in a row with one continuous connecting
line threading through them — not an abstract band diagram as in v1. Role
and dates are both confirmed this round (`.tbc` placeholders gone). "Why
it's here" cut to one paragraph. The `Context` row dropped from PROJECT
INFO.*

*Aug 2026, narrative revision v3 (supersedes v2): the ecosystem is now
explicitly 10+ products, not three — the downtime flow was always three of
them, but v2's copy read as though three was the whole ecosystem. Touched
everywhere the count appears (THE PROBLEM, THE SOLUTION, the diagram
caption, DESIGN DECISIONS block 02, PROJECT INFO's Product row). PROJECT
INFO gained a `What's shown` row disclosing that the video and screens are
from an anonymised webinar demo with invented data, not the client's
shipped product — the real project is under NDA. That same provenance line
now appears in the video's and the diagram's own captions too, per the
brief's explicit instruction that this isn't a footnote, it's stated where
the visual itself is read. THE IMPACT gained a 4th card ("The method
outlived the project," explaining the webinar-demo substitution) — the
shared `.phases` grid defaults to 3 columns, which would have wrapped a
lone 4th card onto its own row, so this page overrides it to 2 columns
(the existing 768px/480px breakpoints already happened to match, so only
the desktop rule needed touching).*

### Structure

```
HEADER          kicker "Designing to sell" · h1 (work-item title) · lede · Why it's here (1 paragraph) · PROJECT INFO (.spec, 5 rows incl. What's shown)
VIDEO           autoplaying, muted, loop, real pause/play toggle — reuses case03-hero.mp4/poster
01 THE PROBLEM
02 THE SOLUTION (inverted)   ← the page's one diagram: one line through the three real downtime-scenario screens
03 THE IMPACT        ← .phases, 3 cards (shared 3-column default; the page-local 2-column override was removed with card 4, Oct 2026)
04 INSIDE THE PRODUCT · DESIGN DECISIONS   ← .decisions, 3 blocks, no images
05 WHAT I'D DO DIFFERENTLY
```

### Header

`.case-lede`: "Twelve years of building a connected ecosystem, and a sales
network still selling the machine."

`.case-body` (one paragraph), two `em.key` emphasis spans: "The case where
design was a commercial instrument. The request wasn't a screen but *a
sales conversation that kept starting in the wrong place*, and the result
was measured in *qualified leads*, not in a design metric." No emphasis
elsewhere in this paragraph.

`.spec` panel (five rows, including "What's shown"): unchanged. The spacing
between `.case-lede` and `.case-body` is widened by a page-scoped
`.case-why { margin-top: 32px }`, matching the same fix applied to Case 01
and Case 02.

### 01 — The problem

**Pre-title:** The product had changed. The selling hadn't.

> Over twelve years the client had built more than ten software products
> around its machine: operator interfaces, quality control, efficiency
> dashboards, planning, service. The sales network still opened on power,
> precision and reliability, which buyers now take for granted, and that
> matters because the price gap with Asian competitors is 30 to 50% and
> hardware alone can't close it. Cycles were getting longer, fairs produced
> few qualified opportunities, and software was folded into the machine
> price to close deals. The ecosystem had no price.

One paragraph. `<strong>` on "30 to 50%."

### 02 — The solution (`.inverted`)

**Pre-title:** One touchpoint, not the whole funnel

> The problem was the whole commercial process. We took one point of it,
> the trade fair stand, because it's the most visible and the easiest to
> measure. The result is a navigable prototype that opens with a question
> about the buyer's business, walks them through a downtime scenario across
> three of the ten products in one continuous flow, and ends as a CRM
> record: role, company, the pages that mattered, the seller's notes.
> Leaving the rest of the funnel alone is why there was a result to report
> after one season.

One paragraph. `<strong>` on "three of the ten products in one continuous
flow," rendered white by the shared `.inverted .case-body strong` rule.
Diagram, scenario labels, step captions and `figcaption`: unchanged except
for scenario 03's caption, "Places the order, planned, not an emergency"
(was an em dash, removed per this round's file-wide rule).

### 03 — The impact

**Pre-title:** +30% qualified leads in the first fair season

> **+30% qualified leads.** At fairs in the first season using the tool,
> against the historical average.
>
> **Priorities surfaced in 5 minutes at the stand.** The seller asks which
> of three areas is most urgent; the answer qualifies the opportunity and
> keeps the decision maker at the table.
>
> **Reused in 4 more contexts.** International fairs, the Innovation Centre
> showroom, one-to-one meetings, internal training.

Three cards, not four: card 4 ("The method outlived the project") is
removed, see the Oct 2026 note above. No `<strong>` in this section.

### 04 · Inside the product · design decisions

**Pre-title:** Three decisions, and what each one cost

> **01 · Built to make the buyer talk**
> The seller opens with a question, where are you on downtime, on training
> new operators, and reaches for the screen only when there's something to
> answer. Each scenario is designed upstream, with the freedom set by how
> much the seller should steer.
> *More work than a free demo, and less control for the presenter. That's
> what makes it work in a junior's hands.*
>
> **02 · Three products, one line**
> The downtime scenario runs through the on-machine interface, the planning
> software and the service module as one path: in five minutes the buyer
> sees their own problem handled end to end. Then the seller asks which
> area is most urgent.
> *Three products shown means seven left out, and believable states and
> data cost more than describing all ten. Slideware ends a conversation
> with a technical buyer.*
>
> **03 · The conversation becomes a record**
> Anything the buyer reacts to is pinned to their profile. The end of the
> day is a structured record, ready for the CRM and for whoever writes the
> proposal.
> *Admin in the middle of a sales conversation. Sellers resist it until
> they see the proposal it produces.*

One plain `<em>` closing sentence per block, three in total.

### 05 — What I'd do differently

**Pre-title:** The scenarios were chosen at a desk

> They came from competitive differentiation, the right criterion and a
> desk-based one; the first season was the first test of whether buyers
> wanted those conversations. I'd track which scenarios go unused and let
> the prototype learn from each season.

One paragraph. `.closing-note` (unchanged): Commercial strategy,
segmentation and sales-cycle design developed with the wider team.

### Video

Reuses `case03-hero.mp4` (2.1MB, H.264, no audio track, baked-in loop-fix
fades) and `case03-hero-poster.jpg` unchanged — both already satisfied every
requirement this round asked for (muted/loop/playsinline, poster frame,
well under the 5MB budget), so nothing needed re-encoding. `autoplay` is
never declared as an HTML attribute; the script calls `.play()` on load only
when `prefers-reduced-motion: reduce` doesn't match, and sets the toggle's
initial label/`aria-label` to match either way — the cleaner, no-flash
version of the pattern this page's original (pre-migration) hero video used,
which instead started with `autoplay` present and reactively removed it.
Sizing follows the standing hero-media convention (`assets/README.md` §A2):
`width:100%;height:auto;max-height:62vh`, no `object-fit`.

### Diagram

"One line, three screens" (THE SOLUTION, inverted). Three real screenshots
— confirmed, not assumed, to be the three steps of the retired walker's
downtime flow (their captions already matched this brief's own step table
exactly) — each in a frame sized from the narrowest native ratio (scenario
03, 1410:801) with `object-fit: contain`, the same sanctioned multi-asset
technique the retired walker used for the same three assets. One continuous
line is a single absolutely-positioned element behind all three frames, not
three separate segments — each frame's own `background: var(--black)`
(matching the inverted section, so any letterboxed edge is invisible)
occludes the line where it passes behind a frame, which is what makes it
read as "entering" and "exiting" each screen rather than stopping at every
gap. Reflows to a vertical line at ≤768px via an explicit media-query
override (a straight `flex-direction: column` alone doesn't reorient a
fixed horizontal bar). `.dgm-label`/`.dgm-sub` (already shared in `case.css`
from the v1 round) reused directly on plain HTML text here rather than SVG
— they resolve correctly either way, since `fill`/`stroke` no-op harmlessly
outside SVG while `color` inheritance (from the inverted section's white)
does the real work.

**Contrast catch, same bug as case-01's inverted section, caught again**:
`.case-body strong` defaults to black text — invisible on this section's
black background. The one `<strong>` inside THE SOLUTION's body copy got the
same inline `color: var(--white)` override case-01 needed for its own
inverted-section `<strong>`.

*Aug 2026, final-refinements round: that inline override was replaced with
a shared `case.css` rule (`.inverted .case-body strong`), and in the
process a second, real bug turned up that the strong-only fix had masked —
the surrounding `<p>` tags in this section had never had a colour override
at all, so they were silently inheriting `--gray-700` (the `.case-body`
default) on a black background, unreadable. A shared `.inverted .case-body
{ color: var(--white) }` rule now covers the paragraphs too. Full
measurement pass on every text element in this section, against the actual
composited background:*

| Element | Colour | Contrast on `--black` | Result |
|---|---|---|---|
| `.case-body` paragraphs (3) | `--white` (was unset → `--gray-700`) | 18.94:1 | now passes; previously failing |
| `.case-body strong` (1) | `--white` (was inline hack) | 18.94:1 | pass, unchanged in effect |
| `.sec-label` ("02 — The solution") | `--white` @ 0.75 opacity | 10.64:1 | pass |
| `.sec-pretitle` ("One touchpoint…") | inherits `section.inverted`'s `--white` directly | 18.94:1 | pass |
| `.dgm-label` (3, scenario labels) | inherits `--white` via cascade — the rule's own `fill: currentColor` is a no-op on HTML text, `color` does the real work | 18.94:1 | pass |
| `.dgm-sub` (3, scenario captions) | inherits `--white` via cascade, same mechanism | 18.94:1 | pass |
| `figcaption` | explicit `--gray-300` (page-local rule) | 13.32:1 | pass |
| `.flow-diagram__line` (decorative, `aria-hidden`) | `background: currentColor` → white | n/a (not text) | auto-flips correctly |

`--accent` (3.15:1 on black — fails 4.5:1, confirmed by hand-calculation
this round) is not used as text anywhere in this section; `.sec-label`
already routes through the white-at-0.75-opacity rule instead. No
`em.key` currently appears inside this section's `.case-body`, so the
italic-accent-with-white-fallback rule the brief asked for has nothing to
apply to yet — the shared rule (`.inverted .case-body em.key { color:
var(--white) }`) exists and is ready if one is added later. The three
scenario screenshots (`case03-scenario01/02/03.png`) were confirmed to
carry no `filter`, `opacity`, or `mix-blend-mode` — unmodified, as
required.

### Open items (carried from the brief, not resolved here)

- **Webinar audience** — left as "a sector webinar" (the video caption, the
  diagram caption, and the `What's shown` row), per the brief's own
  fallback instruction, since no audience was supplied.
- **Authorship of the webinar demo** — the "I rebuilt the approach" claim
  lived only in IMPACT card 4, which the Oct 2026 copy pass removed; the
  page no longer makes this claim anywhere, so this item is moot unless the
  claim is reinstated elsewhere.
- **Video length/poster** — unchanged again this round; the existing file
  already clears every bar every round has set.
- **The three scenario screens** — reconfirmed (not re-investigated,
  nothing new suggested otherwise): their pre-existing captions already
  matched this brief's step table exactly.
- **`more than ten` products, and whether three is the right count for the
  flow** — implemented as given; if the real number of products the
  prototype covers is different, the diagram's "one line" concept may need
  rethinking at a higher count, per the brief's own note.
- **`+30%` qualified-lead figure** — implemented as given; still asks
  confirmation that it's the number the client would recognise.

---

## Open items, site-wide

- **Red Dot credits**: resolved, and resolved against her. Her name does not
  appear in the project credits on the linked page. The link is kept anyway
  (her decision), so the citation is phrased as a studio project rather than
  a personal award. Nothing further to do unless she wants the phrasing
  revisited.
- **Case 01 date against the Red Dot year**: the linked entry is Design
  Concept 2024; the case page and home page carry 2025. To be reported by the
  reconciliation pass in item 5, not resolved here.
- **`.case-nav` position in the DOM**: whether it is currently a child of
  `.case-top` is unknown from this file. It determines whether item 6 is a
  two-line CSS change or also a markup move. To be discovered, not assumed.
