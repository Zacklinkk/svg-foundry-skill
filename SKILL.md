---
description: "Create SVG graphics, icons, illustrations, animations, and data visualizations. Also optimize, animate, or restyle existing SVG code. Triggers on: \"draw SVG\", \"create SVG\", \"SVG icon\", \"SVG animation\", \"SVG chart\", \"SVG illustration\", \"optimize SVG\", \"SVG gradient\", \"SVG filter\", \"SVG pattern\", \"make SVG\", \"generate SVG\", \"SVG logo\", \"SVG spinner\", \"SVG progress\", \"画SVG\", \"做个SVG\", \"SVG图标\", \"SVG动画\", \"SVG图表\", \"优化SVG\"."
---

# SVG Craft — Professional SVG Creation & Optimization

---

## Step 1 — Clarify Requirements

Before writing any SVG code, use `AskUserQuestion` to determine:

**1. Task type** (if not obvious from the request):
- **Create**: Build SVG from scratch based on a description
- **Optimize**: Improve existing SVG code (performance, accessibility, responsiveness)
- **Animate**: Add motion/transitions to static SVG
- **Restyle**: Change colors, gradients, patterns, filters

**2. Output format** (always ask unless user specifies):
- **Pure SVG**: Standalone `.svg` markup, embeddable anywhere
- **HTML + SVG**: Complete HTML page with inline SVG + CSS animations
- **React component**: JSX component with props for customization
- **Vue component**: SFC with template + scoped styles

---

## Step 2 — Design the SVG

Follow this structured approach based on the SVG knowledge base:

### 2.1 Canvas & Coordinate System
- Always set `viewBox` for responsive scaling (ref: essentials ch05)
- Use `preserveAspectRatio` when aspect ratio control matters
- Default to `viewBox="0 0 width height"` with no fixed `width`/`height` for responsive embedding
- Consider the coordinate system: SVG y-axis goes downward

### 2.2 Shape Selection
Choose the right primitives (ref: essentials ch02, ch03):
- `<rect>` — rectangles, rounded rectangles (`rx`/`ry`)
- `<circle>` — circles (center + radius)
- `<ellipse>` — ellipses (center + rx/ry)
- `<line>` — single straight line
- `<polyline>` — connected line segments (open)
- `<polygon>` — closed shapes from points
- `<path>` — complex shapes with M/L/C/S/Q/T/A/Z commands

### 2.3 Path Commands Mastery (ref: essentials ch04, web ch16)
- **M/m** — moveto (start point)
- **L/l** — lineto (straight line)
- **H/h, V/v** — horizontal/vertical line
- **C/c** — cubic Bezier (two control points)
- **S/s** — smooth cubic Bezier (mirrored control)
- **Q/q** — quadratic Bezier (one control point)
- **T/t** — smooth quadratic Bezier
- **A/a** — elliptical arc (rx ry rotation large-arc sweep x y)
- **Z** — close path

### 2.4 Text Handling (ref: text topic)
- `<text>` with `x`, `y`, `font-family`, `font-size`
- `<tspan>` for inline formatting changes
- `<textPath>` for text along a path (xlink:href to path id)
- `dominant-baseline` and `text-anchor` for alignment
- Consider `lengthAdjust` and `textLength` for fitted text

### 2.5 Colors & Fills (ref: colors topic)
- Named colors, hex (#RRGGBB / #RGB), rgb(), hsl()
- `fill` and `stroke` properties
- `fill-opacity`, `stroke-opacity`, `opacity`
- `fill-rule`: `nonzero` (default) vs `evenodd`

### 2.6 Gradients (ref: colors ch09-ch12)
```xml
<!-- Linear gradient -->
<linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
  <stop offset="0%" stop-color="#color1"/>
  <stop offset="100%" stop-color="#color2"/>
</linearGradient>

<!-- Radial gradient -->
<radialGradient id="grad2" cx="50%" cy="50%" r="50%">
  <stop offset="0%" stop-color="#center"/>
  <stop offset="100%" stop-color="#edge"/>
</radialGradient>
```
- `gradientUnits`: `objectBoundingBox` (default, %) vs `userSpaceOnUse` (absolute)
- `spreadMethod`: `pad` (default), `reflect`, `repeat`
- Focal point with `fx`/`fy` on radial gradients

### 2.7 Patterns (ref: colors ch13-ch14)
```xml
<pattern id="pat1" width="20" height="20" patternUnits="userSpaceOnUse">
  <!-- pattern content -->
</pattern>
```
- `patternUnits` vs `patternContentUnits`
- `patternTransform` for rotating/scaling patterns

### 2.8 Transforms (ref: essentials ch06, web ch26-ch27)
- `translate(tx, ty)` — move
- `scale(sx, sy)` — resize (negative = flip)
- `rotate(angle, cx, cy)` — rotate around point
- `skewX(angle)`, `skewY(angle)` — shear
- `matrix(a,b,c,d,e,f)` — full affine transform
- Transforms apply right-to-left when chained
- Use `transform-origin` in CSS for intuitive rotation centers

### 2.9 Clipping & Masking (ref: essentials ch09, web ch34-ch35)
```xml
<!-- Clip: sharp boundary -->
<clipPath id="clip1"><circle cx="50" cy="50" r="40"/></clipPath>
<image clip-path="url(#clip1)" .../>

<!-- Mask: soft/graduated boundary -->
<mask id="mask1"><rect fill="white" .../><circle fill="black" .../></mask>
<image mask="url(#mask1)" .../>
```
- Clip = binary (in/out), Mask = graduated (luminance-based)

### 2.10 Filters (ref: essentials ch11, web ch36-ch37)
Common filter primitives:
- `feGaussianBlur` — blur (stdDeviation)
- `feDropShadow` — drop shadow shorthand
- `feColorMatrix` — color transformations
- `feMorphology` — dilate/erode shapes
- `feDisplacementMap` — distortion effects
- `feTurbulence` + `feDisplacementMap` — organic textures
- `feComposite` — combine with `in`, `over`, `atop`, `xor`, `arithmetic`
- `feBlend` — blend modes (multiply, screen, overlay, etc.)
- `feMerge` — layer multiple filter results

```xml
<filter id="shadow">
  <feDropShadow dx="2" dy="2" stdDeviation="3" flood-color="rgba(0,0,0,0.3)"/>
</filter>
```

### 2.11 Stroke Properties (ref: web ch30-ch31)
- `stroke-width`, `stroke-linecap` (butt/round/square)
- `stroke-linejoin` (miter/round/bevel), `stroke-miterlimit`
- `stroke-dasharray` — dash patterns (e.g., "5,3,2")
- `stroke-dashoffset` — animate for drawing effects
- `vector-effect="non-scaling-stroke"` — constant stroke width when scaled

### 2.12 Markers (ref: web ch32-ch33)
```xml
<marker id="arrow" viewBox="0 0 10 10" refX="5" refY="5"
        markerWidth="6" markerHeight="6" orient="auto-start-reverse">
  <path d="M 0 0 L 10 5 L 0 10 z" fill="currentColor"/>
</marker>
<line marker-end="url(#arrow)" .../>
```

---

## Step 3 — Add Animation (if needed)

Load reference: animation topic for animation patterns.

### 3.1 CSS Animations (preferred for simple animations)
```css
@keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
.spinner { animation: spin 1s linear infinite; transform-origin: center; }
```

### 3.2 SVG SMIL Animations (for path morphing, gradient animation)
```xml
<animate attributeName="opacity" from="0" to="1" dur="1s" fill="freeze"/>
<animateTransform attributeName="transform" type="rotate" from="0" to="360" dur="2s" repeatCount="indefinite"/>
<animateMotion path="M0,0 C30,0 30,60 60,60" dur="3s" repeatCount="indefinite"/>
```

### 3.3 Stroke Drawing Animation
```css
.draw-path {
  stroke-dasharray: 1000;
  stroke-dashoffset: 1000;
  animation: draw 2s ease forwards;
}
@keyframes draw { to { stroke-dashoffset: 0; } }
```
Calculate actual path length with `getTotalLength()` or estimate.

### 3.4 Animation Best Practices (ref: animation ch07)
- Prefer CSS animations for transforms and opacity (GPU-accelerated)
- Use SMIL for SVG-specific attributes (path d, gradient stops)
- `will-change: transform` for performance hints
- Avoid animating filter properties (expensive)
- Use `prefers-reduced-motion` media query for accessibility

---

## Step 4 — Web Integration & Responsiveness

### 4.1 Embedding Methods (ref: web ch04-ch08, modern ch04)
- **Inline SVG**: Best for dynamic/interactive SVG in HTML
- **`<img src="file.svg">`**: Simple, cached, no scripting
- **CSS `background-image`**: Decorative use, no interaction
- **`<object>`**: Interactive, own DOM, fallback content
- **`<iframe>`**: Full isolation, communication via postMessage

### 4.2 Responsive SVG Pattern
```html
<!-- Fluid width, aspect-ratio preserved -->
<svg viewBox="0 0 800 600" style="width: 100%; height: auto;">
  <!-- content -->
</svg>
```

### 4.3 Accessibility (ref: web ch39-ch40)
- Always include `<title>` and `<desc>` elements
- Use `role="img"` on the `<svg>` element
- `aria-labelledby="titleId descId"` for screen readers
- `aria-hidden="true"` for decorative SVGs

### 4.4 Performance Best Practices (ref: web ch45-ch47)
- Simplify paths: reduce control points, use relative commands
- Reuse with `<use href="#id"/>` and `<defs>`
- Remove editor metadata (Illustrator/Inkscape cruft)
- Minimize `<filter>` usage on mobile
- Use CSS classes instead of inline styles for theming

---

## Step 5 — Output & Deliver

### Output Rules:
1. **SVG must be valid** — proper namespace `xmlns="http://www.w3.org/2000/svg"`
2. **Always include viewBox** — no hardcoded width/height unless user specifies
3. **Include `<title>`** for accessibility
4. **Use `<defs>` for reusable elements** (gradients, patterns, filters, clips)
5. **Use semantic IDs** — meaningful names, not `rect1` or `path23`
6. **Indent properly** — readable, well-structured markup
7. **Comment complex paths** — explain what `<path d="...">` draws

### Format-Specific Output:
- **Pure SVG**: Standalone `<svg>...</svg>` block, save as `.svg`
- **HTML + SVG**: Full `<!DOCTYPE html>` page with `<style>` block for animations
- **React**: `export function SvgComponent({ size, color, ...props })` with configurable props
- **Vue**: `<template>` + `<style scoped>` SFC format

---

## References Index — When to Load Which Chapter

Use `Read` to load specific reference files when you need detailed syntax or examples.

### Topic: SVG Essentials (essentials-*)
| File | Topic | Load when... |
|------|-------|-------------|
| ch01 | Cover + TOC | Understanding structure |
| ch02 | Ch1: Basic shapes | Drawing rect, circle, ellipse, line, polygon |
| ch03 | Ch2: Coordinates | Viewport, viewBox, units, coordinate systems |
| ch04 | Ch3: Paths | Path commands (M, L, C, Q, A, Z) |
| ch05 | Ch4: Document structure | `<g>`, `<use>`, `<defs>`, `<symbol>` |
| ch06 | Ch5: Transforms | translate, rotate, scale, skew, matrix |
| ch07 | Ch6: Text | `<text>`, `<tspan>`, font properties |
| ch08 | Ch7: Stroke & fill | Colors, gradients basics, patterns basics |
| ch09 | Ch8: Clip & mask | clipPath, mask elements |
| ch10 | Ch9: Images | Embedding raster images in SVG |
| ch11 | Ch11: Filters | All filter primitives (blur, shadow, color, composite) |
| ch14 | Ch14: Advanced | SVG + JavaScript interaction |

### Topic: Text Layout (text-*)
| File | Topic | Load when... |
|------|-------|-------------|
| ch04 | Ch1: Text basics | `<text>`, positioning, styling |
| ch08 | Ch5: textPath | Text on a path |
| ch11-ch13 | Ch8-10: Advanced | Multi-line, wrapping, international text |

### Topic: Colors, Patterns & Gradients (colors-*)
| File | Topic | Load when... |
|------|-------|-------------|
| ch05 | Ch2: Painter's model | fill/stroke rendering order |
| ch06 | Ch3: Color values | Hex, RGB, HSL, named colors |
| ch07 | Ch4: Opacity | fill-opacity, stroke-opacity, group opacity |
| ch09 | Ch6: Linear gradients | linearGradient basics |
| ch10 | Ch7: Gradient sizing | gradientUnits, gradientTransform |
| ch11 | Ch8: Repeating | spreadMethod: pad/reflect/repeat |
| ch12 | Ch9: Radial gradients | radialGradient, focal point |
| ch13 | Ch10: Patterns | pattern basics, tiles, textures |
| ch14 | Ch11: Advanced patterns | patternTransform, nested patterns |
| ch15 | Ch12: Text painting | Textured/gradient text |
| ch16 | Ch13: Strokes | Stroke gradients, stroke patterns |

### Topic: Animations (animation-*)
| File | Topic | Load when... |
|------|-------|-------------|
| ch03 | Ch1: SVG anatomy | SVG structure for animation |
| ch04 | Ch2: CSS animation | @keyframes, transitions on SVG |
| ch05 | Ch3: Sprites | SVG sprite sheets, hand-drawn animation |
| ch06 | Ch4: Responsive sprites | Responsive animated SVGs |
| ch07 | Ch5: UI/UX animation | Loading, hover, micro-interactions |
| ch08 | Ch6: Data viz animation | Chart animation, data-driven motion |

### Topic: SVG + CSS3 + HTML5 (web-*)
| File | Topic | Load when... |
|------|-------|-------------|
| ch06 | Ch1: SVG overview | SVG on the web, history |
| ch08 | Ch2: SVG + web | Embedding methods, browser support |
| ch10 | Ch3: CSS + SVG | Styling SVG with CSS, cascading rules |
| ch12 | Ch4: Tools | Editor tools, optimization tools |
| ch15 | Ch5: Basic shapes | Detailed shape attributes |
| ch16 | Ch6: Custom paths | Advanced path techniques |
| ch18 | Ch7: Text layout | Graphical text in web context |
| ch21 | Ch8: Coordinate systems | viewBox deep dive, preserveAspectRatio |
| ch23 | Ch9: Cropping | Viewport clipping, overflow |
| ch25 | Ch10: Reuse | `<use>`, `<symbol>`, shadow DOM |
| ch27 | Ch11: Transforms | CSS transforms on SVG elements |
| ch29 | Ch12: Fills | Gradients + patterns (comprehensive) |
| ch31 | Ch13: Strokes | All stroke properties |
| ch33 | Ch14: Markers | Arrow markers, custom markers |
| ch35 | Ch15: Clip + mask | Clipping and masking techniques |
| ch37 | Ch16: Filters | Filter effects with CSS blend modes |
| ch40 | Ch17: Accessibility | ARIA, `<title>`, `<desc>`, roles |
| ch42 | Ch18: Interactivity | Events, hover, click, scripting |
| ch44 | Ch19: Animation | CSS + SMIL animation techniques |
| ch47 | Ch20: Best practices | Performance, optimization, patterns |

### Topic: Modern SVG (modern-*)
| File | Topic | Load when... |
|------|-------|-------------|
| ch04 | Ch1: SVG in web pages | Modern embedding best practices |
| ch05 | Ch2: Tools | Modern SVG tool chain |
| ch06 | Ch3: Advanced paths | Advanced curves and shapes |
| ch07 | Ch4: Colors | Modern color techniques |
