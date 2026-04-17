# `layouts/partials/topbar/area/end.html` override

- **File**: [`layouts/partials/topbar/area/end.html`](../../../../../layouts/partials/topbar/area/end.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/topbar/area/end.html`
- **Risk on Relearn upgrade**: 🟡 MEDIUM

## What this override does

Prepends IEEE IES + IEEE parent-organisation brand logos to the right side of
Relearn's topbar, then re-emits the upstream list of topbar buttons
(edit, source, markdown, print, prev, next, more).

## Why it exists

Previous IES conference sites had partner logos in a separate strip above the
content. Injecting them into Relearn 9.x's `<body>` directly broke the flex
layout (row-reverse ordering pushed the strip off-screen). The cleanest
integration is to place them inside the existing topbar, where Relearn
already has a right-aligned area (`topbar-area-end`) that we override.

## What's custom vs upstream

Upstream is just a call list of button partials. We prepend one `<div>` with
logos + scoped `<style>`, then re-emit the unchanged button list.

The custom block at the top of the file:

```html
<div class="site-brand-logos">
  <a href="https://www.ieee-ies.org/" target="_blank" rel="noopener" aria-label="IEEE IES">
    <img src="{{ "images/ies.png" | relURL }}" alt="IEEE IES">
  </a>
  <a href="https://www.ieee.org/" target="_blank" rel="noopener" aria-label="IEEE">
    <img src="{{ "images/ieee.png" | relURL }}" alt="IEEE">
  </a>
</div>
<style>
  .site-brand-logos { … }
</style>
```

Sizing + responsive breakpoints for `.site-brand-logos` also live in
[`static/css/custom.css`](../../../../../static/css/custom.css) (search for
`#R-topbar .site-brand-logos`).

## Re-applying on upgrade

1. Diff upstream's topbar button list:

   ```bash
   git -C themes/hugo-theme-relearn diff <OLD>..<NEW> -- layouts/partials/topbar/area/end.html
   ```

2. If upstream added, removed, or renamed a button, update our copy's button
   list (after the `<style>` block) to match the new upstream list. The
   button partial names follow the pattern `topbar/button/<name>.html`.

3. If upstream kept the same button list, nothing else to change.

4. Verify: after build, the topbar should still show the IES + IEEE logos
   on the right edge, followed by the Relearn topbar buttons (usually a
   `more` ⋮ button collapsing the rest on mobile).

## Related

- `static/images/ies.png`, `static/images/ieee.png` — the two logo images
- [`styles.md`](../../../../styles.md) — sizing for `.site-brand-logos`
- Upstream list of available topbar button partials:
  `themes/hugo-theme-relearn/layouts/partials/topbar/button/`

## Template-customization note

If a host institution needs to add their logo alongside IES + IEEE, uncomment
the "Add additional partner/host logos here" block inside this override
(e.g. La Trobe University logo in the irai2026 site).
