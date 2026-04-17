# Customizations on top of Hugo Relearn

This folder inventories every deviation this template makes from the upstream
[Hugo Relearn theme](https://mcshelby.github.io/hugo-theme-relearn/). Each
document mirrors the path of the file it describes — e.g.
[`layouts/partials/menu.md`](layouts/partials/menu.md) documents the override
at `layouts/partials/menu.html`.

**Point a new Claude Code session (or a new maintainer) at this folder before
upgrading Relearn.** Each file is self-contained: it explains what the
override does, why it exists, what's different from upstream, and how to
re-apply our patch onto a newer upstream version.

**Theme currently pinned**: Relearn 9.0.3 (commit `8bb66fa6743`, submodule at
`themes/hugo-theme-relearn`).
**Hugo currently pinned**: 0.141.0 (`.tool-versions`).

---

## Per-conference setup (TL;DR)

Five values identify a specific conference, across two files:

| Value | File | Key |
|---|---|---|
| Public URL | `config/_default/hugo.toml` | `baseURL` |
| Full site title (browser tab, OG) | `config/_default/hugo.toml` | `title` |
| Short title / abbreviation (sidebar) *(optional)* | `config/_default/hugo.toml` | `params.linkTitle` (set `""` or omit to hide) |
| Home-page hero title | `content/_index.en.md` front matter | `title` |
| Description (meta, OG) | `content/_index.en.md` front matter | `description` |

The first three are grouped in a banner block at the top of `hugo.toml`; the
last two live alongside home-page content (which you're editing anyway).
`static/CNAME` is generated automatically from `baseURL` by the build
workflow — don't commit it. See [`config.md`](config.md) and
[`content.md`](content.md) for details.

---

## Docs index

### Layout overrides (`layouts/`)

Files here shadow the corresponding upstream files in
`themes/hugo-theme-relearn/layouts/`. Our copy wins Hugo's template lookup,
which means any upstream change is silently masked until we re-sync.

| Doc | Override path | Risk |
|---|---|:---:|
| [`layouts/_default/baseof.md`](layouts/_default/baseof.md) | `layouts/_default/baseof.html` | 🔴 HIGH |
| [`layouts/partials/menu.md`](layouts/partials/menu.md) | `layouts/partials/menu.html` | 🔴 HIGH |
| [`layouts/partials/topbar/area/end.md`](layouts/partials/topbar/area/end.md) | `layouts/partials/topbar/area/end.html` | 🟡 MEDIUM |
| [`layouts/partials/logo.md`](layouts/partials/logo.md) | `layouts/partials/logo.html` | 🟡 MEDIUM |
| [`layouts/partials/custom-header.md`](layouts/partials/custom-header.md) | `layouts/partials/custom-header.html` | 🟢 LOW |
| [`layouts/partials/custom-footer.md`](layouts/partials/custom-footer.md) | `layouts/partials/custom-footer.html` | 🟢 LOW |
| [`layouts/partials/menu-footer.md`](layouts/partials/menu-footer.md) | `layouts/partials/menu-footer.html` | 🟢 LOW |
| [`layouts/partials/heading.md`](layouts/partials/heading.md) | `layouts/partials/heading.html` | 🟢 LOW |
| [`layouts/partials/site-footer.md`](layouts/partials/site-footer.md) | `layouts/partials/site-footer.html` (NEW — not shadowing) | 🟢 LOW |
| [`shortcodes.md`](shortcodes.md) | `layouts/shortcodes/*.html` (all NEW) | 🟢 LOW |

### Styles, config, content

| Doc | Describes |
|---|---|
| [`styles.md`](styles.md) | `static/css/custom.css` — Relearn-selector overrides |
| [`config.md`](config.md) | Non-default values in `config/_default/hugo.toml` and `params.toml` |
| [`content.md`](content.md) | Front-matter migrations (`linkTitle`, home-page `description`) |

---

## Risk tiers at a glance

🔴 **HIGH** — Full copies of upstream templates with our patches inline. If
upstream changes the file, our copy silently shadows those changes. On
upgrade: diff upstream between the old and new pins, merge upstream changes
into our copy, re-apply our patches.

🟡 **MEDIUM** — Shadows upstream *or* targets upstream selectors. Breakage is
usually visible (CSS selectors stop matching, a button goes missing), but not
a build-time error.

🟢 **LOW** — Self-contained or tiny. Rarely breaks, but spot-check rendering
after an upgrade.

---

## Upgrade procedure (short version)

1. Bump the Relearn submodule: `cd themes/hugo-theme-relearn && git fetch --tags && git checkout <new-tag>`.
2. Check `themes/hugo-theme-relearn/theme.toml` for the minimum Hugo version. Bump `.tool-versions` and `.github/actions/build_site/action.yaml` if needed.
3. For each 🔴 HIGH- and 🟡 MEDIUM-risk override, read the corresponding doc in this folder. Follow the "Re-applying on upgrade" section.
4. Clean build: `rm -rf public resources .hugo_build.lock && hugo --environment testing`. Address every `DEPRECATED`/`UNSUPPORTED` warning.
5. `hugo server` — browser-check the pages listed in the checklist at the bottom of [`styles.md`](styles.md).
6. Commit the bumped submodule + any re-synced overrides together.

---

## Commands cheatsheet

```bash
# Current pin
git -C themes/hugo-theme-relearn describe --tags
git -C themes/hugo-theme-relearn rev-parse HEAD

# Latest upstream tags
git -C themes/hugo-theme-relearn fetch --tags
git -C themes/hugo-theme-relearn tag --sort=-v:refname | head

# What changed upstream between two Relearn revisions, for every file we override
OLD=<sha-or-tag>; NEW=<sha-or-tag>
for f in \
  _default/baseof.html \
  partials/menu.html \
  partials/topbar/area/end.html \
  partials/logo.html \
  partials/custom-header.html \
  partials/custom-footer.html \
  partials/menu-footer.html \
  partials/heading.html; do
  echo "=== $f ==="
  git -C themes/hugo-theme-relearn diff "$OLD".."$NEW" -- "layouts/$f"
done
```
