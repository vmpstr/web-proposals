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
