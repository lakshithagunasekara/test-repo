# `layouts/partials/custom-footer.html` override

- **File**: [`layouts/partials/custom-footer.html`](../../../../layouts/partials/custom-footer.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/custom-footer.html`
- **Risk on Relearn upgrade**: 🟢 LOW

## What this override does

Deliberately empty placeholder. Exists only to document that **the visible
site footer is NOT here** — it's in
[`site-footer.html`](site-footer.md), injected from the baseof.html override.

## Why it exists

Relearn 9.x calls `custom-footer.html` at the very end of `<body>`, outside
`#R-body`. Content placed here ends up as a body-level flex item, which the
row-reverse layout pushes out of view. It's meant for trailing `<script>`
tags only.

An earlier revision of this template put the visible footer HTML in this
file (carried over from a Relearn 6.x idiom) — and it was visually invisible.
We moved the footer to `site-footer.html` and left this one empty, with the
explanation baked in as a comment.

## What's custom vs upstream

Upstream has a short comment scaffold for "add trailing scripts here". Ours
is the same idea plus a longer doc-comment explaining where the visible
footer lives. No functional difference.

## Re-applying on upgrade

Nothing to re-apply. If you need to add trailing-of-body `<script>` tags
(for analytics, etc.), this is the correct file to put them in.

## Related

- [`_default/baseof.md`](../_default/baseof.md) — where `site-footer.html` gets injected instead
- [`site-footer.md`](site-footer.md) — the visible site footer
