# TECHNICAL SPECIFICATION & REPORT: VUE.JS AUTHENTICATION MODULE

**Project Title:** Modular Enterprise User Authentication System  
**Author:** Sarita Mangar  
**Stack:** Vue 3 (Composition API, `<script setup>`), Pinia, Vue Router 4, TypeScript, Tailwind CSS  
**Submission Artifact:** Week 2 Internship Task Submission

---

## 1. Executive Summary & Feature Scope

This engineering document presents the technical design, implementation details, and operational workflow of a client-side authentication module built with Vue 3 and TypeScript. Designed to satisfy enterprise security requirements and seamless user navigation, this application implements full user session lifecycles, state persistence, route-level access guards, and client-side form validation.

### Key Functionalities Delivered

- **User Registration:** Interactive signup interface collecting user credentials with dynamic validation rules.
- **User Login:** Authentication interface featuring credential verification, loading state indicators, and reactive error feedback.
- **Session State Persistence:** Pinia store integration synchronized with browser `localStorage` to preserve user authentication states across browser page reloads.
- **Navigation Guard Security:** Router middleware blocking unauthorized access to protected routes (`/dashboard`) while automatically redirecting authenticated users away from public auth pages (`/login`, `/register`).
- **User Logout:** Secure session termination with local state purging and token invalidation.

---

## 2. System Architecture & Visual Flow Diagram

### Authentication & Router Security Workflow

```text
[ User Enters Application URL ]
               |
               v
     +-------------------+
     |   Vue Router 4    |
     +-------------------+
               |
    Matches Route Metadata?
   /                       \
(requiresAuth)           (requiresGuest)
  /                         \
 v                           v
+-------------------+      +-------------------+
| Is Authenticated? |      | Is Authenticated? |
| (Check Pinia)     |      | (Check Pinia)     |
+-------------------+      +-------------------+
   /             \            /             \
(Yes)            (No)      (Yes)            (No)
 /                 \        /                 \
v                   v      v                   v
[ Render Dashboard ] [ Redirect ] [ Redirect ] [ Render Auth ]
                     [ to Login ] [ to Dash  ] [ Component   ]
```

## 3. Deep-Dive Design Decisions & Justifications
  ### A. State Management Architecture: Pinia Setup Stores
  * Choice Justification: Pinia is the standard state management library for modern Vue 3 applications. Implementing the Setup Store Syntax (defineStore('auth', () => { ... })) aligns directly with Vue 3 Composition API idioms (ref, computed).

  * Security & Persistence: State persistence is ensured by reading existing local storage tokens directly upon store initialization. Session updates (login, logout) execute atomic sync operations across memory state and browser storage.

  ### B. Navigation & Security Architecture
  * Route Metadata Protection: Target application routes are tagged with explicit metadata flags (meta: { requiresAuth: true } or meta: { requiresGuest: true }).

  * Global BeforeEach Guard: Route transitions are strictly intercepted prior to component mounting. Unauthenticated access attempts to protected routes store the target route path in URL parameters, redirecting users directly back to their destination upon successful sign-in.

  ### C. Client-Side Input Validation & Error Management
  * Regex Validation: Form submit handlers execute regex checks prior to dispatching state actions, validating standard email formats and minimum password character limits.

  * Reactive UI Feedback: Validation error messages dynamically bind to field components, providing visual error states (red borders and error labels) without triggering page refreshes.

## 4. Setup Instructions & Project Execution Guide
  ### Prerequisites
   * Node.js: v18.0.0 or higher

   * npm: v9.0.0 or higher

### Step-by-Step Installation
* Extract Project Files:

* Unzip Sarita_Mangar_Week2_Auth_Task.zip to your local environment.

* Install Project Dependencies:

Open a terminal inside the project root folder and execute:
Bash
npm install

* Start the Local Development Server:

Launch the Vite development engine:
Bash
npm run dev

* Access the Application:

Open your web browser and navigate to http://localhost:5173.

### Default Testing Credentials
* Email: admin@fleet.com
* Password: Password123!

## 5. Submission Checklist & Package Artifacts

| Deliverable Requirement    | File Location / Artifact                                | Status           |
| :------------------------- | :------------------------------------------------------ | :--------------- |
| **Login Component**        | `src/modules/auth/views/LoginView.vue`                  | Fully Functional |
| **Register Component**     | `src/modules/auth/views/RegisterView.vue`               | Fully Functional |
| **Dashboard Component**    | `src/modules/auth/views/DashboardView.vue`              | Fully Functional |
| **Pinia State Store**      | `src/modules/auth/stores/useAuthStore.ts`               | Fully Functional |
| **Route Guards**           | `src/router/index.ts`                                   | Fully Functional |
| **Documentation & Report** | `README.md` & `VueJS_Authentication_Module_Report.docx` | Included         |
```
