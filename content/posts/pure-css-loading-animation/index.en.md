---
title: "Pure CSS Loading Animation"
date: 2017-08-18 15:03:59
published: true
categories: [development]
tags: [Animation, Css, webdev]
wpid: 1797
---

Showcasing my skills in [Web Development](/services/) where I am constantly [pushing the boundaries](/advanced-composition-of-polymer-webcomponents/), here is my CSS Loading Animation. The experiment here uses CSS keyframes to achieve 60 frames per second animation without repainting the elements.

To build the animated object (the spinner) I use a pair of incomplete CSS Triangle tricks. When you combine the CSS Triangle trick with `border-radius` you find you can curve one edge of the triangle. However, I probably reinvented this method, but I haven't seen any prior art. The nice [Home](/) button on this site's menu bar shows how I use a single rounded triangle.

As I saw that the single rounded triangle worked quite well. So, I started experimenting, leading me to the complete circle design you see in the spinner.

The effect is created by hacking the [CSS Triangle](https://css-tricks.com/snippets/css/css-triangle/) trick to display two opposing triangles instead of one directional arrow. This is created with *two* non-transparent borders instead of one, which would create a single arrow:

```css
.selector {
    border-left: 25px solid transparent;
    border-right: 25px solid transparent;
    border-top: 25px solid #eee;
    border-bottom: 25px solid #eee;
}
```

Once we have the opposing triangles we apply a 50% border radius on the element. This forces a circular appearance with two slices and two gaps. Finally, we create a sibling using the `:after` CSS selector providing the remaining two slices in the gaps from the first element:

```css
.selector:after {
    border-left: 25px solid #ccc;
    border-right: 25px solid #ccc;
    border-top: 25px solid transparent;
    border-bottom: 25px solid transparent;
}
```

The first example shows an effect similar to Apple's "Beach Ball". Both elements animate together. Because the same look can be achieved without two separate elements, the second demo shows a use for keeping them separate:

<figure class="wp-block-embed is-type-rich is-provider-codepen"><div class="wp-block-embed__wrapper"><iframe allowtransparency="true" class="cp_embed_iframe" frameborder="0" height="300" id="cp_embed_YxexMe" scrolling="no" src="https://codepen.io/diddledani/embed/preview/YxexMe?height=300&slug-hash=YxexMe&default-tabs=css,result&host=https://codepen.io" style="width: 100%; overflow: hidden;" title="Pure CSS Loading Spinner Animation"></iframe></div></figure>So, we use two separate elements to create this slightly different effect. Here, we animate each pair of opposite triangles separately. The effect is somewhat pleasing:

<figure class="wp-block-embed is-type-rich is-provider-codepen"><div class="wp-block-embed__wrapper"><iframe allowtransparency="true" class="cp_embed_iframe" frameborder="0" height="300" id="cp_embed_jLZarL" scrolling="no" src="https://codepen.io/diddledani/embed/preview/jLZarL?height=300&slug-hash=jLZarL&default-tabs=css,result&host=https://codepen.io" style="width: 100%; overflow: hidden;" title="Pure CSS Loading Spinner Animation (Alternative)"></iframe></div></figure>