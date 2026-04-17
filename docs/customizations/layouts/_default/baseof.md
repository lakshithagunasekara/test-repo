# `layouts/_default/baseof.html` override

- **File**: [`layouts/_default/baseof.html`](../../../../layouts/_default/baseof.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/_default/baseof.html`
- **Risk on Relearn upgrade**: 🔴 HIGH

## What this override does

A full copy of Relearn's baseof.html with **one** inserted line:
`{{- partial "site-footer.html" . }}` placed inside `#R-body`, just before the
closing `</main>` tag. That injects our visible site footer into the
scrollable main content column.

## Why it exists

Relearn 9.x calls `custom-footer.html` at the very end of `<body>`, outside
`#R-body`. The body element uses `display: flex; flex-direction: row-reverse`,
so any direct children of `<body>` become horizontal row items — our footer
ends up to the left of `#R-body` (covered by the sidebar) and is visually
invisible.

To render the footer at the bottom of the page content, we inject a
`site-footer.html` partial *inside* `#R-body`, after the `<main>`. See
[`layouts/partials/site-footer.md`](../partials/site-footer.md) for the
partial itself.

## What's custom vs upstream

Search for the marker comment in the file:

```go-template
{{- /* Site-wide visible footer (IES branding, social links, copyright).
      Relearn 9.x calls `custom-footer.html` at body-level (meant for
      trailing <script> tags), which ends up to the left of #R-body due
      to `body { display: flex; flex-direction: row-reverse }` and is
      therefore invisible. We inject site-footer.html inside the main
      scrollable column so it appears at the bottom of page content. */ -}}
{{- partial "site-footer.html" . }}
```

Everything else is an unmodified copy of the upstream file.

## Re-applying on upgrade

1. Diff upstream between old and new pins:

   ```bash
   git -C themes/hugo-theme-relearn diff <OLD>..<NEW> -- layouts/_default/baseof.html
   ```

2. If there are changes, copy the new upstream version over ours:

   ```bash
   cp themes/hugo-theme-relearn/layouts/_default/baseof.html \
      layouts/_default/baseof.html
   ```

3. Re-insert the patch: find the closing `</main>` inside `#R-body` and
   immediately before it, paste the `Site-wide visible footer …` comment +
   `{{- partial "site-footer.html" . }}` line shown above.

4. Verify by building and confirming the footer still appears at the bottom
   of any content page (e.g. `/for_authors/`).

## Related

- [`layouts/partials/site-footer.md`](../partials/site-footer.md) — the actual footer partial
- [`layouts/partials/custom-footer.md`](../partials/custom-footer.md) — Relearn's end-of-body slot, now empty
