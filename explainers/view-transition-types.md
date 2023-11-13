### View Transitions: list of types

#### Background

The [View Transitions](https://drafts.csswg.org/css-view-transitions-1/) feature
allows developers to create transitions between DOM states. A significant
portion of the transition is a declarative set up, followed by a script trigger.

For example,
```css
#hero {
  view-transition-name: hero;
}

html::view-transition-group(hero) {
  animation: my-animation;
}
```

```js
element.addEventListener("click", e => {
  document.startViewTransition(() => {
    changeTheDom();
  });
});
```

This works well for a single type of transition. However, developers have
[expressed interest](https://github.com/w3c/csswg-drafts/issues/8960) to be able
to declare multiple transitions in their CSS, and then trigger only one of them
from script.

The list of types proposal attempts to address this use case.

#### Proposal

The proposal is to add an ability to specify a list of types to
startViewTransition, which will match a `:root` pseudo-class if the types match.
To explain this with an example, consider the following:

```css
html:active-view-transition(courage) #hero {
  view-transition-name: hero;
}
html:active-view-transition(greed) #villain {
  view-transition-name: villain;
}

html:active-view-transition(courage)::view-transition-group(hero) {
  animation: my-animation;
}
html:active-view-transition(greed)::view-transition-group(villain) {
  animation: my-dampening;
}
```

```js
element.addEventListener("click", e => {
  document.startViewTransition({
    type: ["courage"],
    update: () => {
      changeTheDom();
    }
  });
});
```

The example above declaratively sets up two different types of animations
"courage" and "greed". They are distinguished in CSS with a `:root` pseudo-class
`:active-view-transition`, which takes a comma separated list of parameters that
match if there is currently a view transition happening that has at least one of
the "types" specified matching at least one of them parameters of the pseudo
class.

In JavaScript, we trigger a "courage" transition in the example, by specifying
that as the list of types. 

The details of this proposal are outlined in the spec: [active view
transition](https://drafts.csswg.org/css-view-transitions-2/#the-active-view-transition-pseudo)
and [additions to the
document](https://drafts.csswg.org/css-view-transitions-2/#additions-to-document-api).
Please note that the spec has several features in it that are being worked on in
parallel. This particular explainer only address the list of types feature.

#### Alternatives Considered

We have considered not adding any feature to this, since the above use-case could also be addressed by regular classes:

```css
html.courage #hero {
  view-transition-name: hero;
}
html.greed #villain {
  view-transition-name: villain;
}

html.courage::view-transition-group(hero) {
  animation: my-animation;
}
html.greed::view-transition-group(villain) {
  animation: my-dampening;
}
```

```js
element.addEventListener("click", e => {
  document.documentElement.classList.add("courage");
  let transition = document.startViewTransition(changeTheDom);
  transition.finished.then(() => {
    document.documentElement.classList.remove("courage");
  });
});
```

This seems less ergonomic, since the pattern of how to add and remove classes isn't as easy to remember.

The proposed API also has two other advantanges:
* One can select based on `html:active-view-transition(*)` without changing the script callsite. This
  matches any time there is an active view transition, regardless of the type.
* Types can also be used in cross-document view transitions. This is still being actively discussed, but the
  polyfill for this would be more difficult, since script is not involved in making cross-document view transitions.
