# Material Design 3 Skill for Claude Code

A comprehensive [Claude Code](https://claude.ai/claude-code) skill for implementing Google's [Material Design 3](https://m3.material.io/) (Material You) UI system.

## What it does

- Guides Claude in generating **MD3-compliant UI code** with correct design tokens, components, theming, layout, and accessibility
- Covers **30+ components** with web element names, attributes, and code examples
- Supports **web** (`@material/web`), **Flutter**, and **Jetpack Compose**
- Includes an **MD3 compliance audit** mode that scores apps across 10 categories
- Covers the **M3 Expressive** update (May 2025): spring motion, emphasized typography, shape morphing

## Installation

Copy the skill into your Claude Code skills directory:

```bash
# Clone the repo
git clone https://github.com/nicegoodthings/material-3-skill.git

# Copy to Claude Code skills directory
cp -r material-3-skill ~/.claude/skills/material-3
```

Or symlink for easy updates:

```bash
ln -s /path/to/material-3-skill ~/.claude/skills/material-3
```

## Usage

### Build MD3 components

```
/material-3 component Create a login form with email and password fields
```

### Generate a theme

```
/material-3 theme Generate a theme from seed color #1A73E8
```

### Scaffold an app

```
/material-3 scaffold Create a responsive app shell with navigation
```

### Audit MD3 compliance

```
/material-3 audit [URL or file path]
```

The audit scores your app across 10 categories (color tokens, typography, shape, elevation, components, layout, navigation, motion, accessibility, theming) and produces a detailed report with specific fixes.

## What's included

| File | Description |
|------|-------------|
| `SKILL.md` | Main skill: philosophy, decision trees, token overview, component table, web implementation patterns, audit procedure |
| `references/color-system.md` | All 29+ color roles, tonal palettes, dynamic color, baseline scheme CSS |
| `references/component-catalog.md` | All 30+ components with `@material/web` elements, attributes, code examples |
| `references/theming-and-dynamic-color.md` | Theme generation, brand colors, dark mode, runtime switching |
| `references/typography-and-shape.md` | Type scale (30 styles), shape corner tokens, elevation levels, motion tokens |
| `references/navigation-patterns.md` | Nav component selection, responsive transitions, complete responsive shell |
| `references/layout-and-responsive.md` | 5 breakpoints, 3 canonical layouts, CSS Grid implementation |

## License

MIT
