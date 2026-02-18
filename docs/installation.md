# Installation Guide

## System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| Odoo | 17.0 | 17.0 latest |
| Python | 3.10 | 3.10+ |
| PostgreSQL | 14 | 15+ |
| RAM | 2 GB | 4 GB+ |
| Disk | 100 MB | 500 MB+ (with analytics data) |

**Compatible with**: Odoo 17.0 Community and Enterprise editions.

---

## Method 1: Odoo App Store (Recommended)

1. Go to [Odoo App Store](https://apps.odoo.com) and search for "Analytics & Performance Dashboard"
2. Purchase and download the module
3. Extract the `analytics_module` folder to your Odoo addons path
4. Restart Odoo
5. Go to **Apps** > Update Apps List
6. Search for "Analytics" and click **Install**

---

## Method 2: Manual Installation

### Step 1: Download

Download the module and extract to your addons directory:

```bash
# Find your addons path
grep addons_path /etc/odoo/odoo.conf

# Example: copy module to addons
cp -r analytics_module /opt/odoo/addons/
```

### Step 2: Restart Odoo

```bash
sudo systemctl restart odoo
```

### Step 3: Install

1. Login to Odoo as administrator
2. Enable **Developer Mode**: Settings > General Settings > Developer Tools > Activate
3. Go to **Apps** > Update Apps List > Update
4. Search for "Analytics & Performance Dashboard"
5. Click **Install**

---

## Method 3: Docker

### docker-compose.yaml

```yaml
version: '3.8'
services:
  web:
    image: odoo:17.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
    volumes:
      - odoo-db-data:/var/lib/postgresql/data

volumes:
  odoo-web-data:
  odoo-db-data:
```

### Steps

```bash
# 1. Place module in addons directory
cp -r analytics_module ./addons/

# 2. Start containers
docker compose up -d

# 3. Create database and install
docker exec odoo-web-1 odoo -d mydb -i analytics_module \
  --stop-after-init --no-http \
  --db_host=db --db_user=odoo --db_password=odoo

# 4. Restart
docker compose restart web
```

---

## Upgrading

When upgrading to a new version:

```bash
# 1. Replace the module files
cp -r analytics_module /opt/odoo/addons/

# 2. Upgrade via command line
./odoo-bin -d mydb -u analytics_module --stop-after-init

# Or via Docker
docker exec odoo-web-1 odoo -d mydb -u analytics_module \
  --stop-after-init --no-http \
  --db_host=db --db_user=odoo --db_password=odoo
```

Or upgrade from the UI:
1. Go to **Apps**
2. Search for "Analytics"
3. Click the module > **Upgrade**

---

## Post-Installation

After installation, the module automatically:

1. Creates **5 default tracked modules**: Inventory, Sales, Purchase, Accounting, CRM
2. Sets up **tracking hooks** for automatic data collection
3. Creates a **default branding** configuration
4. Registers **menu items** under the Analytics top menu

### First Steps

1. **Dashboard**: Navigate to Analytics > Overview
2. **Configuration**: Analytics > Configuration > Tracked Modules
3. **API Keys**: Analytics > Configuration > API Keys (if you need external access)

> See [Configuration Guide](configuration.md) for detailed setup instructions.

---

## Troubleshooting

### Module not appearing in Apps list

```bash
# Update apps list via command line
./odoo-bin -d mydb --update=base --stop-after-init
```

### Permission errors

The module requires the user to be in the **Analytics / Manager** group for full access. Regular users can view dashboards but cannot modify configuration.

### Database migration errors

If upgrading from an older version:

```bash
# Force reinstall
./odoo-bin -d mydb -i analytics_module --stop-after-init
```

> For more help, see [FAQ](faq.md) or contact odoo@momosoft.com
