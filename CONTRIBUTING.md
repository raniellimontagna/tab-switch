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
