<p align="center">
  <img src="assets/banner.svg" alt="SVG Craft Banner" width="100%">
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="License"></a>
  <img src="https://img.shields.io/badge/Version-1.0.0-green.svg" alt="Version">
  <img src="https://img.shields.io/badge/References-127_files-purple.svg" alt="References">
  <img src="https://img.shields.io/badge/SVG_Domains-12-orange.svg" alt="Domains">
  <img src="https://img.shields.io/badge/Output_Formats-4-pink.svg" alt="Formats">
</p>

<p align="center">
  <a href="README_zh.md">中文版</a> · <a href="SKILL_METADATA.yaml">Agent Metadata</a> · <a href="https://zacklinkk.github.io/svg-craft/">Live Demo</a>
</p>

## Overview

SVG Craft is a knowledge-driven AI Skill that creates, optimizes, animates, and restyles SVG graphics from scratch. It includes a comprehensive SVG technical knowledge base covering 12 core areas of the SVG specification, enabling expert-level output for any SVG task.

## Core Capabilities

- **Create** — Generate SVG icons, illustrations, charts, logos, loading animations from natural language descriptions
- **Optimize** — Improve existing SVG code for performance, accessibility, and responsiveness
- **Animate** — Add CSS animations, SMIL animations, or stroke drawing effects to static SVGs
- **Restyle** — Adjust color schemes, gradients, patterns, and filter effects

## Output Formats

| Format | Use Case |
|--------|----------|
| Pure SVG | Standalone `.svg` file, embeddable anywhere |
| HTML + SVG | Complete HTML page with inline SVG and CSS animations |
| React Component | JSX component with customizable props |
| Vue Component | SFC with template + scoped styles |

## Technical Coverage

Covers 12 core areas of the SVG specification:

- Canvas & coordinate systems (viewBox, preserveAspectRatio, responsive scaling)
- Basic & advanced shapes (rect, circle, path, etc.)
- Full path commands (M/L/H/V/C/S/Q/T/A/Z)
- Text layout (text, tspan, textPath, multilingual support)
- Colors & fill systems
- Linear & radial gradients
- Patterns
- Transforms (translate, rotate, scale, matrix, etc.)
- Clipping & masking (clipPath, mask)
- Filters (blur, shadow, colorMatrix, displacement, turbulence, etc.)
- Stroke properties
- Markers

## Knowledge Base

Built-in SVG reference knowledge base with 127 reference files across 6 topics, loaded on demand:

| Topic | Coverage |
|-------|----------|
| SVG Essentials | Shapes, coordinates, paths, transforms, filters |
| Text Layout | Text positioning, text paths, internationalization |
| Colors & Visuals | Colors, gradients, patterns, textures |
| Animations | CSS animations, sprites, responsive animation, data visualization |
| Web Integration | SVG + CSS3 + HTML5 best practices |
| Modern Techniques | Modern toolchains, advanced paths, modern color techniques |

## Installation

**Claude Code:**
```bash
cp -r svg-craft ~/.claude/skills/svg-craft
```

**Other AI Agents:**
Load `SKILL.md` as a system prompt and mount the `references/` directory as a searchable knowledge base. See [SKILL_METADATA.yaml](SKILL_METADATA.yaml) for structured integration details.

## Usage

Automatically triggered by keywords in conversation:

```
draw SVG, create SVG, SVG icon, SVG animation, SVG chart
SVG illustration, SVG gradient, SVG filter, SVG pattern
SVG logo, SVG spinner, SVG progress
```

Or invoke manually via slash command: `/svg-craft`

## Design Philosophy

- **Knowledge-driven** — Guides AI output through structured knowledge, not hardcoded scripts
- **On-demand loading** — Loads only relevant reference chapters for the current task
- **Structured workflow** — 5-step process (clarify → design → animate → integrate → deliver) ensures consistent quality
- **Accessibility-first** — Includes `<title>`, `aria-labelledby`, `prefers-reduced-motion` support by default

## License

Apache License 2.0
