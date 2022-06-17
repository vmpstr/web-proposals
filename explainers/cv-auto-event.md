## CSS content-visibility state change event

This proposal is to add an event that would fire on a `content-visibility: auto`
element when the rendering state of the element changes due to any of the
attributes that make the element [relevant to the
user](https://www.w3.org/TR/css-contain-2/#relevant-to-the-user).

The use-case for this is to let developers have greater control over when to
stop or start rendering in response to the user-agent stopping or starting
rendering of the content-visibility subtree. For example, the developer may want
to stop React updates in a subtree that is not rendered by the user-agent.
Similarly, the developer may want to stop any other script updates (e.g. canvas
updates) when the user-agent is not rendering the element. 

### Bikeshed 1: ContentVisibilityAutoStateChanged

Pros:
  * Unambiguous, and describes exactly what the event represents

Cons:
  * Verbose

### Bikeshed 2: RenderingStateChanged

Pros:
  * Succinct
 
Cons:
  * Unclear if this applies only to content-visibility: auto or any other
    methods that may cause rendering to stop, such as content-visibility: hidden
    or display: none

As a side-note, it's also unclear if maybe we _should_ fire this event in all
cases where rendering is affected, which would affect the scope of the proposal.
We would need to figure out a comprehensive list of properties / states that can
cause this event to be fired.

### Bikeshed 3: ???
