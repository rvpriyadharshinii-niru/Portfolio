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

The profile photo (`assets/profile-priyadharshini.webp`) is still unused,
replaced by the vinyl illustration as described above.

## Homepage tiles — Admin restored, two still hidden, ERP shown first (2026-09-17)

**Summary Builder** and **Claude Code Is My Design Partner** (the
AI-native-process case study) remain commented out in `index.html`'s
`.split-left` (search "HIDDEN FOR NOW"), not deleted — both case study
pages are still fully live, just unlinked from the homepage tile stack.
**Admin** was hidden earlier in the same day but the user immediately
asked for it back ("Don't hide admin bring it back") — it's uncommented
again and visible. With Claude Code Is My Design Partner hidden, **no
visible tile currently has a real screenshot inset** (`.tile-cover-shot`)
— that markup still exists but is inside the hidden AI-native-process
tile; nobody asked for another tile to pick up the screenshot treatment,
so don't add one without being asked.

Currently visible, in order: **ERP** (moved to first, per explicit
request), then **Admin** (restored right after ERP), then **AI Agents**.
Original order before any of this was Admin, AI-native-process, Summary
Builder, AI Agents, ERP. **When the user asks to bring the remaining
hidden tiles back**, restore that original relative order unless they
specify a different one (they've reordered ERP to the front at least
once already, so confirm rather than assuming) — and remove this section
of CLAUDE.md once all tiles are back to how they want them.

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
