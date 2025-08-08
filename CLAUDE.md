# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Development Server:**
```bash
npm run dev          # Start dev server on port 9000 with auto-open
```

**Build:**
```bash
npm run build        # TypeScript check + Vite build
```

**Code Quality:**
```bash
npm run check:code   # ESLint check without fixing
npm run check:style  # Stylelint check for CSS/Less/Vue files  
npm run check:types  # TypeScript type checking
npm run lint:code    # ESLint with auto-fix
npm run lint:style   # Stylelint with auto-fix
npm run format       # Prettier formatting
```

**Testing:**
```bash
npm test             # Run vitest tests
npm run test:coverage # Run tests with coverage
npm run test:watch   # Run tests in watch mode with coverage
```

## Architecture Overview

**Core Framework:** Vue 3 + Tiptap 2 rich text editor with TypeScript

**Key Libraries:**
- **Tiptap**: Prosemirror-based WYSIWYG editor framework
- **@umoteam/editor-external**: External utilities and assets
- **TDesign Vue Next**: UI component library
- **@vueuse/core**: Vue composition utilities
- **@tool-belt/type-predicates**: Runtime type checking utilities

## Project Structure

```
src/
├── components/           # Vue components
│   ├── editor/          # Main editor component
│   ├── menus/           # Editor menu components (toolbar, bubble, context)
│   ├── container/       # Layout containers (page, print, search, toc)
│   └── statusbar/       # Bottom status bar components
├── extensions/          # Tiptap editor extensions
│   ├── audio/           # Audio embedding
│   ├── callout/         # Callout blocks  
│   ├── echarts/         # Chart integration
│   ├── image/           # Image handling
│   ├── table/           # Table functionality
│   └── [others]/        # Various content types
├── composables/         # Vue composables for shared logic
├── utils/               # Utility functions
├── assets/              # Static assets (icons, styles, images)
├── locales/             # i18n translation files
└── options/             # Configuration options
```

## Key Concepts

**Editor Extensions System:**
- Extensions are built on Tiptap's extension API
- Each extension handles a specific content type (images, tables, callouts, etc.)
- Extensions can include node views (Vue components) for complex rendering
- Extensions are configured in `src/extensions/index.ts` via `getDefaultExtensions()`

**Component Architecture:**
- Main editor: `src/components/editor/index.vue`
- Menu system: Three types - toolbar, bubble menu, context menu
- Each menu type has specialized components for different editor actions
- Modal dialogs use `src/components/modal.vue`
- Tooltips use `src/components/tooltip.vue`

**Styling:**
- Uses Less preprocessor with `@prefix: 'umo'` variable
- Component styles are scoped to `.umo-*` classes
- TDesign component integration via auto-import resolvers
- Icons are SVG-based with custom icon system

## Development Guidelines

**TypeScript Usage:**
- Strict typing enabled - assume experienced TypeScript developers
- Use `@tool-belt/type-predicates` for runtime type checking (includes node:util + more)
- Path alias `@/` maps to `src/` directory
- Test files use `*.spec.ts` naming and sit next to source files

**Vue Patterns:**
- Vue 3 Composition API preferred
- Vue Macros and Reactivity Transform enabled via Vite plugins
- Auto-imports for Vue, VueUse, and composables from `src/composables/`
- TDesign components auto-imported

**Code Style:**
- Prettier configured: no semicolons, single quotes, 2-space tabs
- Functional programming preferred, OOP for complex services only
- ESLint with Vue, TypeScript, and custom rules
- Stylelint for Less/CSS validation

**Testing:**
- Vitest with @testing-library/vue
- BDD-style tests using `it()` and `describe()`
- Use `vi.mocked()` for mock typing
- Tests should use `describe.each` and `it.each` for parameterized testing

## Package Management

- Uses `pnpm` as package manager (evident from `pnpm-lock.yaml`)
- Node.js >= 18.0.0 required
- Build targets ES2018

## Extension Development

When adding new Tiptap extensions:
1. Create extension directory in `src/extensions/`
2. Include `index.ts` for extension logic
3. Add `node-view.vue` if custom rendering needed
4. Register in `src/extensions/index.ts` EXTENSIONS object
5. Add menu components in appropriate `src/components/menus/` subdirectory

## Build System

- Vite-based with library build mode
- Builds to `dist/umo-editor.js` ES module
- TypeScript declarations generated to `types/` directory
- SVG icons bundled via vite-plugin-svg-icons
- CSS preprocessed with Less and bundled separately