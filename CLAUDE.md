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
