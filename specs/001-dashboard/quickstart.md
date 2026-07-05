# Quickstart Validation Guide: Revo Pharmacy Dashboard

This guide outlines the validation scenarios to prove that the Dashboard feature behaves correctly end-to-end using mock data.

---

## 1. Prerequisites & Setup

Ensure the application dependencies are installed (once frontend project is initialized):
```bash
cd frontend
npm install
```

---

## 2. Validation Scenarios

### Scenario 1: Verify RTL Layout & Dashboard Summary Cards
- **Action**: Start the React app in development mode:
  ```bash
  npm run dev
  ```
- **Expectation**:
  - The page loads at the root route `/` or `/dashboard`.
  - The direction of the HTML document is RTL (`dir="rtl"`).
  - The sidebar appears on the right side of the screen.
  - Three financial cards display "Sales Today", "Purchases Today", and "Expenses Today" with the correct SDG values matching `dashboard-mock.json`.

### Scenario 2: Verify Operational Alerts Navigation
- **Action**: Click on the "low stock" alert badge on the dashboard.
- **Expectation**:
  - The application navigates to the `/products` route.
  - A query filter for "low stock" is automatically applied in the product list.

### Scenario 3: Verify trailing 7-day Sales Trend rendering
- **Action**: Scroll to the charts section.
- **Expectation**:
  - The Sales Trend chart displays 7 bars representing the last 7 days.
  - Weekday labels are shown in Arabic (السبت, الأحد, etc.).
