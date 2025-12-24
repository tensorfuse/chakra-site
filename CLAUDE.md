# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**WHIP Manifesto** is a single-page marketing website built with Tailwind CSS. The site features creative scroll-based animations, marble textures, glass morphism UI elements, and an immersive visual experience. The project is deployed to GitHub Pages from the `/docs` folder.

## Build System

### Node Version

This project requires Node.js 22:

```bash
nvm use 22
```

### Development Commands

```bash
# Watch mode - auto-compile CSS on file changes
npm run watch:css

# Build CSS only (minified)
npm run build:css

# Full build - compiles CSS and copies HTML to docs/
npm run build
```

### Build Pipeline

The build process:
1. Tailwind CLI compiles `src/input.css` → `docs/output.css` (minified)
2. Copies `src/index.html` → `docs/index.html`

The `/docs` folder is what gets deployed to GitHub Pages.

## Project Structure

```
src/
├── index.html      # Source HTML (single page with all content)
├── input.css       # Tailwind + custom CSS animations and effects
└── output.css      # Old compiled output (ignore - docs/ is used now)

docs/               # Deployment target for GitHub Pages
├── index.html      # Built HTML
└── output.css      # Built CSS (minified)

tailwind.config.js  # Tailwind configuration
```

## Architecture Notes

### CSS Architecture

The `src/input.css` file contains:
- Tailwind base, components, and utilities imports
- Custom CSS variables for animations (`:root` with `--scroll`, aurora colors)
- Performance optimizations (GPU acceleration hints, `will-change`, `contain` properties)
- Custom component classes: `.glass-panel`, `.glass-shard`, `.marble-*`, `.morph-blob`
- Animation keyframes for floating elements, morphing shapes, aurora effects
- Scroll-based effects and cursor glow styles

### HTML Architecture

`src/index.html` is a long-form single page (~2000+ lines) with:
- **Fixed background elements**: animated blobs, particles, watermark text
- **Scroll-driven sections**: Multiple sections that span several viewport heights
- **Interactive elements**: Glass cards with hover effects, magnetic elements
- **Marble textures**: Generated via CSS gradients (`.marble-teal`, `.marble-coral`, etc.)
- **Custom animations**: Coordinated via inline `<script>` at bottom of HTML

The page uses a "scroll story" approach where content unfolds over `min-height: 600vh`.

### Animation System

Animations are managed through:
1. **CSS keyframes** in `input.css` (floating, morphing, pulsing)
2. **JavaScript scroll listeners** in inline `<script>` tags in `index.html`
3. **Tailwind utility classes** for transitions and transforms

**Performance considerations**: GPU acceleration is forced via `will-change` and `contain` properties. Motion preferences are respected via `prefers-reduced-motion` media query.

### Deployment

- **Target**: GitHub Pages
- **Deploy folder**: `/docs` (not `/dist` as mentioned in old README)
- **Branch**: `main`
- **Build required**: Yes - run `npm run build` before pushing

The README mentions `/dist`, but the actual deployment folder is `/docs` (check Settings → Pages).

## Key Design Patterns

1. **Fixed positioning for background elements** - Creates parallax effect during scroll
2. **Absolute positioning for content** - Positioned via top/left percentages for fluid layout
3. **Transform-based animations** - All movement via `transform` for GPU acceleration
4. **Glass morphism** - `rgba()` backgrounds with backdrop-blur and subtle borders
5. **Marble gradients** - Complex radial gradients create realistic marble textures

## Important Notes

- All source files are in `/src`, never edit `/docs` directly
- The build copies files to `/docs`, which is the GitHub Pages source
- Custom fonts loaded via Google Fonts: DM Sans, Space Mono, Material Symbols
- No build tools beyond Tailwind CLI (no webpack, vite, etc.)
- No framework - vanilla HTML/CSS/JS
