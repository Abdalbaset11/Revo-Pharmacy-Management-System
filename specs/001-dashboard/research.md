# Research & Decisions: Revo Pharmacy Dashboard

This document details the research findings and technical decisions made for the Dashboard component structure.

---

## 1. Chart Visualization Library

- **Decision**: Use `recharts` for rendering the 7-day Sales Trend and 30-day Category Distribution charts.
- **Rationale**:
  - `recharts` is a React-native declarative chart library that uses SVG.
  - Native support for responsive containers (`<ResponsiveContainer>`), making it look premium on various screen resolutions.
  - Custom SVG components can be used to handle Right-to-Left (RTL) Arabic labels easily without overlapping.
- **Alternatives Considered**: 
  - `chart.js` with `react-chartjs-2` (rejected because canvas-based rendering makes styling with custom fonts and RTL text configurations more complex than SVG).

---

## 2. Mock Data & State Management

- **Decision**: Implement state management using Redux Toolkit. Create a `dashboardSlice.ts` to manage the local mock state.
- **Rationale**:
  - Codifies the "Contract-First + Mock Data" strategy from the Constitution.
  - Provides a single source of truth for POS sales trends and inventory counts.
  - Once Supabase integration starts (Phase 3), the mock actions in Redux can be swapped for RTK Query endpoints with minimal changes in the UI components.
- **Alternatives Considered**: 
  - Plain React `useState` (rejected because cross-component state access, e.g., low stock alerts badge in the sidebar and count card on the dashboard, requires prop drilling).
