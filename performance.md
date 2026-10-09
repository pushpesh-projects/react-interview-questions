1. What is recursive rendering in react?
Ans- React starts rendering from the root component and recursively traverses down the component/element tree. For each component, React executes it and processes the elements/components returned from it.
It continues this process for the children until it reaches the leaf/host elements such as <div>, <button> , etc
This recursive traversal allows React to build and compare the UI tree during reconcilliation.

2. What are the three phases of the rendering process
   Ans - Render phase - React runs the code from the component that had the state change, and all the descendent components of that component as well. During this phase, it creates the virtual representation of the UI without changing the actual DOM.
   Reconciliation - React compares the new UI tree with the previous UI tree to determine what has changed. It identifies the minimum set of updates required to bring the UI in sync with the latest state and props
   Commit phase - React takes the changes identified during the reconciliation and applies them to the actual DOM. The commit phase is also where React handles things that need to happen after the DOM has been updated.
   (such as running the useLayoutEffect, scheduling the useEffect to run after the commit, updating or removing DOM nodes)

   Render -> (What should the UI look like? ) -> Reconciliation (What actually changed) -> Commit (Apply those changes to the real DOM)

3. Using dev tool to measure performance
   Ans - We should first measure the performance before optimizing anything.

4. StrictMode in react
   Ans - <StrictMode> lets you find common bugs in your components early during development.
   Strict Mode enables the following development-only behaviors:
   Your components will re-render an extra time to find bugs caused by impure rendering.
   Your components will re-run Effects an extra time to find bugs caused by missing Effect cleanup.
   Your components will re-run refs callbacks an extra time to find bugs caused by missing ref cleanup.
   Your components will be checked for usage of deprecated APIs.

5. Code splitting
   Code splitting is a performance optimization technique in React where the application bundle is divided into smaller chunks and loaded on demand instead of loading the entire application at once. React implements code splitting using React.lazy(), Suspense and dynamic imports to improve the initial loading performance.
   In simple words , instead of loading the entire application at once React loads only the code that is required for the current page or component.

   Steps to implement lazy loading
   Recognize the large component which is not necessary for all the users when the app loads
   Import the lazy() and Suspense components from the React package.
   Use the lazy() function to dynamically import the component you want to lazy load. Argument to lazy function should be a function that returns the result of the import() function.
   Wrap the lazy loaded component in a Suspense component which will display a fallback UI while the component is being loaded.

   which page to lazy load for reducing bundle size
   In production React applications, route-level pages are usually lazy-loaded by default because users rarely visit all pages during a session. This reduces the initial bundle size and improves first load performance. However, lazy loading every tiny page can create unnecessary network requests and navigation delays. Therefore, the common approach is to lazy load most routes and heavyweight features while keeping frequently used shared components such as layouts, headers, sidebars, and navigation in the main bundle. This provides the best balance between startup performance and user experience.

   how to measure the bundle size
   npx vite-bundle-visualizer

   Route-level pages should generally be lazy loaded because users do not visit every route. For route-specific subcomponents, lazy loading should be applied selectively. Components that are required immediately for rendering the page should be bundled with the page. Components that are large, contain heavy third-party libraries, appear in tabs, modals, drawers, or are loaded only after user interaction are good candidates for lazy loading. The goal is to reduce the initial JavaScript without introducing unnecessary chunk requests.

    
