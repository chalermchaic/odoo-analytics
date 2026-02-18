# Configuration Guide

## Table of Contents

- [Tracked Modules](#tracked-modules)
- [Branding & White-label](#branding--white-label)
- [Webhook Notifications](#webhook-notifications)
- [API Keys](#api-keys)
- [KPI Targets](#kpi-targets)
- [Multi-Company Setup](#multi-company-setup)

---

## Tracked Modules

The module tracks operations across Odoo modules. By default, 5 modules are pre-configured:

| Module | Odoo Model | Workflow ID | Color | Icon |
|--------|-----------|-------------|-------|------|
| Inventory | stock.move | stock_move | Blue | fa-boxes |
| Sales | sale.order | sale_order | Green | fa-shopping-cart |
| Purchase | purchase.order | purchase_order | Red | fa-truck |
| Accounting | account.move | account_move | Orange | fa-calculator |
| CRM | crm.lead | crm_lead | Purple | fa-users |

### Adding Custom Modules

1. Go to **Analytics > Configuration > Tracked Modules**
2. Click **New**
3. Fill in:
   - **Technical Name**: Unique identifier (e.g., `manufacturing`)
   - **Display Name**: Human-readable name (e.g., `Manufacturing`)
   - **Odoo Model**: The model to track (e.g., `mrp.production`)
   - **Workflow Module ID**: For workflow events (e.g., `mrp_production`)
   - **Color**: Choose a color for charts and UI
   - **Icon**: FontAwesome icon class (e.g., `fa-industry`)
4. Click **Save**

### Disabling Modules

Toggle the **Active** switch to disable tracking for a module without deleting its configuration.

---

## Branding & White-label

Customize the dashboard appearance per company.

**Navigate to**: Analytics > Configuration > Branding

### Available Settings

| Setting | Default | Description |
|---------|---------|-------------|
| Dashboard Title | Analytics Dashboard | Title shown at top of dashboard |
| Primary Color | #875A7B | Main accent color |
| Secondary Color | #00A09D | Secondary accent color |
| Header BG Color | #FFFFFF | Dashboard header background |
| Card BG Color | #FFFFFF | KPI card background |
| Page BG Color | #F8F9FA | Page background |
| Logo | (none) | Custom logo (recommended: 200x50px) |
| Show "Powered by" | Yes | Toggle footer branding text |
| Powered by Text | Powered by Analytics Module | Custom footer text |
| Report Title | Analytics Report | Title for exported reports |
| Report Subtitle | (empty) | Subtitle for exported reports |

### Tips

- Use your company brand colors for Primary and Secondary
- Upload a transparent PNG logo for best results
- Each company can have its own branding in multi-company setup

---

## Webhook Notifications

Send real-time alerts to Slack or Microsoft Teams.

**Navigate to**: Analytics > Configuration > Webhooks

### Setting Up Slack

1. Create a Slack app at [api.slack.com/apps](https://api.slack.com/apps)
2. Enable **Incoming Webhooks** and add to your channel
3. Copy the webhook URL (starts with `https://hooks.slack.com/services/...`)
4. In Odoo, create a new webhook:
   - **Name**: e.g., "Sales Team Alerts"
   - **Type**: Slack
   - **URL**: Paste the Slack webhook URL
   - **Severity filters**: Enable Critical and/or Warning
   - **Module filter**: Choose "All" or a specific module
   - **Daily Digest**: Enable for daily summary
   - **Digest Time**: Set the hour (24h format, e.g., 9.0 = 9:00 AM)

### Setting Up Microsoft Teams

1. In Teams, go to your channel > Connectors > Incoming Webhook
2. Name it and copy the URL (contains `webhook.office.com`)
3. Create a webhook in Odoo with **Type**: Teams

### Notification Types

| Type | When | Content |
|------|------|---------|
| Critical Alert | Real-time | Insight title, severity, module, description |
| Warning Alert | Real-time | Same as critical (if enabled) |
| Daily Digest | Scheduled | Summary of operations, insights, top performers |

### Message Formats

- **Slack**: Uses Block Kit format with colored severity bars
- **Teams**: Uses Adaptive Card format with structured sections

---

## API Keys

Manage access tokens for external integrations.

**Navigate to**: Analytics > Configuration > API Keys

### Creating an API Key

1. Click **New**
2. Fill in:
   - **Name**: Descriptive name (e.g., "Power BI Production")
   - **Description**: Optional notes
   - **Expires At**: Optional expiration date (leave empty for no expiry)
3. Click **Save** -- the key is auto-generated

### Permission Controls

| Permission | Default | Description |
|------------|---------|-------------|
| Can Read Dashboard | Yes | Access dashboard endpoint |
| Can Read Insights | Yes | Access insights endpoint |
| Can Read KPIs | Yes | Access KPI endpoint |
| Can Export Data | Yes | Access CSV export endpoint |

### Security Notes

- Keys are displayed as masked previews (first 8 + last 4 characters)
- `last_used_at` and `request_count` are tracked automatically
- Deactivate keys by toggling **Active** off
- Expired keys are automatically rejected

> Full API documentation: [api-reference.md](api-reference.md)

---

## KPI Targets

Set performance goals and track achievement.

**Navigate to**: Analytics > KPI Targets

### Creating KPI Targets

1. Click **New**
2. Configure:
   - **Name**: e.g., "Overall Success Rate"
   - **KPI Type**: Success Rate (%), Operations per Day, Avg Operation Time (sec), Active Users
   - **Target Value**: Your goal number
   - **Module**: "all" for global, or specific module name
   - **Warning Threshold**: % of target that triggers yellow (default: 80%)
   - **Critical Threshold**: % of target that triggers red (default: 60%)

### Status Indicators

| Status | Color | Meaning |
|--------|-------|---------|
| On Target | Green | Achievement >= warning threshold |
| Below Target | Yellow | Between critical and warning |
| Critical | Red | Achievement < critical threshold |

### Trend Tracking

The system automatically compares current week vs. previous week:
- **Up** arrow: Improving
- **Down** arrow: Declining
- **Stable**: Less than 5% change

---

## Multi-Company Setup

The module supports full multi-company operation with data isolation.

### How It Works

- All records (usage, workflows, insights, etc.) are automatically tagged with the user's current company
- Each company has its own: API keys, webhooks, branding, KPI targets
- Security rules ensure users only see data for their current company
- Admin users can switch companies to view/manage each separately

### Setup Steps

1. Install the module (it applies to all companies automatically)
2. Switch to each company via the company selector (top right)
3. Configure separately:
   - Branding (different title/colors per company)
   - Webhooks (different Slack channels per company)
   - API Keys (per-company access tokens)
   - KPI Targets (different goals per company)

### Tracked Modules

Tracked module configuration is also per-company. Each company can:
- Enable/disable different modules
- Add custom modules specific to that company
- Set different colors and icons
