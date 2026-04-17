# `static/css/custom.css` — custom styles

- **File**: [`static/css/custom.css`](../../static/css/custom.css)
- **Loaded by**: [`layouts/partials/custom-header.html`](../../layouts/partials/custom-header.html) override (Relearn 9.x auto-load convention)
- **Risk on Relearn upgrade**: 🟡 MEDIUM — targets Relearn-internal selectors that can be renamed silently

Every rule in this file is scoped under an ID or class. The intent is
surgical overrides, never blanket resets. Losing a rule just means the
related element falls back to Relearn's default rendering — rarely fatal,
but the visual change is often obvious.

## What each rule block does

| Selector (or prefix) | Purpose |
|---|---|
| `#R-topbar`, `#R-topbar .topbar-wrapper` | Adds the shadow under the topbar + min-height 5rem to match the 6.4.1 look |
| `#R-topbar .topbar-breadcrumbs` | Styles the breadcrumb / page title in IEEE blue (`#0082ba`), larger font, responsive sizes at 991 / 576 px breakpoints |
| `#R-sidebar #R-header-wrapper` | Tightens padding around the sidebar logo area |
| `#R-logo.R-default img.site-logo`, `#R-logo.R-default .logo-image.site-logo-wrap` | Re-enables `.logo-image` (Relearn hides it by default) and caps the logo at 6.5rem height |
| `#R-logo.R-default .logo-title` | Sizes the site title text below the logo |
| `#R-sidebar ul.collapsible-menu > li` and variants | Tightens vertical rhythm between menu items (0.1rem margin-bottom, 0.25rem padding on links) |
| `#R-sidebar ul.collapsible-menu > li > input/label` | Centers the collapse-chevron against the link's first-line height via flex (top 0.25rem, height = 1 * 1.3rem) |
| `#R-sidebar ul.page-toc-menu` and `> li.page-toc-item` | Styles the H1-TOC sub-items emitted by our `menu.html` override — no bullets, indented 1.5rem, 0.95rem font |
| `#R-sidebar .menu-footer-class` | Flex-column, centered layout for the IES + IEEE sidebar-bottom partner logos; max-height 3.75rem |
| `#R-topbar .site-brand-logos` and `img` | Sizes the IES + IEEE logos inside the topbar to match the topbar height (2.25rem) |
| `#R-sidebar #R-homelinks` and descendants | Blanket `background: transparent !important` to kill Relearn's dark "menu header" background (unused since we moved Home into the main menu) |
| `#R-sidebar ul.collapsible-menu > li.active.parent` | When a section is both active AND a parent, the active highlight wraps the parent LI and its child TOC items in one bordered card (instead of just outlining the link) |
| `main#R-body-inner article p` | Justified paragraphs (preserved from the pre-migration nucleus.css tweak) |

## Relearn selectors we depend on

Any rename here breaks styling silently. Watch for these in upstream theme
CSS after each upgrade:

```
#R-topbar, #R-sidebar, #R-homelinks, #R-header-wrapper, #R-logo,
#R-body-inner, .R-default, .R-sidebarmenu, .collapsible-menu,
.logo-title, .topbar-breadcrumbs, .topbar-wrapper,
.menu-footer-class
```

Selectors we *create* (safe from Relearn renames):

```
.page-toc-menu, .page-toc-item, .site-brand-logos, .site-logo,
.site-logo-wrap
```

## Re-applying on upgrade

1. **Quick audit.** Does each Relearn selector still exist in the new
   theme? One-liner:

   ```bash
   for sel in '#R-topbar' '#R-sidebar' '#R-homelinks' '#R-header-wrapper' \
              '#R-logo' '#R-body-inner' '.R-default' '.R-sidebarmenu' \
              '.collapsible-menu' '.logo-title' '.topbar-breadcrumbs' \
              '.topbar-wrapper' '.menu-footer-class'; do
     hits=$(grep -rh "$sel" themes/hugo-theme-relearn/assets/css/ 2>/dev/null | wc -l | tr -d ' ')
     printf "  %-28s %s hits\n" "$sel" "$hits"
   done
   ```

   Zero hits on a selector means upstream removed or renamed it; update
   our CSS to match.

2. **Visual QA after rebuild.** Check each area `custom.css` targets:
   - Topbar shadow + bold-blue breadcrumb
   - Sidebar logo size and title beneath
   - Menu-item spacing, chevron alignment
   - H1 TOC sub-items with correct indent and no bullets
   - Menu-footer partner logos centered
   - Active section on `/for_authors/` wraps parent + children in one box

3. **`!important` audit.** The `#R-homelinks { background: transparent !important }`
   rules exist to fight variant-level CSS. If they ever become unnecessary
   (a cleaner Relearn variant approach, or if we stop using
   `#R-homelinks` entirely), remove them.

## Related

- [`layouts/partials/custom-header.md`](layouts/partials/custom-header.md) — loads this file
- [`layouts/partials/menu.md`](layouts/partials/menu.md) — emits `page-toc-menu` / `page-toc-item` that this file styles
- [`layouts/partials/topbar/area/end.md`](layouts/partials/topbar/area/end.md) — emits `site-brand-logos` that this file sizes
- [`layouts/partials/logo.md`](layouts/partials/logo.md) — emits `site-logo` / `site-logo-wrap` that this file sizes
