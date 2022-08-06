---
title: "WebKit CSS3 text-decoration properties (preview)"
date: 2012-08-01
---

WebKit currently supports the CSS Text Level 2.1 version of the [`text-decoration`](http://www.w3.org/TR/CSS21/text.html#decoration) property. This version treats only about the decoration line types: `underline`, `overline`, `line-through` and `blink` (the latter is not supported on WebKit).

The draft version of CSS Text Level 3 refactors the [`text-decoration`](http://dev.w3.org/csswg/css3-text/#text-decoration) property as a shorthand to 3 newly added properties, named [`text-decoration-line`](http://dev.w3.org/csswg/css3-text/#text-decoration-line), [`text-decoration-style`](http://dev.w3.org/csswg/css3-text/#text-decoration-style) and [`text-decoration-color`](http://dev.w3.org/csswg/css3-text/#text-decoration-color), and also adds the [`text-decoration-skip`](http://dev.w3.org/csswg/css3-text/#text-decoration-skip) property.

Among other WebKit stuff I've been doing lately, this feature implementation is one of the most cool ones I'm enjoying implementing.

I've grabbed the task of implementing all of these *CSS3* text-decoration\* properties on WebKit, and results are great so far!

As you can see below, these are the new text decoration styles (`solid`, `double`, `dotted`, `dashed` and `wavy` - the latter still requires platform support) available:

![Text decoration style layout test results on Qt platform](/assets/images/text-decoration-style-expected.png){:class="center"}

And also specific text decoration colors can be set:

![Text decoration color layout test results on Qt platform](/assets/images/text-decoration-color-expected.png){:class="center"}

These features (with exception to `text-decoration-skip` property) are already implemented on Firefox, thus it gets easier to compare results with different web engines. It is important to notice since *CSS3* specification is still in development, all these properties have a `-webkit-` prefix (ie. `-webkit-text-decoration`), so `text-decoration` still maintains *CSS2.1* specification requirements. The patches are being reviewed and will soon land upstream, let\'s hope it will be soon!
