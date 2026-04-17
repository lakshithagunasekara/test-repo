+++
# ─────────────────────────────────────────────────────────────────────────────
# EXAMPLE — Sidebar link pointing directly to an external URL.
#
# This page file exists ONLY to produce a sidebar entry. The `menuUrl` front
# matter below makes the sidebar link point to the external submission portal
# instead of rendering this page's content. The `_build.render = 'never'`
# block skips producing any HTML for this page, so clicking "Submit paper"
# in the sidebar takes users straight to the portal.
#
# This pattern (Relearn 7.1+) lets you add arbitrary external links to the
# sidebar without creating a placeholder HTML page. Useful for:
#   - submission portals (EasyChair, ConfComm, …)
#   - call-for-papers PDFs
#   - external IEEE resources
#
# To remove: delete this folder. To point elsewhere: change `menuUrl`.
# ─────────────────────────────────────────────────────────────────────────────

title     = "Submit paper"
linkTitle = "Submit paper"
weight    = 3.5   # sits between "For Authors" (3) and "For Attendees" (4)

# External URL the sidebar entry links to.
menuUrl = "https://confcomm.ieee-ies.org/app/general/conferences/IRAI26/initial-submission"

[_build]
  render = 'never'  # no HTML output — sidebar link goes straight to the external URL
+++
