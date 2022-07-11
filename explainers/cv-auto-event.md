## CSS `content-visibility` state change event

This proposal is to add an event that would fire on a `content-visibility: auto`
element when the rendering state of the element changes due to any of the
attributes that make the element [relevant to the
user](https://www.w3.org/TR/css-contain-2/#relevant-to-the-user).

The use-case for this is to let developers have greater control over when to
stop or start rendering in response to the user-agent stopping or starting
rendering of the `content-visibility` subtree. For example, the developer may want
to stop React updates in a subtree that is not rendered by the user-agent.
Similarly, the developer may want to stop any other script updates (e.g. canvas
updates) when the user-agent is not rendering the element. 

### Proposal: ContentVisibilityAutoStateChanged

`ContentVisibilityAutoStateChanged` event inherits from `Event`. It provides
an additional parameter:

  * `ContentVisibilityAutoStateChanged.skipped`: read-only, bool value that
    indicates whether the state of the `content-visibility` element became
    skipped.

This event's target is the element with `content-visibility: auto` style that
changed the state. The event bubbles.

#### Considerations

This event makes it easier for developer to skip work within a subtree affected
by `content-visibility: auto`. Since `content-visibility: auto` subtree elements
are exposed to accessibility, developers must take care to ensure that the work
that they choose to skip does not negatively impact accessibility. We propose
adding a note to the spec to elaborate on this.

Note that even without this event, it is still possible to polyfill the behavior
with a combination of `IntersectionObserver` and `MutationObserver`. From this
perspective, the event does not add a capability to observe new behaviors, but
rather an easier way to discover events that are should already be discoverable.
