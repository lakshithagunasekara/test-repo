# `layouts/partials/logo.html` override

- **File**: [`layouts/partials/logo.html`](../../../../layouts/partials/logo.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/logo.html`
- **Risk on Relearn upgrade**: 🟡 MEDIUM

## What this override does

Replaces Relearn's default logo rendering (which delegates to
`auto-logo.html`, expects an inline SVG, and adds `lazy`, `figure-image`,
`bg-white`, `border` classes to the `<img>`) with a minimal `<img>` sourced
from `params.logo.src` and sized to fit the sidebar header.

Includes scoped inline CSS that re-enables the `.logo-image` container
(Relearn's default CSS hides it with `display: none` when no inline SVG
logo is present).

The sidebar title under the logo is **opt-in** via `params.linkTitle`:
- Set to a non-empty string → that string renders below the logo image.
- Set to `""` or unset → only the logo image renders, no text. Importantly,
  we do NOT fall back to `site.Title` here (that's reserved for the browser
  tab and OpenGraph). This keeps the sidebar uncluttered for conferences
  whose logos already include the conference name as part of the image.

## Why it exists

The auto-logo rendering was designed for SVG logos. A PNG logo rendered
through it gets:

- Unwanted classes (`bg-white border` → white box + border around the logo)
- `lazy` loading attribute (causes a visible pop-in on first load)
- `height: auto; width: auto;` (renders at natural PNG dimensions — usually
  too big for the sidebar)

Our override strips all of that and renders a plain `<img>` with explicit
`max-height: 6.5rem`.

## What's custom vs upstream

The whole file is ours — upstream's logo.html is a one-liner that delegates
to auto-logo.html. Key parts of our version:

```go-template
{{- $title := site.Params.linkTitle | default site.Title }}
{{- $logoSrc := (site.Params.logo).src | default "/images/logo.png" }}
<a id="R-logo" class="R-default direction-column" href="{{ partial "relLangPrettyUglyURL.hugo" (dict "to" site.Home) }}">
  <span class="logo-image default-animation site-logo-wrap">
    <img src="{{ $logoSrc | relURL }}" alt="{{ $title }}" class="site-logo">
  </span>
  <span class="logo-title default-animation">{{ $title }}</span>
</a>
<style>
  /* Relearn 9.x hides .logo-image via `#R-logo.R-default .logo-image { display: none }` — re-enable it. */
  #R-logo.R-default .logo-image.site-logo-wrap { display: block; … }
  #R-logo.R-default img.site-logo { max-height: 6rem; … }
</style>
```

Also referenced from `static/css/custom.css` which further bumps the
`max-height` to 6.5rem and styles the title below.

## Re-applying on upgrade

1. Diff upstream's `auto-logo.html` (not `logo.html`, which is just a
   one-line delegate):

   ```bash
   git -C themes/hugo-theme-relearn diff <OLD>..<NEW> -- layouts/partials/auto-logo.html
   ```

2. If auto-logo.html's HTML *structure* changed (new wrapper elements,
   renamed classes), our override may need matching class names.

3. Check if Relearn 9.x+ still hides `.logo-image` by default:

   ```bash
   grep -n 'logo-image' themes/hugo-theme-relearn/assets/css/theme.css | head
   ```

   If the `display: none` rule is gone, our CSS reset becomes redundant
   (harmless but worth removing).

4. Verify: after build, the sidebar shows `static/images/logo.png` cleanly,
   no white background/border, sized to roughly 6rem tall, followed by the
   site title.

## Related

- [`config.md`](../../config.md) — `params.logo.src = "/images/logo.png"`
  config (this is what our override reads)
- [`styles.md`](../../styles.md) — `#R-logo.R-default img.site-logo`
  sizing rules
- `static/images/logo.png` — the logo image itself
