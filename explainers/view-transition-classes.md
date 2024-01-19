### View Transitions: view-transition-class

#### Background

The [View Transitions](https://drafts.csswg.org/css-view-transitions-1/) feature
allows developers to create transitions between DOM states. The setup for the
transition involves creating unique `view-transition-name` values for
participating elements and some optional styles that can customize the animation
behavior

The algorithm for running a view transitions creates, among other things, a tree
of pseudo elements with `:root` as the originating element. The general
structure is as follows:

```
:root
 |
 +- ::view-transition
    |
    +- ::view-transition-group(foo)
    |  |
    |  +- ::view-transition-image-pair(foo)
    |     |
    |     +- ::view-transition-old(foo)
    |     |
    |     +- ::view-transition-new(foo)
    |
    +- ::view-transition-group(bar)
    |  |
    |  +- ::view-transition-image-pair(bar)
    |     |
    |     +- ::view-transition-old(bar)
    |     |
    |     +- ::view-transition-new(bar)
    |
    ...
```
 
For each `view-transition-name` specified, the algorithm creates a subtree
starting with `::view-transition-group`. The custom-ident specified as the value
of `view-transition-name` property becomes the parameter in the brackets of the
view transion pseudo elements. This is important, since now they can be selected
by name (with some syntactic sugar added that makes all of these elements
selectable from root directly):

```css
::view-transition-group(foo) {
  background: green;
}
::view-transition-old(bar) {
  animation: none;
}
```

The downside of this approach, and the motivation for this proposal, is that the
styles specified can get repetitive if there are a lot of items that have to be
styled in similar ways. For example, consider the following:

```css
::view-transition-group(list-item-1),
::view-transition-group(list-item-2),
::view-transition-group(list-item-3),
::view-transition-group(list-item-4),
::view-transition-group(list-item-5),
::view-transition-group(list-item-6),
::view-transition-group(list-item-7) {
  background: green;
}
```

We have to repeat each list item because there is no other way to select just
the list items. Note that there is a `*` which will match any parameter, but
that may select more than desired.


### Proposal

Similar to other style, repeating ids can get tedious which is why CSS has
classes. This proposal aims to bridge the gap between CSS outside of view
transition and CSS in the view transitions context.

We'd like to introduce a new property called `view-transition-class`, which
would take a list of custom-idents that will identify the elements by a class
name. Note that the new property will not impact the construction of the pseudo
tree: this is still built solely based on the `view-transition-name` property.

With the following CSS, the class can be applied to all of the list items, which
will cause a view transition class to be `vt-list-item` for them:
```css
.list-item {
  view-transition-class: vt-list-item;
}
```

Now, the syntax to select just the list items becomes the following:
```
::view-transition-group(.vt-list-item) {
  background: green;
}
```

Note that I'm omitting both the `view-transition-name` style and the actual HTML
that constructs the list items, since these aspects don't change meaningfully in
this proposal.

### Alternatives Considered

We haven't considered alternatives to solve the problem as a whole. However,
there was some discussion of different syntax.

Specificaylly, Instead of using `view-transition-class` we considered applying
the CSS class directly to the view transition pseudo elements:

```html
<style>
::view-transition-group(.vt-list-item) {
  background: green;
}
</style>
<div class="vt-list-item" style="view-transition-name: list-item"></div>
```

We deciced aagainst this approach, because it conflates the notion of class too
much. That is, the class here applies to the element and hoisting that knowledge
to a pseudo element that is constructed from this element seemed too magical.

You can find more discussion in the [github issue that proposed this
feature](https://github.com/w3c/csswg-drafts/issues/8319)
