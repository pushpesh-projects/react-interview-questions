# React Performance & Rendering Notes

## 1. What is Recursive Rendering in React?

### Answer

React starts rendering from the root component and recursively traverses down the component/element tree.

For each component, React executes it and processes the elements/components returned from it.

It continues this process for the children until it reaches the leaf (host) elements such as:

- `<div>`
- `<button>`
- `<input>`
- `<span>`

This recursive traversal allows React to build and compare the UI tree during reconciliation.

---

## 2. What are the Three Phases of the Rendering Process?

### Render Phase

React runs the code from:

- The component that had the state change.
- All descendant components of that component.

During this phase, React creates a virtual representation of the UI without changing the actual DOM.

### Reconciliation Phase

React compares the new UI tree with the previous UI tree to determine what has changed.

It identifies the minimum set of updates required to bring the UI in sync with the latest state and props.

### Commit Phase

React takes the changes identified during reconciliation and applies them to the real DOM.

This phase is also where React performs work that must happen after the DOM update, such as:

- Running `useLayoutEffect`
- Scheduling `useEffect`
- Updating DOM nodes
- Removing DOM nodes

### Summary

```text
Render
   ↓
What should the UI look like?
   ↓
Reconciliation
   ↓
What actually changed?
   ↓
Commit
   ↓
Apply those changes to the real DOM
```

---

## 3. Using DevTools to Measure Performance

### Answer

Before optimizing anything, always measure performance first.

Common tools include:

- React DevTools Profiler
- Chrome DevTools Performance Tab
- Lighthouse
- Bundle Analyzers

### Rule

> Measure first, optimize second.

Premature optimization often solves the wrong problem.

---

## 4. StrictMode in React

### Answer

`<StrictMode>` helps you find common bugs in your components early during development.

Strict Mode enables the following **development-only** behaviors:

- Components re-render an extra time to catch impure rendering.
- Effects re-run an extra time to catch missing cleanup logic.
- Ref callbacks re-run an extra time to catch missing ref cleanup.
- Deprecated API usage is detected and reported.

### Example

```jsx
<StrictMode>
  <App />
</StrictMode>
```

### Important

These checks only happen in development mode and do not affect production builds.

---

# 5. Code Splitting

## What is Code Splitting?

Code splitting is a performance optimization technique in React where the application bundle is divided into smaller chunks and loaded on demand instead of loading the entire application at once.

React commonly implements code splitting using:

- `React.lazy()`
- `Suspense`
- Dynamic imports (`import()`)

### Simple Definition

Instead of loading the entire application at once, React loads only the code required for the current page or component.

---

## What Problem Does Code Splitting Solve?

Code splitting using `React.lazy()` reduces the amount of JavaScript that must be:

- Downloaded
- Parsed
- Executed

during the initial application load.

### Benefits

- Smaller bundle size
- Faster initial page load
- Better perceived performance
- Reduced JavaScript execution time

### What It Does NOT Solve

Code splitting does **not** improve:

- API response times
- Database performance
- Server-side processing
- Network latency

If page slowness is caused by slow APIs, database queries, or backend issues, those must be solved separately.

---

## Steps to Implement Lazy Loading

### Step 1: Identify a Good Candidate

Recognize a large component or page that is not immediately required when the application loads.

Examples:

- Reports Page
- Admin Dashboard
- Analytics Page
- Rich Text Editor
- Charting Components

---

### Step 2: Import `lazy` and `Suspense`

```jsx
import React, { lazy, Suspense } from "react";
```

---

### Step 3: Create a Lazy Component

```jsx
const ProductsList = React.lazy(() => import("./ProductsList"));
```

### Requirements of `React.lazy()`

`React.lazy()` expects:

- A function as its argument.
- That function must return a Promise.
- The Promise should come from a dynamic `import()`.
- The imported module must have a default export.

Example:

```jsx
const ProductsList = React.lazy(() => import("./ProductsList"));
```

```jsx
export default ProductsList;
```

### How It Works

`import()` is a JavaScript dynamic import.

```jsx
import("./ProductsList");
```

returns:

```js
Promise<Module>
```

React waits for that Promise to resolve, extracts the module's default export, and renders it.

---

### Suspended Component

```jsx
const ProductsList = React.lazy(() => import("./ProductsList"));
```

You can think of `ProductsList` as a placeholder that is waiting for the component code to finish downloading.

---

### Important Rule

Always declare lazy imports at the top level.

✅ Correct

```jsx
const ProductsList = React.lazy(() => import("./ProductsList"));

function App() {
  return <ProductsList />;
}
```

❌ Incorrect

```jsx
function App() {
  const ProductsList = React.lazy(() => import("./ProductsList"));

  return <ProductsList />;
}
```

---

### Step 4: Wrap with `Suspense`

```jsx
<Suspense fallback={<h2>Loading...</h2>}>
  <ProductsList />
</Suspense>
```

While the JavaScript chunk is downloading, React displays the fallback UI.

---

## Which Pages Should Be Lazy Loaded?

### Recommended Production Approach

Route-level pages should generally be lazy loaded because users typically do not visit every page during a session.

Examples:

```text
/users
/products
/reports
/settings
/admin
```

This reduces the initial bundle size and improves startup performance.

---

## What Should Stay in the Main Bundle?

Frequently used shared components are usually kept in the main bundle.

Examples:

- Layout
- Header
- Sidebar
- Navigation
- Theme Provider

These components are needed immediately when the application starts.

---

## Avoid Overusing Lazy Loading

Lazy loading every small component can create:

- Too many network requests
- Additional navigation delays
- Unnecessary complexity

The goal is to balance startup performance with user experience.

---

## Route-Specific Subcomponents

Route-specific subcomponents should be lazy loaded selectively.

### Keep Bundled With the Page

Components required immediately for rendering:

```text
UsersPage
 ├── UsersHeader
 ├── UsersTable
 └── UsersFilters
```

These should typically be bundled with the page.

---

### Good Candidates for Lazy Loading

Components that are:

- Large
- Expensive to load
- Not immediately visible
- Loaded after user interaction

Examples:

- Charts
- Report Builders
- Rich Text Editors
- Modals
- Drawers
- Tab Content

Example:

```jsx
const AnalyticsModal = React.lazy(() =>
  import("./AnalyticsModal")
);
```

---

## How to Measure Bundle Size

### Vite

```bash
npx vite-bundle-visualizer
```

This helps identify:

- Largest chunks
- Duplicate dependencies
- Bundle growth over time

---

## What Happens Internally?

### Step 1

You write:

```jsx
const UsersPage = React.lazy(() => import("./UsersPage"));
```

---

### Step 2

When the project is built, the bundler:

- Vite
- Webpack
- Rollup

detects the dynamic import and generates a separate JavaScript chunk.

Instead of:

```text
main.js
```

you might get:

```text
main.js
users.chunk.js
reports.chunk.js
admin.chunk.js
```

---

### Step 3

The user opens:

```text
/
```

The browser downloads only the code needed initially:

```text
main.js
home.chunk.js
```

---

### Step 4

The user navigates to:

```text
/users
```

React Router matches the route and attempts to render:

```jsx
<UsersPage />
```

---

### Step 5

Because `UsersPage` is lazy loaded, React executes:

```jsx
import("./UsersPage")
```

This triggers a new network request to download:

```text
users.chunk.js
```

from the frontend server.

---

### Step 6

While the chunk is downloading:

```jsx
<Suspense fallback={<Spinner />}>
```

renders the loading UI.

---

### Step 7

Once the Promise resolves:

- React receives the component code.
- React renders the component.
- The loading UI disappears.

---

## Complete Flow

```text
User opens app
        ↓
Main bundle downloaded
        ↓
Home page renders
        ↓
User clicks /users
        ↓
React Router matches route
        ↓
React.lazy triggers import()
        ↓
Browser requests users.chunk.js
        ↓
Suspense fallback displayed
        ↓
Chunk downloaded
        ↓
Promise resolves
        ↓
UsersPage renders
```

---

## Interview Summary

### What is Code Splitting?

Code splitting is a performance optimization technique that breaks a large JavaScript bundle into smaller chunks and loads them on demand.

---

### How is it implemented in React?

Using:

```jsx
React.lazy()
Suspense
import()
```

---

### Why do we use it?

- Reduce bundle size
- Improve initial page load
- Download code only when required

---

### Does it Improve API Performance?

No.

Code splitting only optimizes JavaScript loading on the frontend.

Backend performance issues such as:

- Slow APIs
- Slow database queries
- Network latency

must be addressed separately.

---

### What Should Be Lazy Loaded?

- Route-level pages
- Admin sections
- Reports pages
- Analytics dashboards
- Modals
- Drawers
- Rich text editors
- Heavy chart libraries

---

### What Should Not Be Lazy Loaded?

- Header
- Sidebar
- Layout
- Navigation
- Components required for the initial screen

---

### What Happens When a User Navigates to a Lazy-Loaded Route?

1. React Router matches the route.
2. React executes the dynamic `import()`.
3. The browser downloads the corresponding JavaScript chunk.
4. `Suspense` displays the fallback UI.
5. The Promise resolves.
6. React renders the component.
