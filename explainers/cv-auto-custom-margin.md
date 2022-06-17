## CSS `content-visibility` custom margin

This proposal is to add an enhancement to the `content-visibility: auto` feature
which would allow the developer to specify a custom margin to use for detecting
visibility of affected elements.

Currently, being on screen is defined as being within a [user-agent defined
margin](https://www.w3.org/TR/css-contain-2/#relevant-to-the-user) around the
viewport. However, it's hard for the user-agent to pick a good default that
would be ideal for all pages. In fact, the developer is in a better position to
consider what is a good default that works for them.

We propose adding a way for the developer to specify this margin.

### Option 1: extend `content-visibility` syntax.

Proposed syntax amends the "auto" keyword:

`content-visibility: visible | hidden | (auto || (<length-percentage> | auto))`

Pros:
* This doesn't add a new property.
* Length value is physically next to the auto keyword, reducing confusion

Cons:
* `content-visibility: auto auto`, which would be the default value, looks a
  little awkward

### Option 2: add a new property, `content-visibility-margin`.

Proposed new property:

`content-visibility-margin: <length-percentage> | auto`

Pros:
* New property with a single value
* Doesn't modify existing syntax, so compat story is easier

Cons:
* Has no effect except when `content-visibility: auto` is specified.
* Need to figure out whether the property should be inherited
