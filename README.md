# Institute of Knowledge — README

One-line summary
A responsive, PWA-capable React frontend for managing an educational institute (students, teachers, classes, attendance, fees, batches). Intended for administrators, teachers, and students; connects to a backend API for authentication and data persistence.

Badges / status
- Framework: Vite + React (PWA)
- Primary language: JavaScript
- Build: `vite build`
- Dev server: `vite --port 3000`

Table of contents
- What this is
- Stack
- System requirements
- Repository layout (high-level)
- Architecture & design
- Installation (developer) — quick start
- Environment variables & configuration
- Running (dev / build / preview)
- Frontend → Backend integration (API notes)
- Features & user flows
- Testing strategy & suggestions
- Maintenance & developer workflow
- Security best practices
- Troubleshooting
- Contributing / changelog / contact

---

## What this is
A single-page React application (PWA-ready) that provides role-based interfaces for Admins, Teachers and Students to manage classes, batches, attendance, fees and profiles. The UI uses client-side routing, local stores for state, and integrates with a RESTful backend over JWT-secured endpoints.

### Stack
- Language(s): JavaScript (React)
- Framework / runtime: Vite + React (React 19) with client-side routing (react-router-dom)
- Notable libraries:
  - axios — HTTP client for API calls
  - zustand — lightweight global state management
  - tailwindcss — utility-first CSS
  - framer-motion — UI animations
  - react-hot-toast — notifications
  - vite-plugin-pwa — PWA support
  - @react-google-maps/api — Google Maps integration

---

## System requirements
- Node.js LTS (recommended >= 18.x). Vite works with modern Node versions.
- npm >= 9 or yarn/bun compatible with the project dependencies
- Browser: Modern evergreen browsers (Chrome, Firefox, Edge, Safari) with service worker support for PWA features
- Optional: Vercel (or another static host) for deployment; backend API reachable over HTTPS
- Minimum memory: 2GB for development on typical machines

---

## How it's organized
Top-level overview (important files and directories):

src/
  App.jsx                 — application entry (routes, auth gating, layout)
  main.jsx                — client bootstrap and PWA service-worker registration
  index.css               — global/tailwind css
  pages/                  — route pages (LandingPage, Dashboard, Admin pages, Student/Teacher pages)
  components/             — UI components (LoginModal, NavigationLayout, ID card, etc.)
  stores/                 — zustand stores (useAuthStore, useLoginStore, useClassStore, etc.)
  api/                    — API wrappers / axios setup (interceptors, base client)
  util/                   — helper utilities
  constants/              — app-wide constants
  assets/                 — static assets (images/fonts)

Other files:
- package.json            — scripts and dependency list
- vite.config.js          — Vite configuration (plugins, aliases)
- vercel.json             — Vercel deployment hints
- bun.lock / package-lock.json — lock files (several formats present)
- BACKEND_API_DOCUMENTATION.md — backend endpoints and schema guide
- BACKEND_API_REFERENCE.md — more detailed backend reference
- CREATECLASS_IMPLEMENTATION_GUIDE.md — feature-specific implementation notes
- DUMMY_DATA.json & DUMMY_DATA_USAGE_GUIDE.md — sample data and usage

How it fits together
- main.jsx boots the React app and registers the service worker (PWA).
- App.jsx mounts routes and applies protected routing wrappers (role-based).
- Stores (zustand) manage session, UI state and provide API call methods (e.g., useClassStore for class-related endpoints).
- Components and pages render UI and call store methods to fetch or mutate backend data using axios.

---

## Design & architecture details

High-level responsibilities
- Presentation: React components and pages under src/components and src/pages.
- State management: local component state + central stores (zustand) in src/stores for auth, UI and domain data.
- Networking: axios-based API client (interceptor handles JWT token inclusion). Backend base URL is provided by VITE_BACKEND_URL.
- Routing & access control: react-router-dom + ProtectedRoute and ProtectedRouteRoleBased for RBAC.
- Offline & PWA: vite-plugin-pwa + registerSW (service worker) to cache assets and support offline usage.

Core modules & flows
- Authentication store (src/stores/useAuthStore): loads user on app start, holds token and role.
- UI store (src/stores/useUiStateStore): dark mode, modals state, layout preferences.
- Class-related store (src/stores/useClassStore): CRUD operations for classes and features (frontend uses API endpoints described in BACKEND_API_DOCUMENTATION.md).
- ProtectedRoute wrappers: check user authentication & role before rendering pages.

Data flow (example)
1. User navigates to a protected route -> ProtectedRoute checks auth store.
2. If authorized, page component mounts and calls a store method (e.g., getClasses).
3. Store uses axios to call backend at VITE_BACKEND_URL + /api/...
4. Response updates store -> components re-render.

Notes on API contract
- Backend endpoints and expected payloads are documented in BACKEND_API_DOCUMENTATION.md and BACKEND_API_REFERENCE.md. Dates are ISO-8601 and responses use a consistent { success, message, ... } envelope.
- All API calls require JWT Bearer tokens in Authorization header.

---

## Installation — developer quick start

1. Clone
```bash
git clone https://github.com/ronit5036m/Institute-of-Knowledge.git
cd Institute-of-Knowledge
