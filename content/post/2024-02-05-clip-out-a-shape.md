---
title: Clip out a shape
date: 2024-02-05
slug: clip-out-shape
categories:
  - tech
tags:
  - SVG
  - canvas
thumbnailImage: /img/uploads/shapes-33-.svg
---
This article is all about how to make this

<!--more-->

![](/img/uploads/shapes-34-.svg)

this. It means we need to clip a shape out along a path.

![](/img/uploads/shapes-33-.svg)

Although we have two options for that: `mask` or `clip`, `mask` is so heavy to apply its effect. When we make 60fps highly performant application, `clip` is much better than `mask`. What we want to do is to clip a shape, then what we need to use to `clip` naturally.

It's not so difficult once you get the basic idea. First of all, we need a path that covers the whole target. Second and last of all, we need a path for clipping out.

Here is the example image of those two paths. Blue one is the former, red one is the latter.

![](/img/uploads/screenshot-2024-02-05-193004.png)

We're going with `evenodd` clip-mode today, so their directions don't matter. If you're going with `nonzero`, they do matter so that be careful of them.

## HTML Canvas

We already have enough setup for HTMLCanvas. Let's just clip it out.

```javascript
ctx.save();
const region = new Path2D();

/* Apply the wrapper path to region */
/* Apply the inner path to region */

ctx.clip(region, "evenodd");

/* Draw any shapes */

ctx.restore();
```

## SVG

It's almost same as HTMLCanvas though, we need a little bit attention to `<path>` element. We have to make one `<path>` element to represent both the wrapper path and the inner path.

```svg
<clipPath id="clip-id">
  <path clip-rule="evenodd" d="the wrapper + the inner" />
</clipPath>
<g clip-path="url(#clip-id)">
  <!-- target shapes -->
</g>
```

Then, boom! That's it.

![](/img/uploads/shapes-33-.svg)
