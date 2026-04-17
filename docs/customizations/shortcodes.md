# Custom shortcodes

- **Files**: [`layouts/shortcodes/*.html`](../../layouts/shortcodes/)
- **Shadows upstream**: none — all six are brand-new shortcodes added for
  IES conference needs
- **Risk on Relearn upgrade**: 🟢 LOW (self-contained Hugo templates)

Relearn 9.0.3 ships its own set of shortcodes (`notice`, `tabs`, `expand`,
`button`, etc.) which remain available. We add six more for
conference-specific content patterns:

| Shortcode | File | Purpose |
|---|---|---|
| `slideshow` | [`slideshow.html`](../../layouts/shortcodes/slideshow.html) | Auto-advancing image carousel with overlay text, dots, arrows |
| `card` | [`card.html`](../../layouts/shortcodes/card.html) | Bootstrap-style card with coloured header and body |
| `imagesRow` | [`imagesRow.html`](../../layouts/shortcodes/imagesRow.html) | Responsive row of portraits / logos with captions |
| `imageWithOverlay` | [`imageWithOverlay.html`](../../layouts/shortcodes/imageWithOverlay.html) | Single image with overlaid text |
| `pdf-embed` | [`pdf-embed.html`](../../layouts/shortcodes/pdf-embed.html) | Inline PDF viewer (external or internal URL) |
| `spacer` | [`spacer.html`](../../layouts/shortcodes/spacer.html) | Vertical whitespace in units of line-height |

## `slideshow`

```hugo
{{< slideshow path="images/slideshow" largeText="Conference Location" smallText="Conference Dates" >}}
```

- Renders every `.jpg`, `.jpeg`, `.png` under the given path.
- Fits a 1120 × 460 frame preserving aspect ratio.
- Auto-advances every 4 s; left/right arrows + dot pagination.
- Responsive (scales on mobile).
- All styling is scoped inline via `<style>` and ids like `#slideshow`.

## `card`

```hugo
{{< card title="Topics of Interest" >}}
- Adaptive and Intelligent Control Systems
- Autonomous Robotic Systems, AI, and Machine Learning
{{< /card >}}
```

Renders a card with a blue header containing `title` and the inner markdown
as the card body.

## `imagesRow`

```hugo
{{< imagesRow images=`[
  {"src": "/images/sessions/a.jpeg", "alt": "…", "name": "Jane Doe", "company": "University X", "location": "City, Country"},
  {"src": "/images/sessions/b.jpg",  "alt": "…", "name": "John Doe", "company": "University Y", "location": "City, Country"}
]` >}}
```

Each entry renders as a thumbnail with name + affiliation + location below.
Accepts the `images` array as a raw-string argument containing JSON.

## `imageWithOverlay`

Simple image + text-overlay component. See the source for the exact syntax.

## `pdf-embed`

```hugo
{{< pdf-embed type="external" url="https://example.org/paper.pdf" >}}
```

- `type` can be `external` (embed by URL) or `internal` (relative path).
- Renders via `<iframe>` or `<embed>`.

## `spacer`

```hugo
{{< spacer lines=2 >}}
```

Adds `n` line-heights of vertical space. Useful between sections on pages
that don't warrant a visual divider.

---

## Re-applying on upgrade

Nothing to re-apply at Relearn upgrade time — shortcodes don't shadow
theme files. However:

- **Check for collisions.** If Relearn ships a shortcode named (say)
  `card`, our `layouts/shortcodes/card.html` wins — users expecting
  upstream behaviour get ours. Unlikely, but worth grepping after a
  major Relearn bump:

  ```bash
  for f in layouts/shortcodes/*.html; do
    name=$(basename "$f" .html)
    [ -f "themes/hugo-theme-relearn/layouts/shortcodes/${name}.html" ] && \
      echo "COLLISION: $name (ours shadows upstream)"
  done
  ```

- **Verify inline CSS/JS still works.** Each shortcode contains its own
  scoped `<style>` and sometimes `<script>`. Those rely on basic CSS /
  vanilla JS and shouldn't break, but visual spot-check after major
  browser/Hugo version changes.

## Related

- [`content.md`](content.md) — content files that call these shortcodes
