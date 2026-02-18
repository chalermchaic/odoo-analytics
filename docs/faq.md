# Frequently Asked Questions

## General

### What Odoo versions are supported?

Odoo **17.0** (Community and Enterprise). The module uses OWL components and Odoo 17-specific APIs.

### Does it work with Odoo.sh?

Yes. Upload the module as a custom addon in your Odoo.sh project. It has no external Python dependencies beyond what Odoo 17 includes.

### How does data collection work?

The module uses **tracking hooks** that automatically monitor CRUD operations on configured modules (Inventory, Sales, Purchase, Accounting, CRM). When a record is created, updated, or deleted, the hook logs the operation with timestamp, user, duration, and status.

### Does it slow down my Odoo?

No. The tracking hooks add minimal overhead (< 5ms per operation). All heavy computation (insights, aggregation, performance scoring) runs in scheduled background jobs, not during user operations.

---

## Dashboard

### The dashboard shows no data

1. Check that **demo data** or real operations exist
2. Verify the **date range** includes recent data (default: last 30 days)
3. Ensure **Tracked Modules** are active: Analytics > Configuration > Tracked Modules
4. Check that the current user's **company** has data

### Charts are not rendering

1. Clear browser cache (Ctrl+Shift+R)
2. Check browser console (F12) for JavaScript errors
3. Ensure you're using a modern browser (Chrome, Firefox, Edge)

### How do I export data?

Click the **Export** button on the main dashboard to download CSV files. You can also use the [REST API](api-reference.md) endpoint `/export/{data_type}`.

---

## Insights

### How are insights generated?

The insights engine runs periodically (via scheduled action) and analyzes:
- **Failure rates**: Operations with status "failed" exceeding thresholds
- **Duration anomalies**: Operations taking significantly longer than average
- **Usage patterns**: Unusual spikes or drops in activity
- **Bottlenecks**: Workflow steps with high average completion time

### Can I resolve insights?

Yes. Open an insight and toggle the **Resolved** switch. Resolved insights are hidden from the main view by default (filter: "Unresolved").

### Auto-resolve: do old insights disappear?

The system automatically resolves insights older than 30 days if the underlying condition has improved. This prevents the insights list from growing indefinitely.

---

## API

### How do I get an API key?

Go to **Analytics > Configuration > API Keys** > New. The key is auto-generated. Copy it before leaving the page (it's shown masked afterward).

### What's the rate limit?

There is no built-in rate limit, but the API enforces a maximum of **1000 records per request** via the `limit` parameter. For large datasets, use pagination with `offset`.

### Can I use the API with tools like Power BI or Grafana?

Yes! See [Integration Examples](api-reference.md#integration-examples) in the API reference.

### API returns "Invalid API key"

Check that:
1. The key exists and is **Active** (not deactivated)
2. The key has not **expired** (check Expires At field)
3. You're sending the header correctly: `X-API-Key: your-key-here`
4. The key belongs to the same **company** you're trying to access

---

## Webhooks

### Slack notifications not arriving

1. Verify the webhook URL starts with `https://hooks.slack.com/services/`
2. Test the webhook: open the webhook record and click **Test Webhook**
3. Check the **Last Error** field for error messages
4. Ensure severity filters match (Critical/Warning must be enabled)

### Teams notifications not arriving

1. Verify the URL contains `webhook.office.com`
2. Check that the Teams connector is still active
3. Test with the **Test Webhook** button

### Daily digest not sending

1. Check that **Daily Digest** is enabled on the webhook
2. Verify the **Digest Time** is set correctly (24h format)
3. The scheduled action must be active: Settings > Technical > Scheduled Actions > search "digest"

---

## Multi-Company

### I can't see data from another company

This is by design. The module enforces **data isolation** per company. Switch to the target company using the company selector (top right corner) to view its data.

### Webhooks/API keys not working for other companies

Each company needs its own configuration. Switch companies and create separate webhooks and API keys for each.

---

## Performance

### Dashboard loading slowly

1. Check the **date range** -- very large ranges (1+ year) load more data
2. Run the **Cleanup** scheduled action to remove old records
3. Verify database indexes exist (they're created automatically during install)
4. Consider reducing the number of widgets on custom dashboards

### Database growing too large

The module includes a cleanup mechanism. Go to **Settings > Technical > Scheduled Actions** and find "Analytics Cleanup". Configure it to run periodically and delete records older than your retention period.

---

## Troubleshooting

### Module won't install

```
Error: Module analytics_module not found
```
- Ensure the module folder is in your Odoo addons path
- Run: Apps > Update Apps List
- Check file permissions

### Upgrade errors

```
Error: column does not exist
```
This can happen when upgrading across multiple versions. Try:
```bash
./odoo-bin -d mydb -u analytics_module --stop-after-init
```

### Need more help?

- **Email**: odoo@momosoft.com
- **GitHub Issues**: [github.com/chalermchaic/odoo-analytics/issues](https://github.com/chalermchaic/odoo-analytics/issues)
