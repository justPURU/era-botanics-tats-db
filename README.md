# TAT Analytics Dashboard — Setup Guide

## Files
- `Code.gs` → Google Apps Script backend (paste into Apps Script editor)
- `index.html` → Frontend dashboard (host anywhere: GitHub Pages, Drive, local)

---

## Step 1: Set Up Google Apps Script Backend

1. Go to [script.google.com](https://script.google.com) → **New Project**
2. Delete any existing code, paste the entire content of `Code.gs`
3. At the top of `Code.gs`, update `SHEET_CONFIG`:

```js
const SHEET_CONFIG = [
  { label: "Jan 2025", sheetId: "1AbCdEfGhIjK...", sheetName: "January" },
  { label: "Feb 2025", sheetId: "1AbCdEfGhIjK...", sheetName: "February" },
];
```

- `sheetId` = the long ID in your Google Sheet URL: `docs.google.com/spreadsheets/d/**THIS_PART**/edit`
- `sheetName` = exact tab name in the spreadsheet

4. Update `HOLIDAYS` array for your holiday list
5. Adjust `CUTOFF_HOUR` (default: 15 = 3 PM)

### Deploy as Web App
1. Click **Deploy → New Deployment**
2. Type: **Web App**
3. Execute as: **Me**
4. Who has access: **Anyone** (or "Anyone with Google account" for internal use)
5. Click **Deploy** → **Authorize** → Copy the Web App URL

---

## Step 2: Required Sheet Column Headers

Your Google Sheets must have these exact header names in row 1:

| Column | Description |
|--------|-------------|
| `AWB_No` | Unique shipment ID |
| `Order_Date` | Date + time order was placed |
| `Assigned_At` | Date + time assigned to warehouse |
| `Delivered Date` | Actual delivery date |
| `Pickup Date` | Actual pickup date |
| `Status` | Current shipment status |
| `Shipping_City` | Destination city |
| `Shipping_State` | Destination state |
| `Shipping_Zip_Code` | Destination pincode (for tier mapping) |
| `Client_Location` | Warehouse name |

---

## Step 3: Host the Frontend

### Option A: GitHub Pages (recommended)
1. Create a new GitHub repository
2. Upload `index.html` → commit to `main` branch
3. Go to **Settings → Pages → Source: main branch / root**
4. Your dashboard is live at `https://yourusername.github.io/repo-name`

### Option B: Local
Just open `index.html` directly in a browser — no server needed.

### Option C: Google Drive
Upload `index.html` to Drive, right-click → Share → Anyone with link → Open.

---

## Step 4: Configure the Dashboard

1. Open the dashboard in your browser
2. Click **⚙ Config** (top right)
3. Paste your **Apps Script Web App URL**
4. Add your months: Label = "Jan 2025", SheetID|TabName = `1AbCd...|January`
5. Click **Save & Close**
6. Click **Sync Data**

Config is saved in your browser's localStorage — no login needed.

---

## Tier Mapping (Pincode-Based)

Tiers are assigned by the first 3 digits of the pincode.

To customize, edit these arrays in `Code.gs`:
```js
const TIER1_PINCODES = ["110","400","500",...]; // Delhi, Mumbai, Hyderabad...
const TIER2_PINCODES = ["530","440","395",...]; // Other major cities
// Everything else = Tier 3
```

---

## Adding New Months

1. Add a new row to `SHEET_CONFIG` in `Code.gs`
2. Re-deploy the Apps Script (or use same deployment)
3. In the dashboard Config, add the new month row
4. Done — it appears in the Sync dropdown automatically

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "No Apps Script URL" | Open Config, paste your Web App URL |
| "Sheet not found" | Check sheetId and sheetName match exactly |
| "Missing columns" | Ensure all 10 column headers exist in row 1 |
| CORS error | Re-deploy Apps Script as "Anyone" access |
| Blank charts | Sync data first — charts render after data loads |
| Old data showing | Click Sync to refresh; cached data shown on load |
