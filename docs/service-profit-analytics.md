# Service Profit Analytics for Odoo 17 & 18

**True Project & Client Profitability, Billable Utilization & Unbilled WIP — Without Excel.**

Built for IT companies, creative agencies, and consulting firms running **Odoo 17.0 or 18.0 (Community & Enterprise)**. Service Profit Analytics aggregates Projects, Timesheets, and Posted Invoices in read-only PostgreSQL views so you can spot margin leaks and unbilled work in seconds.

---

## Key Capabilities

### 1. Four Core SQL-View Reports + OWL Executive Dashboard
- **OWL Executive Dashboard**: Year-to-date revenue, cost, margin, utilization KPIs, top projects by revenue, at-risk projects, and a 6-month Revenue vs Cost trend chart.
- **Project Lifetime Profitability**: Lifetime revenue, cost, margin, margin %, and unbilled WIP cost per project with colour-coded list, pivot, and bar chart views plus smart buttons to drill into invoices and timesheets.
- **Monthly Project P&L**: Period-by-period breakdown per project with quick date filters and Projects × Months pivot.
- **Client Profitability Rollup**: Aggregated revenue, cost, margin, and unbilled WIP per client across all their projects.
- **Billable Utilization**: Employee × Project × Month breakdown showing billable hours, non-billable hours, and utilization %.

### 2. Date-Effective Employee Cost Rates
- Dedicated `spa.employee.cost.rate` model supporting effective date ranges (`date_from` / `date_to`) per employee and company.
- PostgreSQL `LATERAL` join preserves historical project margins accurately when employee hourly cost rates change over time.

### 3. Odoo 17 Multi-Plan `analytic_distribution` Proration
- Reads Odoo 17's `analytic_distribution` JSONB column directly so multi-plan invoice lines are prorated accurately across projects with zero double-counting.
- Supports both **Confirmed Invoices** (default) and **Timesheet-Based Revenue** modes.

### 4. Per-Company Margin & Unbilled-WIP Alert Digest (`17.0.4.0.0`)
- Configure per-company **Minimum Margin (%)** and **Max Unbilled WIP Cost** (in company currency) in **Settings > Service Profit Analytics**.
- Setting a threshold to `0` disables that criterion.
- An optional scheduled digest (`ir.cron`, off by default, enabled from Settings) evaluates at-risk projects per company and privately notifies that company's Profit Managers (`group_spa_manager`) only about their own company's projects.

---

## Scale, Security & Architecture

- **Non-Invasive**: Zero modifications to standard Odoo tables — all reports use read-only PostgreSQL views (`_auto = False`).
- **3-Tier Multi-Company Security**: User, Team Lead (own projects), and Manager roles with multi-company record rules.
- **Benchmarked Performance**: Evaluated on 150,000 timesheet rows in ≤ 0.25 s.
- **Automated Test Suite**: Backed by 97+ automated tests (98% line coverage).

---

## Installation & Configuration

1. Install **Service Profit Analytics** (`service_profit_analytics`) from Apps (`17.0.4.0.0` for Odoo 17, `18.0.4.0.0` for Odoo 18).
2. Complete the **2-minute Setup Wizard** (choose IT / Agency / Consulting preset or custom thresholds).
3. Configure employee hourly cost rates under **Service Profit > Configuration > Employee Cost Rates**.
4. (Optional) Enable the **Margin & WIP Alert Digest** under **Settings > Service Profit Analytics**.

### Upgrading from Odoo 17 to 18

Moving an existing database from Odoo 17 to Odoo 18 uses Odoo's official database upgrade service. Once your database is upgraded to Odoo 18:

1. Place the `18.0` series of `service_profit_analytics` (`18.0.4.0.0`) in your Odoo 18 addons path and upgrade the module (`-u service_profit_analytics`).
2. **No manual module migration steps are required** — employee cost rates and settings are preserved, and the read-only SQL report views are recreated automatically.
3. **Post-upgrade checks:**
   - Open the OWL Executive Dashboard and the 4 SQL-view reports (Project Profitability, Monthly P&L, Client Profitability, Billable Utilization) to confirm data renders cleanly.
   - Verify your Margin & WIP Alert Digest settings and scheduled action if enabled.

---

## License & Support

- **License**: Odoo Proprietary License v1.0 (`OPL-1`). Copies previously distributed under `LGPL-3` remain under `LGPL-3`.
- **Support**: `odoo@momosoft.com` (MomoSoft)
