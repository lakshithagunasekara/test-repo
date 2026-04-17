# `layouts/partials/menu.html` override

- **File**: [`layouts/partials/menu.html`](../../../../layouts/partials/menu.html)
- **Shadows upstream**: `themes/hugo-theme-relearn/layouts/partials/menu.html`
- **Risk on Relearn upgrade**: 🔴 HIGH (longest + most internally-coupled override)

## What this override does

A full copy of Relearn's menu.html with **two** patches:

1. **Home as the first main-menu item.** Relearn 9.x renders Home in a
   separate `#R-homelinks` region with its own typography and container
   background. We suppress that (via `params.disableLandingPageButton = true`)
   and instead synthesize a `Home` `<li>` as the first child of the main
   sidebar `<ul>`. This way Home blends visually with the rest of the menu.

2. **H1 TOC as collapsible sub-menu for leaf pages.** Upstream renders a leaf
   page (one with no child pages in the filesystem) as a simple `<li><a>`.
   We extend that branch so if the page's Markdown has any H1 headings, they
   render as a nested `<ul class="collapsible-menu page-toc-menu">` under
   the link. Each H1 becomes a menu item linking to the in-page anchor.
   Collapsible + default-closed.

This is the biggest and most fragile override in the repo.

## Why it exists

### Home-as-first-item
Otherwise Home renders at a smaller size with a dark container background in
the zen-light variant, visually inconsistent with the section list.

### H1 TOC injection
The previous design (wictemplate24 on Relearn 6.4.1, and irai2026) injected
each page's `.TableOfContents` into the sidebar so users could jump to major
sections from any page. Relearn 9.x dropped that behaviour. We re-implement
it using `.Fragments.Headings` (a Hugo 0.111+ API) filtered to level-1 only,
matching the original UX.

## What's custom vs upstream

Two marker blocks — both inside the inline `partials/inline/page-tree` and
`partials/inline/page-walker` template definitions.

### Patch 1 — prepend Home

Inside the main UL rendering (search for `<ul class="{{ $classes }}collapsible-menu">`):

```go-template
{{- /* wictemplate24 customization: prepend "Home" as the first entry of the
      MAIN sidebar menu so it renders inline with the rest, rather than in a
      separate visually-different #R-homelinks block. Gated on $config.main
      so this only applies to the primary menu. */ -}}
{{- if ($config.main | default true) }}
  {{- $homeUrl := partial "permalink.gotmpl" (dict "to" site.Home) }}
  {{- $homeTitle := T "Home" | default "Home" }}
  {{- $isHomeActive := eq site.Home $currentNode }}
          <li class="{{if $isHomeActive }}active{{end}}" data-nav-id="{{ $homeUrl }}"><a class="padding" href="{{ $homeUrl }}">{{ $homeTitle }}</a></li>
{{- end }}
```

### Patch 2 — H1 TOC in the leaf branch

Inside `partials/inline/page-walker`, find the `{{- else if $url }}` branch
(the one that handles leaf pages with no visible children). Replace it with:

```go-template
{{- else if $url }}
  {{- /* Build a list of H1 headings from this page to render under the link. */ -}}
  {{- $h1s := slice }}
  {{- range .Fragments.Headings }}
    {{- if eq .Level 1 }}
      {{- $h1s = $h1s | append . }}
    {{- end }}
  {{- end }}
  {{- if $h1s }}
    {{- /* render as collapsible <li>; the inner <ul class="collapsible-menu page-toc-menu">
          iterates $h1s and emits <li class="page-toc-item"><a href="{{ $url }}#{{ .ID }}">...</a></li> */ -}}
    ...
  {{- else }}
    {{- /* original simple leaf rendering preserved here */ -}}
    ...
  {{- end }}
{{- end }}
```

The full code of this branch lives in the override file. See lines 177–230
of `layouts/partials/menu.html` for the complete rendering.

## Re-applying on upgrade

1. Diff upstream:

   ```bash
   git -C themes/hugo-theme-relearn diff <OLD>..<NEW> -- layouts/partials/menu.html
   ```

2. If upstream changed ANY of the following, tread carefully:
   - The `partials/inline/page-tree` template definition (affects Patch 1)
   - The `partials/inline/page-walker` template definition (affects Patch 2)
   - Signatures of `permalink.gotmpl` or `_relearn/linkAttributes.gotmpl`

3. If changes are minor (cosmetic, comments, unrelated branches), manually
   merge upstream hunks into our file.

4. If changes are structural (whole templates rewritten), the cleanest path
   is:
   - Copy the new upstream over ours: `cp themes/hugo-theme-relearn/layouts/partials/menu.html layouts/partials/menu.html`
   - Re-apply **Patch 1** inside `partials/inline/page-tree` right after the `<ul class="{{ $classes }}collapsible-menu">` opening tag.
   - Re-apply **Patch 2** inside `partials/inline/page-walker` at the `{{- else if $url }}` branch.

5. Verify by building, then browser-check:
   - Home page: "Home" shows as the first main-menu entry, active/highlighted.
   - `/for_authors/`: section expands, shows H1 TOC items (Important Dates,
     Call for Papers, etc.) under the link, no numbering, chevron vertically
     aligned.
   - Other leaf pages with no H1s (e.g. `/about/`): render as simple `<li><a>`.

## Related

- [`config.md`](../../config.md) — `params.disableLandingPageButton = true` +
  `markup.tableOfContents.startLevel = endLevel = 1`
- [`styles.md`](../../styles.md) — `.page-toc-menu`, `.page-toc-item`,
  `#R-sidebar` CSS that styles what this override emits
- [`content.md`](../../content.md) — content pages whose H1s feed this TOC
