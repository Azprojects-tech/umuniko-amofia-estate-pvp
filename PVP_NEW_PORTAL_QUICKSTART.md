# PVP New Portal - Quick Start Guide
**Version 1.0** | Last Updated: March 15, 2026  
**Authors:** AI Assistant + A&Z Projects Team  
**Purpose:** Rapid deployment of new Parcel Vision Pro portals using proven workflow

---

## 📋 OVERVIEW

This guide covers the **complete workflow** for building a new PVP portal from scratch:

```
Client GeoJSON → CSV Extraction → Google Sheet → GitHub → Netlify → LIVE ✅
      (5 min)      (10 min)        (5 min)     (2 min)   (3 min)
```

**Previous portals built using this method:**
- ✅ Pineleaf Estate (Nibo, Awka)
- ✅ Silverstone Estate (Ogbeke, Ogidi)
- ✅ Umuniko Amofia Estate (Eha-Amufu, Enugu) - March 15, 2026

---

## 🚀 COMPLETE WORKFLOW (Step-by-Step)

### STEP 1: Client Provides GeoJSON (Checklist)

**What to receive from client:**
```
□ GeoJSON file (from CAD/GIS software)
□ Estate name (e.g., "Umuniko Amofia Estate")
□ Location (e.g., "Eha-Amufu Township, Enugu State")
□ Contact details (+234... phone number)
□ Number of parcels (e.g., 100)
□ Admin password (e.g., "Skillnet2026")
```

**CRITICAL: Validate GeoJSON Before Processing**

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "Text": "1",              // ← MUST be "Text" (case-sensitive!)
        "Shape_Area": 450.57      // ← MUST be number, not string!
      },
      "geometry": {
        "type": "Polygon",        // ← MUST be 2D (not Polygon ZM)
        "coordinates": [[[7.77, 6.65], ...]]  // ← Longitude first!
      }
    }
  ]
}
```

**Validation Checklist:**
- [ ] File opens in https://geojson.io
- [ ] Coordinates are [7.XX, 6.XX] (NOT [800000, 850000])
- [ ] Properties include **EXACTLY** `Text` and `Shape_Area` (case-sensitive!)
- [ ] Polygon is 2D (no Z/M values)
- [ ] All features have properties
- [ ] No syntax errors when parsed

**If not valid → Ask client to fix in QGIS/ArcGIS before proceeding**

---

### STEP 2: Extract CSV from GeoJSON (AI Process)

**What I do automatically:**
```python
# Read GeoJSON
for each feature in features:
    ParcelID = feature.properties.Text
    Area = feature.properties.Shape_Area (rounded to 2 decimals)

# Create CSV with standard 12 columns
# Output: ESTATE_NAME_TEMPLATE.csv (100% ready for import)
```

**CSV Generated with ALL 12 Required Columns:**

| Column | Type | Example | Status | Notes |
|--------|------|---------|--------|-------|
| **A: ParcelID** | Text | 1, 2, P66, T | Pre-filled from GeoJSON `Text` | DO NOT CHANGE |
| **B: Status** | Dropdown | Available | Pre-filled "Available" | Can be changed later |
| **C: Purchaser_Name** | Text | John Doe | EMPTY | Client fills in |
| **D: Phone_Number** | Text | 2348164532286 | EMPTY | Client fills in |
| **E: Email_Address** | Text | email@domain.com | EMPTY | Client fills in |
| **F: Purchase_ID** | Text | UD_2024_001 | EMPTY | **CRUCIAL** - Client fills in |
| **G: Display_Preference** | Dropdown | none / name / id | EMPTY | Privacy control |
| **H: Area_SqM** | Number | 450.57 | Pre-filled from GeoJSON | Rounded to 2 decimals |
| **I: Price_Naira** | Number | 3500000 | EMPTY | Client fills in |
| **J: Sale_Date** | Date | 2024-02-20 | EMPTY | Client fills in |
| **K: Service_Fee_Due** | Number | 81000 | EMPTY | Client fills in |
| **L: Payment_Status** | Dropdown | pending / paid | EMPTY | Client fills in |

**⚠️ CRITICAL WARNINGS:**

1. **Column A (ParcelID)** - DO NOT modify
   - Must match exactly with GeoJSON `Text` property
   - Used to sync parcel geometry with Google Sheets data
   - If changed, parcel polygons won't display

2. **Column H (Area_SqM)** - Already rounded to 2 decimals
   - Do NOT convert to string
   - Do NOT add units like "sqm" in the cell
   - Must remain as pure number for calculations

3. **COLUMN F (Purchase_ID)** - THIS IS CRUCIAL
   - Used for purchase tracking and referral system
   - Empty by default - client MUST fill in
   - Format: Whatever client uses (e.g., UD_2024_001)

4. **Data Type Conversions** - Already done by AI
   - ✅ GeoJSON `text` property → renamed to `Text` (capital T)
   - ✅ Shape_Area string → converted to number
   - ✅ Shape_Area → rounded to 2 decimal places
   - ✅ Do NOT repeat these conversions

---

### STEP 3: Create Google Sheet & Import CSV

**Steps:**
1. Create new Google Sheet: `MASTER_TEMPLATE_DATA`
2. Rename tab from `Sheet1` → `ESTATE_NAME_PARCELS`
   - Example: `UMUNIKO_AMOFIA_PARCELS` (exactly, case-sensitive!)
3. Import CSV data starting at Row 2
4. **Share:** File → Share → "Anyone with link" → Viewer
5. **Copy Sheet ID** from URL: `docs.google.com/spreadsheets/d/[COPY_THIS]/edit`

**Google Sheets Configuration for Code:**
```javascript
const GOOGLE_SHEETS_CONFIG = {
    spreadsheetId: '[YOUR_SHEET_ID]',           // ← From URL
    apiKey: 'AIzaSyCkLewazfYqcQ_llw_Adj_mTNK71T2iRL0',  // ← Shared API key
    sheetName: 'ESTATE_NAME_PARCELS',           // ← Exact tab name!
    sheetUrl: 'https://docs.google.com/spreadsheets/d/[YOUR_SHEET_ID]/edit'
};
```

---

### STEP 4: Clone Template & Customize

**Command:**
```bash
git clone https://github.com/Azprojects-tech/silverstone-estate-pvp.git NEW_ESTATE_NAME
cd NEW_ESTATE_NAME
git remote set-url origin https://github.com/Azprojects-tech/NEW_ESTATE_NAME-pvp.git
```

---

### STEP 5: CRITICAL - Replace All Estate References

**This is where most errors occur.** ALL locations below must be updated, or old estate data will display.

#### 🔴 LOCATIONS TO REPLACE - ESTATE NAME

Search for each term and replace with new estate name:

| Location | File | Line(s) | Find | Replace With |
|----------|------|---------|------|--------------|
| **Page Title** | index.html | ~501 | `Silverstone Estate - Real Estate Mapper` | `Umuniko Amofia Estate - Real Estate Mapper` |
| **Main Heading** | index.html | ~1169 | `Silverstone Estate` | `Umuniko Amofia Estate` |
| **Welcome Modal H2** | index.html | ~1204 | `WELCOME TO PINELEAF ESTATE` | `WELCOME TO UMUNIKO AMOFIA ESTATE` |
| **Zoom Message** | index.html | ~1987 | `Viewing Silverstone Estate - Ogbeke` | `Viewing Umuniko Amofia Estate - Eha-Amufu` |
| **Zoom Sub-location** | index.html | ~1987 | `Oranwezueaku` | `Eha-Amufu` |

#### 🟠 LOCATIONS TO REPLACE - CONTACT INFO

| Location | File | Lines | Find | Replace With |
|----------|------|-------|------|--------------|
| **WhatsApp Button 1** | index.html | ~4061 | `+234 814 704 2804` | `+2347025005871` |
| **WhatsApp Button 2** | index.html | ~4079 | `+234 814 704 2804` | `+2347025005871` |
| **Admin Panel Contact** | index.html | Various | Old phone | New phone |
| **Footer Email** | index.html | Footer | `azprojects2025@...` | `azprojectslimited@gmail.com` |

#### 🔵 LOCATIONS TO REPLACE - PASSWORDS & CONFIG

| Location | File | Line | Find | Replace With |
|----------|------|------|------|--------------|
| **Admin Password** | index.html | ~2597 | `'pineleaf2026'` | `'Skillnet2026'` (or client password) |
| **GeoJSON File** | index.html | ~1313 | `'./Oranwezu.geojson'` | `'./subdivision_data.json'` |
| **Sheet ID** | index.html | ~2857 | `'1kkYjO9PU1LfI....'` | Your new Sheet ID |
| **Sheet Name** | index.html | ~2859 | `'Sheet1'` | `'UMUNIKO_AMOFIA_PARCELS'` |

#### 🟡 LOCATIONS TO REPLACE - DATA & GEOMETRY

| Location | File | Line | Find | Replace With |
|----------|------|------|------|--------------|
| **GeoJSON File (rename)** | Project folder | — | `Oranwezu.geojson` | `subdivision_data.json` |
| **Map Center Coords** | index.html | ~1442 | `[6.XX, 7.XX]` | New estate coords |
| **Zoom Level** | index.html | ~1442 | `15` | Adjust as needed |
| **Estate Config Object** | estate-config.js | ~1-50 | All old data | New estate data |

---

## 📊 CSV COLUMN DETAILED REFERENCE

### Generated Columns (AI provides these)

**ParcelID (Column A)**
```
Source: GeoJSON feature.properties.Text
Examples: 1, 2, 3, P66, T, Q
Rule: MUST match GeoJSON exactly
Impact: Syncs parcel polygon to spreadsheet row
```

**Status (Column B)**
```
Type: Dropdown (Available / Reserved / Sold)
Default: "Available"
Client can change anytime
Map colors update in real-time based on this
```

**Area_SqM (Column H)**
```
Source: GeoJSON feature.properties.Shape_Area
Type: NUMBER (not text!)
Example: 450.57 (rounded to 2 decimals)
Pre-converted: ✅ Already done
Do NOT convert again
```

### Client-Editable Columns

**Purchase_ID (Column F)** - ⚠️ MOST CRITICAL
```
Type: Text
Default: Empty
Why: Tracks which parcel ties to which purchase agreement
Example formats: UD_2024_001, PL_001, SILVERSTONE_P01
Client MUST define their naming convention
If left blank: Portal still works but tracking is lost
```

**Purchaser_Name (Column C)**
```
Buyer's full name
Can be modified by admin anytime
Updates in real-time on map popups
```

**Phone_Number (Column D)**
```
Format: Country code + number (e.g., 2348164532286)
Used for WhatsApp integration
Portal can auto-send messages to these numbers
```

**Price_Naira (Column I)**
```
Type: NUMBER
Format: Full price (e.g., 3500000 for ₦3.5M)
NOT "3.5M" or "3500000 naira"
Used in calculations and reports
```

---

## ⚠️ DATA TYPE & CONVERSION CHECKLIST

### DO NOT REPEAT (Already Handled by AI)

- ❌ Do NOT rename `text` → `Text` again
- ❌ Do NOT convert Shape_Area from string to number again
- ❌ Do NOT round Shape_Area to 2 decimals again
- ❌ Do NOT extract/reformatformat coordinates

### WHAT AI DOES AUTOMATICALLY

```python
✅ Validates GeoJSON format
✅ Renames text → Text (capital T)
✅ Converts Shape_Area string → float
✅ Rounds Shape_Area to 2 decimals
✅ Extracts all 100+ parcels
✅ Generates CSV with 12 columns
✅ Pre-fills ParcelID and Area_SqM
✅ Output: Ready-to-import CSV
```

---

## 🔄 DEPLOYMENT CHECKLIST (Final Before Going Live)

### Before GitHub Push
- [ ] All estate names replaced (page title, headings, welcome modal)
- [ ] All contact numbers updated
- [ ] Admin password changed
- [ ] Sheet ID correct in GOOGLE_SHEETS_CONFIG
- [ ] Sheet name exact match (case-sensitive)
- [ ] GeoJSON file renamed to `subdivision_data.json`
- [ ] Map center coordinates updated
- [ ] All "Pineleaf" or previous estate references removed
- [ ] Zoom message updated
- [ ] Location markers showing correct emoji (📍)

### Before Netlify Deployment
- [ ] Git commits clean: `git log --oneline -3`
- [ ] All files staged: `git status` (working tree clean)
- [ ] Pushed to main: `git push origin main`
- [ ] Netlify showing "Deploy in progress"

### After Netlify Goes Live
- [ ] Portal loads at https://ESTATE-NAME.netlify.app
- [ ] No errors in browser console (F12)
- [ ] 100+ parcels appear on map in correct location
- [ ] Welcome modal shows correct estate name
- [ ] Admin password works (Skillnet2026 or custom)
- [ ] Admin Sync button returns data without 403 errors
- [ ] Zoom to Subdivision popup shows correct estate name
- [ ] My Location button shows 📍 icon correctly
- [ ] Parcels change color when status updated in Google Sheet

### Google Cloud API Configuration
- [ ] Domain added to API restrictions: `https://ESTATE.netlify.app/*`
- [ ] Waited 5 minutes for propagation
- [ ] Hard refresh (Ctrl+Shift+R) in browser
- [ ] Console shows: ✅ Public parcel statuses updated

---

## 📌 COMMON MISTAKES TO AVOID

| Mistake | Impact | Fix |
|---------|--------|-----|
| Forgot to update admin password | Old portal accessible to competitors | Search `'pineleaf2026'` and replace |
| Sheet name mismatch: `Sheet1` vs `UMUNIKO_AMOFIA_PARCELS` | 404 error, no data loads | Check tab names match EXACTLY (case!) |
| Sheet ID copied wrong | 403 Forbidden error | Verify full Sheet ID from URL |
| GeoJSON file still named `Oranwezu.geojson` | Map shows blank | Rename to `subdivision_data.json` |
| Forgot "Viewing Pineleaf Estate" in zoom popup | Confuses clients | Update line 1987 in index.html |
| Didn't add Netlify domain to API restrictions | 403 Forbidden on live portal | Add `https://ESTATE.netlify.app/*` to Google Cloud |
| Changed ParcelID column values | Parcels won't sync with map | Keep Column A exactly matching GeoJSON |
| Converted Shape_Area to text (added "sqm") | Calculations break | Keep as pure numbers: 450.57 not "450.57 sqm" |
| Mixed up Password and Sheet ID | Tests fail confusingly | Use password for admin, Sheet ID for Google |

---

## 📂 FILES CREATED DURING BUILD

After completion, you'll have:

```
NEW_ESTATE_FOLDER/
├── index.html                          # Main portal (4895 lines)
├── estate-config.js                    # Estate settings
├── subdivision_data.json                # GeoJSON with 100+ parcels
├── ESTATE_NAME_TEMPLATE.csv            # CSV template (100 rows)
├── netlify.toml                        # Netlify deployment config
├── README.md                           # Project readme
├── .gitignore                          # Git ignore
├── .git/                               # Git repository
└── Documentation/
    ├── SETUP.md
    ├── QUICK_REFERENCE.md
    └── MASTER_GUIDE.md
```

---

## 🚀 RAPID BUILD TIMELINE

**Optimized workflow for next builds:**

| Step | Time | Who | Output |
|------|------|-----|--------|
| Client sends GeoJSON | — | Client | `.geojson` file |
| Validate & extract CSV | 5 min | AI | `.csv` file |
| Create Google Sheet | 5 min | You | Sheet ID |
| Clone template | 2 min | You | New repo folder |
| Update configuration | 10 min | AI | Modified index.html |
| Replace all references | 5 min | AI | Clean code |
| Git commit & push | 2 min | You/AI | GitHub repo |
| Deploy to Netlify | 3 min | Netlify (auto) | Live portal |
| Configure API key | 2 min | You | 403 error solved |
| **TOTAL** | **~34 minutes** | **SEAMLESS** | **✅ LIVE** |

---

## 📞 QUICK REFERENCE BY ISSUE

**"Portal shows Pineleaf"**
- Line 1204: Welcome modal
- Line 1987: Zoom popup
- Line 2597: Password check

**"403 Forbidden error"**
- Google Cloud → Credentials → Add Netlify domain
- Wait 5 minutes → Hard refresh (Ctrl+Shift+R)

**"Parcels not showing"**
- Check Sheet ID correct in line 2857
- Check Sheet tab name matches: `UMUNIKO_AMOFIA_PARCELS`
- Sheet must be publicly shared

**"Password doesn't work"**
- Line 2597: Check exact value (e.g., `'Skillnet2026'`)
- Quotes matter: Single quotes only

**"Location button shows ??**
- Lines 1949, 1960: Should show `📍` icon
- Use proper Unicode, not corrupted characters

---

## 📋 PRE-BUILD CHECKLIST (For Next Portal)

Before starting the build, client should provide:

```
☐ GeoJSON file (validated, WGS 84, 2D Polygon)
☐ Estate name (official spelling)
☐ Location description (township, state)
☐ Admin phone number
☐ Admin password (secure, memorable)
☐ Number of parcels
☐ Map coordinates (center point)
☐ Google Sheets API enabled (they may need to open Google Cloud)
```

---

## 🎯 SUCCESS CRITERIA

Portal is READY TO DELIVER when:

✅ Welcome modal shows correct estate name  
✅ All 100+ parcels display on map  
✅ Parcels colored by status (Available=green, Reserved=yellow, Sold=red)  
✅ Admin password unlocks admin panel  
✅ Admin Sync button works (no 403 errors)  
✅ Google Sheet updates sync to map in real-time  
✅ No console errors (F12 developer tools)  
✅ Mobile view works (landscape mode)  
✅ No references to previous estates (Pineleaf, Silverstone)  

---

**Last Updated:** March 15, 2026  
**Next Build:** [Date TBD]  
**Ready for:** Rapid deployment of remaining estates in pipeline
