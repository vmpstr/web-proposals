### Self-Review Questionnaire: Security and Privacy for ContentVisibilityAutoStateChanged event

> 01.  What information might this feature expose to Web sites or other parties,
>      and for what purposes is that exposure necessary?

This event provides a convenient way to discover when the rendering state of a
`content-visibility: auto` element changes. The motivation is to allow developers
"hook into" the state changes and skip additional work, such as canvas updates
to improve page performance.

> 02.  Do features in your specification expose the minimum amount of information
>      necessary to enable their intended uses?

Yes.

> 03.  How do the features in your specification deal with personal information,
>      personally-identifiable information (PII), or information derived from
>      them?

The feature does not deal with such information.

> 04.  How do the features in your specification deal with sensitive information?

The feature does not deal with such information.

> 05.  Do the features in your specification introduce new state for an origin
>      that persists across browsing sessions?

No.

> 06.  Do the features in your specification expose information about the
>      underlying platform to origins?

No.

> 07.  Does this specification allow an origin to send data to the underlying
>      platform?

No.

> 08.  Do features in this specification enable access to device sensors?

No.

> 09.  Do features in this specification enable new script execution/loading
>      mechanisms?

No.

> 10.  Do features in this specification allow an origin to access other devices?

No.

> 11.  Do features in this specification allow an origin some measure of control over
>      a user agent's native UI?

No.

> 12.  What temporary identifiers do the features in this specification create or
>      expose to the web?

None.

> 13.  How does this specification distinguish between behavior in first-party and
>      third-party contexts?

It makes no such distinction. The `content-visibility: auto` property does not change
rendering state when a third-party context element (e.g. iframe) changes rendering
state. Any information used for this event is already observable by first-party script.

> 14.  How do the features in this specification work in the context of a browser’s
>      Private Browsing or Incognito mode?

This feature does not change behavior in Privable Browsing or Incognito mode contexts.

> 15.  Does this specification have both "Security Considerations" and "Privacy
>      Considerations" sections?

This feature is an addendum to css-contain spec, where `content-visibility` is
specified, and it includes both "Security Considerations" and "Privacy Consideratinos"
sections.

> 16.  Do features in your specification enable origins to downgrade default
>      security protections?

No.

> 17.  How does your feature handle non-"fully active" documents?

This feature does not make a distinction between "fully active" and non-"fully active"
documents.

> 18.  What should this questionnaire have asked?

All good. 👍
