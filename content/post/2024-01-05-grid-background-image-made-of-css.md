---
title: Grid background image made of CSS
date: 2024-01-05
slug: grid-background-css
categories:
  - tech
tags:
  - tech
thumbnailImage: /img/uploads/screenshot-2024-01-05-142348.png
---
How to make some grid appearance variations for dynamic grid feature via CSS.

<!--more-->

These appearances can be drawn via HTML Canvas API though, it could be costly since there're squared number of items to be rendered as grid appearance. Fortunately, we can easily substitute CSS for it.

Suppose location and size of the grid are variables passed by outside.

```tsx
interface Props {
  x: number;
  y: number;
  size: number;
}
```

### Dots

![Dots](/img/uploads/screenshot-2024-01-05-143221.png)

```tsx
export const GridBackground: React.FC<Props> = ({ x, y, size }) => {
  return (
    <div
      style={{
        backgroundRepeat: "repeat",
        backgroundPosition: `${size / 2 - x}px ${size / 2 - y}px`,
        backgroundSize: `${size}px ${size}px`,
        backgroundImage: "radial-gradient(circle, #000 2px, rgba(255, 255, 255, 0) 2px)",
      }}
    />
  );
};
```

### Borders

![Borders](/img/uploads/screenshot-2024-01-05-143138.png)

```tsx
export const GridBackground: React.FC<Props> = ({ x, y, size }) => {
  return (
    <div
      style={{
        backgroundRepeat: "repeat",
        backgroundPosition: `${-x}px ${-y}px`,
        backgroundSize: `${size}px ${size}px`,
        backgroundImage: "linear-gradient(to right, #000 1px, transparent 1px), linear-gradient(to bottom, #000 1px, transparent 1px)",
      }}
    />
  );
};
```

### Dashed borders

![Dashed borders](/img/uploads/screenshot-2024-01-05-142348.png)

This one is quite tricky if we insist on using only CSS. Handy workaround is getting help from SVG realm.

```tsx
export const GridBackground: React.FC<Props> = ({ x, y, size }) => {
  return (
    <div
      style={{
        backgroundRepeat: "repeat",
        backgroundPosition: `${-x}px ${-y}px`,
        backgroundSize: `${size}px ${size}px`,
        backgroundImage: `url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 10 10'%3E%3Cpath d='M 0 0 L 10 0 M 0 0 L 0 10' stroke='%23000' stroke-dasharray='1' stroke-dashoffset='0.5' stroke-width='0.2' /%3E%3C/svg
      %3E")`,
      }}
    />
  );
};
```

Ref: https://stackoverflow.com/questions/76141466/how-to-make-dashed-line-grid-paper-like-background-in-css