# AGENTS.md - Coding Guidelines for ITX Mobile Shop SPA

## Build / Lint / Test Commands

```bash
# Development
npm run START          # Start dev server with MSW enabled, opens /spa/

# Build
npm run BUILD          # Production build with Vite

# Linting
npm run LINT           # Run ESLint on src/ (max 0 warnings)
npx eslint src --fix   # Auto-fix ESLint issues

# Testing
npm run TEST           # Run all tests in watch mode
npx vitest --run       # Run tests once (CI mode)
npx vitest src/App.test.jsx  # Run single test file
npx vitest -t "test name"    # Run tests matching pattern

# Formatting
npm run PRETTIER       # Format all files with caching
npx prettier --write src/specific/file.jsx
```

## Code Style Guidelines

### Formatting (Prettier)
- **Single quotes** for strings
- **2-space indentation**
- **120 character** print width
- **Trailing commas** everywhere (all)
- **Semi-colons** required
- **Auto end-of-line**

### Import Order (Prettier Plugin)
Imports are auto-sorted by `@trivago/prettier-plugin-sort-imports`:
1. React/React-DOM/React-Router
2. Third-party modules
3. Internal aliases (assets, components, hooks, etc.)
4. Relative imports (../, ./)

```javascript
import { useState } from 'react';
import { Provider } from 'react-redux';

import { store } from 'app/store.js';
import { ProductListPage } from 'pages/product/List/ProductListPage.jsx';

import './App.css';
```

### Naming Conventions
- **Components**: PascalCase (e.g., `ProductListPage.jsx`)
- **Functions/Variables**: camelCase
- **Constants**: UPPER_SNAKE_CASE for true constants
- **Files**: Match default export name (PascalCase for components)
- **Redux slices**: camelCase filename, export slice name matching file

### React Patterns
- Use **React 19** with StrictMode
- **Functional components** with arrow functions
- **React Compiler** enabled (babel-plugin-react-compiler)
- Use **Redux Toolkit** for global state
- Use **RTK Query** for API calls via `createApiBase` helper
- Error handling: Check `isLoading`, `error` states from queries

### Redux/RTK Query Patterns
```javascript
// Slices: Export actions and reducer
export const { setNumberOfItems } = cartSlice.actions;
export default cartSlice.reducer;

// APIs: Export hooks from createApiBase
export const { useGetProductsQuery } = productApi;
```

### Testing (Vitest + happy-dom)
- **Environment**: happy-dom (faster than jsdom)
- **Globals**: true (no need to import describe/it/expect)
- **MSW**: Mock Service Worker for API mocking
- Use `@testing-library/react` for component tests
- Use `screen`, `render`, `within` from testing-library
- Wrap components with Redux `<Provider store={store}>`

### Error Handling
- API errors: Handle `error` state from RTK Query hooks
- Root element check: Throw descriptive error if container not found
- MSW: `onUnhandledRequest: 'bypass'` in dev, `'error'` in tests

### Project Structure
```
src/
├── app/           # Redux store configuration
├── config/        # Global variables (globalVariables.js)
├── features/      # Redux slices (e.g., cart/cartSlice.js)
├── layouts/       # Layout components (MainLayout)
├── msw/           # Mock Service Worker setup
├── pages/         # Page components (feature/Action/PageName.jsx)
├── services/      # API endpoints & RTK Query (feature/featureApi.js)
│   ├── base/      # createApiBase helper
│   └── feature/
│       └── msw/   # MSW handlers and mocks
└── test/          # Test utilities and setup
```

### Husky Hooks
- **pre-commit**: Prettier + ESLint
- **pre-push**: ESLint (0 warnings) + Tests

## Environment Variables

Use `globalVariables.js` to centralize env vars:
- Supports `import.meta.env` and `window._env_` overrides
- Docker/production compatible without rebuild

## MSW (Mock Service Worker)

- Development: Enabled via `VITE_MSW=true`
- Tests: Server mode with `onUnhandledRequest: 'error'`
- Place mocks in `src/services/<feature>/msw/mocks/`
- Place handlers in `src/services/<feature>/msw/` and combine in `src/msw/handlers.js`
