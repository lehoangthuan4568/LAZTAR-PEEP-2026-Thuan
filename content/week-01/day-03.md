+++
title = "Day 03 - 17/09/2026 (Remote)"
weight = 3
+++

# PRACTICE REPORT — DAY 03

## 1. Objectives
* Initialize and configure a Next.js 16 project integrated with React 19 and Tailwind CSS.
* Develop a personal portfolio interface following an Editorial design aesthetic, emphasizing User Experience (UX) and User Interface (UI) quality.
* Implement Responsive Design principles to ensure cross-device compatibility.
* Utilize Git and GitHub for source code management adhering to team collaboration standards.
* Deploy the application to the Vercel platform.

## 2. Implementation Process
* **Project Setup:** Utilized `create-next-app` to establish a standard directory structure. Integrated global CSS variables to maintain a consistent color palette and typography system.
* **UI Development:**
  * Applied Grid Layout and Flexbox to structure components logically and systematically.
  * Incorporated a paper grain texture and Serif typography (Playfair Display) to enhance visual depth and aesthetic identity.
  * Modularized the codebase into independent React Components (`Hero`, `Projects`, `Now`, `Contact`, `Footer`) to maximize code reusability.
* **Micro-interactions Optimization:** Implemented smooth scroll-triggered animations utilizing the `Intersection Observer API` and dynamic hover states via CSS Transitions.
* **Deployment Phase:** Linked the GitHub repository with Vercel, establishing a Continuous Integration/Continuous Deployment (CI/CD) pipeline for automated deployment.

## 3. Key Learnings
* **Next.js Architecture (App Router):** Gained proficiency in directory organization, routing mechanisms, and the rendering paradigms (Server vs. Client Components) within modern Next.js.
* **Responsive Web Design:** Mastered the application of Tailwind CSS utility classes (`sm:`, `md:`, `lg:`) to dynamically adjust layouts, typography, and spacing across various screen resolutions.
* **Git Workflow Management:** Effectively executed branching strategies (`feature/trang-ca-nhan`), maintaining a clean, linear commit history following Conventional Commits standards.
* **Cloud Deployment Workflow:** Acquired an understanding of PaaS (Platform as a Service) operations on Vercel, encompassing repository authorization, build processes, and static asset distribution.

## 4. Challenges & Resolutions
* **Challenge 1: Code duplication and maintenance overhead**
  * *Issue:* Initially, hardcoding static content directly within UI components resulted in a bloated and rigid codebase.
  * *Resolution:* Abstracted content into a standalone configuration file (`data/profile.ts`). Implemented a Data-driven approach to render UI dynamically using the `Array.map()` method.
* **Challenge 2: Efficient scroll state detection**
  * *Issue:* Required dynamic rendering of elements based on scroll position (e.g., blurred navigation bar, back-to-top button) without degrading performance.
  * *Resolution:* Leveraged the `useEffect` hook combined with optimized Event Listeners (`passive: true`) and the `Intersection Observer API` to minimize DOM manipulation overhead compared to continuous scroll event tracking.

## 5. Project Links
- **Live Deployment (Vercel):** [Lê Hoàng Thuận](https://laztar-portfolio.vercel.app/)
- **GitHub Repository:** [lehoangthuan4568/laztar-portfolio](https://github.com/lehoangthuan4568/laztar-portfolio)
