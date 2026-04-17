# `layouts/partials/site-footer.html` (NEW partial)

- **File**: [`layouts/partials/site-footer.html`](../../../../layouts/partials/site-footer.html)
- **Shadows upstream**: nothing — this is a new partial we added
- **Risk on Relearn upgrade**: 🟢 LOW

## What this partial does

Renders the visible site footer that appears at the bottom of every page's
content area. Contains:

- A "Powered by the Web & Information Committee of the IEEE Industrial
  Electronics Society" line.
- Social media icon links (Facebook, Twitter, LinkedIn, YouTube) pointing to
  the IES channels.
- An orange "Invitation to join IEEE IES" CTA button.
- A copyright line.

Scoped inline `<style>` gives it a top border, max-height social icons,
flex-column layout on small viewports, and the brand orange
(`#ec8c00`) for the CTA.

## Why it exists

Relearn 9.x doesn't provide a slot for a visible site footer — its
`custom-footer.html` is intended for trailing `<script>` tags and, due to
the `body { flex-direction: row-reverse }` layout, can't show visible
content anyway. We introduced this partial and wired it into the
[baseof.html override](../_default/baseof.md) so the footer lands inside
`#R-body`, at the bottom of the main scrollable column.

## What's custom vs upstream

Entirely ours — no upstream counterpart.

## Re-applying on upgrade

Nothing to do at Relearn upgrade time. The only coupling is:

- [`_default/baseof.md`](../_default/baseof.md) must still have the
  `{{- partial "site-footer.html" . }}` line inside `#R-body`. If the
  baseof override is re-synced to upstream, don't forget this line.
- CSS in this file uses class names like `.footer-section`, `.social-section`,
  `.btn-primary` that are local to this file — no risk of Relearn collisions.

## Related

- [`_default/baseof.md`](../_default/baseof.md) — wires this partial into the page layout
- [`custom-footer.md`](custom-footer.md) — the other ("upstream-level") footer slot, intentionally empty
- External images: social icons are fetched from
  `https://www.ieee-ies.org/modules/mod_ieeesocialmedia/icons/*.png` —
  if IES moves those, update the `<img src>` here.

## Template-customization note

The "Powered by the Web & Information Committee…" line and copyright are
generic IEEE IES boilerplate. You may want to add a conference-specific
line above or below (e.g., venue sponsor, dates). Edit the `<div class="footer-content">`.
