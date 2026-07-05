# Implementation Plan: Revo Pharmacy Dashboard

**Branch**: `001-dashboard` | **Date**: 2026-07-05 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `/specs/001-dashboard/spec.md`

**Note**: This template is filled in by the `/speckit-plan` command. See `.specify/templates/plan-template.md` for the execution workflow.

## Summary

The dashboard serves as the central entry point and operational command center for Revo. It displays daily aggregates (sales, purchases, expenses), system alerts for low stock and near-expiry drugs, and interactive Arabic/RTL charts for sales trends (last 7 days) and category distribution (last 30 days). The dashboard will be built as a React component running in Electron, using mock data slices via RTK Query that align with the `docs/API.md` and `docs/Database.md` schemas.

## Technical Context

**Language/Version**: TypeScript 5.x / HTML5

**Primary Dependencies**: React 18+, Tailwind CSS, shadcn/ui, Redux Toolkit, RTK Query, Recharts, Lucide React

**Storage**: Local cache (Redux state) / Mock data provider

**Testing**: Vitest / React Testing Library

**Target Platform**: Windows Desktop (Electron) & Web browsers

**Project Type**: desktop-app / frontend

**Performance Goals**: Page rendering and metric computation under 500ms (SC-001)

**Constraints**: RTL rendering (Arabic), offline-capable with mock data rendering when backend is unreachable (SC-002)

**Scale/Scope**: Single dashboard view, modular sidebar, 4 metric cards, 2 alert panels, 2 charts

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Governance Rule | Status | Validation / Rationale |
|---|---|---|
| UUID Primary Keys | Pass | In accordance with `docs/Database.md`, all referenced entities (e.g., profiles, products, sales_invoices) use UUID. |
| Created/Updated Timestamps | Pass | All backing tables in `docs/Database.md` include `created_at` and `updated_at`. |
| Soft Delete | Pass | In accordance with `docs/Database.md`, deleted_at is included on products, suppliers, customers. |
| Audit Logging | Pass | In accordance with `docs/Database.md`, audit log captures all state modifications. |
| RLS Enforced | Pass | All Supabase queries obey Row-Level Security based on active user profiles. |
| Approved Tech Stack | Pass | Uses only React, Tailwind, RTK, and Recharts. No unapproved packages. |
| Development Strategy | Pass | Contract-First. Frontend built using mock data; no active Supabase connections in this phase. |

## Project Structure

### Documentation (this feature)

```text
specs/001-dashboard/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/           # Phase 1 output (API schema snapshots)
└── checklists/
    └── requirements.md  # Specification Quality Checklist
```

### Source Code (repository root)

```text
frontend/
├── src/
│   ├── components/
│   │   ├── Sidebar.tsx             # RTL Sidebar navigation
│   │   ├── Header.tsx              # User profile header
│   │   ├── MetricCard.tsx          # SDG Financial Summary cards
│   │   └── AlertsPanel.tsx         # Low stock & near expiry alerts
│   ├── pages/
│   │   └── Dashboard.tsx           # Dashboard container layout
│   ├── state/
│   │   ├── store.ts                # Redux store config
│   │   └── slices/
│   │       └── dashboardSlice.ts   # Mock state and selectors for sales, purchases, alerts
│   └── main.tsx
└── tests/
    └── dashboard.test.tsx          # Dashboard page rendering tests
```

**Structure Decision**: Option 2 (Web application/frontend layout) was chosen since we are in the frontend design phase. Backend directories are excluded for this feature as it runs purely on Mock Data.

## Complexity Tracking

*No violations identified. The plan fully complies with all principles in docs/Constitution.md.*
