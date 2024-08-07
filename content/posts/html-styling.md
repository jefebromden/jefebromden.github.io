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

The following section and its subsections are just for reference, we're going to focus on stylesheets, because it's the most convenient. Attribute and inline styles are not needed when you can use an online tool for testing.

## Style Sources
### Style Attributes
This type is useful to test a CSS property on a particular element, use on a *production website* should be avoided.

Add a style attribute to the tag, with key/values separated by a colon (:), and ending with semicolon (;). You can add as many key/value pairs as you want.
```html
<h1 style="color: green;">Leonard Shelby</h1>
```

### Inline Styles
This method is useful to test your style before using it on external stylesheet. Also, some *frameworks* use it to *inject* styles using this method.

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

### External Stylesheets
You must write valid CSS on a separate file first:
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

## What is a CSS Selector
> In CSS, selectors declare which part of the markup a style applies to by matching tags and attributes in the markup itself. - Wikipedia

Imagine you have a book, and you want to highlight different words and parts of a text in different ways. What you can do is take a bunch of markers of different colors to highlight passages, and a pencil to mark words and take notes. The *rules* you use is up to you. Which color is for definitions? Which one for quotes? Rounding a word with a circle or a square have different meaning?

In the context of CSS, selectors are like markers and pencils, and the stylesheet is a list of rules. In our example, we use markers and pencils in different ways (a circle in pencil around a word) to reference something (a verb).

Lets make an HTML *snippet* as an example:
```html
<!DOCTYPE html>
<html>
  <body>
  <!-- This is a minimal HTML document for testing -->
  </body>
</html>
```

If we want to change *background color* on this document, we should take *body tag* as selector.

Syntax for the selector is, name of selector, and the properties you want to modify between curly braces:
```css
body {
  /* In CSS, properties must be between curly braces */
}
```

Properties, on the other hand, are key/value pairs, separated by a colon (:), and ending with a semicolon (;):
```css
body {
  background-color: lightblue;
}
```

You can use this CSS code as is for testing.

For a list of color names for all browsers, take a look at [W3Schools page](https://www.w3schools.com/cssref/css_colors.php).

If you want a list of color names grouped by *color palette*, visit [Austin Gil blog](https://austingil.com/css-named-colors/#palettes).

## References
- [CSS - Wikipedia](https://en.wikipedia.org/wiki/CSS)
- [Style: The Style Information element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/style)
