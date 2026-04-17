# `layouts/partials/menu-footer.html` override

- **File**: [`layouts/partials/menu-footer.html`](../../../../layouts/partials/menu-footer.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/menu-footer.html`
- **Risk on Relearn upgrade**: 🟢 LOW

## What this override does

Renders two partner logos (IEEE IES + IEEE parent organisation) stacked
vertically at the bottom of the sidebar. Replaces Relearn's default
"Built with ❤ using Hugo" promo line.

## Why it exists

IES conferences credit their sponsoring societies in the sidebar footer.
This is the slot Relearn provides for that.

## What's custom vs upstream

Upstream:

```html
<p>Built with <a href="https://github.com/McShelby/hugo-theme-relearn">…</a> by <a href="https://gohugo.io/">Hugo</a></p>
```

Ours:

```html
<div class="menu-footer-class">
    <a href="https://www.ieee-ies.org" target="_blank" rel="noopener">
        <img src="/images/ies.png" alt="IEEE IES Logo">
    </a>
    <a href="https://www.ieee.org" target="_blank" rel="noopener">
        <img src="/images/ieee.png" alt="IEEE Logo">
    </a>
    <!-- Add additional partner/host logos here as needed, e.g.:
    <a href="https://www.your-host-university.example" target="_blank" rel="noopener">
        <img src="/images/host-logo.png" alt="Host Logo">
    </a>
    -->
</div>
```

Sizing + flex centering are in
[`static/css/custom.css`](../../../../static/css/custom.css) under
`#R-sidebar .menu-footer-class` (3.75rem max-height, flex-column, centered).

## Re-applying on upgrade

Nothing to re-apply from upstream — our HTML is independent. After each
upgrade, confirm the sidebar bottom still shows both logos centered and the
images render at a reasonable size.

## Related

- [`styles.md`](../../styles.md) — `.menu-footer-class` sizing rules
- `static/images/ies.png`, `static/images/ieee.png`

## Template-customization note

If a host institution needs to add their logo (e.g. La Trobe for irai2026),
uncomment the commented block inside this file.
