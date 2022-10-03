# WPT Interop Proposal: css-contain-2

## Description

CSS Containment Mdule, Level 2, specifies the `contain` and `content-visibility`
properties. These properties are important primitives that allow websites to
scale the amount of content they provide while maintaining good performance. In
particular `content-visibility: auto` is simple to adopt and automatically
manages content _rendering_ in order to avoid excessive off-screen work.

## Rationale

CSS Containment Module Level 2 provide performance primivites to help developers
scale their content. Specifically the `content-visibility` property enables
developers to control how much or how little of their content is rendered. This
is particularly useful for long pages (infinite scrollers) since most of the
content is off-screen and should not be rendered.

Without containment, the user-agent cannot easily guarantee that content does
not appear on-screen without doing the necessary rendering work to determine its
position.

However, containment makes this possible by providing guarantees, such as that
the element is not sized by its content (size containment) and that the content
of the element do not paint outside of their bounds (paint containment).

Additionally, `content-visibility: auto` provides an easy and convenient way to
manage containment which assures that off-screen content is not rendered unless
absolutely necessary.

When compared to `display: none`, `content-visibility: auto` elements have an
added benefit of being _searchable_ by find-in-page, and available to assistive
technologies via the accessibility tree. This further unlocks features such as
`hidden=until-found`.

* Blink: `css-contain-2`, including `content-visibility` is implemented.
    Over (4%)[https://chromestatus.com/metrics/css/timeline/popularity/662) of
    page loads use content-visibility, despite it only being available in Blink
    at the moment.
* WebKit: work ongoing: [content-visibility bug / patch list](https://bugs.webkit.org/buglist.cgi?quicksearch=content-visibility)
* Gecko: work ongoing: [content-visibility bug / patch list](https://bugzilla.mozilla.org/buglist.cgi?quicksearch=content-visibility)
* Mozilla RFP status: [worth prototyping](https://github.com/mozilla/standards-positions/issues/135#issuecomment-650923098)

Community feedback:
React / Meta engineer:
> Just did a quick test and boi oh boi — the upcoming `content-visibility` CSS property has the potential to be one of the biggest performance improvements to SPAs (if properly leveraged) I've seen in a long time.
- [Vincent Riemer](https://twitter.com/vincentriemer/status/1288668233457758208)

Google engineer:
> CSS "content-visibility:auto" is amazing: skip rendering & painting offscreen content until needed. I got a ~1s faster render on a long HTML document on desktop, ~3s on mobile.
- [Addy Osmani](https://twitter.com/addyosmani/status/1368479029683122180)

Meta, Databricks engineer:
> Saved over 1 seconds of restyle time on a very large list with 2 lines of CSS using content-visibility, under 1 hour of work
- [Benoit Girard](https://twitter.com/b56girard/status/1417918695792201732)


## Specification:

https://www.w3.org/TR/css-contain-2/
