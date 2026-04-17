# `layouts/partials/heading.html` override

- **File**: [`layouts/partials/heading.html`](../../../../layouts/partials/heading.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/heading.html`
- **Risk on Relearn upgrade**: 🟢 LOW

## What this override does

Renders an empty `<h1></h1>`, suppressing the automatic page title Relearn
normally renders above the article content.

## Why it exists

Our content pages typically start with a custom hero element (a
`{{< slideshow >}}`, a banner image, or the first section's H1 from the
markdown) — rendering the Hugo-derived page title above that duplicates
information and breaks the visual flow.

irai2026 carried this same override; we preserved it.

## What's custom vs upstream

Upstream:

```go-template
{{- $title := partial "title.gotmpl" (dict "page" .) }}
<h1 id="{{ $title | plainify | anchorize }}">{{ $title }}</h1>
```

Ours:

```html
<h1></h1>
```

(Literally one line, no template logic.)

## Re-applying on upgrade

Nothing to re-apply. If at some point you want the Hugo page title back,
just delete this file — Relearn's version will take over.

## Related

- Content authors: since there's no automatic `<h1>`, remember to start
  each section page's Markdown with an explicit H1 (e.g. `# Important Dates`)
  if you want a visible title.
