# Rect vite rtk SPA Skeleton

This project is a **Single Page Application (SPA) built with **React**, **Redux Toolkit**, **React Router**, and styled with **Chakra UI**. It is prepared for testing with **Vitest** and **MSW**.


---
## 📖 Table of Contents

- [🛠️ Main Technologies](#-main-technologies)
- [🖥️ Environment Variables and `globalVariables`](#-environment-variables-and-globalvariables)
- [🚀 Available Scripts](#-available-scripts)
- [📂 Project Structure](#️-project-structure)
- [🧪 Testing](#-testing)
- [📝  Additional Notes](#-additional-notes)

---


## 🛠️ Main Technologies

- **React 19**: User interface development.
- **Redux Toolkit / RTK Query**: Global state management and API consumption.
- **React Router DOM 7**: SPA routing.
- **Vite 7**: Modern bundler and development server.
- **MSW 2**: Mock Service Worker for API mocking during development and testing.
- **Vitest 4**: Unit and integration testing.
- **Testing Library / Jest DOM**: React component testing.
- **ESLint / Prettier**: Code linting and formatting.
- **Cross-env**: Portable environment variables.
- **Chakra UI** (work in progress): Accessible and styled component library.

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
