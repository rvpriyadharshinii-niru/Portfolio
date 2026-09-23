# Portfolio site — working notes

Static site: `index.html` (homepage) + `resume.html` + five case studies
(`case-study-ai-agents.html`, `case-study-admin.html`, `case-study-erp.html`,
`case-study-summary-builder.html`, `case-study-ai-native-process.html`),
sharing `styles.css` (case study design system, with per-project themes via
`body.theme-purple` / `body.theme-green`) and `home.css` (homepage/resume
only). `case-study-ai-native-process.html` is a process/meta case study
("Claude Code is my design partner.") rather than a product one — built to
be shown in interviews, using real BAIS Admin/Agent Studio screenshots plus a
sample PRD the user provided, with a couple of slots filled by honestly-
captioned stand-ins from other projects (see its own placeholder note below).

## Homepage — split layout (rebuilt from a reference Framer portfolio)

`index.html` is no longer a single scrolling page of sections (hero → about
→ timeline → project grid → footer). It's now a two-column split, modeled
directly on a reference portfolio the user shared (screenshots of
clarissepsicat.com/valoi and a Framer portfolio by Harismita Govindaraj):

- `.split-layout` (`display:flex`) holds `.split-left` (flex: auto) and
  `.split-right` (flex: 0 0 380px, `position: sticky; top:0; height:100vh`).
- `.split-left` is a stack of full-height (88vh) `.project-tile` elements,
  one per visible case study. **Most tiles are a "cover" design, not a
  screenshot** — `.tile-cover` is a colored pattern background (per-project
  variant: `--teal`, `--teal-alt`, `--purple`, `--green`, matching each case
  study's own theme) with just the project name (`.tile-cover-name`) and a
  small monospace tag line (`.tile-cover-tag`) — deliberately no product
  screenshot, per explicit request ("thumbnail with projectname alone...
  some kind of pattern design... lets not show the screen for all"). Exactly
  **one** tile (currently the featured "Claude Code Is My Design Partner")
  additionally carries a small inset real screenshot via
  `.tile-cover-shot` (capped at 340px / 60% width) — the one deliberate
  exception, kept intentionally small rather than full-bleed.
- Behance projects are **not** shown as tiles in `.split-left` anymore
  (they were removed entirely, along with `.project-tile--behance`). A
  dedicated **"Behance" pill** now sits next to "About" in
  `.split-right-top` (both wrapped in `.split-pill-row`), linking out to
  the Behance profile directly.
- `.split-right` is the identity sidebar: top row is the About/Behance
  pill row + a "PV" monogram; center is name + a short one-line bio (the
  longer About copy lives only on `resume.html`) + social icons + an
  "Email" pill; bottom is "more ↓" + the vinyl-player illustration (spins
  on hover, reveals a Behance/"other work" card popping upward, since it
  sits in a narrow sidebar).
- A floating `✉ Email` pill (`position: fixed`, bottom-left) sits over
  `.split-left` at all scroll positions.
- Below 980px, `.split-layout` stacks vertically with `.split-right` first
  (`order: -1`, `position: static`) so identity/bio show before the tile
  stack, and tile heights shrink (70vh).
- The old homepage sections (`.hero-intro`, `.timeline`, `.projects`,
  `.behance-grid`-as-grid, `.home-footer`) are gone from `index.html`, but
  their CSS in home.css was left in place rather than deleted, since
  `resume.html` still shares home.css and uses `.home-header` / `.home-nav`
  / `.brand*` / `.home-footer` / `.footer-*` / `.prose` for its own page —
  don't delete those rules without checking resume.html first. Likewise
  `.tile-label`/`.tile-title`/`.tile-tags`/`.project-tile--behance` from
  the first split-layout pass are now unused dead CSS (superseded by
  `.tile-cover*`) but were left in place rather than deleted.
- `resume.html`'s nav previously linked to `index.html#work` /
  `index.html#about`; those anchors no longer exist (the homepage is one
  unified view now), so both links now just point to plain `index.html`.

The profile photo (`assets/profile-priyadharshini.webp`) is still unused.

### Sidebar footer — vinyl illustration replaced (2026-09-17)

The hand-drawn SVG vinyl-player illustration (spun on hover, revealed a
Behance/"other work" card) was removed per repeated explicit feedback
("i dont ike the recoder...vinyl...give it better", then "it's looking so
bad...enhance this...u don't have to follow reference on this one" — full
creative freedom given). It had two real problems beyond just looking
unpolished: the reveal only worked on `:hover`, so the Behance link inside
it was unreachable on touch/mobile, and the "more ↓" text above it wasn't
a link or bound to any behavior — pure dead affordance.

Replaced both with `.split-right-bottom` now containing: a `.status-chip`
("● Open to new opportunities", pulsing dot animation) and a plain
`.elsewhere-link` ("More work on Behance ↗") pointing straight at the
Behance profile — both always visible, no hover required, work identically
on touch and desktop. All `.vinyl-*` CSS and the `.more-hint` rule were
deleted from `home.css` entirely (not commented out/hidden) since this was
a disliked design being replaced, not a feature being hidden — unlike the
project-tile hide/show pattern elsewhere in this file.

**Update, same day**: the user generated a real illustrated portrait of
themselves (a "textured indie illustration" style, per a JSON style-prompt
sheet they shared) and asked for it to go in that same spot, above the
status chip/link (not instead of them). It's now `.sidebar-illustration`
(`assets/illustration-priyadharshini.png`), a rounded 190px image with a
soft shadow, sitting at the top of `.split-right-bottom`. **Getting the
file was the hard part**: three separate images pasted inline earlier in
this session (two tile-background references, then this illustration
itself, twice) never reached a filesystem path this session could read —
confirmed each time via the uploads directory. What finally worked: the
user uploaded the PNG to **Google Drive**, and it was fetched with
`mcp__Google_Drive__list_recent_files` (sorted by recency, found instantly
as the newest file) + `mcp__Google_Drive__download_file_content` (returns
base64; the response is large enough to exceed the tool's inline output
limit, so it lands as a saved tool-result file — decode client-side, e.g.
`python3 -c "import json,base64; d=json.load(open(path)); open(out,'wb').write(base64.b64decode(d['content']))"`,
rather than trying to read the tool result directly). **If the user wants
to swap in more of their own photos/art in the future and pasting inline
doesn't produce a readable file, ask them to upload to Google Drive and
repeat this exact flow** rather than re-litigating the paste-vs-attach
question from scratch.

## Homepage tiles — per-project display type; two reference photos pending (2026-09-17)

Each tile's `.tile-cover-name` now carries its own typeface instead of
sharing one generic bold sans, matching a "Somali Museum UK" logotype
reference the user shared as an example of a project having its own
distinct type character: `.tile-cover-name--erp` (Archivo Black,
uppercase), `.tile-cover-name--admin` (Fraunces serif), `.tile-cover-
name--process` (Space Mono), `.tile-cover-name--summary` (Instrument
Serif italic), `.tile-cover-name--agents` (Space Grotesk bold). Fonts
added to the Google Fonts `@import` at the top of `home.css`.

The user also shared two reference **photos** to use as real tile
backgrounds — a dark abstract purple/blue/pink smoke-wave shot for
**Summary Builder** (first said "admin", immediately corrected to
"summary builder"), and a dark teal ribbon/circuit swirl for **AI
Agents** — both pasted inline in chat rather than attached as file
uploads, so **neither image was ever saved to a path this session could
read** (confirmed by checking the uploads directory; unlike PDFs/
screenshots shared earlier in this project, which did get a saved path).
Built `.tile-cover--indigo` (now on Summary Builder, replacing its old
`--purple` variant) and `.tile-cover--circuit` (AI Agents, replacing its
old `--teal-alt`) as CSS-gradient approximations of each photo's mood/
palette as a stand-in. Admin is back on its original `--teal` variant —
it was never supposed to change. **If the user asks why their photo
didn't show up, or wants the real images in**: they need to send the file
as a proper attachment (not a pasted/dragged inline image) so it gets a
filesystem path; once available, save it to `assets/`, reference it as a
real `<img>` or background-image on the tile (per the "for one project we
can show [a real screenshot], but small" precedent already set for the
Claude Code Is My Design Partner tile), and the CSS approximation can be
removed.

## Homepage tiles — all five visible again, ERP shown first (2026-09-17)

All five case studies are now uncommented and visible in `index.html`'s
`.split-left`. After a round of hiding/restoring individual tiles earlier
the same day, the user asked to bring back whatever was still hidden
(Summary Builder and Claude Code Is My Design Partner). Restored them in
the original relative order (Admin, AI-native-process, Summary Builder,
AI Agents) around the one deliberate reorder that's still standing — ERP
moved to show first, per an earlier explicit request that was never
countermanded. Current order: **ERP, Admin, Claude Code Is My Design
Partner, Summary Builder, AI Agents**. The "Claude Code Is My Design
Partner" tile is the one carrying the small real-screenshot inset
(`.tile-cover-shot`) — it's visible again now too. If tiles get
hidden/reordered again, use the same "comment out, don't delete" pattern
and give this section a fresh writeup rather than reviving this one.

## Image placeholders — currently removed, on purpose

The case studies used to have "IMAGE NEEDED" placeholder boxes
(`.placeholder`, `.placeholder-tag`, `.placeholder-name`, `.placeholder-desc`,
`.placeholder-purpose` in styles.css) marking real screenshots the user
hadn't sent yet. As of the change that added this file, the user asked to
**remove every remaining placeholder** across all four case studies so
unfinished sections read as plain, seamless text instead — they didn't have
more screenshots to send right now.

**When the user asks to bring placeholders back** (e.g. "add the placeholder
back", "I have more images now, put the placeholder back so I can see where
to fill it"), restore the `.placeholder` block pattern used elsewhere in the
same file (see `styles.css` for the classes — they were never deleted, only
the HTML instances were removed). Match the surrounding section's existing
placeholder style (`placeholder--full`, `--wide`, `--panorama`, `--crop`,
etc.) rather than inventing a new one.

## Admin case study — "one mental model" framing added (2026-09-17)

Follow-up to the Personas addition below, using more of the same BAIC PDF:
added a Today/Tomorrow comparison ("Each team builds its own admin" vs
"One mental model. Right scope at every level.") to the end of the
**Background** section, reusing `.persona-grid`/`.persona-card` with a new
muted tag variant (`.persona-card-tag--muted`, for the "Today"/problem
side) so it doesn't compete visually with the Personas section's own
teal-tagged cards further down the page. Also added a new **THE PROMISE**
section (dark, right before "My contribution") with three numbered
`.promise-card`s — One mental model / The right tool for the right person /
Coherent as it scales — from the PDF's closing "what this gives customers"
slide. New CSS: `.persona-card-tag--muted` and `.promise-grid`/
`.promise-card`/`.promise-card-num`. Same PDF as below; it still isn't in
the repo, so ask the user for it again if this content needs revisiting.

## Admin case study — Personas section added (2026-09-17)

The user attached a PDF ("BAIC platform — Administration, by design") laying
out a Platform Admin / App Admin persona framework and asked for it to be
used "fully" to make the Admin case study richer, with personas explained
**before** the screen-by-screen walkthrough starts. Added a new `PERSONAS`
section in `case-study-admin.html` right after Information Architecture and
before Organization, containing: two `.persona-card`s (Platform Admin /
App Admin), a `.compare-table` mapping three admin tasks (set up org,
manage users/access, configure tools/apps) across both personas, a
Platform→App flow diagram, and a "Set up once, configure per app"
`.solution-card` with the PDF's Salesforce shared-credential example
folded into its copy. Then added a one-line `.persona-note` callout after
the real screenshot(s) in each of the following sections — Organization,
Governance, Users & Access, Integrations, Credits & Usage, Observability,
Settings — tying each back to which persona/altitude it belongs to, using
the PDF's own per-section captions (e.g. "Platform sees the full bill, App
sees its own consumption"). New CSS added to `styles.css`: `.persona-grid`/
`.persona-card`/`.persona-card-tag` (card pair), `.compare-table`/
`.compare-row`/`.compare-tag` (the task-comparison table, collapses to
labeled stacked rows below 760px), and `.persona-note` (the inline callout,
reuses the `.annotation-card` left-border-accent look). None of the PDF's
own mockup screenshots (light-theme BAIC/CSAI admin UI) were used as
images — they're a different visual style from the real dark Uniphore
product screenshots already on the page, so the PDF's content was turned
into text/diagrams in the existing design system instead of pasted in as
pictures. The PDF source file isn't in the repo; if this section needs
updating later and the framework details are unclear, ask the user for the
PDF again rather than guessing.

## Admin case study — hero starts with text only (2026-09-17)

`case-study-admin.html`'s hero used to open with a `.collage-grid` of four
screenshots (org-structure, credits, health, users) right under the title —
the user felt starting a case study with images before any text was wrong
("I don't think that's necessary start with textual ..then show images in
the place accordingly"). Removed that hero collage entirely; the hero is
now just the meta tags, `<h1>`, and `.hero-sub` — no images. Nothing was
lost: all four of those exact screenshots already recur later in their own
contextual sections (Organization, Users & Access, Credits & Usage,
Observability), so the case study still shows them, just in place rather
than up front. `case-study-ai-native-process.html` has the same
`.collage-grid` hero pattern and wasn't touched — only apply the same fix
there if asked.

Removed placeholder instances (for reference, in case content needs to be
reconstructed):
- **AI Agents**: X-Console feature mapping/migration audit (Discovery &
  Audit section — annotation-row of Migrate/Review/Redesign/Gap tags kept),
  NLU UI details (rule-based Agent mechanics section — fully removed, no
  replacement content), Version History (kept the annotation-row--five of
  questions), Administration UI collage (kept the Users→Roles→Permissions→
  Capabilities flow-vertical, un-nested from the grid-2), and two showcase
  items — "05 Versioning" and "06 Permissions / Restricted States" — removed
  entirely from the Agent Workspace showcase (items 01–04 remain).
- **Summary Builder**: Question Selection UI (optional, after the Standard/
  Custom branch diagram), Test Question/Test Template screen (before the
  Configure→...→Publish pill-flow diagram), Trace Question screen (Trace/
  Debug section, now just heading + one paragraph).
- **AI-Native Process**: two "Figma review with comments" placeholders —
  Section 7 (Feedback Loop, after the pill-flow diagram — kept the caveat
  paragraph and closing statement) and the "05 Figma / Team Feedback"
  showcase item under "What this looked like on BAIS" (removed entirely;
  the following item was renumbered 05, so the showcase now runs 01–05).
  Both were removed because there was no honest stand-in for "review
  comments" specifically — everything else on that page that was missing a
  real screenshot was instead filled with an existing, honestly-captioned
  asset from elsewhere in the repo (see the page's `iter-note` captions),
  which is the preferred move over a placeholder when a reasonable stand-in
  exists.

## ERP tile + Restaurantware client intro (2026-09-19)

The user shared a real photo (a dining-room shot with table settings —
plates, glassware, place settings) to use as the ERP homepage tile
background, plus a reference layout (a Behance-style ERP case study hero:
full-bleed photo, dark gradient, meta row, big overlaid title) as a style
guide for how to use it — this arrived as a real file this time (a direct
"[Image: source: ...]" attachment, not a chat-pasted image), so it saved
correctly and is now `assets/erp-restaurantware-hero.jpg`.

- **Homepage tile**: ERP's `.tile-cover` switched from the CSS pattern
  (`--green`) to a new `.tile-cover--photo-restaurantware` variant in
  `home.css` — the real photo as `background-image`, with a bottom-heavy
  dark gradient overlay so the title/tag text (which `.tile-cover` always
  puts at the bottom via `flex; justify-content:flex-end`) stays legible.
  The photo fits because ERP's actual client, **Restaurantware**, sells
  restaurant/hospitality tableware — the dining photo is directly on-topic,
  not a generic stock mood shot.
- **Case study page**: added a `.client-card` component (new CSS in
  `styles.css` — a bordered card with a `.client-mark` wordmark chip +
  one paragraph) at the top of the "The Challenge" section in
  `case-study-erp.html`, introducing Restaurantware as a company (hospitality
  supplies, thousands of SKUs, growing catalog) before diving into the ERP
  2.0 pain points — company context now comes before the product problem.
- **Logo**: `.client-mark` is currently a plain text wordmark chip
  ("Restaurantware" in a pill), **not their real logo** — no actual logo
  file was provided or is in the repo, and their real mark shouldn't be
  guessed/recreated. If the user sends the actual logo file (as a direct
  attachment, which now works — see the illustration flow above for what
  a working attachment looks like vs. a chat-pasted one that doesn't save),
  swap it in as an `<img>` inside `.client-mark` in place of the text.

## ERP — real Restaurantware logo + factual company copy (2026-09-19)

Follow-up to the client-card above: the user sent the real Restaurantware
logo (a direct attachment — cropped from a mostly-whitespace 1414x2000
PNG down to just the logo mark via PIL bbox-detection, saved as
`assets/restaurantware-logo.png`) plus real facts pasted from
restaurantware.com/pages/about-us (fetching that URL directly is blocked
by this environment's network egress proxy, same as other external
domains — the user pasted the text instead). `.client-mark` in
`case-study-erp.html` now renders that logo as an `<img>` (28px tall,
width auto) instead of a text wordmark chip; `.client-mark` CSS in
styles.css updated accordingly (no more border/padding/pill styling —
just sizes the image). The paragraph was rewritten using their real
numbers: founded/operating since 2010, 17,000+ products, 250+ new
products launching every quarter — replacing the earlier inferred
"thousands of SKUs, dozens of categories" language. Not copied verbatim
from their site (that copy also mentions a 98.6% in-stock rate and
same-day shipping before 3pm CST, which weren't relevant to the
ERP-scaling narrative here, so were left out) — paraphrased and scoped to
what's relevant to why their systems needed to scale.

## ERP split into a hub + three sub-case-studies (2026-09-19)

Per the user's explicit restructure request: `case-study-erp.html` is now a
**hub page**, not the full story. It covers only Hero, The Challenge (incl.
the Restaurantware client-card), My Role + Discovery, and Design Strategy
— then a new **"Explore further"** section with three `.subcase-card`
links (new `.subcase-grid`/`.subcase-card` CSS in `styles.css`) to three
placeholder pages, then Impact, Constraints & Reflection, and the Closing
stay as the hub's own wrap-up.

The three new stub pages (all "Coming soon", linking back to
`case-study-erp.html`, following the hero/section-intro pattern of every
other case study):
- **`case-study-erp-returns.html`** — Returns / Product Lifecycle
- **`case-study-erp-components.html`** — ERP System & Reusable Components
  (Quick Edit, column components, grid patterns, design-system components)
- **`case-study-erp-modules.html`** — ERP Modules & Experiences (Dashboard,
  scheduled jobs, home page, other core ERP experiences)

**Nothing was deleted.** The five sections removed from the main page —
Quick Edit, Object-Oriented Components, Returns Management, Dashboard +
Task Center, and the closing "Design System" editorial chapter — were
moved into the matching stub page above, wrapped in an HTML comment
(`<!-- RESERVED CONTENT ... -->`) rather than rendered, so all the original
copy/image references are preserved as raw material. The user plans to
share a fresh batch of screens and re-sort content into these three pages
properly later ("will share all screens then you can sort it under each
sub case study then we can decide on the story") — **when that happens,
pull from the matching reserved-content comment block first** rather than
starting from scratch, then delete the comment once real content replaces
it.

The homepage tile still points at `case-study-erp.html` (the hub) —
correct, no change needed there.

## ERP sub-case-study cards — big cover thumbnails (2026-09-19)

Follow-up to the hub split above: the three `.subcase-card` links looked
too small/easy to miss ("those three links make it big thumbnail so its
easily visible"). Added a `.subcase-card-cover` — a large 4:3 gradient
thumbnail with a bold white "01"/"02"/"03" numeral — above the tag/title/
description in each card, so the cards now read as prominent visual tiles
rather than compact bordered text blocks. Each card gets its own pattern
variant via `nth-child` (diagonal stripes for card 1, mirrored diagonal
stripes for card 2, dot-grid for card 3), all built from the theme's own
`--teal-900/850/800` and `--cyan-rgb` tokens so they match the page's green
theme rather than introducing new colors. Verified at both desktop and
mobile (390px) widths — cards stack cleanly in a single column on mobile
with no overflow.

## Returns / Product Lifecycle sub-case-study — built out from stub (2026-09-19)

The user shared a large Figma spec export (`Section_1.pdf`, a "Listing"
frame covering the ERP's Products nav) plus three small reference crops —
the existing-products lifecycle tab bar (Created/Purchased/In Transit/
Live/Dead/Not Live), a product-specification header (tabs: Details, Sales
Channel, Category & Facets, Content, Photography, Vendors, Inventory,
Variation, CEO), and a family's variant list (color variants + retail/RTL
variants) — with explicit instructions: *"under product nav we will show
three items — product ideation (draft product), existing product, and
product specification... all these designs dont show big big screens...
make it explain and bring the story in terms of ux... give more
explanation about the process."* This directly matches
`case-study-erp-returns.html`'s own title ("Returns / Product Lifecycle"),
so that's where the new content went.

**Deliberately no screenshots for the new content** — per the explicit
request, this section is text/diagram-led, reusing existing design-system
components rather than pasting in dense form screenshots (the PDF's own
screens are thousands of placeholder fields like "9999" / "Lorem ipsum",
not presentable as-is; the three crops shared as reference also have
unusable aspect ratios for the `.shot` component — e.g. 1058×88 and
266×858 — confirming they were meant as context for me, not final assets).
Four new sections were added right after the hero:
1. **Products overview** — `.persona-grid--three` (new CSS modifier, same
   card look as the existing `.persona-grid` used for Admin's personas,
   just three columns) introducing the three surfaces, plus a paragraph on
   the actual design process (shadowing purchasing/warehouse/merchandising
   teams found nobody thought of "a product" as one object — each team
   only ever needed one lifecycle-stage slice of it).
2. **Existing products / lifecycle** — the six lifecycle states as an
   `.annotation-row--six` (new CSS modifier, extends the existing
   `.annotation-row`/`--five` pattern) of tag + one-line description.
3. **Product Ideation** — a `.flow-vertical--pill` diagram (Draft → Content
   team → Reviewer team → Approved & merged), reusing the same node/arrow
   components already used for flows in the AI Agents case study.
4. **Product Specification** — the nine ownership tabs as two
   `.annotation-row`/`--five` rows, plus a `.solution-card` design-principle
   callout ("Show what's missing, not just what's there").

**Also un-reserved the Returns Management section** — its `RESERVED
CONTENT` HTML comment (real screenshots: `erp-returns-flow-logic.png`,
`erp-returns-before-1/2.png`, `erp-returns-after.png`, all already in
`assets/`) was uncommented and placed after the new Products content,
since this page's title already promised Returns content and the assets
were sitting ready — no reason to leave it stubbed while filling in the
rest of the page. The "Coming soon" section was removed entirely now that
the page has real content. Verified full-page render via local server +
Playwright (forcing `.reveal` elements to `.is-visible` for the
screenshot, since scroll-triggered IntersectionObserver reveals don't
always fire reliably in a scripted headless pass) — all sections, the
pipeline diagram and the un-reserved Returns section render correctly.

`case-study-erp-components.html` and `case-study-erp-modules.html` are
still stubs — the PDF only covered the Products nav item, not Quick
Edit/columns or Dashboard/scheduled jobs, so nothing to add there yet.

## ERP Modules page — Dashboard & Task Center un-reserved (2026-09-22)

User said "lets continue erp thing" with no new material attached; asked
which of the two remaining stub pages to continue and they picked **ERP
Modules & Experiences**. No new screens came with it, but
`case-study-erp-modules.html`'s `RESERVED CONTENT` block already held a
real, complete section — Sales Summary Dashboard & Task Center — with
working screenshots (`erp-dashboard-builder.png`, `erp-dashboard-kpi.png`,
`erp-task-center.png`, all already in `assets/`) from before the ERP
hub/sub-case-study split. Same move as the Returns page: uncommented it,
removed the "Coming soon" banner, and added one small `.iter-note` at the
end ("Scheduled jobs, the home page and other core module experiences are
still being written up — more to come.") since this page's title promises
more than Dashboard + Task Center alone covers. Verified render via local
server + Playwright (forcing `.reveal` to `.is-visible`) — screenshots and
the note display correctly.

`case-study-erp-components.html` is still a bare stub — its reserved
content (Quick Edit, Object-Oriented Components, the closing Design
System editorial chapter) is real too and could be un-reserved the same
way if asked next, but wasn't touched this round since the user only
picked Modules.

## Design-partner heading renamed; screenshot reduction pass (2026-09-23)

Two separate but related requests, both about how much real product
detail the site exposes:

**1. "Claude Code Is My Design Partner" → "AI-Native Design Process".**
The user asked for a professional, generic title instead of the
personified tagline. Renamed everywhere it appeared: the homepage tile
(`.tile-cover-name` + alt text) in `index.html`, and the `<title>`/`<h1>`
in `case-study-ai-native-process.html`. The `proj-tag` header and tile
tag line already said "AI-Native Design Process" / "Design Process" —
this just made the main heading match instead of standing out as the
odd one out. The hero-sub paragraph explaining the Claude Code workflow
was left as-is; it already read professionally.

**2. Screenshot reduction, explicitly split by sensitivity.** The user's
framing: *"imagine if you are signed [an] NDA to not share things... for
admin project reduce the size, it's fine to show the data, but for ai
agents and summary builder let's not [show the data]."* Two different
treatments followed from that:

- **Admin (`case-study-admin.html`)** — screenshots stayed, just
  smaller. All 8 `.shot--full` (full-bleed) screenshot wrappers got
  `style="max-width:760px;margin:0 auto;"` added via a sitewide sed
  (every `.shot--full` in this file was a real screenshot worth
  shrinking — none needed to be excluded). Sibling elements like
  `.insight-strip`/`.annotation-row` sit outside that wrapper and stayed
  full width, so the text cards next to each shrunk screenshot are
  unaffected.
- **AI Agents (`case-study-ai-agents.html`) and Summary Builder
  (`case-study-summary-builder.html`)** — actual removal, not just
  resizing. Both pages already leaned heavily on this design system's
  diagram components (`flow-vertical`, `annotation-row`, `comparison`,
  `ia-grid`, `branch-diagram`), so most real screenshots were flat-out
  redundant with a diagram or tag-row already sitting next to them.
  Kept exactly **one** screenshot per page (AI Agents: the Agent Studio
  overview in the Agent Workspace showcase, shrunk to 680px; Summary
  Builder: the hero list-view shot, shrunk to 680px) and removed every
  other real product screen — roughly 9 removed from AI Agents, 7 from
  Summary Builder — replacing each with the annotation-row/tag-row/
  comparison content that was already describing the same screen in
  words. Where a caption or note existed only to describe a now-removed
  image (an orphaned `iter-note`, a "see screenshots below" reference),
  it was rewritten to stand alone or folded into nearby text rather than
  left dangling. Competitor-benchmark alt text that named specific
  competitor products was also made generic while doing this pass.
  **If the user sends new screenshots for these two case studies going
  forward, default to this same posture** (diagram/text first, a real
  screen only as a last resort, at most one per page) unless told
  otherwise.

Both files also got the same repeated-paragraph trim already applied to
the ERP Returns page: every multi-sentence section-intro paragraph
across both case studies was cut to one tight sentence, removing
repeated "X changed, this meant Y" framing that recurred section to
section.

Verified all three edited case studies (Admin, AI Agents, Summary
Builder) via local server + Playwright, forcing `.reveal` to
`.is-visible` and checking for failed asset requests — none, aside from
the expected Google Fonts network block in this sandboxed environment.
