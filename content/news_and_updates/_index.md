+++
title = "News and Updates"
weight = 1
+++

{{< pageHero
    title="News and Updates"
    subtitle="Latest announcements from the organising committee."
>}}

The most recent updates appear at the top. Use the `{{</* newsItem */>}}`
shortcode for each new post — pick a `type` to colour-code it
(`update`, `deadline`, `announcement`, `call`, `warning`).

{{< newsItem date="2026-04-12" type="deadline" title="Paper submission deadline extended" >}}
Following many requests, the paper-submission deadline has been extended by
two weeks to **30 April 2026**. No further extensions are planned. See the
[For Authors](../for_authors/) page for the full timeline.
{{< /newsItem >}}

{{< newsItem date="2026-03-28" type="announcement" title="Fourth keynote speaker confirmed" >}}
We are delighted to announce the fourth keynote speaker, **Prof. Emily Davis**
(InnovateCorp, Germany), presenting *The Digital Twin: The Why and the How*.
Full keynote line-up on the [Program](../program/) page.
{{< /newsItem >}}

{{< newsItem date="2026-03-15" type="call" title="Industry Forum call for talks is open" >}}
The [Industry Forum](../industry-forum/) is accepting talk proposals from
practitioners until **15 April 2026**. Talks emphasise real deployments and
open challenges. Send a one-page abstract to the organising committee.
{{< /newsItem >}}

{{< newsItem date="2026-02-20" type="update" >}}
Tutorial programme has been finalised — three pre-conference tutorials will
be offered on Day 0. Details on the [Tutorials](../tutorials/) page.
{{< /newsItem >}}

{{< newsItem date="2026-01-10" type="announcement" title="Registration now open" >}}
Registration is open at reduced early-bird rates until **15 May 2026**.
Full fee table on the [Registration](../registration/) page.
{{< /newsItem >}}

{{< newsItem date="2025-11-14" type="call" >}}
Several special sessions are accepting submissions — see the
[Call for Special Sessions](../for_authors/#call-for-special-sessions).
{{< /newsItem >}}

{{< newsItem date="2025-10-22" type="update" >}}
The paper-submission system is now [open](../for_authors/#manuscript-submission) for all tracks.
{{< /newsItem >}}

---

### Adding a new post

Copy the template below to the top of this file (above the most recent item)
and fill in your values. Then commit — the site rebuilds automatically.

```
{{</* newsItem date="YYYY-MM-DD" type="update" title="Optional headline" */>}}
Your announcement in **Markdown**. Links, lists, emphasis all work.
{{</* /newsItem */>}}
```

- `date` — any human date (`2026-04-12`, `12 April 2026`, `Apr 12, 2026`)
- `type` — `update`, `deadline`, `announcement`, `call`, or `warning`
- `title` — optional short headline; omit for one-line posts

Relearn's built-in `{{</* notice */>}}` is still available when you want a
highlighted callout box inside a page (not a dated post), e.g.:

{{% notice style="info" %}}
Contact [info@your-conference.example](mailto:info@your-conference.example) for any questions.
{{% /notice %}}
