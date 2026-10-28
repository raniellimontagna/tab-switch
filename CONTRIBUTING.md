# Contributing Guidelines

Internal coding standards for the **tab-switch** project. All contributions must follow these rules.

---

## 1. Naming Conventions

| Element             | Convention      | Example                        |
| ------------------- | --------------- | ------------------------------ |
| Files / Directories | **kebab-case**  | `error-boundary.tsx`           |
| Components          | **PascalCase**  | `ErrorBoundary`                |
| Functions / Vars    | **camelCase**   | `handleSubmit`, `isLoading`    |
| Constants           | **UPPER_SNAKE** | `FORM_DEFAULTS`, `INTERVAL`    |
| Types / Interfaces  | **PascalCase**  | `TabSchema`, `CustomInputProps`|
| Hooks               | **camelCase**   | `useHome` (file: `use-home.ts`)|
| Test files          | **kebab-case**  | `error-boundary.test.tsx`      |

## 2. Imports

- **Absolute imports** (`@/...`) for all internal files.
- **Sort order**: Built-in → External → Internal.
- Biome handles import organization automatically via `organizeImports`.

## 3. Exports

- **Named exports only**. No `export default`.
- **No `export *`** in barrel files. Use explicit named re-exports.

```ts
// ✅ Correct
export { ErrorBoundary } from './error-boundary'
export { CustomInput } from './custom-input/custom-input'

// ❌ Wrong
export default App
export * from './button'
```

## 4. Components

- **Arrow functions** for all functional components.
- Keep components small and focused (Single Responsibility).
- Extract complex logic into **custom hooks**.

```tsx
// ✅ Correct
export const ErrorFallback = ({ error }: ErrorFallbackProps): ReactNode => {
  // ...
}

// ❌ Wrong
export function ErrorFallback({ error }: ErrorFallbackProps) {
  // ...
}
```

## 5. TypeScript

- **No `any`**. Use `unknown` or specific types.
- **Explicit return types** for all functions.
- Validate external data (API responses, storage, user inputs) with **Zod**.

## 6. Accessibility

- Use **semantic HTML** (`<header>`, `<main>`, `<section>`, `<button>`).
- All inputs must have associated `<label>` or `aria-label`.
- Interactive elements must be **keyboard accessible**.

## 7. Comments

- English only.
- Only when strictly necessary (complex business rules, non-obvious decisions).
- No redundant comments. No dead/commented-out code.
- Prefer self-documenting code via clear naming.

## 8. File Structure

```
src/
├── @types/           # Global type declarations
├── assets/           # Static assets (SVGs, images)
├── background/       # Chrome extension background script
├── components/       # Reusable components
│   ├── ui/           # Primitive UI components (button, input, etc.)
│   ├── custom-input/ # Domain-specific components (kebab-case dirs)
│   └── index.ts      # Barrel with named exports
├── constants/        # Application constants (UPPER_SNAKE)
├── containers/       # Page-level containers
│   └── home/         # Home page container + schema + hook
├── hooks/            # Custom hooks (use-*.ts)
├── libs/             # Shared libs (storage, i18n, logger, etc.)
├── services/         # External service integrations
├── styles/           # Global styles
├── test/             # Test setup and utilities
└── utils/            # Pure utility functions
```

## 9. Testing

- Test files colocated in `__tests__/` directories.
- Test file names in **kebab-case**: `error-boundary.test.tsx`.
- Use **Vitest** + **Testing Library**.
- Run: `pnpm test`, `pnpm test:coverage`.

## 10. Formatting & Linting

- **Biome** handles formatting and linting.
- Run `pnpm lint` / `pnpm lint:fix` before committing.
- **Lefthook** runs checks on pre-commit automatically.

## 11. Language

- **Code / Comments / Variables**: English.
- **UI / User-facing text**: Portuguese (PT-BR) via i18next.

## 12. Workflow

1. Fork the repository and clone your fork.
2. Install dependencies with `pnpm install` and the git hooks with `pnpm prepare` (Lefthook runs Biome and the type-check before each commit, and a full check plus a build before each push).
3. Create a branch such as `feat/my-feature` or `fix/my-fix`.
4. Make your change, add tests where possible and make sure `pnpm build` passes.
5. Commit with [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:` and so on) and open a pull request against `main`.

## 13. Debugging and Troubleshooting

- **Popup:** open the popup, right-click it and choose **Inspect**.
- **Background service worker:** on `chrome://extensions/`, click **Service worker** on the extension card.
- **Logs:** the logger is verbose in development builds; build with `VITE_DEBUG=true pnpm build` to keep it on in production builds.
- **No hot reload:** after a change, run `pnpm build` and click the reload icon on the extension card.
- **The extension does not load:** make sure you built it and selected the `build/` folder, not `src/`. Errors show under **Details > Errors** on the extension card.
- **Tabs are not created or managed:** check that `public/manifest.json` still has the `tabs` permission, then reload the extension.
- **Translations missing:** run `pnpm validate:translations` and check that `build/locales/` was generated.
- **Corrupted settings:** in the popup DevTools, run `chrome.storage.local.clear()` to reset the stored data.
- **Build fails after a dependency change:** remove `node_modules` and `build`, then run `pnpm install` and `pnpm build` again.

## 14. Releases

Versions follow [Semantic Versioning](https://semver.org/) and are managed by [release-please](https://github.com/googleapis/release-please):

- `feat:` bumps the minor version, `fix:` the patch version, and `feat!:` or `fix!:` (breaking changes) the major version. Other types do not create a release.
- On every push to `main`, release-please opens or updates a release pull request with the new version and the `CHANGELOG.md` entries. Merging it creates the tag and the GitHub release.
