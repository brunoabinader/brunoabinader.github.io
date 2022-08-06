---
title: "CSS Level 3 Text Decoration on WebKit and Blink - status"
date: 2013-05-15
---

It's been a while since I wrote the last post about progress on implementing [CSS Level 3 Text Decoration](http://www.w3.org/TR/css-text-decor-3/) features in WebKit. I've been involved with other projects but now I can finally resume this work. So far I have implemented:

- [`text-decoration-line`](http://www.w3.org/TR/css-text-decor-3/#text-decoration-line-property)
- [`text-decoration-style`](http://www.w3.org/TR/css-text-decor-3/#text-decoration-style)
- [`text-decoration-color`](http://www.w3.org/TR/css-text-decor-3/#text-decoration-color)
- [`text-underline-position`](http://www.w3.org/TR/css-text-decor-3/#text-underline-position)

These properties are currently available under `-webkit-` prefix on WebKit, and guarded by a feature flag `CSS3_TEXT` which is enabled by default on both EFL and GTK ports. On Blink, plans are to get these properties un-prefixed and under a runtime flag, which can be activated by enabling the ["Experimental Web Platform Features"](http://src.chromium.org/viewvc/chrome?view=revision&revision=210820) flag (see [chrome://flags](chrome://flags/#enable-experimental-web-platform-features) inside Google Chrome/Chromium).

There are still some [Skia](https://skia.org/)-related issues to fix on Blink to enable proper <span style="text-decoration: underline dashed;">dashed</span> and <span style="text-decoration: overline dotted">dotted</span> text decoration styles to be displayed. In the near future, we shall also have the text-decoration shorthand as specified on CSS Level 3 specification.

> **Note:** Do not confuse text-decoration-line's `blink` value with Google's Blink project :smile: