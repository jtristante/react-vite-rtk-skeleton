# React Vite RTK SPA Skeleton

Opinionated React SPA skeleton focused on scalability, testability, and developer experience. Powered by React 19, Vite, Redux Toolkit, RTK Query, MSW, and Vitest, with a clean architecture and flexible environment configuration.

---

## 📖 Table of Contents

- [🛠️ Main Technologies](#-main-technologies)
- [🖥️ Environment Variables and `globalVariables`](#-environment-variables-and-globalvariables)
- [🚀 Available Scripts](#-available-scripts)
- [📂 Project Structure](#-project-structure)
- [🧪 Testing](#-testing)
- [📝 Additional Notes](#-additional-notes)

---

## 🛠️ Main Technologies

- **React 19**: User interface development with React Compiler for automatic memoization.
- **Redux Toolkit / RTK Query**: Global state management and API consumption.
- **React Router DOM 7**: SPA routing.
- **Vite 7**: Modern bundler and development server.
- **MSW 2**: Mock Service Worker for API mocking during development and testing.
- **Vitest 4**: Unit and integration testing with happy-dom environment.
- **Testing Library / Jest DOM**: React component testing.
- **ESLint / Prettier**: Code linting and formatting with import sorting.
- **Husky**: Git hooks for pre-commit and pre-push checks.
- **Cross-env**: Portable environment variables.

---

## 🖥️ Environment Variables and `globalVariables`

We use `.env` files for different environments:

- `.env` → Global environment variables.
- `.env.development` → Development variables (MSW enabled).

Additionally, we have a **`globalVariables`** module in the app to:

1. Centralize all environment variables used in the app.
2. Allow overriding variables from `window._env_` in environments where they are injected dynamically (e.g., Docker or production without rebuild).
3. Facilitate testing and mocking without directly relying on `import.meta.env`.

This ensures the same app can run in **development**, **testing**, and **production** without rebuilding for different URLs, timeouts, or flags like `VITE_MSW`.

---

## 🚀 Available Scripts

```bash
# Start the app in development mode
npm run START

# Build for production
npm run BUILD

# Run tests
npm run TEST

# Run linter
npm run LINT

# Format code
npm run PRETTIER

```

---

## 📂 Project Structure

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

---

## 🧪 Testing

This project uses **Vitest** with **happy-dom** for fast DOM simulation and **MSW** for API mocking. Tests are co-located with source files using the pattern `*.test.jsx`.

### Running Tests

```bash
# Run tests in watch mode (development)
npm run TEST

# Run tests once (CI mode)
npx vitest --run

# Run a single test file
npx vitest src/App.test.jsx

# Run tests matching a pattern
npx vitest -t "test name"
```

### Testing Guidelines

- Wrap components with Redux `<Provider store={store}>` when testing connected components
- Use `screen`, `render`, and `within` from `@testing-library/react`
- MSW runs in server mode during tests with `onUnhandledRequest: 'error'` to catch unmocked requests

---

## 📝 Additional Notes

### Git Hooks (Husky)

This project uses Husky for automated checks:

- **pre-commit**: Runs Prettier and ESLint on staged files
- **pre-push**: Runs ESLint (max 0 warnings) and all tests

### Import Sorting

Imports are automatically sorted by `@trivago/prettier-plugin-sort-imports`:

1. React/React-DOM/React-Router
2. Third-party modules
3. Internal aliases (assets, components, hooks, etc.)
4. Relative imports (../, ./)

### React Compiler

React Compiler (via babel-plugin-react-compiler) is enabled to automatically memoize components and values, reducing the need for manual useMemo and useCallback optimizations.
