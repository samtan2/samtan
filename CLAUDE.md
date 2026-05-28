# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```sh
npm run dev        # start dev server (localhost:5173)
npm run build      # production build
npm run preview    # preview production build
npm run lint       # prettier check + eslint
npm run format     # auto-format with prettier
```

There are no tests configured yet.

## Stack

- **SvelteKit** with **Svelte 5** (runes mode enforced for all non-node_modules files via `svelte.config.js`)
- **Tailwind CSS v4** via the Vite plugin (`@tailwindcss/vite`) — imported with `@import 'tailwindcss'` in CSS, no config file needed
- **adapter-auto** — no deployment target locked in yet
- **ESLint** + **Prettier** (with `prettier-plugin-svelte` and `prettier-plugin-tailwindcss`)

## Architecture

```
src/
  app.html              # HTML shell
  lib/
    index.js            # barrel export for $lib imports
    assets/             # static assets imported as modules (e.g. favicon.svg)
  routes/
    +layout.svelte      # root layout — imports layout.css, sets favicon
    layout.css          # global styles (Tailwind entry point)
    +page.svelte        # home page
```

SvelteKit file-system routing: every `+page.svelte` is a route, `+layout.svelte` wraps child routes, `+server.js` files are API endpoints. Use `$lib` alias for imports from `src/lib/`.

Svelte 5 runes (`$state`, `$derived`, `$effect`, `$props`) are the only reactivity model — legacy `export let` / `$:` syntax is disabled project-wide.
