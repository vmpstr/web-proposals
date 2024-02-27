### Tab Knowledge API

_Note that this is an early stage proposal intended to convey the idea and
fascilitate a discussion, rather than impose a particular API shape._

The Tab Knowledge API is a set of JavaScript APIs exposed to web pages that
provide some information about the user's journey outside of the current page.
Specifically, it provides information about other tabs or web contents opened to
the same origin as the page using the API. This allows the page to construct
some UI that presents other pages opened by the user to this origin. Other than
being an information source, this proposal also discusses a possibility of
controling the focus of these tabs.

#### Terminology

* **Tab** -- This refers to a web entry that the user can interact with.
  Typically, browsers organize these entries into tabs displayed in the browser
  in some fashion. Although this API talks specifically about tabs, it should be
  understood to include other methods of organizing web entries. Concretely, any
  names in the specific API functions should allow for the possibility of other
  organization methods.

* **Focus** -- For the purposes of this document, focus refers to the browser's
  ability to bring a particular tab or web entry to user's attention. This could
  be switching a tab in a window, focusing a different window, or both.

#### Motivation

There are several compelling examples that require tab knowledge and focus
control. Most of them revolve around organizing existing web entries.

![An animated image showing user opening tabs that also appear as circles on the
webpage itself](/resources/tab-knowledge/gather.gif)

The above example is a basic starting point for this API. It allows developers
to get access to tabs open to the same origin, with some inforation, such as the
URL and favicon. This information is then can be used to build arbitrary UI that
displays that information:

* This is useful for building reading lists. For instance, GitHub can have a
  widget that displays other GitHub issues that you have open, organizing them
  into whatever makes sense. This would be true for other bug trackers or
  forum-like sites. 
* Online productivity tools can include a menu item that lists other documents
  opened by this user.
* Commerce sites can list similar products opened in other tabs with an option
  to visit a comparison site to weigh the pros and cons of opened products.

Note that it's evident from some of these examples that having the ability to
focus one of these entries would also be a nice feature. For example, clicking
on an item in the list of documents opened should focus the tab or the window
that has this document opened.

#### Strawman Proposal

```idl
interface WebEntryInfo {
  // The URL of the web entry.
  URL url;

  // The URL of the favicon for the web entry.
  URL favicon;

  // The web entry's document title.
  DOMString title;

  // When invoked, the user-agent will focus this web entry as needed.
  void focus();
};

sequence<WebEntryInfo> getWebEntries();
```

This is a basic outline of what this API may look like. I think it's also
worthwhile to consider some change-like events to notify pages when the list of
web entries changes. 

#### Design Considerations

There are several design considerations worth surfacing:

* Any type of event should be asynchronous and with timing that does not permit
  timing attacks to determine the presence or absence of a web entry. This is
  also to permit performant implementations -- user agents should be free to
  notify pages in a rate limited way.
* The order of the web entries may reveal a piece of information that is not
  otherwise available to sites: the order of tabs. To prevent this, the proposal
  is to limit the order to be chronological.
* There are also some ideas to expose additional controls for web entries, such
  as reordering, grouping, or closing of tabs. However, this proposal is limited
  to minimal useful non-destructive actions. Further enhancements can be
  discussed separately.

