---
title: "HTML Styling"
author: "JefeBromden"
date: 2024-08-07T13:34:17-03:00
draft: true
---
*HTML* allow us to present information on a browser. But in order to manage and maintain that information, there has to be a separation of concerns between *content* (actual data) and *presentation* (the way we show that data).

Format and looks you see on any website (presentation) is called *styling*, and it's done by using a *style sheet language*. The de facto language is *CSS* (Cascading Style Sheets).

With CSS you can declare a style for a *tag* or group of tags, and apply it on a page in three ways:
1. *Style Attributes*: Using tag attributes on the very same tag you want to apply it to.
2. *Inline Styles*: Putting CSS inside a *style tag*.
3. *External Stylesheets*: Using a external document containing CSS, and "calling" it from a style tag. 

The options numbering is on purpose. That means style attributes has the highest precedence, it will override what was used on inline styles, and the latter will override stylesheets.

## Style Attributes
This type is useful to test a CSS property on a particular element, use on a *production website* should be avoided.

Add a style attribute to the tag, with key/values separated by a colon (:), and ending with semicolon (;). You can add as many key/value pairs as you want.
```html
<h1 style="color: green;">Leonard Shelby</h1>
```

## Inline Styles
This type is useful to test your style before using it on external stylesheet. Also, some *frameworks* use it to *inject* styles using this method.

To use it, inside a *html tag*, and before *body tag*, add a style tag inside a *head tag*. Valid CSS must be placed inside the style tag.
```html
<head>
  <style>
    h1 {
      color: green;
    }
  </style>
</head>
```

## External Stylesheets
You must write valid CSS on a separate file:
```css
/* This is a comment */
/* Save this content on style.css */
h1 {
  color: green;
}
```

And then "call it" from your HTML document:
```html { hl_lines=2}
<head>
  <style href="style.css" rel="stylesheet" type="text/css"></style>
</head>
```

You can omit `type="text/css"`, it's there just for completeness.

### References
- [CSS - Wikipedia](https://en.wikipedia.org/wiki/CSS)
- [Style: The Style Information element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/style)
