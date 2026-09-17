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
- `.split-left` is a stack of full-height `.project-tile` elements (88vh
  each), one per case study in order, each a plain `<a>` wrapping an `<img>`
  (the existing `project-thumb-*.png` assets) with a small white
  `.tile-label` card overlaid top-left (title + tags). After the 5 case
  studies, the 3 Behance projects continue the same stack as shorter
  (`project-tile--behance`, 60vh) tiles linking out to Behance.
- `.split-right` is the identity sidebar: top row is an "About" pill
  (→ `resume.html`) and a "PV" monogram; center is name + a short one-line
  bio (the old 2-paragraph About copy was cut down further — the longer
  version lives only on `resume.html` now) + social icons + an "Email"
  pill; bottom is "more ↓" + the vinyl-player illustration (unchanged from
  before, still spins on hover and reveals the Behance/"other work" card,
  repositioned to pop upward instead of sideways since it now lives in a
  narrow sidebar).
- A floating `✉ Email` pill (`position: fixed`, bottom-left) sits over
  `.split-left` at all scroll positions, matching the reference's
  persistent contact CTA.
- Below 980px, `.split-layout` stacks vertically with `.split-right` first
  (`order: -1`, `position: static`) so identity/bio show before the tile
  stack, and tile heights shrink (70vh / 46vh for Behance).
- The old homepage sections (`.hero-intro`, `.timeline`, `.projects`,
  `.behance-grid`-as-grid, `.home-footer`) are gone from `index.html`, but
  their CSS in home.css was left in place rather than deleted, since
  `resume.html` still shares home.css and uses `.home-header` / `.home-nav`
  / `.brand*` / `.home-footer` / `.footer-*` / `.prose` for its own page —
  don't delete those rules without checking resume.html first.
- `resume.html`'s nav previously linked to `index.html#work` /
  `index.html#about`; those anchors no longer exist (the homepage is one
  unified view now), so both links now just point to plain `index.html`.

The profile photo (`assets/profile-priyadharshini.webp`) is still unused,
replaced by the vinyl illustration as described above.

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
