# Latest patch: customers, stock tabs, compact challan and SO registry

See PATCH_UPDATE.txt for installation and the scope of this changed-files-only
release. It merges into the previous Balaka project. Dates display DD/MM/YYYY;
stored ISO dates are retained. SO portal accounts supply the invoice officer
selector, with server validation. Mother settings use the top-right gear;
sidebar settings are dealer-specific. Remote payment logos have branded text
fallbacks; bank badges are not official artwork when a logo cannot load.
71 automated checks and production build passed; browser QA unavailable.

# Latest: reference stock movement layout
See UPDATE_MOVEMENT_LAYOUT.txt. Eight-column movement table now includes saved
unit rates, line amounts, shop/company and godown, signed color-coded quantities
and clickable source references. Multiple godowns have a selector; one godown
is displayed automatically. Product search and export/print filters are retained.
Build and 70 automated tests passed. Browser visual/touch QA was unavailable.

# Latest update: SO performance and compact records

SO portal opens My performance by default. Choose any month (including previous
years) for posted net sales, separate approved payment collections and cash at
sale. Admin: SO Control > Performance > select officer. Only exact saved soId
matches count; old records with no officer ID are not assigned by name. Only
records in the selected dealer are available. Pending and rejected submissions
are excluded. Sales use current returns/corrections and exclude cancelled
invoices, grouped by invoice date. Collection uses payment date and does not
include cash already recorded on invoices. Cash at sale is a separate metric.
No new login rights are granted to SO users.

Invoice History, Dashboard recent invoices, Payment history, SO requests and
admin Approvals use compact responsive card lists. Invoice/payment requests
have separate filters. Long lists are paginated. Existing invoice details,
adjustments, print, payment edits/deletes and approval actions remain available.
Payment entry is opened with New collection (or Edit) instead of occupying a
permanent column. Party-ledger collection links open the preselected form.

This ZIP also includes the previous organized sidebar, finance, DO/challan,
settings and same-Wi-Fi changes. See UPDATE_PERFORMANCE_COMPACT.txt.

Validation: 67 automated tests passed and production build passed. Browser
visual/touch QA was not available in this environment. No live business database
or credentials were used for development checks.

# Latest update: organized sidebar and finance

See UPDATE_SIDEBAR_FINANCE_WIFI.txt for this release and replacement steps.

Inventory: Stock, Challan, DO Entry, Undelivery.
Finance: Payment, Expense, Company Ledger, Shop Ledger, Transaction Report,
History, Report. Party and Settings have their own expandable submenus.
Sidebar remembers expanded groups and opens the selected page’s group.

Expense and Company Credit support create/edit/delete with audited reasons.
Company Ledger follows the supplied SPA’s Credit minus Debit convention:
challan cost and side DO sales are debit; DO paid, side company-bank payments
and manual company credits are credit. Positive balance means advance.
Shop Ledger follows Debit minus Credit and carries earlier transactions into
its opening balance. Transaction Report lists entries without adding unrelated
sales/payment amounts together. Report has Sales, Party Collections and Stock
Movement, with date filters, export and print. Sales reflect current adjusted
invoice values grouped by invoice date. Effective stock movements exclude
opening/manual stock edits; this report is not a complete inventory audit log
or a profit-and-loss statement. History retains existing invoice workflows.

Same Wi-Fi: run npm run dev and open the printed Network URL on your phone.
If necessary run ipconfig on Windows and use http://<Wi-Fi IPv4>:5173.
Allow Node.js on Private networks in Windows Firewall. Keep the computer and
terminal running. Open that Network URL on the computer before copying SO
portal links so links use the network IP instead of localhost.
HTTP LAN access is for trusted local Wi-Fi; do not enable router port forwarding.
Existing owner and SO accounts continue to use the same local SQLite database.

Validation: 63 automated checks passed, including memory-database LAN Host/Origin
proxy requests, financial isolation and native page render checks. Production
build passed. Browser visual/touch QA was unavailable in the execution environment;
actual reachability on your Wi-Fi depends on the router and firewall.

# Balaka Business — working React application

Native React screens for Dashboard, Invoice, Party (Regular / Side Delivery), Payment and History, backed by a working Node.js + SQLite server. No legacy HTML wrapper, feature-summary screens, sample account or cloud configuration is required.

## Settings and godowns

Use the Settings navigation tab for Mother settings, Dealer settings and Godown
settings. Mother values are the defaults; dealer overrides take precedence.
Each dealer may keep its own godown configuration, contacts and dispatch default.
Godown IDs remain stable when renamed or disabled. Stock entry, invoices, stock
views, printing and SO approvals use this configuration. Existing records retain
their original stock/party links. See UPDATE_COMPLETE.txt for update steps.
Native DO Entry, Undelivered and Challan screens now follow the supplied SPA's
shared pending/FIFO receipt rules. See UPDATE_COMPLETE.txt for the combined
update, operations and validation details.

## DO, Undelivered and Challan

Use the admin navigation to enter DOs and receive challans. A DO adds pending;
a challan clears pending FIFO and adds stock to the chosen godown. Side delivery
shares this pending pool; regular invoices use physical stock without clearing
DOs again. The Undelivered screen shows product totals, clearing traces, pending
correction/reversal and balanced product transfers. DO purchase discounts use
two gross-based CN percentages, independently of retail invoice discounts.

## SO portals and approvals

Open **SO Control → Create SO portal**. Choose the officer’s name, username,
password and phone. Copy the generated `/so/<unique-id>` URL and give it to
that officer. Each account belongs to the company selected when created.

SO users sign in on their separate portal, create regular/side invoices and
submit payments. They can see their own submissions and decisions. They cannot
access admin data exports, settings, stock edits or approvals. Only approved
entries enter the official ledger. Pending invoices do not reserve stock or
receive an official invoice number.

The admin Dashboard shows the approval inbox; **SO Control → Approvals** also
provides review, approve and reject actions. Stock, DO availability and party
balances are checked at approval time, within one transaction. Repeated approval
requests do not post the same entry twice. Rejections require a reason.

Manage access from the officer card to reset a password or disable the portal.
Both actions revoke existing SO sessions. A disabled account cannot submit
entries, and must be enabled before its pending entries can be approved.

## Run locally

Install **Node.js 24** (the version used to test this app), extract this project, then open a terminal in the folder containing `package.json`:

```sh
npm ci
npm run dev
```

Open **http://127.0.0.1:5173/**. Keep the terminal open. The frontend and backend start together. On first use, enter your email and choose a password to create this installation's owner account. Existing Firebase credentials are not used.

Windows users can also double-click `START_WINDOWS.bat` after installing Node.js. It installs packages if necessary and starts both processes.

## Start using the app

1. Open Party and add your Regular or Side Delivery parties, including opening due/advance.
2. Open Invoice → Products & stock, add products, prices and opening quantities for its configured godowns. For side delivery against company DO, add its pending DO quantities in the same dialog.
3. Create and save an invoice. Stock and party balance are changed in one database transaction.
4. Record a collection in Payment. The party balance and Dashboard update immediately.
5. Open History to view/print invoices, record returns, revise invoices, correct prices/items, reverse adjustments, or cancel invoices with a reason.

To import existing records, use **Dashboard → Restore backup** and choose a `BALAKA_GLOBAL_BACKUP` JSON exported from your existing app. The import preserves company IDs, record IDs, counters, histories and unknown fields. It saves a checkpoint of this app's previous data first. Download prior checkpoints from **Restore checkpoints**. The importer has not been verified against a current real business backup, which was not included in the source archive; compare your imported totals before treating this as your authoritative ledger.

## Included functionality

- Dashboard: real stock/party balances, invoice count and net sales, date-based collections/expenses, 7/30/90-day sales and collections chart, highest due accounts, notes, backup/restore.
- Invoice: Regular and Side Delivery; company DO or godown source; Chakaria/Keranihat availability; default prices; multiple items; two sequential CN discounts; product/instant discounts; cash/due; sales officer metadata; manual sale; automatic numbering; tab-session draft; save, preview, print and browser Save as PDF.
- Party: Regular/Side tabs; add/edit; search; due/advance filters; protected deletion; audited balance adjustment; dated running ledger; CSV and printing.
- Payment: Regular/Side entries; Cash/bKash/Nagad/Bank/Other; bank reference/company account flag; advance calculation; edit/delete with reason; history search/date/party filters; CSV; printed receipts; small-balance write-off up to ৳500 and reversal.
- History: search, type/month/date filters, pagination, net totals, original invoice details, printable A5 invoice pages, revision history, returns/reversal, price corrections/reversal, item corrections/reversal, cancellation with stock/DO rollback and retained audit record.
- Multi-company selector and settings, business contact details, SO names/phones, light/dark theme, responsive sidebar/forms and mobile card lists.

Native browser dialogs provide focus handling. Amounts are BDT. CSV exports guard spreadsheet formula injection. New ordinary invoice discounts follow the original sequential arithmetic; revision/item correction totals follow the original two-decimal adjustment rules.

## Data and security

- Official business data is stored in **`data/balaka.sqlite`**, not browser localStorage. Restarting the server or browser does not clear it. While the server is running SQLite may also maintain `-wal` and `-shm` files; do not delete them.
- Passwords use salted scrypt hashes; authenticated sessions use HttpOnly, SameSite cookies. Passwords are not shipped in source. Server actions validate inputs and use transactions, stale-version checks and idempotency IDs.
- The backend binds to `127.0.0.1`; npm run dev exposes the frontend on your local network. No Firebase/Supabase credentials or cloud-write code are included.
- Browser sessionStorage holds only the current invoice draft; localStorage holds the theme preference. Backup restoration affects only this app's local database.
- Download regular backups to another location. A restore checkpoint in the same database is useful for undoing a restore but is not an independent disaster-recovery backup.
- To move this installation, stop it and copy the **whole `data/` folder**, or export/restore a JSON backup. New JSON backups include SO accounts (salted password hashes) and approval history, but exclude the owner password and all sessions. Keep backups private. Restoring an older business-only backup keeps existing SO accounts, closes pending requests and signs SO users out, so old requests cannot silently post into restored accounts. Restore checkpoints include the previous SO configuration too.

## Production build / local server

```sh
npm run build
npm start
```

Open **http://127.0.0.1:3001/**. The Node server serves the React build and API together. This app requires a persistent backend and disk; uploading only `dist/` to a static hosting service will not provide the database/API. SO login and admin approvals are included. Public hosting, HTTPS/reverse-proxy configuration and Android are not configured by this local update. A localhost portal URL is only reachable on that computer; officers on the same trusted Wi-Fi can use the development Network URL; internet access requires a reachable HTTPS deployment.

Optional server environment variables: `PORT` (default `3001`), `HOST` (default `127.0.0.1`), `BALAKA_DB` (absolute SQLite file path), `COOKIE_SECURE=1` only when deployed behind HTTPS. Keep the server's Host/Origin consistent when placing it behind a proxy. Do not expose the development server as public production hosting.

## Verification

```sh
npm test
npm run build
```

Tests cover authentication/HTTP writes, stock and balance changes, DO fulfilment/cancellation, returns, corrections/revisions/reversals, payment edits/deletion, write-offs, preservation of imported fields, stale writes, idempotent retries, database persistence, backup checkpoints, SO account migration/authentication, role and company isolation, submission/approval idempotency, delayed stock validation, SO backup/checkpoints and rendering business/approval pages and print output. Browser interaction, responsive screenshots and actual printer/PDF layout could not be exercised in this environment; these still need acceptance testing on your devices.

## Source layout

- `src/pages/`: business screens, SO Control and the separate SO portal.
- `src/components/`: shadcn/ui primitives, approval review, searchable controls and print layouts.
- `shared/domain.mjs`: validated accounting/inventory actions shared with tests.
- `server/`: authenticated API and persistent SQLite transactions.
- `tests/`: domain, persistence, HTTP and React rendering checks.

Shared UI uses Tailwind CSS v4 and local shadcn/ui components. See UI_COMPONENTS.md.
"# Balaka-Enterprise-apk" 
