# 🔥 FireGuard — Fire Extinguisher Management System

A fully offline-capable, single-file web application for managing fire extinguishers across **700+ sites**. No backend required — runs entirely in the browser using `localStorage` for persistence.

---

## ✨ Features

### Asset Management
- Register extinguishers with unique auto-generated codes (`FE-{Region}-{Type}-{Site}-{Seq}`)
- Full asset registry with Type, Capacity, Region, Entity, Center, Location, Zone, Vendor, Manufacturer
- **Entities:** 1P-GYM, 2P-GYM, 3P-GYM, 1P-GX, 2P-GX, Fitso, EBO, Warehouse
- **Types:** ABC 6kg, Modular 5kg, CO2 4.5kg, Fire Ball

### QR Codes
- Every extinguisher gets a unique QR code that encodes a **direct action URL**
- Anyone can scan the QR with their phone camera — no app required
- Scanning opens a mobile-friendly action page to:
  - ✅ Move to Vendor for Refill / HPT
  - ✅ Mark as Received from Vendor (with new dates)
  - ✅ Report Defect / Issue

### Dashboard
- Live fleet health stats (Active / Refill Required / With Vendor / Expired / Issues)
- Auto-alert banners for overdue and upcoming units
- Bar charts by Status and Type
- **Filters:** Type, Capacity, Region, Entity, Center/Site, Building, Vendor, Manufacturer, Status

### Vendor Operations (4-tab workflow)
| Tab | Description |
|-----|-------------|
| Due for Service | HPT or Refill due ≤30 days — Accept & Send button |
| Pending Acceptance | Units flagged Refill Required / Expired |
| With Vendor | Currently out — Mark Received or Not Received |
| Completed | Returned & compliant units |

- Accept & Send modal: service type (Refill / HPT), vendor name, pickup date, expected return
- Mark Received: new HPT date, new Refill date → status → **Compliant**
- Not Received: reason logging, unit returns to Refill Required queue

### Issue Reports (3-stage workflow)
| Stage | Action |
|-------|--------|
| Open | Report logged, Order Part button |
| Part Ordered | Parts, supplier, ETA tracked |
| Closed | Invoice number + file upload required |

- Filters: Issue Type, Severity, Entity, Center/Site
- Invoice upload (PDF / image) stored in browser
- Closed issues viewable with invoice preview

### Alerts
- Auto-expiry: units past PT or Refill date auto-move to Expired / Refill Required
- **30-day advance alerts** for upcoming HPT and Refill dates
- Tabbed alert views: All / Expired PT / Refill Due / Upcoming / Issues

### Bulk Operations
- **Bulk CSV upload** with downloadable template + sample file
- Row-level validation with inline error display
- **Bulk QR Print**: select multiple units → live preview → Print as PDF (A4, 4-per-row)
- Each printed QR card shows: code, site, HPT due, refill due, status, entity

### Search
- Asset Registry: Filter Search tab (Type/Region/Entity/Center/Status + text) + QR Code Lookup tab
- Active filter pills with individual clear buttons
- QR lookup shows instant result card with quick-action buttons

### Export
- **Dashboard:** CSV export of full fleet
- **Vendor Ops:** Excel export with 4 sheets (Due / Pending / With Vendor / Completed)
- **Issues:** Excel export with 4 sheets (Open / Part Ordered / Closed / All)

---

## 🚀 Getting Started

### Option 1 — Open directly
```bash
open index.html   # macOS
start index.html  # Windows
xdg-open index.html  # Linux
```

### Option 2 — Serve locally (recommended for QR scan links)
```bash
npx serve .
# or
python3 -m http.server 8080
```
Then open `http://localhost:8080` in your browser.

### Option 3 — Deploy to any static host
Upload `index.html` to:
- **GitHub Pages** (free, in this repo — see below)
- **Netlify** — drag & drop
- **Vercel** — `vercel --prod`
- Any web server (Nginx, Apache, S3 static hosting)

---

## 📱 QR Code Scanning

When deployed to a web server, every QR code encodes:
```
https://yoursite.com/index.html?scan=FE-N-ABC-001-1001
```

Scanning with any phone camera opens a mobile-optimised action page — no login needed. Field staff can:
- Move extinguisher to vendor with service details
- Mark as received back with new dates
- Report a defect with checklist and severity

---

## 📋 Bulk Upload Template

Download the template from **Bulk Upload** in the sidebar. Required columns:

| Column | Required | Values |
|--------|----------|--------|
| `type` | ✅ | ABC 6kg, Modular 5kg, CO2 4.5kg, Fire Ball |
| `capacity` | ✅ | 1 kg, 2 kg, 4 kg, 6 kg, 9 kg, 12 kg, 25 kg, 2 L, 6 L, 9 L |
| `region` | ✅ | North, South, East, West |
| `entity` | ✅ | 1P-GYM, 2P-GYM, 3P-GYM, 1P-GX, 2P-GX, Fitso, EBO, Warehouse |
| `center` | ✅ | Free text — site name |
| `location` | ❌ | Building / Floor |
| `zone` | ❌ | Room or area |
| `install_date` | ✅ | YYYY-MM-DD |
| `pt_due_date` | ✅ | YYYY-MM-DD |
| `refill_due_date` | ✅ | YYYY-MM-DD |
| `vendor` | ❌ | Supplier name |
| `manufacturer` | ❌ | Manufacturer name |

---

## 🗂 Data Storage

All data is stored in **browser localStorage**:
- `fe_data` — extinguisher records
- `fe_issues` — issue reports

> ⚠️ Data is local to the browser. For multi-user / multi-device use, replace localStorage with a backend API (see [Roadmap](#roadmap)).

---

## 🛣 Roadmap

- [ ] Backend API (Node.js / Supabase / Firebase) for multi-user sync
- [ ] User authentication & role-based access (Admin / Vendor / Viewer)
- [ ] Email / SMS alerts for overdue extinguishers
- [ ] Native camera QR scanning (no manual URL entry)
- [ ] Photo capture on issue reports
- [ ] Service history timeline per unit
- [ ] Compliance certificate PDF generation
- [ ] IoT sensor integration (pressure, tamper, weight)
- [ ] Mobile PWA (installable on phone)

---

## 🧱 Tech Stack

| Layer | Technology |
|-------|-----------|
| UI | Vanilla HTML + CSS + JS (zero framework) |
| QR Generation | [qrcode.js](https://github.com/davidshimjs/qrcodejs) |
| QR Scanning | [jsQR](https://github.com/cozmo/jsQR) |
| Excel Export | [SheetJS (xlsx)](https://sheetjs.com/) |
| Storage | Browser localStorage |
| Fonts | Google Fonts — DM Sans, DM Mono |

---

## 📜 License

MIT — free to use, modify, and deploy.
