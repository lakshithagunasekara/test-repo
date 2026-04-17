# IEEE IES Conferences — Website Template

A Hugo-based website template for IEEE IES conferences. Uses the upstream
[Hugo Relearn theme](https://mcshelby.github.io/hugo-theme-relearn/index.html)
with a thin layer of project-root overrides for IES branding, layout tweaks,
and custom shortcodes.

Built for **Hugo 0.141.0** (pinned via `.tool-versions`) and **Relearn 9.0.3**
(pinned as a git submodule under `themes/hugo-theme-relearn`).

---

## Setup a new conference from this template

1. **Create your repo** — on GitHub, click **Use this template → Create a new
   repository** on the template's GitHub page.

2. **Edit three lines** at the top of [`config/_default/hugo.toml`](config/_default/hugo.toml)
   (the GitHub web editor is enough — no clone needed):

   ```toml
   baseURL           = "https://your-conference.ieee-ies.org"
   title             = "IEEE International Conference on <Topic> 2026"  # full form
   params.linkTitle  = "IEEE <Short> 2026"                                # short form for sidebar
   ```

   - `baseURL` drives absolute URLs, RSS, sitemap, OpenGraph — and is
     written to `static/CNAME` automatically by the build workflow.
   - `title` (full) goes into the browser tab (after the page name),
     OpenGraph `site_name`, and RSS feed.
   - `params.linkTitle` (short) is shown under the logo in the sidebar.
     **Optional** — set to `""` or remove the line entirely if your logo
     image already includes the conference name and you don't want
     additional text under it.

3. **Edit the home-page hero** — [`content/_index.en.md`](content/_index.en.md)
   front matter:

   ```toml
   +++
   title       = "Welcome to <your conference>"       # appears above the home content
   description = "A short one-sentence conference tagline."  # meta + OG description
   +++
   ```

4. **Commit.** The `docs-build-deployment` workflow builds the site and
   pushes the result to the `gh-pages` branch.

5. **Settings → Pages → Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: **`gh-pages` / `(root)`** (the branch appears after the first
     successful deploy)

6. *(optional)* **Custom domain**
   - Settings → Pages → Custom domain → enter the same host that's in
     `baseURL` (e.g. `your-conference.ieee-ies.org`)
   - At your DNS provider, add a `CNAME` record pointing to
     `<your-username>.github.io`

The site is now live. All subsequent edits to content files auto-deploy on push.

---

## Editing content

All content lives under `content/`:

```
content/
├── _index.en.md              # Home page (hero + welcome text)
├── about/
├── committee/
├── for_attendees/
├── for_authors/
├── news_and_updates/
├── program/
├── registration/
└── sponsors/
```

Each section is a directory containing an `_index.md` (or `_index.en.md`).
These are standard Hugo Markdown files with TOML front matter and can be
edited directly through the GitHub web UI (pencil icon on any `.md` file).

### Front matter basics

```toml
+++
title     = "Page Title"
linkTitle = "Sidebar Label"   # optional, overrides title in the sidebar
weight    = 10                 # optional, orders sections in the sidebar
+++
```

To hide a page while keeping it in the menu (e.g. under construction), add:

```toml
[_build]
  render = 'never'
```

Remove that block when you're ready to publish the page.

### Static files

Put images, PDFs, and other unprocessed assets in `static/`. They're served
from the site root. For example, `static/images/slideshow/1.jpg` is reachable
at `/images/slideshow/1.jpg` and can be referenced as `images/slideshow/` in
shortcodes.

Note: `static/CNAME` is **auto-generated** from `baseURL` at build time — do
not edit or commit it.

---

## Custom shortcodes

### `slideshow`

Responsive auto-advancing image carousel with overlay text, dot navigation,
and keyboard/arrow controls.

```hugo
{{< slideshow path="images/slideshow" largeText="Conference Location" smallText="Conference Dates" >}}
```

- Renders every `.jpg`, `.jpeg`, or `.png` in the given path.
- Images auto-fit a 1120×460 frame, maintaining aspect ratio.
- Auto-advances every 4 seconds; arrows and dots for manual navigation.

### `card`

A Bootstrap-style card with a coloured header and body content.

```hugo
{{< card title="Topics of Interest" >}}
- Adaptive and Intelligent Control Systems
- Autonomous Robotic Systems, AI, and Machine Learning
- ...
{{< /card >}}
```

### `imagesRow`

A responsive row of people/logo thumbnails with name/affiliation captions.

```hugo
{{< imagesRow images=`[
  {"src": "/images/sessions/emily_davis.jpeg", "alt": "Emily Davis", "name": "Emily Davis", "company": "University X", "location": "City, Country"},
  {"src": "/images/sessions/oliver_wang.jpg",  "alt": "Oliver Wang", "name": "Oliver Wang", "company": "University Y", "location": "City, Country"}
]` >}}
```

### `imageWithOverlay`

A single image with text overlaid on it.

### `pdf-embed`

Embeds a PDF viewer for a local or external PDF.

```hugo
{{< pdf-embed type="external" url="https://example.org/paper.pdf" >}}
```

### `spacer`

Vertical whitespace in units of line-height.

```hugo
{{< spacer lines=2 >}}
```

---

## Local development

If you want to preview changes locally before pushing:

```bash
# one-time setup
git clone --recurse-submodules <your-repo-url>
cd <your-repo>
asdf install hugo 0.141.0   # or install Hugo 0.141.0 any way you like

# run the dev server (hot-reload)
hugo server
```

Then open <http://127.0.0.1:1313/>.

---

## Project structure

```
.
├── config/_default/
│   ├── hugo.toml       # per-conference values at the top + generic machinery below
│   └── params.toml     # Relearn theme params
├── content/            # Markdown content
├── layouts/            # Project-level overrides of theme partials + custom shortcodes
│   ├── _default/baseof.html
│   ├── partials/       # custom-header, custom-footer, logo, menu, etc.
│   └── shortcodes/     # slideshow, card, imagesRow, pdf-embed, spacer, imageWithOverlay
├── static/
│   ├── css/custom.css  # site-specific styling
│   └── images/         # favicon, logo, and conference-specific images
├── themes/hugo-theme-relearn/   # submodule — do not edit directly
└── .github/
    ├── workflows/docs-build-deployment.yaml
    └── actions/{build_site,deploy_site}/action.yaml
```

---

## Customizations on top of Relearn

Every deviation this template makes from stock Relearn — layout overrides,
custom CSS, shortcodes, config tweaks, front-matter migrations — is
documented in [`docs/customizations/`](docs/customizations/). That folder
is structured so you can open the per-file doc for any override and see:

- What it does, and why it exists
- Exactly which lines differ from upstream
- How to re-apply our patch on a newer Relearn
- A risk rating (🔴 HIGH / 🟡 MEDIUM / 🟢 LOW) for upgrade impact

Start at [`docs/customizations/README.md`](docs/customizations/README.md)
for the index and upgrade checklist.

## Upgrading the Hugo Relearn theme

High-level flow:

```bash
cd themes/hugo-theme-relearn
git fetch --tags
git checkout <new-tag>    # e.g. 9.1.0
cd ../..
git add themes/hugo-theme-relearn
# rebuild, browser-check, re-sync any overrides that drifted
git commit -m "Bump Relearn to <new-tag>"
```

**Before committing**, walk through the checklist in
[`docs/customizations/README.md`](docs/customizations/README.md#upgrade-procedure-short-version).
The HIGH-risk overrides (`layouts/_default/baseof.html`,
`layouts/partials/menu.html`) are full copies of upstream templates — any
upstream changes between the old and new pin need to be merged back into
our copy manually.

Also bump `.tool-versions` and `.github/actions/build_site/action.yaml` if
the new Relearn requires a newer Hugo (check
`themes/hugo-theme-relearn/theme.toml` for the `[module.hugoVersion].min`
value).
