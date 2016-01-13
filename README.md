# Colored SVG Sprite

A technique for coloring external `.svg` files using external `.css` files.

## Why?

Traditionally, the only way to style an SVG was either inline `<svg>` tag, or
inline css (inside a `.svg` file). There is now a new way

| **CSS Interactions (e.g. `:hover`)** | **CSS Animations** | **SVG Animations (SMIL)**
---- | ---- | ---- | ----
`<img>` | No | Yes only if inside `<svg>` | Yes
CSS background image | No | Yes only if inside `<svg>` | Yes
`<object>` | Yes only if inside `<svg>` | Yes only if inside `<svg>` | Yes
`<iframe>` | Yes only if inside `<svg>` | Yes only if inside `<svg>` | Yes
`<embed>` | Yes only if inside `<svg>` | Yes only if inside `<svg>` | Yes
`<svg>` (inline) | Yes | Yes | Yes
**`<svg><use xlink:href="sprite.svg"></use></svg>`** | **Yes** | **Yes** | ?

The last way is supported by all browsers (including IE + Edge with a [tiny
shim](https://github.com/jonathantneal/svg4everybody)). Read on for more!

## What?

This repo showcases 2 separate but complimentary points:

1. Using external SVGs
2. Styling those SVGs with CSS

### Using external SVGs

_[See svg4everybody](https://github.com/jonathantneal/svg4everybody) for more
details._

Given an external file [`sprite.svg`](sprite.svg):

```html
<svg xmlns="http://www.w3.org/2000/svg">
  <symbol id="camera" viewBox="0 0 512 512">
    <!-- ... -->
  </symbol>
  <symbol id="pencil" viewBox="0 0 512 512">
    <!-- ... -->
  </symbol>
</svg>
```

We can load that into our page like so:

```html
<svg>
  <use xlink:href="sprite.svg#pencil"></use>
</svg>
```

A couple of points to note here:

* [Use of `<symbol>` instead of
  `<g>`](https://css-tricks.com/svg-symbol-good-choice-icons/) inside the svg
  allows setting the `viewBox` property, as well as `title` for accessibility.
* The `xlink:href` attribute points to the `id` of the `<symbol>` you'd like to
  use (in this example, we point at the `pencil`).

With the [svg4everybody](https://github.com/jonathantneal/svg4everybody) shim,
this works in all browsers.

### Styling those SVGs with CSS

Now, we can set a class on the containing `<svg>` element in the html, and target that with an external stylesheet:

```html
<svg class="color-svg">
  <use xlink:href="sprite.svg#pencil"></use>
</svg>
```

```css
.color-svg {
  width: 150px;
  height: 150px;
}
```

#### Color

Our `#pencil` svg looks like this:

```html
<symbol id="pencil" viewBox="0 0 512 512">
  <rect x="0" y="0" width="512" height="512" rx="30" ry="30"/>
  <path fill="currentColor" d="..."/>
</symbol>
```

To change the color of the `<rect>`, we make use of the `fill` property:

```css
.color-svg {
  width: 150px;
  height: 150px;
  fill: black;
}
```

Notice too that the `<path>` has set `fill="currentColor"`. This is a [neat css
trick](http://codepen.io/FWeinb/post/quick-tip-svg-use-style-two-colors) which
tells the `<path>` to inherit the color from its nearest parent that has
defined a `color` style:

```css
.color-svg {
  width: 150px;
  height: 150px;
  fill: black;
  color: #fff;
}
```

So now, our `<path>` is colored `#fff`, and the `<rect>` is colored `black` :D

[Try the demo](http://jesstelford.github.io/color-svg-sprite), and hover + click to see different color combos.
