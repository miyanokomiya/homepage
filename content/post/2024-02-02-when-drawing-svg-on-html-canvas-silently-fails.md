---
title: When drawing SVG on HTML canvas silently fails
date: 2024-02-02
slug: draw-svg-on-canvas-fail
categories:
  - tech
tags:
  - SVG
  - HTMLCanvas
thumbnailImage: /img/no-image.jpg
---
One simple answer is that the SVG should have `xmlns:xlink="http://www.w3.org/1999/xlink"` attribute.

<!--more-->

Another answer, rather a workaround, is to append `Image` element that loads target SVG to `document`. The element should be visible but can be transparent: `display: "none;"` is NG, but `opacity: 0;` is OK. Then, `drawImage` with the element should work in the page.
