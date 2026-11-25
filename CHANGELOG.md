# Changelog

All notable changes to the Analytics & KPI Dashboard module.

## [17.0.9.0.0] - 2026-11-25

### Store Readiness & BI Endpoint Tester
- **In-Odoo API Endpoint Tester**: Select any `/api/v1/analytics/*` endpoint on the API Key form and test it in-process with status code and JSON preview
- **Ready-to-Paste BI Snippets**: Copyable Power BI (M Query), Grafana (JSON API), and cURL snippets using `<YOUR_API_KEY>`
- **App Title & License**: Renamed to **Analytics & KPI Dashboard** and updated module license to `OPL-1` (previously distributed LGPL-3 copies remain LGPL-3)
- **Demo Data**: Updated demo API keys (generated via ORM) and inactive-by-default demo webhooks

## [17.0.8.x] - 2026-11-24

### API Controls, Webhooks & Scale Verification
- **API Key Generation**: One-click **Generate / Regenerate Key** action (shown once, stored hashed)
- **Rate Limiting**: Fixed-window per-key rate limiting returning `429 Too Many Requests` with `Retry-After` header
- **Date Parameter Validation**: Strict `YYYY-MM-DD` validation and `date_from <= date_to` check returning `400 Bad Request`
- **HTTPS-Only Webhooks**: Slack (`hooks.slack.com`) and Teams (`*.webhook.office.com`) webhooks over HTTPS
- **Scale Benchmark & Coverage**: Verified ≤ 0.35 s dashboard queries on 1,000,000 usage rows and 320+ automated tests (96% coverage)

## [7.0.0] - 2026-02-15

### Enterprise Performance Optimization

- **6 composite database indexes** for fast queries on large datasets
- **Batch SQL queries** replacing N+1 ORM loops in cleanup, aggregation, and widget data
- **API pagination** with configurable `offset`/`limit` (max 1000 records per request)
- **Batch CSV export** with streaming to prevent out-of-memory on large exports
- **Parameterized SQL** in KPI calculations
- **Batch user performance** calculation via single SQL query
- 17 new performance tests

## [6.0.0] - 2026-02-10

### Configurable Tracked Modules

- **Dynamic module configuration** via UI (Analytics > Configuration > Tracked Modules)
- 5 pre-configured modules: Inventory, Sales, Purchase, Accounting, CRM
- Add custom modules (Manufacturing, HR, etc.) without code changes
- Per-module **color** and **icon** configuration
- **Archive/activate** modules to control tracking
- Per-company module configuration for multi-company setups
- 18 new tracked module and tracking hooks tests

## [5.0.0] - 2026-02-05

### Custom Dashboard Builder

- **7 widget types**: KPI Card, Line Chart, Doughnut Chart, Table, Insight List, Module Breakdown, Top Performers
- **Create unlimited dashboards** per user
- **Edit mode** with Add Widget catalog, resize, reorder, remove
- **Default layout** with 8 widgets on new dashboards
- Bootstrap grid sizing (3/4/6/8/12 columns)
- Single RPC batch data loading for all widgets
- Odoo Store listing page (index.html) with screenshots

## [4.0.0] - 2026-02-01

### Store Listing & Screenshots

- Professional store listing page
- 12 screenshots covering all features
- Banner image for Odoo App Store

## [3.0.0] - 2026-01-25

### KPI Targets & User Performance

- **KPI Target** model with 4 metric types (success rate, operations/day, avg duration, active users)
- Visual **achievement tracking** with progress bars and status indicators (On Target / Below Target / Critical)
- **Week-over-week trend** comparison with up/down/stable arrows
- **User Performance** analytics with efficiency scoring (0-100)
- **Leaderboard** with rankings
- Automated **training recommendations** based on performance data

## [2.0.0] - 2026-01-20

### Notifications & Branding

- **Slack integration** with Block Kit formatted messages
- **Microsoft Teams** integration with Adaptive Cards
- **Webhook management** UI with test button
- Severity and module filtering for notifications
- **Daily digest** scheduled summaries
- **White-label branding**: custom title, logo, 5 color zones
- Per-company branding settings
- Custom report titles and footer text

## [1.0.0] - 2026-01-15

### Initial Release

- **Real-time dashboard** with KPI cards, charts, and insights panel
- **Module Usage tracking** for Inventory, Sales, Purchase, Accounting, CRM
- **Workflow Events** monitoring with duration tracking
- **Insights Engine** with anomaly detection, bottleneck identification, trend analysis
- **REST API** with 9 endpoints and API key authentication
- **Multi-company** support with full data isolation
- **CSV export** for usage, workflow, and insights data
- **OWL-based** dashboard with Chart.js visualizations
- Period-over-period **comparison mode**
- Quick date presets (7d, 30d, 90d, YTD)
