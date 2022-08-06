---
title: "CSS Level 3 Text Decoration properties now available in Google Chrome Canary!"
date: 2013-08-20
---

As of Monday (August 19th), [Google Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html) now supports CSS3 Text Decoration shorthand property. Once in Canary, you can enable the _Experimental Web Platform Features_ runtime flag by checking the link below:

[chrome://flags/#enable-experimental-web-platform-features](chrome://flags/#enable-experimental-web-platform-features)

After enabling it, restart your browser and you should be able to use
the following pattern:

```css
text-decoration: underline dashed red;
```

Gives the following result: <span style="text-decoration: underline dashed red;">underline dashed red</span>

As specified in [CSS Level 3 Text Decoration shorthand specification](http://www.w3.org/TR/css-text-decor-3/#text-decoration-property). I am also [proposing these properties to go enabled by default in WebKit](https://lists.webkit.org/pipermail/webkit-dev/2013-August/025297.html) - stay tuned for further updates!
