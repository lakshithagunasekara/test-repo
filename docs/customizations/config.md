# Config customizations

Documents every value we set in `config/_default/hugo.toml` and
`config/_default/params.toml` that deviates from the Relearn default.

Files covered:
- [`config/_default/hugo.toml`](../../config/_default/hugo.toml)
- [`config/_default/params.toml`](../../config/_default/params.toml)

---

## Per-conference values (edit these when cloning the template)

These four values make a template copy identify as a specific conference.

| Value | File + key | Used for | Example |
|---|---|---|---|
| **baseURL** | `hugo.toml` → `baseURL` | Absolute URLs, RSS, sitemap, OpenGraph, auto-generated `/CNAME` | `https://irai2026.ieee-ies.org` |
| **Full title** | `hugo.toml` → `title` | Browser tab suffix, OG `site_name`, RSS feed | `IEEE International Conference on Responsible AI 2026` |
| **Short title / abbreviation** *(optional)* | `hugo.toml` → `params.linkTitle` | Sidebar header below the logo. Set to `""` or remove the line entirely to show only the logo image (no text under it). | `IEEE IRAI 2026` |
| **Home-page hero title** | `content/_index.en.md` front matter → `title` | Browser tab prefix + home-page breadcrumb | `Welcome to IRAI 2026` |
| **Description** | `content/_index.en.md` front matter → `description` | Meta description, OpenGraph / Twitter card description | `The IEEE conference on responsible AI, held in Melbourne, Sep 2026.` |

The first three live in `hugo.toml` (grouped in a banner block at the top of
the file). The last two live in the home page's front matter — the user
edits that file anyway when customising the home page content, so it's not
extra burden.

**`static/CNAME` is generated automatically** from `baseURL` by
[`.github/actions/build_site/action.yaml`](../../.github/actions/build_site/action.yaml)
at build time — do not edit or commit it.

---

## Non-default Relearn settings in `hugo.toml`

Settings that aren't Relearn defaults, beyond the per-conference block above:

### `[outputs]`

```toml
[outputs]
  home = ["HTML", "RSS"]
  section = ["HTML", "RSS"]
  page = ["HTML", "RSS"]
```

Relearn's default output set includes `SEARCH` and `SEARCHPAGE` per-kind
outputs, and Hugo's defaults include `Sitemap` per-kind. We strip those
(Relearn 6→9 deprecated them; Hugo renders a single site-wide `sitemap.xml`
automatically).

### `[markup.tableOfContents]`

```toml
[markup.tableOfContents]
  startLevel = 1
  endLevel = 1
  ordered = false
```

Set to match our sidebar TOC customization — see
[`layouts/partials/menu.md`](layouts/partials/menu.md). `.TableOfContents`
(if called anywhere) produces only H1 anchors, unnumbered.

### `[markup.goldmark.renderer]`

```toml
[markup.goldmark.renderer]
  unsafe = true
```

Allows raw HTML in Markdown content. Needed for conference sites that embed
iframes, custom tables, or inline styling.

### `[markup.highlight]`

```toml
[markup.highlight]
  lineNumbersInTable = false
  noClasses = false
```

Relearn's default highlighting settings — kept as-is from the template's
starting state.

### `[languages.en]`

```toml
[languages.en]
  weight = 1
  languageName = "English"
  [languages.en.params]
    landingPageName = "Home"
```

`title` is intentionally absent — it falls back to the top-level `title`.
`landingPageName` is the label for the built-in Home link, though we
override that behaviour in `menu.html` and render "Home" ourselves.

---

## Non-default Relearn settings in `params.toml`

| Setting | Value | Why | Related |
|---|---|---|---|
| `logo = { src = "/images/logo.png" }` | — | Our `logo.html` override reads this path | [`layouts/partials/logo.md`](layouts/partials/logo.md) |
| `disableLandingPageButton` | `true` | We suppress the default `#R-homelinks` Home button and render Home as the first main-menu item ourselves | [`layouts/partials/menu.md`](layouts/partials/menu.md) |
| `alwaysopen` | `false` | Sidebar sections stay collapsed until clicked (was `""` / undefined — now explicitly false) | — |
| `math.disable` | `true` | MathJax disabled — unused on conference sites, saves bandwidth | — |
| `mermaid.disable` | `true` | Mermaid diagrams disabled — unused, saves bandwidth | — |
| `openapi.disable` | `true` | OpenAPI/Swagger disabled — unused | — |
| `search.disable` | `false` | Explicitly enabled (Relearn's default is also enabled, but we set it explicitly after the 8.0 rename from `disableSearch`) | — |

---

## Re-applying on upgrade

Relearn releases regularly rename or deprecate params. The build surfaces
these as `DEPRECATED` / `UNSUPPORTED` warnings with a URL to the migration
notes.

### Typical upgrade flow

1. Run `hugo --environment testing` after bumping the submodule.
2. For each deprecation warning, follow the link:
   - If the param was **renamed**, update `params.toml` accordingly.
   - If the param is **under a different namespace**, migrate (e.g.
     `disableSearch = false` → `search.disable = false` happened in 8.0).
   - If the param is **removed**, delete the line.
3. Rebuild until the warnings are gone.

### Past migrations absorbed in this repo

- **6.0**: `params.description` (site-level) → home-page front matter
- **7.0**: `disableMathJax` → `math.disable`
- **7.0**: `menuTitle` front matter → `linkTitle`
- **7.0**: dropped `search` / `searchpage` per-kind output formats
- **8.0**: `disableMermaid` → `mermaid.disable`
- **8.0**: `disableOpenApi` → `openapi.disable`
- **8.0**: `disableSearch` → `search.disable`

---

## Related

- [`content.md`](content.md) — front-matter migrations (`linkTitle`, home-page `description`)
- [`layouts/partials/menu.md`](layouts/partials/menu.md) — reads `params.alwaysopen`, `params.disableLandingPageButton`
- [`layouts/partials/logo.md`](layouts/partials/logo.md) — reads `params.logo.src`
- [`styles.md`](styles.md) — the `static/css/custom.css` file, loaded via `custom-header.html`
