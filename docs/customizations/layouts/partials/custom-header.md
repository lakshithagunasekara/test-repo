# `layouts/partials/custom-header.html` override

- **File**: [`layouts/partials/custom-header.html`](../../../../layouts/partials/custom-header.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/custom-header.html`
- **Risk on Relearn upgrade**: 🟢 LOW

## What this override does

Head-only content. Auto-loads `static/css/custom.css` and `static/js/custom.js`
if either exists. Adds a documentation comment explaining that **body-level
content does NOT belong here** (Relearn 9.x calls this partial inside
`<head>`, so anything non-head-legal gets moved by browsers or silently
dropped).

## Why it exists

- Preserves Relearn 9.x's built-in auto-load convention while allowing our
  CSS + JS overrides to drop into `static/` without extra template edits.
- The comment serves as a guardrail: an earlier version of this file
  injected the `<div class="navbar">` logos here, which rendered broken
  because `<div>` isn't valid HTML inside `<head>`. The site-wide navbar
  now lives in [`topbar/area/end.md`](topbar/area/end.md).

## What's custom vs upstream

Upstream's version is functionally similar but shorter. Ours:

- Uses `fileExists "static/%s"` (no leading slash — Hugo's `fileExists`
  takes paths relative to project root, NOT filesystem root).
- Has a docblock warning about not putting body content here.

The auto-load logic itself is a direct copy of upstream's pattern:

```go-template
{{- $assetBusting := partialCached "assetbusting.gotmpl" site.Home }}
{{- $filePath := "css/custom.css" }}
{{- with (resources.Get (printf "/%s" $filePath)) }}
    <link href="{{ .RelPermalink }}{{ $assetBusting }}" rel="stylesheet">
{{- else }}
  {{- if (fileExists (printf "static/%s" $filePath)) }}
    <link href="{{ printf "%s" $filePath | relURL }}{{ $assetBusting }}" rel="stylesheet">
  {{- end }}
{{- end }}
```

(Same block repeated for `js/custom.js`.)

## Re-applying on upgrade

1. Diff upstream:

   ```bash
   git -C themes/hugo-theme-relearn diff <OLD>..<NEW> -- layouts/partials/custom-header.html
   ```

2. Check if the `assetbusting.gotmpl` partial was renamed upstream. If yes,
   update the `partialCached` call in our file.

3. Otherwise no action needed. The auto-load mechanism is simple and has
   been stable across Relearn versions.

## Related

- [`styles.md`](../../styles.md) — `static/css/custom.css` loaded by this partial
- [`topbar/area/end.md`](topbar/area/end.md) — where the site logos actually go (body, not head)
