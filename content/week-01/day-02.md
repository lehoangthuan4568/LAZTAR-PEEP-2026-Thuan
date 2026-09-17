+++
title = "Day 02 - 16/09/2026 (Remote)"
weight = 2
+++

# PRACTICE REPORT — DAY 02

## A. THEORETICAL FOUNDATION

### Part 1. React Fundamentals

**1. What is React?**
React is an open-source JavaScript library developed by Facebook, primarily used for building user interfaces (UI), especially for single-page applications (SPAs). It operates on a unidirectional data flow and utilizes a Virtual DOM to optimize rendering performance.

**2. What is a Component in React? What are the types of components?**
Components are the fundamental building blocks in React, allowing the UI to be split into independent, reusable pieces.
There are two main types:
- **Class Components:** ES6 classes extending `React.Component`, capable of managing state and lifecycle methods.
- **Functional Components:** Simple JavaScript functions that accept props and return React elements. Since React 16.8 (Hooks), Functional Components have become the standard due to their conciseness and efficiency.

**3. What is JSX?**
JSX (JavaScript XML) is a syntax extension for JavaScript that allows writing HTML directly within JavaScript files. It improves code readability and is compiled into `React.createElement()` calls under the hood by Babel.

**4. What are Props?**
Props (Properties) are the mechanism for passing data from a parent component down to a child component. Props are strictly read-only; a child component cannot modify the props it receives.

**5. What is State? How does it differ from Props?**
- **State** is the internal data of a component that can change over time through user interactions or system responses. When state changes, the component automatically re-renders.
- **Difference:** Props are passed externally (parent to child) and cannot be modified by the receiving component, whereas State is managed locally within the component and can be modified by the component itself.

**6. What is the Virtual DOM? Why does React use it?**
The Virtual DOM is a lightweight in-memory representation of the Real DOM.
React uses it to optimize the UI update process. When state or props change, React creates a new Virtual DOM, compares it (Diffing) with the previous one to identify differences, and only updates (Patching) the actual changed parts in the Real DOM, minimizing expensive browser reflows.

**7. What are Hooks? Name some common Hooks in React.**
Hooks are special functions introduced in React 16.8 that allow Functional Components to use state and other React features (like lifecycle) without writing Classes.
Common Hooks: `useState`, `useEffect`, `useContext`, `useRef`, `useMemo`, `useCallback`.

**8. What is `useState` used for?**
It is used to declare and manage local state in a Functional Component. It returns an array with two elements: the current state value and a function to update it.

**9. What is `useEffect` used for?**
It is used to perform side effects in Functional Components, such as data fetching, setting up subscriptions, or manually manipulating the DOM. It replaces lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

**10. What are the phases of a React Component's Lifecycle?**
- **Mounting:** When the component is created and inserted into the DOM.
- **Updating:** When the component re-renders due to changes in state or props.
- **Unmounting:** When the component is removed from the DOM.

**11. What is Client-Side Rendering (CSR)?**
CSR is the rendering technique where the UI is rendered on the client side (browser). Initially, the server sends an empty HTML page along with JavaScript files. The UI is fully drawn only after the browser downloads and executes the JavaScript.

**12. What is React Router?**
React Router is the standard library for managing routing in React applications. It allows navigation between different views/pages within a SPA without requiring a full page reload.

**13. Does raw React support Routing, SEO, and API Server?**
No. Raw React is solely a UI library. To achieve Routing (requires `react-router-dom`), good SEO (requires SSR/SSG), and API Server capabilities (requires a separate backend), developers must combine it with other libraries or frameworks (like Next.js).

**14. What is the Context API? When should it be used?**
The Context API is a mechanism that allows passing data deeply through the component tree without manually passing props at every level (avoiding "props drilling").
It should be used for global data shared by multiple components at various levels, e.g., theme (dark/light), authenticated user data, or localization.

**15. What is a SPA (Single Page Application)?**
A web application containing only a single HTML page. Instead of reloading the entire page from the server upon navigation, a SPA fetches data (usually via API) and uses JavaScript to dynamically re-render necessary UI components, offering a smooth, desktop-like experience.


### Part 2. Comparing React and Next.js

**1. What is Next.js?**
Next.js is an open-source React framework developed by Vercel. It provides architecture and optimization tools (like SSR, SSG, integrated Routing) to build production-ready React web applications.

**2. What is the core difference between React and Next.js?**
- React is a library focused entirely on building the UI (CSR).
- Next.js is a framework wrapped around React, offering comprehensive solutions including Server-Side Rendering (SSR), File-based routing, SEO optimization, and API Routes.

**3. How does routing differ between React and Next.js?**
- React: Requires manual configuration via code using third-party libraries (`react-router-dom`).
- Next.js: Utilizes File-based Routing. Routes are automatically generated based on the file structure (in `pages` or `app` directories).

**4. How does rendering differ between React and Next.js?**
- React: Defaults to Client-Side Rendering (CSR).
- Next.js: Supports versatile rendering strategies: Server-Side Rendering (SSR), Static Site Generation (SSG), Incremental Static Regeneration (ISR), and CSR.

**5. Why does Next.js support SEO better than raw React?**
Because Next.js can render complete HTML directly from the Server (SSR/SSG). When search engine bots (like Googlebot) crawl the site, they receive fully populated HTML files to index, contrasting with React's CSR (which returns an empty HTML file waiting for JS execution).

**6. How does First Load performance differ between React and Next.js?**
Next.js generally has a significantly faster First Load because the server returns pre-rendered HTML, allowing users to see content immediately while waiting for JS to load and hydrate. React requires users to wait for JS to download and execute before any UI is visible.

**7. How does the project structure differ between React and Next.js?**
React projects (like Create React App or Vite) often have a flexible `src` directory. Next.js enforces specific structural conventions to automate framework features, particularly the `app` (App Router) or `pages` directories for routing.

**8. Does Next.js replace React? Why?**
No. Next.js is built *on top of* React. Next.js depends on React to handle UI management (Components, State, Hooks). It simply augments raw React with architectural features it natively lacks.

**9. When should you use raw React and when should you use Next.js?**
- **Raw React:** For internal applications (Dashboards, Admin panels) where SEO is not a priority and client-side interactivity is high.
- **Next.js:** For public-facing websites requiring strong SEO (E-commerce, Landing pages, Blogs) and optimized page load speeds (Core Web Vitals).


### Part 3. Next.js Deep Dive

**1. What are App Router and Pages Router in Next.js?**
- **Pages Router:** The legacy routing model (pre-Next 13), based on the `pages` directory.
- **App Router:** The modern routing model (since Next 13), supporting Server Components by default, nested layouts, and advanced data fetching, based on the `app` directory.

**2. What is the difference between Server Components and Client Components?**
- **Server Component:** Executes and renders exclusively on the Server. Cannot use Hooks (`useState`, `useEffect`) or event listeners (`onClick`). Reduces the JS bundle size sent to the client.
- **Client Component:** Executes on the Client (or hydrates on the client). Can utilize Hooks and UI interactivity. In the App Router, must be explicitly declared with `"use client"` at the top of the file.

**3. What is SSR (Server-Side Rendering)?**
The UI is rendered into HTML on the server *for every incoming request*, ensuring the client always receives the most up-to-date data.

**4. What is SSG (Static Site Generation)?**
The UI is pre-rendered into HTML *at build time*. This HTML file is reused for all subsequent requests, resulting in exceptionally fast load times.

**5. What is ISR (Incremental Static Regeneration)?**
A technique allowing static pages (SSG) to be updated in the background after the initial build, based on a defined time interval (e.g., every 60 seconds), ensuring data remains fresh without sacrificing SSG performance.

**6. How does File-based Routing work in Next.js?**
Application routes are mapped directly from the file system tree. For example, the file `app/about/page.tsx` automatically generates the `/about` route path.

**7. What is a Dynamic Route in Next.js?**
Routes that accept dynamic parameters, defined using square brackets `[]`. For example, `app/blog/[id]/page.tsx` will match `/blog/1`, `/blog/2`, etc. The `id` parameter can be extracted within the component.

**8. What is `layout.tsx` used for in the App Router?**
Used to create a shared wrapper UI for one or multiple nested pages (e.g., Headers, Footers, Sidebars), ensuring this UI does not re-render upon navigation.

**9. What are API Routes (Route Handlers) in Next.js?**
A feature allowing the creation of API endpoints (Backend) directly within a Next.js project (typically written in `app/api/route.ts`).

**10. What are `getStaticProps` and `getServerSideProps`? When are they used?**
These are data fetching functions in the legacy **Pages Router** model.
- `getStaticProps`: Fetches data at build-time (for SSG).
- `getServerSideProps`: Fetches data on every request (for SSR).
*(In the modern App Router, these functions have been superseded by the native `fetch()` API).*

**11. How does `next/image` optimize images?**
The `<Image />` component automatically:
- Compresses images and serves modern formats (WebP/AVIF).
- Implements lazy loading (only loads when scrolled into view).
- Automatically handles responsive sizing based on the device.
- Prevents Cumulative Layout Shift (CLS).

**12. What is Middleware in Next.js?**
Code that executes before a request is completed. Commonly used for Authentication, Redirects, or logging before a user accesses a specific page.

**13. How do you navigate between pages in Next.js?**
Utilize the `<Link href="...">` component instead of standard `<a>` tags. This allows Next.js to pre-fetch data and transition seamlessly like a SPA (without full page reloads). Alternatively, use the `useRouter()` hook.

**14. How are Metadata and SEO handled in Next.js?**
In the App Router, manage meta tags (title, description, open graph) flexibly using the exported `metadata` object or the `generateMetadata()` function within `layout.tsx` or `page.tsx` files.

**15. Does Next.js support TypeScript?**
Yes, it offers excellent built-in support. Next.js automatically configures `tsconfig.json` upon initialization and provides strict typings throughout the framework.

**16. On which platforms can a Next.js project be deployed?**
Optimally on Vercel (the creators of Next.js). It can also be deployed on AWS, Google Cloud, DigitalOcean, Netlify, or self-hosted via Docker containers.

---

## B. PRACTICAL APPLICATION: LAZTAR CORPORATE LANDING PAGE

### 1. Overview
Practiced building a corporate Landing Page for LAZTAR, emphasizing high professionalism using Next.js (App Router), Tailwind CSS, and Framer Motion.

### 2. Key Learnings
- **Enterprise UI/UX Architecture:** Applied the **Swiss Minimalist & Editorial** design pattern. Utilized structured Bento grids and removed excessive stylistic elements (glowing orbs, heavy gradients) to establish a premium, trustworthy corporate identity.
- **Advanced Micro-interactions:** Integrated `framer-motion` to handle sophisticated scroll-triggered stagger animations and refined hover states, significantly elevating the UX beyond standard CSS transitions.
- **Static Asset Management:** Integrated a standardized icon suite (Lucide React) replacing static SVGs for scalable sizing and dynamic coloring. Configured `next.config.ts` to authorize external image domains (Unsplash).

### 3. Challenges Encountered
- **Inconsistent Design Language and Brand Misalignment:** Early iterations of the interface suffered from an overly complex aesthetic, characterized by unnecessary neon gradients and excessive border-radius. This approach undermined the serious, trustworthy tone required for a B2B corporate landing page and lacked professional cohesion.
- **Icon Library Compatibility Issues:** Encountered build errors when importing brand icons (Facebook, LinkedIn) from `lucide-react` due to their removal in the latest library version.

### 4. Resolutions
- **Implemented Minimalist Design Principles:** Conducted independent research into enterprise UI/UX patterns and Minimalist design to overhaul the visual hierarchy. Standardized the color palette (strict Navy/White contrast), integrated the Playfair Display font to elevate typographic formality, and restructured convoluted UI components into clean, professional grid systems.
- **Localized SVG Asset Management:** Addressed the `lucide-react` error by directly embedding raw SVG source code for social media icons into the Footer component. This approach ensures long-term stability and eliminates unnecessary dependencies on third-party library updates.
