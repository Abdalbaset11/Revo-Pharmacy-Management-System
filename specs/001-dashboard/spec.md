# Feature Specification: Revo Pharmacy Dashboard

**Feature Branch**: `001-dashboard`

**Created**: 2026-07-05

**Status**: Draft

**Input**: User description: "Initialize the Revo Pharmacy Management System Dashboard feature specification, establishing the entry point and design foundation for system-wide reporting, sales tracking, and operational visibility."

---

## Clarifications

### Session 2026-07-05

- Q: What is the time range for the Category Distribution chart? → A: Trailing 30 days (Option B)

---

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Financial Performance Summary (Priority: P1)

As a Pharmacy Owner or Manager, I want to see a real-time summary of today's key financial metrics (Sales, Purchases, Expenses) so that I can understand the business's performance at a single glance.

**Why this priority**: Highly critical for daily operations; provides immediate business value and acts as the core dashboard landing area.

**Independent Test**: Verify that the dashboard displays the total SDG values for sales, purchases, and expenses generated today.

**Acceptance Scenarios**:
1. **Given** that sales of 50,000 SDG, purchases of 20,000 SDG, and expenses of 5,000 SDG have occurred today, **When** the Owner loads the dashboard, **Then** they see cards displaying:
   - "Sales Today": 50,000 SDG
   - "Purchases Today": 20,000 SDG
   - "Expenses Today": 5,000 SDG
2. **Given** that no transactions have occurred today, **When** the Manager loads the dashboard, **Then** all summary cards show "0 SDG" with empty/neutral status indicators.

---

### User Story 2 - Real-Time Operational Alerts (Priority: P1)

As a Pharmacy Manager or Pharmacist, I want to be alerted immediately to critical operational conditions (items with low stock levels and items near expiry) so that I can take preventive action before sales are disrupted.

**Why this priority**: Prevents lost revenue and legal/regulatory compliance issues by stopping expired drug sales.

**Independent Test**: Verify that clicking the alerts navigates the user directly to the filtered product lists.

**Acceptance Scenarios**:
1. **Given** 5 products are below their minimum stock threshold and 2 products are expiring within 30 days, **When** the Manager views the dashboard alerts section, **Then** they see:
   - A warning badge showing "5 items out of stock / low stock".
   - A critical badge showing "2 items expiring soon".
2. **Given** no stock issues exist, **When** the user loads the dashboard, **Then** the alerts section displays a green checkmark or neutral "All inventory is healthy" message.

---

### User Story 3 - Sales Visual Trends (Priority: P2)

As a Pharmacy Owner or Accountant, I want to view a visual chart of sales trends over the past 7 days so that I can identify patterns and project stock replenishment requirements.

**Why this priority**: Helps in business planning and purchase forecasting.

**Independent Test**: Verify that the chart displays 7 data points representing the past 7 calendar days.

**Acceptance Scenarios**:
1. **Given** variable daily sales over the last week, **When** the Owner views the sales trend chart, **Then** they see a bar or line chart showing correct relative heights for each day, with the Arabic labels for the weekdays (e.g., السبت, الأحد).

---

## Edge Cases

- **No Database Connection**: When the application runs in offline fallback mode, the dashboard MUST show a status bar indicating "Offline Mode: displaying cached data from last sync" and allow all display actions to proceed without crashing.
- **Extreme Currency Values**: When daily sales exceed millions of SDG, the dashboard cards MUST format the values legibly (e.g., using thousand separators like 1,500,000 SDG) and prevent layout breaks or overlapping text.

---

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The dashboard MUST display a summary panel containing three metrics: Today's Total Sales, Today's Total Purchases, and Today's Total Expenses in Sudanese Pounds (SDG).
- **FR-002**: The dashboard MUST show a distinct, high-visibility "Operational Alerts" panel containing:
  - Count of items with zero or low stock (below minimum configured threshold).
  - Count of items expiring within 30 days.
- **FR-003**: The dashboard MUST render a Sales Trend chart showing daily sales for the trailing 7 days.
- **FR-004**: The dashboard MUST render a Category Distribution chart showing the share of sales across drug categories (e.g., Antibiotics, Chronic, OTC) for the trailing 30 days.
- **FR-005**: All text, charts, and metrics MUST support Right-to-Left (RTL) reading direction, with labels in Arabic.
- **FR-006**: The dashboard MUST display a quick-navigation sidebar with links to: POS, Products, Purchases, Inventory, Customers, Suppliers, Employees, Reports, and Settings.

### Key Entities

- **FinancialSummary**: A snapshot entity containing daily aggregated values of sales, purchases, and expenses.
- **OperationalAlert**: An entity representing a critical state of an inventory item (e.g., low stock, near expiry) requiring staff attention.

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: The dashboard page loads and displays all summaries and charts in under 500 milliseconds from navigation.
- **SC-002**: 100% of the daily metrics and charts remain interactive and readable when the system switches to offline mode.
- **SC-003**: Users can successfully navigate to the POS or Inventory screen from the dashboard in a single click.

---

## Assumptions

- The user's screen resolution is desktop-class (at least 1024x768 pixels).
- The system timezone matches Sudan Standard Time (UTC+2) to calculate "Today's" boundaries correctly.
- Offline cached data is synced automatically in the background when a connection becomes active; the dashboard is not responsible for initiating sync itself.
