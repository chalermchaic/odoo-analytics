# REST API Reference

## Base URL

```
http://your-odoo-server:8069/api/v1/analytics
```

## Authentication

All endpoints (except health check) require an API key via the `X-API-Key` header:

```bash
curl -H "X-API-Key: YOUR_API_KEY" \
  http://localhost:8069/api/v1/analytics/dashboard
```

API keys can be created at **Analytics > Configuration > API Keys**. Click **Generate / Regenerate Key** to create a key (shown once upon generation and stored hashed). You can also test endpoints in-process and copy ready-to-paste **Power BI (M Query)**, **Grafana (JSON API)**, and **cURL** snippets directly from the **Endpoint Tester & BI Snippets** tab.

## Rate Limiting & Validation

- **Rate Limit**: Each API key is rate-limited per minute (configurable via `analytics_module.api_rate_limit`, default `60` requests/min). Exceeding the limit returns HTTP `429 Too Many Requests` with a `Retry-After` header (seconds).
- **Date Parameters**: `date_from` and `date_to` must use `YYYY-MM-DD` format with `date_from <= date_to`. Invalid values return HTTP `400 Bad Request`.

## Pagination

Endpoints that return lists support pagination:

| Parameter | Default | Max | Description |
|-----------|---------|-----|-------------|
| `limit` | 50 | 1000 | Number of records to return |
| `offset` | 0 | - | Number of records to skip |

---

## Endpoints

### 1. GET /health

Health check endpoint. **No authentication required.**

```bash
curl http://localhost:8069/api/v1/analytics/health
```

**Response:**
```json
{
  "success": true,
  "status": "healthy",
  "version": "17.0.7.0.0",
  "timestamp": "2026-02-17T10:30:00Z"
}
```

---

### 2. GET /dashboard

Main dashboard metrics and summary data.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `date_from` | string | 30 days ago | Start date (YYYY-MM-DD) |
| `date_to` | string | today | End date (YYYY-MM-DD) |

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/dashboard?date_from=2026-01-01&date_to=2026-01-31"
```

**Response:**
```json
{
  "success": true,
  "data": {
    "total_operations": 4294,
    "success_rate": 78.4,
    "avg_duration": 7.0,
    "active_insights": 231,
    "operations_by_day": [...],
    "operations_by_module": [...],
    "slowest_operations": [...],
    "recent_insights": [...]
  }
}
```

---

### 3. GET /comparison

Period-over-period comparison (current vs. previous period).

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `date_from` | string | 30 days ago | Start date (YYYY-MM-DD) |
| `date_to` | string | today | End date (YYYY-MM-DD) |

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/comparison?date_from=2026-02-01&date_to=2026-02-15"
```

**Response:**
```json
{
  "success": true,
  "data": {
    "current_period": { "total_operations": 2100, "success_rate": 80.2 },
    "previous_period": { "total_operations": 1950, "success_rate": 75.1 },
    "changes": {
      "total_operations": { "value": 150, "percent": 7.7 },
      "success_rate": { "value": 5.1, "percent": 6.8 }
    }
  }
}
```

---

### 4. GET /insights

List insights with optional filtering and pagination.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `severity` | string | (all) | Filter: `critical`, `warning`, `info` |
| `module` | string | (all) | Filter by module name |
| `limit` | int | 50 | Records per page (max 1000) |
| `offset` | int | 0 | Skip records |

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/insights?severity=critical&limit=10"
```

**Response:**
```json
{
  "success": true,
  "count": 10,
  "total": 45,
  "offset": 0,
  "limit": 10,
  "data": [
    {
      "id": 1,
      "title": "High failure rate detected in Inventory",
      "description": "Failure rate reached 18.5% in the last 24 hours",
      "severity": "critical",
      "module": "inventory",
      "metric_type": "failure_rate",
      "detected_at": "2026-02-15T08:42:40Z",
      "threshold_value": 10.0,
      "actual_value": 18.5,
      "recommendation": "Review recent inventory operations for errors..."
    }
  ]
}
```

---

### 5. GET /kpis

All KPI targets with current values and status.

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/kpis"
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "name": "Overall Success Rate",
      "kpi_type": "success_rate",
      "target_value": 85.0,
      "current_value": 78.4,
      "achievement_percent": 92.2,
      "status": "warning",
      "trend": "up",
      "trend_percent": 3.5
    }
  ]
}
```

---

### 6. GET /leaderboard

User performance rankings.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `date_from` | string | 30 days ago | Start date (YYYY-MM-DD) |
| `date_to` | string | today | End date (YYYY-MM-DD) |
| `limit` | int | 10 | Top N users |

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/leaderboard?limit=5"
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "rank": 1,
      "user": "Alice Johnson",
      "total_operations": 520,
      "success_rate": 94.2,
      "efficiency_score": 87.5
    }
  ]
}
```

---

### 7. GET /module/{module_name}

Detailed statistics for a specific module.

**Path Parameters:**

| Parameter | Description |
|-----------|-------------|
| `module_name` | Module technical name: `inventory`, `sales`, `purchase`, `accounting`, `crm` |

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `date_from` | string | 30 days ago | Start date (YYYY-MM-DD) |
| `date_to` | string | today | End date (YYYY-MM-DD) |

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/module/sales?date_from=2026-02-01"
```

**Response:**
```json
{
  "success": true,
  "data": {
    "module": "sales",
    "total_operations": 937,
    "success_rate": 82.1,
    "avg_duration": 6.5,
    "operations_by_action": {
      "create": 234, "read": 375, "update": 281, "delete": 47
    },
    "operations_by_day": [...]
  }
}
```

---

### 8. GET /workflow

Workflow events summary.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `date_from` | string | 30 days ago | Start date (YYYY-MM-DD) |
| `date_to` | string | today | End date (YYYY-MM-DD) |

```bash
curl -H "X-API-Key: YOUR_KEY" \
  "http://localhost:8069/api/v1/analytics/workflow"
```

**Response:**
```json
{
  "success": true,
  "data": {
    "total_events": 1623,
    "completed": 1350,
    "in_progress": 150,
    "failed": 123,
    "avg_duration_minutes": 12.5,
    "events_by_module": {...},
    "events_by_operation": {...}
  }
}
```

---

### 9. GET /export/{data_type}

Export data as CSV file.

**Path Parameters:**

| Parameter | Values | Description |
|-----------|--------|-------------|
| `data_type` | `usage` | Module usage records |
| | `workflow` | Workflow events |
| | `insights` | Insights data |

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `date_from` | string | 30 days ago | Start date |
| `date_to` | string | today | End date |

```bash
# Download usage data as CSV
curl -H "X-API-Key: YOUR_KEY" -o usage_report.csv \
  "http://localhost:8069/api/v1/analytics/export/usage?date_from=2026-01-01"

# Download insights as CSV
curl -H "X-API-Key: YOUR_KEY" -o insights.csv \
  "http://localhost:8069/api/v1/analytics/export/insights"
```

**Response:** CSV file download with `Content-Disposition` header.

---

## Error Responses

All endpoints return consistent error format:

```json
{
  "success": false,
  "error": "Invalid API key"
}
```

### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 400 | Bad request (invalid parameters) |
| 401 | Unauthorized (missing or invalid API key) |
| 403 | Forbidden (key lacks permission) |
| 404 | Not found (invalid module name) |
| 500 | Server error |

---

## Integration Examples

### Power BI

Use Power BI's **Web** data source:
1. Get Data > Web
2. URL: `http://your-odoo:8069/api/v1/analytics/dashboard`
3. HTTP Headers: `X-API-Key: YOUR_KEY`
4. Transform JSON data in Power Query Editor

### Grafana

Use the **JSON API** datasource plugin:
1. Install "JSON API" datasource in Grafana
2. URL: `http://your-odoo:8069/api/v1/analytics`
3. Add header: `X-API-Key: YOUR_KEY`
4. Create panels using `/dashboard`, `/insights`, `/kpis` endpoints

### Python

```python
import requests

API_URL = "http://localhost:8069/api/v1/analytics"
HEADERS = {"X-API-Key": "YOUR_KEY"}

# Get dashboard data
resp = requests.get(f"{API_URL}/dashboard", headers=HEADERS)
data = resp.json()["data"]

print(f"Total Operations: {data['total_operations']}")
print(f"Success Rate: {data['success_rate']}%")
```
