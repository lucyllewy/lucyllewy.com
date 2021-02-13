---
title: "Advanced composition of Polymer Webcomponents"
date: 2016-11-30 23:02:31
published: true
categories: [development]
tags: [Polymer, Web-Components]
wpid: 1419
---

In a request for help sent to the Polymer web components mailing list, [a user wondered about lists](https://groups.google.com/d/msg/polymer-dev/LqhR65un-h0/joFOrptJBQAJ) specifically based on the example provided in the [Shop Demo](https://shop.polymer-project.org/) that the Polymer team created. This user wanted to understand how to change the list so that it may use a different markup to the one provided for in the original example.

As I thought about and researched this problem I encountered a [stack overflow post](https://stackoverflow.com/questions/34645114/polymer-dom-repeat-binding-to-content/34751655#34751655) that talks about a parent providing the markup for a dom-repeat template inside a child's shadow root. This lead me to experimentation with multiple levels of nesting where only the top-level element or document provided the markup templates for a list and the list items within that list.

Markup included in the document
-------------------------------

My top-level document that I settled upon dictates how the children behave. Notably, each element's innards are wrapped inside a &lt;template&gt; to prevent display when the child elements stamp the light dom into their shadow root.

```html
<my-list items="[[items]]">
  <template item-outer>
    <my-list-item item="[[item]]">
      <template item-inner>
        <img src="[[item.icon]]" />
      </template>
    </my-list-item>
  </template>
</my-list>
```

The reference to a variable named items maps to the following array of objects (purely an example):

```json
[
  { title: "cat", icon: "https://placekitten.com/128/128/" },
  { title: "random", icon: "https://unsplash.it/128/128/" }
]
```

The magic isn't so much in the above invocation, but the individual elements ***my-list***, and ***my-list-item***. They both follow similar layout but need slight differences due to the ***my-list-item*** not being able to hijack a dom-repeat template like the ***my-list*** is able to.

The *my-list* Element
---------------------

```html
<dom-module id="my-list">
  <template>
    <content></content>
    <template is="dom-repeat" id="repeater" items="[[items]]"></template>
  </template>
  <script>
    Polymer({
      is: "my-list",
      properties: {
        items: {
          type: Array,
          value: function() { return []; },
          notify: true
        }
      },
      ready: function() {
        this.$.repeater.templatize(this.querySelector('[item-outer]'));
        Polymer.Bind.prepareModel(this.$.repeater);
        Polymer.Base.prepareModelNotifyPath(this.$.repeater);
      }
    });
    </script>
</dom-module>
```

The magic is achieved via the combination of the insertion point (`<content></content>`) providing the templated contents from the light dom, and the `templatize()` function. The function takes a querySelector match of the `<template>` tag from the light dom by matching on an attribute I assigned. Because I then assign that template to the dom-repeater template stub inside the element definition when the dom-repeat cycles through the items array and stamps itself to this element's shadow root it will use the code from the light dom as the actual rendered output for each item.

This element has a property that holds the items array, which is then assigned to the dom-repeat template for iteration.

The `my-list-item` Element
--------------------------

With very similar behaviour to the `my-list` element, the my-list-item element is largely identical with only minor differences to account for using a `<template>` that doesn't, unlike the dom-repeat above, stamp itself to the DOM automatically.

```html
<dom-module id="my-list-item">
  <template>
    <p>[[item.title]]</p>
    <content></content>
    <template is="dom-template" id="tmpl"></template>
  </template>
  <script>
    Polymer({
      is: "my-list-item",
      properties: {
        item: {
          type: Object,
          value: function() { return {}; },
          notify: true
        }
      },
      ready: function() {
        this.$.tmpl.templatize(this.querySelector('[item-inner]'));
        Polymer.Bind.prepareModel(this.$.tmpl);
        Polymer.Base.prepareModelNotifyPath(this.$.tmpl);
        var stamped = this.$.tmpl.stamp({item: this.item});
        this.appendChild(stamped.root);
      }
    });
  </script>
</dom-module>
```

This element has a property which holds a single item object as provided by the dom-repeat in our parent element `<my-list>`. We follow the same pattern as in the list element excepting that, due to using a `<template>` which does not automatically stamp itself into the DOM, we include two extra lines of javascript, which I've highlighted in bold above.

The first extra line stamps the template into a variable with the item property passed through to the template under the same name. Finally, we add the stamped template into the shadow root with `appendChild()`.

[I have immortalised the full code in a working example at Codepen](https://codepen.io/diddledan/pen/bBrxZR)

What now?
---------

With this example, it should be possible to see how advanced composition can be achieved allowing for complex scenarios where supporting elements can provide for themselves to be extended without their knowledge. The example of a list taking a list-item template is the most obvious one given our nascent experience with web components, but it is clear that developing this concept can open many new patterns and behaviours.

If you're a web developer and still haven't dipped your toe into the pool of web components and Polymer then I urge you to read more and try out some simple examples. Take your own website and find one thing, no matter how small, which is repeated over multiple pages but currently takes more code to reproduce than you'd like. With that thing, try turning it into a fully self-contained web component and replace ever instance of it throughout your site with an instance of the component instead. Finally start thinking of other more complex things that are repeated and try componentizing those, too. Before you know it I hope that you'll be knee-deep into the world of components and are better-off for it.

Furthering your skillset
------------------------

Some resources that are helpful for beginner and advanced developer alike include:

- [Real-time Polymer-specific support in the Polymer slack](https://polymer-slack.herokuapp.com/)
- [Transactional Polymer-specific support through the Polymer mailing list](https://groups.google.com/forum/#!forum/polymer-dev)
- [Everything web components can be found at Webcomponents.org](https://webcomponents.org/)
- [Many more reusable elements at customelements.io](https://webcomponents.org)
- [General web components real-time support in the dedicated gitter.im chat](https://gitter.im/webcomponents/community)