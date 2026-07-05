# Research & Decision Log — Revo Pharmacy Management System

This document captures the architectural decisions, tech stack validation, and integration patterns chosen for the Revo system.

---

## 1. Core Architectural Decisions

### Frontend Framework: React + TypeScript + Vite
- **Decision**: Use React with TypeScript, scaffolded using Vite.
- **Rationale**: 
  - React offers a mature ecosystem for single-page applications (SPA).
  - TypeScript provides strong typing, which is critical for complex domain logic like unit conversion (Box/Strip/Piece) and co-pay calculations.
  - Vite offers extremely fast build times and Hot Module Replacement (HMR), ensuring instant feedback during local development.
- **Alternatives Considered**: Next.js (rejected because Revo is a desktop-first app; server-side rendering is unnecessary and adds complexity for offline desktop deployment).

### Backend: Supabase (PostgreSQL + Auth + Storage)
- **Decision**: Use Supabase as the backend-as-a-service.
- **Rationale**:
  - PostgreSQL is a highly reliable relational database that natively supports transactions, constraints, and JSON querying.
  - Supabase Auth provides secure, out-of-the-box user management.
  - Row-Level Security (RLS) policies allow declarative security directly at the database layer.
  - Natively supports offline sync via client-side libraries or local pg-sync configurations in the future.
- **Alternatives Considered**: Custom Node.js/Express API with manual PostgreSQL setup (rejected to speed up development and focus on the UI/UX first).

### Desktop Wrapper: Electron
- **Decision**: Wrap the web application using Electron.
- **Rationale**:
  - Enables full access to native Windows APIs (direct thermal printing, USB barcode scanner hardware integration).
  - Allows packaging the app as a standalone Windows executable (.exe).
  - Supports local database storage fallback when internet is down.
- **Alternatives Considered**: Tauri (rejected due to the need for simple, mature node-level integrations for thermal receipt printing and hardware device access).

---

## 2. Integration & Best Practices

### Barcode Scanning (ZXing)
- ZXing will be integrated to handle barcode decoding from device inputs or camera interfaces. For standard USB hand scanners acting as keyboard inputs, a global keyboard event listener will debounce and capture barcode prefixes.

### Printing (react-to-print + Electron)
- The app will render a hidden print layout component in React. `react-to-print` will trigger the print job. Under Electron, the print dialog can be bypassed (`silent: true`) to immediately output receipts to the default thermal printer, satisfying the KPI of `< 30s` transaction time.

### Offline State & Local Database Fallback
- Local caching of critical lookup tables (products, prices, customers) will be maintained in the frontend state (Redux). 
- When offline:
  - POS sales will be written to a local queue (IndexedDB/RxDB).
  - Synchronization logic will push queued sales to Supabase once connectivity is restored.
