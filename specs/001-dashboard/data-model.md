# Data Model: Revo Pharmacy Dashboard

This document details the interface schemas and mock data shapes representing the entities used by the Dashboard.

---

## 1. Interface Schemas

### `FinancialSummary`
Holds daily aggregate amounts for financial cards.

| Field | Type | Description |
|---|---|---|
| `sales_today` | `number` | Total sales revenue in SDG today |
| `purchases_today` | `number` | Total purchase expenses in SDG today |
| `expenses_today` | `number` | Total operational expenses in SDG today |

### `OperationalAlert`
Represents an alert card or badge for critical inventory status.

| Field | Type | Description |
|---|---|---|
| `id` | `string` (UUID) | Unique alert identifier |
| `type` | `'low_stock' \| 'expiring_soon'` | Severity/type class |
| `count` | `number` | Count of affected products |
| `message` | `string` | Human-readable description in Arabic |

### `SalesTrendPoint`
Represents a single day's sales for the 7-day trend chart.

| Field | Type | Description |
|---|---|---|
| `date` | `string` (YYYY-MM-DD) | Calendar date |
| `day_name` | `string` | Day name in Arabic (e.g., "السبت") |
| `amount` | `number` | Total sales revenue in SDG |

### `CategoryDistributionPoint`
Represents sales shares across drug categories.

| Field | Type | Description |
|---|---|---|
| `category` | `string` | Category name in Arabic (e.g., "مضادات حيوية") |
| `amount` | `number` | Total sales revenue in SDG |
| `percentage` | `number` | Share percentage (0-100) |
