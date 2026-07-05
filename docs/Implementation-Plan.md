# Revo Pharmacy Management System — Implementation Plan

**Version**: 1.1
**Date**: 2026-07-05
**Status**: Active

---

## Team Roles

| Role | Responsibilities |
|------|-----------------|
| Product Manager + System Analyst + UX Lead | Requirements, flows, wireframes (PNG sketches), UX decisions |
| UI Designer + Frontend Developer (AI) | UI Specification, frontend code, spec-kit execution |

---

## Tech Stack

### Frontend

| Layer | Technology |
|-------|-----------|
| Framework | React + TypeScript + Vite |
| Styling | Tailwind CSS + shadcn/ui |
| State Management | Redux Toolkit + RTK Query |
| Forms | React Hook Form + Zod |
| Tables | TanStack Table |
| Charts | Recharts |
| Icons | Lucide React |
| Barcode | ZXing |
| Printing | react-to-print |

### Backend & Infrastructure

| Layer | Technology |
|-------|-----------|
| Backend | Supabase |
| Database | PostgreSQL (via Supabase) |
| Authentication | Supabase Auth |
| Storage | Supabase Storage |
| Desktop Wrapper | Electron (Windows) |

### Development Strategy

**Contract-First + Mock Data**

```
PRD
 ↓
API Contract (docs/API.md)
 ↓
Frontend (Mock Data)
 ↓
Connect to Supabase
```

- API contracts are defined **before** frontend development begins.
- Frontend is built against **mock data** (no JSON Server).
- Final step: swap mock data for live Supabase queries.

---

## Notifications (Version 1)

| Type | Trigger | Delivery |
|------|---------|----------|
| In-App | Low stock, near-expiry, operation success/error, backup failure | Always |
| Windows Toast | Low stock, expired product | Electron only (important alerts) |
| Email / SMS | — | ❌ Not in v1 |

---

## Phases Overview

| Phase | Deliverables | Status |
|-------|-------------|--------|
| Phase 1 — Foundation | Constitution ✅, PRD, Database Design | 🔄 In Progress |
| Phase 2 — Design & Frontend | Wireframes (PNG), UI Guidelines, Frontend | ⏳ Pending |
| Phase 3 — Backend & Release | Supabase integration, Testing, Release | ⏳ Pending |

---

## Phase 1 — Foundation

- [x] `docs/Constitution.md` — Project constitution (v1.1.0 ratified)
- [x] `docs/Implementation-Plan.md` — This file
- [ ] `docs/PRD.md` — Product Requirements Document
- [ ] `docs/Database.md` — PostgreSQL schema, tables, indexes, ERD
- [ ] `docs/API.md` — API contract (tables, queries, mutations, responses, validation)

---

## Phase 2 — Design & Frontend

### Per-Screen Workflow

```
PRD (module section)
        ↓
User Flow diagram
        ↓
Wireframe PNG (provided by PM)
        ↓
UI Specification (written by AI)
        ↓
Frontend Implementation (React + TS)
```

### Screen Order

| # | Screen | Purpose |
|---|--------|---------|
| 1 | Dashboard | Sets Design Language for all screens |
| 2 | POS | Core revenue operation |
| 3 | Products | Product catalog management |
| 4 | Purchases | Purchase invoices & returns |
| 5 | Inventory | Stock levels, adjustments, alerts |
| 6 | Customers | Regular, recurring, insurance |
| 7 | Suppliers | Supplier management & balances |
| 8 | Employees | Staff & role management |
| 9 | Reports | Business analytics & exports |
| 10 | Settings | System configuration |

---

## Phase 3 — Backend & Release

- [ ] Connect frontend to Supabase (swap mock → live)
- [ ] `docs/API.md` finalized and validated against implementation
- [ ] Testing: unit, integration, E2E
- [ ] Electron build & packaging for Windows
- [ ] Release preparation

---

## PRD Structure (`docs/PRD.md`)

1. Project Overview
2. Objectives
3. Users
4. User Roles
5. Functional Requirements
6. Non-Functional Requirements
7. System Modules
8. Module Details
9. User Flows
10. Permissions Matrix
11. Reports
12. Notifications
13. MVP Scope
14. Future Scope

---

## Project Directory Structure

```
Revo Pharmacy Management System/
│
├── docs/                          ← Permanent project reference docs
│   ├── Constitution.md            ✅ Done
│   ├── Implementation-Plan.md     ✅ This file
│   ├── PRD.md                     ⏳ Next
│   ├── Database.md                ⏳ Phase 1
│   ├── API.md                     ⏳ Phase 1
│   └── UI-Guidelines.md           ⏳ Phase 2
│
├── specs/                         ← Spec-kit per-feature specs
│   ├── 001-authentication/
│   │   ├── spec.md
│   │   ├── plan.md
│   │   └── tasks.md
│   ├── 002-dashboard/
│   ├── 003-product-management/
│   └── ...
│
├── design/
│   └── wireframes/                ← PNG wireframes from PM
│
├── frontend/                      ⏳ Phase 2
│
├── backend/                       ⏳ Phase 3 (Supabase config, migrations)
│
└── database/                      ⏳ Phase 1 (SQL migrations, seed data)
```

---

## Spec-Kit vs docs/ — Roles

| Tool | Purpose | Audience |
|------|---------|----------|
| `docs/` | Stable reference documentation (PRD, DB, API, UI Guidelines) | Humans, stakeholders |
| `specs/NNN-feature/` | Per-feature spec, plan, tasks — drives AI implementation | AI agent (spec-kit workflow) |

Both are active simultaneously. `docs/` never changes per-feature; `specs/` is feature-scoped and evolves during development.

---

## Notes

- Dashboard is the **first screen** — it establishes the Design Language for the entire system.
- All decisions MUST comply with `docs/Constitution.md`.
- Wireframes are **PNG images** shared in conversation — Pencil files are not part of the repo.
- Frontend is built before Supabase integration to allow UI/UX validation first.
- Conventional commits are required (`feat:`, `fix:`, `docs:`, etc.).
