# Portfolio site — working notes

Static site: `index.html` (homepage) + `resume.html` + four case studies
(`case-study-ai-agents.html`, `case-study-admin.html`, `case-study-erp.html`,
`case-study-summary-builder.html`), sharing `styles.css` (case study design
system, with per-project themes via `body.theme-purple` / `body.theme-green`)
and `home.css` (homepage/resume only).

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
