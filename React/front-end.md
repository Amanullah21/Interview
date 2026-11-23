
**1. What is the difference between `==` and `===` in JavaScript?**

* `==` (loose equality) performs type coercion before comparing values (e.g., `'5' == 5` is `true`). `===` (strict equality) checks both value and data type without conversion (`'5' === 5` is `false`). Always default to `===` to avoid unexpected coercion bugs.

**2. Explain `let`, `const`, and `var`—how do scope, hoisting, and reassignment differ?**

* `var` is function-scoped, hoisted with `undefined`, and can be redeclared/reassigned. `let` and `const` are block-scoped (`{}`), hoisted into the Temporal Dead Zone (throwing a `ReferenceError` if accessed before declaration), and cannot be redeclared. `let` allows reassignment; `const` prevents binding reassignment (though object/array contents can still mutate).

**3. What is closure in JavaScript, and give a practical example of where you would use it?**

* A closure occurs when a function remembers and accesses variables from its outer lexical scope even after the outer function has finished executing. Practical uses include data privacy/encapsulation, debouncing/throttling helpers, and factory functions that pre-configure arguments.

**4. How does the JavaScript event loop handle call stack, microtasks (Promises), and macrotasks (`setTimeout`)?**

* JS executes synchronous code on the single-threaded call stack first. Once the stack is empty, the event loop drains the entire **microtask queue** (Promises, `queueMicrotask`, `MutationObserver`) before picking one task from the **macrotask queue** (`setTimeout`, `setInterval`, DOM events), then repeats.

**5. What is event delegation, and why is it useful in lists or dynamic UIs?**

* Event delegation attaches a single event listener to a common parent element instead of individual child elements, relying on event bubbling (`event.target` / `event.target.closest()`). It lowers memory usage and automatically works for dynamically added or removed child items.

**6. Explain `this` keyword behavior in regular functions vs. arrow functions.**

* In regular functions, `this` is determined dynamically by how the function is called (object method, `new`, `call/apply/bind`, or global/`undefined` in strict mode). Arrow functions do not have their own `this`; they lexically inherit `this` from the enclosing scope where they were defined.

**7. What are Promises and `async/await`, and how do you handle errors in `async/await`?**

* A Promise represents the eventual completion or failure of an asynchronous operation (`pending`, `fulfilled`, `rejected`). `async/await` is syntactic sugar over Promises that lets async code read synchronously. Errors are handled with `try...catch` blocks around `await` calls, or by chaining `.catch()` on the returned Promise.

**8. What is the difference between `localStorage`, `sessionStorage`, and cookies?**

* `localStorage` (~5–10MB) persists across browser restarts until cleared. `sessionStorage` (~5MB) lasts only for the current tab/session. Cookies (~4KB) are sent automatically with HTTP requests to the server and support expiry dates and flags like `HttpOnly`, `Secure`, and `SameSite`.

**9. Explain the CSS box model and the difference between `content-box` and `border-box`.**

* Every HTML element is a rectangular box made of content, padding, border, and margin. With `content-box` (default), `width` and `height` apply only to content, so padding and border add to total rendered size. With `border-box`, padding and border are included inside the declared `width` and `height`, making layouts predictable.

**10. When would you use Flexbox vs. CSS Grid for a layout?**

* Use **Flexbox** for one-dimensional layouts (aligning items along a single row or column, like navbars or button groups). Use **CSS Grid** for two-dimensional layouts where you need simultaneous control over rows and columns (page skeletons, card matrices, dashboard grids).

**11. What is CSS specificity, and how do classes, IDs, inline styles, and `!important` interact?**

* Specificity determines which CSS rule wins when multiple rules target the same element. Weight order from lowest to highest: element/pseudo-element selectors `(0,0,1)` < classes/attributes/pseudo-classes `(0,1,0)` < IDs `(1,0,0)` < inline styles `(1,0,0,0)`. `!important` overrides normal cascade rules regardless of specificity, unless beaten by another `!important` rule with higher specificity.

**12. How does the Virtual DOM work in React, and why does React use `key` props in lists?**

* The Virtual DOM is an in-memory UI representation. On state changes, React builds a new Virtual DOM tree, diffs it against the previous tree (reconciliation), and batches minimal updates to the real DOM. `key` props uniquely identify list siblings across renders so React reorders or updates items accurately instead of recreating them or mixing up component state.

**13. What is the difference between controlled and uncontrolled components in React?**

* In a **controlled** component, form input values are driven by React state via `value` and `onChange`. In an **uncontrolled** component, the DOM maintains its own internal state, accessed via `ref` when needed (e.g., on form submit). Controlled inputs simplify instant validation; uncontrolled inputs reduce boilerplate for simple forms.

**14. Explain how `useState` and `useEffect` work, including common pitfalls with dependency arrays.**

* `useState` preserves reactive state across renders and triggers re-renders on updates. `useEffect` runs side effects (data fetching, subscriptions, DOM sync) after render. Common pitfalls include omitting dependencies (causing stale closures), passing newly created objects/functions in dependencies (causing infinite loops), and forgetting cleanup functions for timers or listeners.

**15. What is semantic HTML, and why does it matter for accessibility and SEO?**

* Semantic HTML uses meaningful tags (`<header>`, `<nav>`, `<main>`, `<article>`, `<button>`) rather than generic `<div>`s. Screen readers and keyboard navigation rely on semantic elements for built-in roles and landmarks, while search engines use them to understand page structure and content hierarchy.

---

### Part 2: Experienced Frontend Questions & Answers (16–30)

**16. How do you prevent unnecessary re-renders in React, and when does memoization (`React.memo`, `useMemo`, `useCallback`) actually hurt?**

* First, fix state placement: colocate state closer to where it is used, split contexts, or pass components as `children` so parent state updates do not re-render unaffected subtrees. Use `React.memo`, `useMemo`, and `useCallback` only when profiling shows expensive renders or when preserving referential equality for memoized children. Overusing memoization adds memory overhead, extra prop-comparison work on every render, and harder-to-read code—and breaks silently if a single unstable prop slips in.

**17. Compare Server-Side Rendering (SSR), Static Site Generation (SSG), Incremental Static Regeneration (ISR), and Client-Side Rendering (CSR) in Next.js.**

* **SSR:** Renders fresh HTML per request on the server—best for personalized or real-time pages needing SEO.
* **SSG:** Builds HTML once at build time—fastest CDN delivery for static docs/marketing pages.
* **ISR:** Serves cached static pages and regenerates them in the background after a revalidation interval—ideal for catalogs/blogs that update periodically.
* **CSR:** Renders in the browser after JS loads—good for authenticated dashboards where SEO is irrelevant.

**18. How do you design state management across local UI state, shared client state, and server cache (e.g., TanStack Query / SWR)?**

* Keep ephemeral UI state (modals, input drafts, toggles) local with `useState`/`useReducer`. Put server-derived data in a dedicated server-state library (TanStack Query/SWR) for caching, deduplication, background refetching, and optimistic updates rather than duplicating API responses in Redux/Zustand. Reserve global client stores (Zustand/Context) strictly for cross-app client state like theme, auth session metadata, or multi-step workspace state.

**19. Explain TypeScript generics, union vs. intersection types, and how you type polymorphic or complex component props.**

* **Generics (`<T>`)** let functions and components preserve caller-specific types without losing safety (e.g., a typed `<Select<T>>` returning the exact option type in `onChange`). **Unions (`A | B`)** model mutually exclusive states (discriminated unions like `{ status: 'loading' } | { status: 'success'; data: User }`), whereas **intersections (`A & B`)** combine multiple contracts. For polymorphic components (`as` prop), generic constraints (`<T React.ElementType="button" extends>`) merge custom props with `React.ComponentPropsWithoutRef<T>`.

**20. How do you debug and fix Core Web Vitals issues like LCP, INP, and CLS?**

* **LCP (Largest Contentful Paint):** Preload hero images, serve modern formats (WebP/AVIF), avoid lazy-loading above-the-fold images, and minimize render-blocking CSS/JS.
* **INP (Interaction to Next Paint):** Break up long main-thread tasks, debounce heavy handlers, defer non-urgent updates with `startTransition`, and reduce large DOM trees.
* **CLS (Cumulative Layout Shift):** Always set explicit `width`/`height` or `aspect-ratio` on images/embeds, reserve space for dynamic banners/ads, and use `font-display: swap` with matched fallback metrics.

**21. How does browser rendering work from receiving HTML/CSS/JS to painting frames—and what triggers layout thrashing?**

* The browser parses HTML into the DOM and CSS into the CSSOM, merges them into the Render Tree, calculates geometry (**Layout/Reflow**), fills pixels into layers (**Paint**), and combines layers on the GPU (**Composite**). **Layout thrashing** happens when JS repeatedly reads layout properties (`offsetHeight`, `getBoundingClientRect()`) immediately after mutating styles in a loop, forcing synchronous reflows mid-frame. Batch DOM reads before DOM writes and animate GPU-friendly properties (`transform`, `opacity`).

**22. When would you use WebSockets vs. Server-Sent Events (SSE) vs. short/long polling for real-time updates?**

* **WebSockets:** Full-duplex, low-latency bidirectional communication—best for live chat, multiplayer collaboration, or real-time voice/interactive sessions.
* **SSE:** Unidirectional server-to-client streaming over standard HTTP with automatic reconnection—ideal for LLM token streaming, live feeds, or notifications.
* **Polling:** Simpler fallback when updates are infrequent (every 30–60s) or infrastructure restricts persistent connections.

**23. How do you handle race conditions, stale closures, and request cancellation (`AbortController`) in async UI flows?**

* Pass an `AbortController` signal into `fetch` inside `useEffect` and call `controller.abort()` in the cleanup function so outdated requests cancel on rapid filter/route changes. Guard state updates with request IDs or query libraries that ignore stale responses, and ensure effect dependencies or refs stay current so callbacks never read outdated state.

**24. What security risks do frontend apps face (XSS, CSRF, token storage risks, clickjacking), and how do you mitigate them?**

* **XSS:** Never inject unsanitized HTML (`dangerouslySetInnerHTML` without DOMPurify), escape user content, and enforce a strict Content Security Policy (CSP).
* **Token theft / CSRF:** Store session tokens in `HttpOnly`, `Secure`, `SameSite=Lax/Strict` cookies rather than `localStorage`, and validate anti-CSRF tokens on state-changing requests.
* **Clickjacking:** Set `X-Frame-Options: DENY` or CSP `frame-ancestors 'none'`.

**25. How do you architect a scalable design system or shared component library without over-engineering variants?**

* Build on accessible primitives (Radix UI / React Aria) wrapped with design tokens (CSS variables for colors, spacing, typography) for consistent theming. Keep component APIs composable (compound components or `children` slots) rather than adding dozens of boolean props for every layout edge case. Enforce visual regression tests, clear prop contracts, and strict bundle tree-shaking.

**26. How do you handle large datasets or long lists in the browser (virtualization, pagination, debouncing, caching)?**

* Render only visible viewport rows using list virtualization (`@tanstack/react-virtual` / `react-window`) so DOM node count stays flat regardless of dataset size. Pair server-side cursor pagination or infinite scroll with debounced search inputs (250–300ms) and client-side query caching so revisiting previous filters is instant.

**27. Explain error handling strategies at the component, route, and global levels (Error Boundaries, fallback states, graceful degradation).**

* Wrap isolated widgets (charts, sidebars) in React **Error Boundaries** so one crashing widget degrades gracefully without blanking the whole page. Place route-level boundaries/error pages for navigation failures, handle async API failures explicitly with retryable empty/error UI states, and attach global handlers (`window.onerror`, `unhandledrejection`) wired to telemetry (e.g., Sentry).

**28. How do you optimize bundle size, code splitting, and third-party script impact in production?**

* Inspect bundles with Webpack/Vite Bundle Analyzer, replace heavy libraries with lightweight alternatives or native APIs, and ensure barrel files do not block tree-shaking. Split heavy routes and modals using `React.lazy` / `next/dynamic`, and load analytics/chat widgets asynchronously (`afterInteractive` / web workers) so they do not block hydration or INP.

**29. What is your approach to testing frontend apps—unit tests, integration/component tests, and end-to-end tests—and where do you draw the line?**

* Follow the testing trophy:
* **Unit tests:** Pure utilities, formatters, complex reducers, and validation logic.
* **Integration/component tests (React Testing Library):** Test user-visible behavior (rendering, form submission, mocked API states) rather than internal state or implementation details.
* **E2E tests (Playwright):** Reserve for critical revenue/user flows (login, checkout, core workflow) to keep CI fast and flake-free.



**30. Walk through how you diagnose a production performance drop or memory leak in a single-page or Next.js application.**

* Reproduce locally with production build + CPU/network throttling. Use Chrome DevTools **Performance** tab to spot long main-thread tasks, excessive React commits (React DevTools Profiler), or layout shifts. For memory leaks, take heap snapshots before and after repeating a user flow (e.g., opening/closing a view) and inspect retained detached DOM nodes, uncleaned `setInterval`s, WebSocket/event listeners, or unbounded caches.

---

Want to turn these into a mock interview round where I ask you 5 questions one by one and critique your answers?
