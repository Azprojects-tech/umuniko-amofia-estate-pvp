# PVP Portal - Find & Replace Checklist
**Rapid Estate Name Replacement Guide**

Use this document to quickly replace all custom values when building a new portal.

---

## 🔴 CRITICAL: Estate Name References
**These will confuse clients if not updated.**

### In `index.html`:

| Line(s) | Search For | Replace With | Impact |
|---------|-----------|--------------|--------|
| ~501 | `Silverstone Estate - Real Estate Mapper` | `[NEW ESTATE] - Real Estate Mapper` | Page title |
| ~1169 | `Silverstone Estate` | `[NEW ESTATE]` | Main heading (H1) |
| ~1204 | `WELCOME TO PINELEAF ESTATE` | `WELCOME TO [NEW ESTATE]` | Welcome modal |
| ~1987 | `Viewing Silverstone Estate - Ogbeke` | `Viewing [NEW ESTATE] - [LOCATION]` | Zoom popup |

### In `estate-config.js`:

| Line(s) | Search For | Replace With | Impact |
|---------|-----------|--------------|--------|
| ~5 | `name: "Silverstone Estate Ogbeke"` | `name: "[NEW ESTATE]"` | Config object |
| ~6 | `location: "Ogbeke, Ogidi"` | `location: "[LOCATION]"` | Location display |

---

## 🟠 Contact Information References

### In `index.html`:

| Line(s) | Search For | Replace With | Context |
|---------|-----------|--------------|---------|
| ~4061 | `+234 814 704 2804` | `[NEW PHONE]` | WhatsApp button message |
| ~4079 | `+234 814 704 2804` | `[NEW PHONE]` | Contact info panel |
| ~650 | `jeff@realty.com` | `[NEW EMAIL]` | Footer email |

### In `estate-config.js`:

| Line(s) | Search For | Replace With | Context |
|---------|-----------|--------------|---------|
| ~11-15 | Contact block | New contact details | Phone, email, WhatsApp |
| ~38 | `footer_contact: "...@email.com"` | `[NEW EMAIL]` | Footer |

---

## 🔵 Password & Authentication

### In `index.html`:

| Line(s) | Search For | Replace With | CRITICAL |
|---------|-----------|--------------|----------|
| ~2597 | `'pineleaf2026'` | `'[NEW PASSWORD]'` | ⚠️ If not changed, old portal still accessible! |

---

## 🟢 Google Sheets Configuration

### In `index.html` (Lines 2856-2863):

| Line(s) | Search For | Replace With | Impact |
|---------|-----------|--------------|--------|
| ~2857 | spreadsheetId | `[NEW SHEET ID]` | No data loads if wrong |
| ~2859 | sheetName | `'[NEW SHEET_TAB_NAME]'` | Must match exactly (case-sensitive!) |
| ~2861 | sheetUrl | Updated URL with new ID | For reference only |

### Example:
```javascript
// BEFORE:
const GOOGLE_SHEETS_CONFIG = {
    spreadsheetId: '1kkYjO9PU1LfI...',
    sheetName: 'SILVERSTONE_PARCELS',
    ...
};

// AFTER:
const GOOGLE_SHEETS_CONFIG = {
    spreadsheetId: '1fDTPcYI u6dRf...',
    sheetName: 'UMUNIKO_AMOFIA_PARCELS',
    ...
};
```

---

## 📁 File Renames & Moves

| Current Filename | New Filename | Reason |
|------------------|-------------|--------|
| `Oranwezu.geojson` | `subdivision_data.json` | Standard naming for all portals |
| `SILVERSTONE_TEMPLATE.csv` | `[ESTATE]_TEMPLATE.csv` | Organization |
| `SILVERSTONE_PARCELS` (sheet tab) | `[ESTATE]_PARCELS` | Clarity |

### GeoJSON Reference Update:
In `index.html` line ~1313, the `loadGeoJSON()` function should load:
```javascript
fetch('./subdivision_data.json')  // ← This standard name
```

---

## 📍 Map Coordinates

### In `index.html` (Lines ~1442-1446):

| Item | Search | Replace With | Example |
|------|--------|---------------|---------|
| Center Latitude | `6.46` | New latitude | `6.65` |
| Center Longitude | `7.42` | New longitude | `7.77` |
| Zoom Level | `16` | Optimal zoom | `15-16` |

```javascript
// Example:
map = L.map('map').setView([6.65, 7.77], 15);  // Umuniko Amofia coords
```

---

## 🎯 STEP-BY-STEP REPLACEMENT PROCESS

### Phase 1: Estate Branding (5 minutes)
```bash
1. Open index.html in VS Code
2. Find & Replace (Ctrl+H):
   - Find: "Silverstone Estate"
   - Replace: "[NEW ESTATE]"
   - Click "Replace All" (verify ~8 matches)

3. Find & Replace:
   - Find: "Oranwezu" 
   - Replace: "[NEW LOCATION]"
   - Click "Replace All" (verify ~2-3 matches)

4. Find & Replace:
   - Find: "Pineleaf Estate"
   - Replace: "[NEW ESTATE]"
   - Click "Replace All" (verify 1-2 matches)
```

### Phase 2: Contact Information (2 minutes)
```bash
5. Find & Replace:
   - Find: "+234 814 704 2804"
   - Replace: "[NEW PHONE]"
   - Click "Replace All" (verify 2+ matches)

6. Find & Replace:
   - Find: "jeff@realty.com"
   - Replace: "[NEW EMAIL]"
   - Click "Replace All"
```

### Phase 3: Authentication (1 minute)
```bash
7. Find & Replace:
   - Find: "pineleaf2026"
   - Replace: "[NEW PASSWORD]"
   - Click "Replace All" (verify exactly 1 match)
```

### Phase 4: Google Sheets Config (2 minutes)
```bash
8. Manually update lines 2856-2862:
   - spreadsheetId: [NEW SHEET ID]
   - sheetName: '[NEW_SHEET_TAB_NAME]'
   - sheetUrl: [NEW URL]

9. Open estate-config.js
   - Update: name, location, contact, coordinates
```

### Phase 5: File Renames (1 minute)
```bash
10. Rename files:
    - Oranwezu.geojson → subdivision_data.json
    - SILVERSTONE_TEMPLATE.csv → [ESTATE]_TEMPLATE.csv

11. Right-click sheet tab:
    - Rename: SILVERSTONE_PARCELS → [ESTATE]_PARCELS
```

---

## ✅ VERIFICATION CHECKLIST

After all replacements, verify in browser:

```
Opening Portal (http://localhost:8000):

☐ Title bar shows "[NEW ESTATE] - Real Estate Mapper"
☐ Main heading shows "[NEW ESTATE]"
☐ Welcome modal shows "WELCOME TO [NEW ESTATE]"
☐ Zoom to Subdivision shows: "Viewing [NEW ESTATE] - [LOCATION]"
☐ Admin button prompts for password
☐ Password works (shows admin panel)
☐ 100+ parcels appear on map
☐ Parcels are in correct geographic location
☐ No console errors (F12 → Console tab)
☐ Map loads within 2 seconds

Admin Panel:
☐ Sync button works
☐ Database shows correct parcel data
☐ Colors match Google Sheet status
☐ WhatsApp button shows "[NEW PHONE]"
☐ Footer shows "[NEW EMAIL]"
```

---

## 🚀 WORKFLOW TEMPLATE

**Use this exact sequence for next 3 portals:**

```bash
# Step 1: Clone template
git clone https://github.com/Azprojects-tech/silverstone-estate-pvp.git NEW_ESTATE
cd NEW_ESTATE
git remote set-url origin https://github.com/Azprojects-tech/NEW_ESTATE-pvp.git

# Step 2: Do all Find & Replace (follow Phase 1-5 above)

# Step 3: Verify locally
# Open in browser, test all features

# Step 4: Commit
git add .
git commit -m "Setup: [NEW ESTATE] PVP Portal"
git push -u origin main

# Step 5: Deploy
# Open https://app.netlify.com
# New site from Git → Select repo → Deploy
```

---

## 🚨 COMMON REPLACEMENT MISTAKES

| Mistake | Result | Prevention |
|---------|--------|-----------|
| Didn't update password in line 2597 | Old clients access new portal | Always verify `CLIENT_PASSWORD = '[NEW_PASSWORD]'` exists |
| Replaced Sheet ID but not name | 404 error on sync | Verify BOTH line 2857 (ID) AND 2859 (name) |
| Forgot to update admin password | Security risk | Search for `'pineleaf2026'` - should find 0 results |
| Changed Welcome modal but not zoom popup | Users confused | Update BOTH line 1204 AND line 1987 |
| Forgot GeoJSON rename | Map shows blank | Verify `subdivision_data.json` exists and is referenced in code |

---

## 📋 QUICK REFERENCE (Print This!)

### ALWAYS Replace These 5 Things:

1. **Estate Name** - All H1, titles, welcome modal
2. **Contact Phone** - Admin panel, WhatsApp message  
3. **Admin Password** - Line 2597 ONLY (⚠️ Critical)
4. **Google Sheet ID & Tab Name** - Lines 2857 & 2859
5. **Map Coordinates** - Center and zoom level

### NEVER Change These:

- Column order in CSV (A-L)
- Data types (ParcelID=Text, Area=Number, etc)
- ParcelID values (must match GeoJSON)
- API key (shared across all portals: `AIzaSyCkLewazfYqcQ_llw_Adj_mTNK71T2iRL0`)

---

## 📞 IF YOU GET STUCK

**"Portal loads but shows Pineleaf"**
- Check lines 1204, 1987 - both must be updated

**"403 Forbidden on Admin Sync"**
- Verify Google Sheet ID is correct (line 2857)
- Verify sheet is publicly shared
- Add Netlify domain to Google Cloud API restrictions

**"Parcels not appearing"**
- Verify `subdivision_data.json` exists
- Verify GeoJSON is valid (test in geojson.io)
- Clear browser cache (Ctrl+Shift+Del)

**"Password doesn't work"**
- Check line 2597 - must be exact (including quotes)
- No spaces before/after password

---

**Version:** 1.0  
**Created:** March 15, 2026 (Umuniko Amofia Estate build)  
**Ready for:** Next 3-4 portal builds  
**Estimated time to replace all values:** 10 minutes
