<!--
SYNC IMPACT REPORT
==================
Version Change:   1.1.0 → 1.2.0
Bump Type:        MINOR — §15 Engineering Constitution expanded with the full,
                  approved technology stack (frontend, backend, desktop, tooling).
                  Development strategy (Contract-First + Mock Data) codified.
                  Notification strategy (In-App + Windows Toast) added.

Modified Sections:
  - §15 Engineering Constitution — concrete tech stack added (React/TS/Vite,
        Tailwind CSS, shadcn/ui, Redux Toolkit + RTK Query, React Hook Form + Zod,
        TanStack Table, Recharts, Lucide React, ZXing, react-to-print,
        Supabase [Auth / DB / Storage], Electron, PostgreSQL)
  - §15 Development Strategy   — Contract-First + Mock Data approach codified
  - §15 Notifications          — In-App + Windows Toast (v1) documented

Added Sections: None
Removed Sections: None

Templates Status:
  ✅ .specify/templates/plan-template.md   — no changes required
  ✅ .specify/templates/spec-template.md   — no changes required
  ✅ .specify/templates/tasks-template.md  — no changes required

Deferred TODOs: None.
-->

# Revo Pharmacy Management System — Project Constitution

**Version**: 1.2.0 | **Ratified**: 2026-07-05 | **Last Amended**: 2026-07-05

> This document is the single source of truth for product, design and
> engineering decisions. All specifications, plans, and implementation work
> MUST comply with this document.

---

## 1. Purpose

This constitution defines the mandatory principles governing the Revo Pharmacy
Management System. Every feature specification, implementation plan, and task
list MUST reference and comply with this document.

---

## 2. Vision

Build a fast, reliable, offline-first pharmacy management system tailored to
Sudan, with a scalable architecture for future expansion into the Middle East
and Africa.

---

## 3. Mission

Enable pharmacy owners and staff to manage sales, purchases, inventory,
finance, and reporting through a simple and efficient platform that requires
minimal training and operates reliably under Sudanese infrastructure conditions.

---

## 4. Product Philosophy

- Simplicity over complexity.
- Speed over unnecessary customization.
- Reliability over feature count.
- Every feature MUST solve a real pharmacy problem.

---

## 5. Core Values

| Value         | Non-Negotiable Rule                                                   |
|---------------|-----------------------------------------------------------------------|
| Simplicity    | Choose the simpler solution. Complexity MUST be explicitly justified. |
| Performance   | Every interaction MUST feel instant. Speed is a core feature.        |
| Reliability   | Data consistency and transaction integrity MUST never be compromised. |
| Consistency   | One unified design language and interaction model across all modules. |
| Scalability   | Architecture MUST support future expansion without major redesign.    |
| Security      | Every operation MUST be traceable; authorization always enforced.     |

---

## 6. Product Goals

The system MUST support the following operational modules:

- Sales & POS
- Inventory
- Purchases
- Suppliers
- Customers & Recurring Customers
- Insurance
- Employees
- Expenses
- Reports
- Settings

---

## 7. Target Market

**Primary**: Sudan

**Future**: Middle East and Africa

---

## 8. Target Users

| Role        | Primary Need                              |
|-------------|-------------------------------------------|
| Owner       | Visibility into business performance      |
| Manager     | Operational control                       |
| Pharmacist  | Fast dispensing workflow                  |
| Cashier     | Rapid checkout                            |
| Accountant  | Accurate financial records                |

---

## 9. Personas

### Owner

Needs real-time visibility into business performance: sales trends, inventory
status, expenses, and employee activity.

### Manager

Needs operational control over daily workflows, staff, stock levels, and
purchase orders.

### Pharmacist

Needs a fast dispensing workflow with barcode scanning, product search, and
clear stock visibility.

### Cashier

Needs rapid checkout: minimal clicks, barcode scanning, receipt printing, and
clear payment confirmation.

### Accountant

Needs accurate, exportable financial records: sales summaries, purchase
histories, expense reports, and debt tracking.

---

## 10. Business Rules

- MUST NOT allow a sale when product stock equals zero.
- MUST support barcode scanning for product lookup.
- MUST support barcode generation and printing.
- MUST support selling by Box, Strip, and Piece with correct unit conversion.
- MUST support purchase returns and sales returns.
- MUST support recurring customers with purchase history.
- MUST support insurance pricing per customer/product configuration.
- MUST record an audit log entry for every significant operation.

---

## 11. Platforms

| Release   | Platforms                       |
|-----------|---------------------------------|
| Version 1 | Windows Desktop, Web            |
| Future    | Android, iOS                    |

---

## 12. Localization

- **Language**: Arabic (RTL) only — Version 1.
- **Currency**: Sudanese Pound (SDG) — Version 1.
- All UI layouts, text alignment, and icon placement MUST correctly render RTL.

---

## 13. Product KPIs

The following performance targets define success at the product level:

- Standard sale operation completed in **< 30 seconds**.
- Product search returns results within a **perceptibly short time**.
- Inventory discrepancy rate reduced relative to manual processes.
- New staff operational with **minimal training** (hours, not days).

---

## 14. Design Constitution

### UI Philosophy

Inspired by modern SaaS dashboards. The interface MUST be:

- **Minimal** — no visual clutter; only essential elements are visible
- **Spacious** — comfortable density with appropriate whitespace
- **Professional** — rounded cards, clean tables, consistent typography
- **Approachable** — sidebar + topbar layout, familiar navigation patterns

The reference style is inspiration only and MUST NOT be copied exactly.

### Layout

- Fixed sidebar navigation
- Top navigation bar
- Card-based content areas
- Responsive desktop-first design

### Components

Every module MUST use the following unified component set:

- Unified buttons (primary, secondary, danger)
- Unified form inputs and validation messages
- Unified data tables with sorting and filtering
- Modal dialogs for confirmations and data entry
- Toast notifications for operation feedback
- Defined Empty, Loading, and Error states for all async operations

### RTL

Every screen MUST fully support RTL layout. This is non-negotiable.

### Keyboard

Frequently used operations MUST support keyboard shortcuts to minimize
mouse dependency during high-throughput workflows (e.g., POS, dispensing).

---

## 15. Engineering Constitution

### Architecture

- **Modular** — features are self-contained modules with clear boundaries.
- **Feature-first** — directory and code organization follows features,
  not technical layers.
- **Offline-first** — core operations MUST function without internet access.

### Approved Technology Stack

#### Frontend

| Concern | Technology |
|---------|----------|
| Framework | React + TypeScript + Vite |
| Styling | Tailwind CSS + shadcn/ui |
| State Management | Redux Toolkit + RTK Query |
| Forms & Validation | React Hook Form + Zod |
| Tables | TanStack Table |
| Charts | Recharts |
| Icons | Lucide React |
| Barcode | ZXing |
| Printing | react-to-print |

#### Backend & Infrastructure

| Concern | Technology |
|---------|----------|
| Backend | Supabase |
| Database | PostgreSQL (via Supabase) |
| Authentication | Supabase Auth |
| File Storage | Supabase Storage |
| Desktop (Windows) | Electron |

No technology outside this approved stack MUST be introduced without an explicit
amendment to this constitution and a documented rationale.

### Development Strategy

**Contract-First + Mock Data**

1. Write `docs/PRD.md` defining all functional requirements.
2. Define API contract in `docs/API.md` (tables, queries, mutations, responses,
   validation) **before** any frontend work begins.
3. Build the frontend against **mock data** — no live Supabase calls.
4. Final integration step: replace mock data with live Supabase queries.

No JSON Server or intermediate mock API server is used.

### Database

- UUID MUST be used as the primary key type.
- Every table MUST include `created_at` and `updated_at` timestamps.
- Soft delete MUST be used where appropriate; hard deletes require justification.
- An audit log table MUST capture actor, action, target, and timestamp.

### API

- Supabase client (PostgREST) is the primary data access layer.
- Validation MUST occur (via Zod on the frontend) before any data mutation.
- All list queries MUST support pagination, filtering, and search.
- Row-Level Security (RLS) MUST be enforced in Supabase for all tables.

### Notifications (Version 1)

| Type | Trigger Examples | Delivery |
|------|-----------------|----------|
| In-App | Low stock, near-expiry, operation success/error, backup failure | All screens |
| Windows Toast | Expired product, critically low stock | Electron only |
| Email / SMS | — | ❌ Not in v1 |

### Performance

- Lazy loading MUST be applied to large data sets.
- Database indexes MUST be applied to all frequently queried columns.
- N+1 query patterns are prohibited; use Supabase joins or RPC calls.

### Security

- Supabase Auth MUST be used for all authentication — no custom auth.
- Row-Level Security (RLS) MUST be enabled on every Supabase table.
- Least privilege principle MUST be applied to all role assignments.
- Audit logging MUST capture all sensitive operations (see §10).

### Git Workflow

| Branch Type | Pattern |
|-------------|---------|
| Feature | `feature/*` |
| Bug Fix | `bugfix/*` |
| Release | `release/*` |

Conventional commits are required for all commits
(`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`).

---

## 16. Decision Framework

A feature SHOULD be accepted only if it meets **all** of the following criteria:

1. Solves a real, documented pharmacy problem.
2. Simplifies existing workflows rather than adding complexity.
3. Improves staff productivity in a measurable way.
4. Provides clear, measurable business value.

Features that do not meet all four criteria MUST be deferred or rejected.

---

## 17. Product Commandments

These rules are absolute and override any other guidance:

1. **Never sacrifice usability for features.** A confusing feature is worse than no feature.
2. **Performance is a feature.** Slowness is a bug.
3. **Every workflow MUST minimize clicks.** Friction is the enemy.
4. **Never compromise data integrity.** Incorrect data destroys trust.
5. **Build software pharmacists enjoy using.** Delight is the goal.

---

## 18. Definition of Done

A feature is considered **Done** only when all of the following are verified:

- [ ] All functional requirements implemented and working.
- [ ] All automated tests pass.
- [ ] RTL layout verified on all affected screens.
- [ ] Offline behavior verified (core operations work without internet).
- [ ] Documentation updated (spec, plan, README as applicable).
- [ ] Audit log entries confirmed for all applicable operations.
- [ ] Role-based access control verified for all new endpoints/actions.

---

## 19. Definition of Success

Revo succeeds when pharmacies can operate faster, with fewer errors, and with
better business visibility — and when pharmacy staff trust and enjoy using it
every day.

---

## 20. Non-Goals for Version 1

The following features are **intentionally excluded** from Version 1:

- Batch Tracking
- Multiple Warehouses
- Multi-language Support
- Multi-currency Support
- Loyalty Points System
- Online Ordering
- Cloud Synchronization (core operations only; sync deferred)

---

## 21. Governance

This constitution supersedes all other documented practices, guidelines, and
conventions within this project.

### Amendment Procedure

1. Any amendment MUST be proposed with a written rationale.
2. The version number MUST be incremented following semantic versioning:
   - **MAJOR** — removal or backward-incompatible redefinition of a principle.
   - **MINOR** — new principle or section added; material guidance expansion.
   - **PATCH** — clarifications, wording improvements, typo fixes.
3. `LAST_AMENDED_DATE` MUST be updated to the ISO date of the amendment.
4. All dependent templates (plan, spec, tasks) MUST be reviewed for alignment
   after any amendment.

### Compliance

- All feature specifications MUST reference and comply with this constitution.
- Implementation plans MUST include a Constitution Check gate before Phase 0.
- Any deviation from a principle MUST be documented in the Complexity Tracking
  table of the relevant `plan.md` and requires explicit approval.
- The Definition of Done (§18) MUST be applied to every feature before closure.

### Runtime Guidance

Refer to `.specify/` directory for agent integration, templates, and workflow
definitions. This constitution is the authoritative governance document;
templates operationalize it per feature.
