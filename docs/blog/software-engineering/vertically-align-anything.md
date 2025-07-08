# Vertically align anything

> Originally published: 30 September 2018

## Reflection: Don't use this anymore

If you're doing things when you set `display` to `block` or `inline-block` then this might still be useful for you.

However, I actually recommend switching to
using [Flexbox](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox)
or [Grids](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Grids).  
Personally, I find Flexbox more powerful as Flexbox is one-dimensional and Grids is two-dimensional, so you can have
Flexbox be two-dimensional by putting Flex inside Flex, which can work really nicely when dynamically handling different
sized screens.

## Original Post

Taken from [here](http://zerosixthree.se/vertical-align-anything-with-just-3-lines-of-css/)

It's a bit of CSS that will allow you to instantly vertically align a `<div>` within another `<div>`:

```css
.element {
    position: relative;
    top: 50%;
    transform: translateY(-50%);
}
```

Note: In Firefox, you may need to set the parent element's `height`.  
This was discovered when using this CSS on a child of `body` to position an element in the middle of the screen;

```css
body {
    height: 100%;
}
```
