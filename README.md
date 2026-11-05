# RST Electronics Enterprise Suite — Hosting Deployment Guide & Strategic Roadmap

## Part 1: cPanel Web Hosting Upload & Deployment Guide

This section provides a step-by-step procedure for deploying the **RST Electronics Enterprise Suite v7.0** to any standard cPanel / Linux Web Hosting environment with PHP 8.1+ and MySQL 8.0+.

### 1. File Structure Overview

Prepare the following files for upload to your cPanel `public_html` directory:

```
public_html/
├── index.html              # Core Web Application (Storefront + POS + Repair + ERP Control Panel)
├── config.php              # Database credentials & Oman VAT settings
├── database.php            # PDO Database Connection Singleton
├── schema.sql              # MySQL Database Structure
├── seed.sql                # Initial Seed Data (Categories, Default Admin, Sample Products)
├── pos_checkout.php        # POS Sales Checkout Endpoint API
├── repair_create.php       # Device Repair Ticket Creation API
├── backup_cron.php         # Automated Google Drive Backup & Logout Trigger Script
└── uploads/                # Directory for product logos, customer invoices, and vouchers

```

### 2. Step-by-Step cPanel Deployment Process

#### Step 1: Upload Front-End & PHP Scripts

1. Log into your **cPanel Account** (e.g., `https://yourdomain.com:2083`).

2. Open **File Manager** and navigate to `public_html`.

3. Click **Upload** and upload `index.html`, `config.php`, `database.php`, `pos_checkout.php`, `repair_create.php`, and `backup_cron.php`.

4. Create a folder named `uploads/` inside `public_html` and set permissions to `755` or `777` for document picture uploads.

#### Step 2: Create MySQL Database & User

1. In cPanel, navigate to **MySQL® Database Wizard**.

2. **Database Name:** Enter `rst_erp` (Full name: `username_rst_erp`).

3. **Database User:** Create user `rst_admin` (Full name: `username_rst_admin`) and generate a secure password.

4. **Privileges:** Select **ALL PRIVILEGES** and click **Make Changes**.

#### Step 3: Import Database Schema & Seed Data

1. Open **phpMyAdmin** in cPanel.

2. Select your newly created database (`username_rst_erp`).

3. Click the **Import** tab.

4. Choose `schema.sql` and click **Go**.

5. Repeat the import for `seed.sql` to populate initial product categories, OMR exchange rates, and business details.

#### Step 4: Configure Environment (`config.php`)

In File Manager, edit `config.php` to match your cPanel MySQL user credentials:

```
<?php
return [
    'db_host' => 'localhost',
    'db_name' => 'username_rst_erp',
    'db_user' => 'username_rst_admin',
    'db_pass' => 'YOUR_SECURE_MYSQL_PASSWORD',
    'vat_rate' => 0.05,
    'currency' => 'OMR',
    'google_drive_folder_id' => 'YOUR_GOOGLE_DRIVE_FOLDER_ID'
];
?>

```

#### Step 5: Enable SSL / HTTPS

1. Go to **Let's Encrypt™ SSL** or **AutoSSL** in cPanel.

2. Issue an SSL certificate for your main domain and subdomains.

3. *Note:* HTTPS is mandatory for Web Bluetooth thermal receipt printing and barcode/IMEI camera scanners.

### 3. Automated 24-Hour Dual Google Drive Backup & System Logout Workflow

To enforce system safety and zero data loss, the ERP executes **twice-daily automated cloud backups** to Google Drive at **12:00 PM** and **7:00 PM** (Oman Time / GST).

#### Cron Job Configuration in cPanel:

Navigate to **cPanel > Cron Jobs** and add two daily cron jobs:

1. **Mid-day Shift Backup (12:00 PM Oman Time / 08:00 UTC):**

   ```
   0 8 * * * /usr/bin/php /home/username/public_html/backup_cron.php >/dev/null 2>&1
   
   ```

2. **Evening Shift Backup (7:00 PM Oman Time / 15:00 UTC):**

   ```
   0 15 * * * /usr/bin/php /home/username/public_html/backup_cron.php >/dev/null 2>&1
   
   ```

#### Workflow Execution Protocol:

* **At 12:00 PM & 7:00 PM:**

  1. The server compiles a complete SQL snapshot containing all ledger accounts, products, IMEI serials, vouchers, repair tickets, and customer balances.

  2. The SQL archive is automatically uploaded via Google Drive API to the designated business owner's Google Drive storage.

  3. All active cashiers and non-admin operators are automatically logged out to finalize shift balances.

## Part 2: Strategic Product & Enterprise Roadmap (2026 – 2028)

```
+---------------------------------------------------------------------------------------+
|                              RST ELECTRONICS ERP ROADMAP                              |
+---------------------------+---------------------------+-------------------------------+
| PHASE 1: STABILIZATION    | PHASE 2: AUTOMATION & SYNC| PHASE 3: AGENTIC AI HUB       |
| Q4 2026                   | Q1 - Q2 2027              | Q3 - Q4 2027                  |
| - IMEI/Barcode Scanner    | - Dual Google Drive Backup| - Free Agentic AI Engine      |
| - Admin Role Permissions  | - Inventoria Stock Sync   | - Quranic & Hadith Reminders  |
| - Official A4 Quotations  | - Document File Uploads   | - Automated Loss Prevention   |
+---------------------------+---------------------------+-------------------------------+

```

### Phase 1: Local Retail Stabilization & POS Optimization (Q4 2026)

*Target: Immediate execution for high-efficiency store operations in Muscat.*

1. **Integrated Barcode & IMEI Serial Scanner Engine:**

   * Universal barcode scanning in POS, product lookup query, warranty claims, and return receipts.

   * Dual IMEI/Serial tracking per unit for mobile devices and smart security cameras.

2. **Admin-Controlled Role & Visibility Management (RBAC):**

   * Non-developer Admin UI toggles to enable/disable or hide specific software modules (e.g., hide wholesale cost prices from cashier operators).

   * Dedicated access controls: POS & Product Lookup only for standard operators; full financial ledger and invoice re-edits exclusive to Admin login.

3. **Official Oman A4 Tax Quotation & Invoice Engine:**

   * Gorgeous A4 invoice and quote templates compliant with Oman Tax Authority $5\%$ VAT regulations.

   * Re-edit capabilities for Admin users with full audit trail logging (no unauthorized record deletions).

### Phase 2: Automated Cloud Backup & Enterprise Sync (Q1 – Q2 2027)

*Target: Cloud resilience and multi-channel inventory synchronization.*

1. **Automated 24-Hour Twice-Daily Cloud Backup (12:00 PM & 7:00 PM Oman Time):**

   * Direct API synchronization to Google Drive storage before mandatory auto-logout.

   * One-click full system database export/restore (Accounts, Products, Balances, Vouchers).

2. **Document & Media Upload Vault:**

   * Attachment facilities for customer identity documents, repair job photos, purchase vouchers, and supplier invoices.

   * Local directory storage linked to customer and supplier profile cards.

3. **Inventoria Application Integration:**

   * Real-time stock movement synchronization between Inventoria software and RST Enterprise ERP across multiple branch locations (Muscat, Salalah, Sohar).

### Phase 3: Free Agentic AI Hub & Ethical Business Audio Alerts (Q3 – Q4 2027)

*Target: AI-driven predictive logistics and automated business advice.*

1. **Free AI Agent Integration (Gemini 3 Flash & Open Models):**

   * Zero-cost AI diagnostic engine for technician logic board repairs and micro-soldering component troubleshooting.

   * Automated inventory intelligence: dead stock detection, profit margin optimization, and demand forecasting.

2. **Automated 15-Minute Ethical & Quranic Voice Reminders:**

   * Short audio AI alerts featuring Quranic verses, Hadith on fair trade, and Oman market business insights integrated directly into the ERP dashboard.

   * Loss prevention alerts notifying Admin of suspicious price modifications, missing stock, or drawer imbalances.

3. **Multi-Currency & Regional Expansion:**

   * Live dynamic currency switching (OMR, USD, EUR, AED, SAR) with auto-conversion on all printable quotes, thermal receipts, and web invoices.
