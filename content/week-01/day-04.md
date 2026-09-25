+++
title = "Day 04 - 18/09/2026 (Office)"
weight = 4
+++

# PRACTICE REPORT — DAY 04

## 1. Objectives
* Refactor and elevate the personal portfolio UI to a "Premium Utilitarian Minimalism" standard.
* Integrate `framer-motion` (via `motion/react`) to replace manual intersection observers with GPU-accelerated stagger reveals.
* Improve typographic hierarchy and macro-whitespace (editorial spacing).
* Replace generic emojis with professional vector icons (`lucide-react`).

## 2. Implementation Details

### A. UI/UX Refactoring
- **Anti-Slop Design:** Removed generic UI trends such as heavy drop shadows, pill-shaped borders (`rounded-full`), and oversaturated gradient text. 
- **Bento Grid:** Redesigned the Skills section into a sharp, asymmetric bento box layout with 1px subtle borders.
- **Micro-interactions:** Applied a subtle physical scale effect (`scale: 0.98`) to buttons and cards instead of excessive hover shadows.

### B. Motion & Performance
- Replaced the custom `useScrollReveal` hook with `motion.div` and `whileInView` for better performance and synchronized staggering.
- Ensured animations rely only on `transform` and `opacity` properties for hardware acceleration.

## 3. Results
- Successfully applied the new aesthetic guidelines while maintaining the existing Warm Cream color palette.
- Committed and pushed all changes to the `feature/trang-ca-nhan` branch on GitHub.
- Deployed automatically via Vercel.

## 4. References
- [Framer Motion Documentation](https://motion.dev/)
- [Lucide Icons](https://lucide.dev/)
