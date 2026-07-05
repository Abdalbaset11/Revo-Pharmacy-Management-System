# Tasks: Revo Pharmacy Dashboard

**Input**: Design documents from `/specs/001-dashboard/`

**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Vitest tests will be added in the final Polish phase to verify component layout.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Web app**: `frontend/src/`

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Initialize React + Vite frontend directory with dependencies (recharts, @reduxjs/toolkit, react-redux, tailwindcss, lucide-react) under `frontend/`
- [ ] T002 Configure tailwind CSS settings and root CSS file `frontend/src/index.css` for RTL (Arabic font and direction utilities)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core layout, state store, and base components that must be complete before any user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T003 Setup Redux store configuration in `frontend/src/state/store.ts`
- [ ] T004 Create basic quick-navigation RTL Sidebar component in `frontend/src/components/Sidebar.tsx`
- [ ] T005 Create Header component showing active logged-in user profile name & role in `frontend/src/components/Header.tsx`
- [ ] T006 Implement base Dashboard container page component in `frontend/src/pages/Dashboard.tsx` coordinating Sidebar and Header layouts

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Financial Performance Summary (Priority: P1) 🎯 MVP

**Goal**: Display real-time card aggregates for today's Sales, Purchases, and Expenses.

**Independent Test**: Verify that the three cards render the values 125,350 SDG, 45,000 SDG, and 12,400 SDG respectively.

### Implementation for User Story 1

- [ ] T007 [P] [US1] Create reusable metric card component in `frontend/src/components/MetricCard.tsx`
- [ ] T008 [P] [US1] Create `dashboardSlice` in `frontend/src/state/slices/dashboardSlice.ts` managing mock state for `FinancialSummary`
- [ ] T009 [US1] Integrate `MetricCard`s in `frontend/src/pages/Dashboard.tsx` and connect to Redux state selectors

**Checkpoint**: User Story 1 is functional and testable independently.

---

## Phase 4: User Story 2 - Real-Time Operational Alerts (Priority: P1)

**Goal**: Provide warnings for low stock items and medicines expiring soon.

**Independent Test**: Verify the alert panel shows the correct counts and navigates to the list views when clicked.

### Implementation for User Story 2

- [ ] T010 [P] [US2] Create alerts display component in `frontend/src/components/AlertsPanel.tsx`
- [ ] T011 [P] [US2] Update `frontend/src/state/slices/dashboardSlice.ts` to include mock operational alerts data
- [ ] T012 [US2] Integrate `AlertsPanel` into `frontend/src/pages/Dashboard.tsx` and configure mock route actions

**Checkpoint**: User Story 2 is functional and testable independently.

---

## Phase 5: User Story 3 - Sales Visual Trends (Priority: P2)

**Goal**: Render visual charts of trailing 7-day sales and 30-day category distribution.

**Independent Test**: Check that both charts display with correct labels (in Arabic) and respond correctly to screen resizing.

### Implementation for User Story 3

- [ ] T013 [P] [US3] Add sales trend and category distribution mock datasets to `frontend/src/state/slices/dashboardSlice.ts`
- [ ] T014 [US3] Implement trailing 7-day bar chart using Recharts in `frontend/src/pages/Dashboard.tsx`
- [ ] T015 [US3] Implement trailing 30-day category distribution pie chart using Recharts in `frontend/src/pages/Dashboard.tsx`

**Checkpoint**: All user stories are independently functional.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Testing, verification, and polishing

- [ ] T016 [P] Add component rendering tests using Vitest in `frontend/tests/dashboard.test.tsx`
- [ ] T017 Run quickstart.md validation steps in local browser environment to check CSS responsiveness
- [ ] T018 Document final build features in walkthrough.md

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: Can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2)
- **User Story 2 (P2)**: Can start after Foundational (Phase 2)
- **User Story 3 (P3)**: Can start after Foundational (Phase 2)

---

## Parallel Example: User Story 1

```bash
# Launch mock slice and component model tasks in parallel:
Task: "Create reusable metric card component in frontend/src/components/MetricCard.tsx"
Task: "Create dashboardSlice in frontend/src/state/slices/dashboardSlice.ts..."
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Verify today's financial totals render correctly.
