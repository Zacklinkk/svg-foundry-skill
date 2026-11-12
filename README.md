# SVG Foundry (`svg-craft`)

[English](README.md) | [中文](README_zh.md)

**SVG Foundry** provides the `svg-craft` skill: structured authoring, optimization, animation, and restyling instructions for AI agents working with Scalable Vector Graphics.

The skill guides models to author SVG across four output formats:

- **Standalone SVG**: Clean `.svg` markup with explicit namespaces and viewBoxes.
- **HTML + SVG**: Complete HTML documents containing inline SVG and CSS animations.
- **React JSX**: Configurable React components accepting props such as `size`, `color`, and styling overrides.
- **Vue SFC**: Single-File Components (`<template>` and `<style scoped>`).

---

## Scope and Technical Guidance

`svg-craft` provides direct guidelines for common SVG authoring tasks:

- **Canvas & Coordinates**: Guides `viewBox` usage, explicit or contextual sizing, and coordinate space awareness.
- **Shapes & Paths**: Covers primitives (`<rect>`, `<circle>`, `<path>`, etc.) and path commands (`M`, `C`, `S`, `Q`, `A`, `Z`).
- **Styling & Defs**: Reusable linear/radial gradients, patterns, marker definitions, and filter primitives (`feDropShadow`, `feGaussianBlur`, `feColorMatrix`, etc.).
- **Modern Standards**: Defaults to modern SVG 2 `href` attributes rather than deprecated `xlink:href`, unless older target runtimes specifically require legacy attributes.
- **Component Safety**: Requires unique IDs across instances for definitions (gradients, clips, masks, and ARIA references) to prevent cross-instance collisions when rendered multiple times on a single page.
- **Animation**: Covers CSS keyframe animations and SMIL (`<animate>`, `<animateTransform>`). Promotes `prefers-reduced-motion` handling and avoids speculative claims about GPU acceleration.
- **Accessibility**: Choose an accessible name for informative inline SVG, for example a `<title>` referenced by `aria-labelledby`, and an appropriate role. Hide decorative inline SVG from assistive technology when it adds no information. Embedded `<img>` elements use an appropriate `alt` value. These choices depend on context.

---

## Workflow and Validation

The repository supplies instructions and references. Rendering and validation use the tools available in the host project; no custom rendering engine or automated compatibility suite is bundled.

When local tooling is available in the agent harness, the skill encourages XML well-formedness parsing and visual verification. An XML parse verifies syntax only; it does not guarantee aesthetic balance, font availability, or cross-browser fidelity.

---

## Reference Collection and License Distinction

The repository includes an [Apache-2.0 license](LICENSE). Third-party reference material has separate provenance and permissions.

The repository indexes 127 reference files derived from six published SVG volumes (*SVG Essentials*, *SVG Text Layout*, *SVG Colors Patterns & Gradients*, *SVG Animations*, *Using SVG with CSS3 and HTML5*, and *Modern SVG*). Redistribution rights for material derived from these third-party works have not been verified. The presence of the project license does not establish permission to redistribute those texts. Verify permissions or replace those files with original notes and links before redistributing them.

---

## Installation and Agent Integration

Invocation mechanisms depend on the host harness. While some platforms support slash commands like `/svg-craft`, others load skills automatically via system prompts or configuration files.

### 1. Registering the Skill
Register the repository directory under the name `svg-craft` using your host's supported skill mechanism. Keep `SKILL.md` and `references/` together so relative paths work. There is no universal installation path shared by every agent.

*Tip for multi-agent setups:* If you use multiple local agents, symlink to a single canonical checkout rather than copying the directory, ensuring updates remain synchronized across tools.

### 2. Invocation

- **In harnesses with command routing**: Trigger via `/svg-craft <your prompt>`.
- **In standard chat or API harnesses**: Reference the skill by name or paste the instructions into your system or workspace context.

---

## Example Requests

```text
Use svg-craft to create a 24px settings icon that inherits currentColor.
Use svg-craft to convert this SVG to React with unique IDs per instance.
Use svg-craft to add a loading animation with a reduced-motion alternative.
```

The skill uses the project format when known and otherwise defaults to standalone SVG. It asks only about unresolved choices that materially affect the result.

## Markup Examples

### Standalone Decorative Icon

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
  <circle cx="12" cy="12" r="10"/>
  <polyline points="12 6 12 12 16 14"/>
</svg>
```

### Informative Inline SVG with Gradient

The following IDs are for one instance; generate unique IDs when reusing the graphic in a component. The curve is illustrative, not a chart of measured data.

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100" role="img" aria-labelledby="chart-title">
  <title id="chart-title">Sales growth trend line</title>
  <defs>
    <linearGradient id="line-grad" x1="0%" y1="100%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#2563eb"/>
      <stop offset="100%" stop-color="#7c3aed"/>
    </linearGradient>
  </defs>
  <path d="M 10 80 Q 40 20 90 20" stroke="url(#line-grad)" stroke-width="4" fill="none" stroke-linecap="round"/>
</svg>
```
