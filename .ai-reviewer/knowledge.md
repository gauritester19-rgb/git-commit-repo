# git-commit-repo reviewer notes

## Architecture
This is a small React 19 single-page application built with Vite and TypeScript. The entry point is `src/main.tsx`, which renders the single `App` component from `src/App.tsx` under `StrictMode`; styling is imported from `src/index.css` and `src/App.css`. Build, lint, and development workflows are defined in `package.json`, with Vite configured through `vite.config.ts`.

## Conventions
- Use TypeScript React components in `.tsx` files; ESLint applies recommended JavaScript, TypeScript, React Hooks, and React Refresh rules to `**/*.{ts,tsx}` (`eslint.config.js`).
- Keep the application mounted through `createRoot` and preserve `StrictMode` in `src/main.tsx`.
- Use named TypeScript unions and typed props for constrained values. For example, `IconName` in `src/App.tsx` restricts icon names, and `Icon` accepts `{ name: IconName }`.
- Reusable visual data is represented as module-level constants and rendered with `.map()`. The `features` tuple array in `src/App.tsx` drives the feature-card list, which uses `title` as its React key.
- Prefer semantic HTML and accessibility attributes already used throughout `src/App.tsx`: `<header>`, `<nav aria-label>`, `<main>`, `<section>`, `<footer>`, form labels, required inputs, and `role="status"` for submission feedback.
- Use hash links for in-page navigation (`#home`, `#about`, `#features`, `#contact`) rather than introducing a router; section IDs are part of that contract.
- Use local SVG icon paths through the `Icon` component rather than adding an icon dependency. Icons are decorative and intentionally marked `aria-hidden="true"` (`src/App.tsx`).
- Follow the existing no-semicolon formatting and single-quote import style visible in `src/*.tsx`, `vite.config.ts`, and `eslint.config.js`.

## Intentional non-standard choices
- The contact form is deliberately client-side only: `handleSubmit` prevents the browser submission and sets `formSent` after native validity checks. It does not send data to a backend (`src/App.tsx`).
- The page is intentionally implemented as one large `App` component with inline JSX for this small template; do not flag the absence of separate files for each section unless the change materially increases complexity.
- The illustration and branding use text/symbol characters (`◆`, `⚛`, `ϟ`) and CSS-driven elements rather than external image assets (`src/App.tsx`).

## Watch out for
- Do not add feature names to `features` without updating `IconName` and the `paths` map; otherwise the typed icon lookup will break (`src/App.tsx`).
- Preserve unique, stable keys for mapped feature cards; avoid changing `key={title}` to an array index if feature ordering or insertion can change.
- Changes to navigation must keep the corresponding section IDs synchronized, or the hash links will stop working (`src/App.tsx`).
- New interactive state or effects must comply with the React Hooks ESLint configuration; avoid conditional hook calls.
- Keep build output excluded from linting; `dist` is intentionally ignored in `eslint.config.js`.