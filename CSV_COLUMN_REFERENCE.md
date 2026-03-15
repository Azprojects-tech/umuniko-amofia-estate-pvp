# CSV Column Reference - Quick Checklist

**Use this when importing CSV to Google Sheets for new portals**

---

## ✅ REQUIRED 12 COLUMNS (In Order)

```
A  │ ParcelID          │ Pre-filled from GeoJSON (DO NOT CHANGE)
B  │ Status            │ Pre-filled "Available" (client can change)
C  │ Purchaser_Name    │ Empty (client fills: buyer name)
D  │ Phone_Number      │ Empty (client fills: 234-country-code format)
E  │ Email_Address     │ Empty (client fills: buyer email)
F  │ Purchase_ID       │ Empty (CRUCIAL: client fills: reference ID)
G  │ Display_Preference│ Empty (dropdown: none/name/id)
H  │ Area_SqM          │ Pre-filled from GeoJSON (ROUNDED to 2 decimals)
I  │ Price_Naira       │ Empty (client fills: sale price in naira)
J  │ Sale_Date         │ Empty (client fills: YYYY-MM-DD format)
K  │ Service_Fee_Due   │ Empty (client fills: fee amount)
L  │ Payment_Status    │ Empty (dropdown: pending/paid)
```

---

## 🚨 CRITICAL WARNINGS

### Column A (ParcelID)
```
✅ DO: Keep exactly as generated from GeoJSON
❌ DON'T: Change, modify, or add prefixes
WHY: Portal matches parcels on map using this ID
IMPACT: If changed → parcel polygons won't display
```

### Column H (Area_SqM)
```
✅ DO: Keep as pure numbers (450.57)
✅ DO: Accept 2-decimal rounding already done
❌ DON'T: Convert back to string
❌ DON'T: Add "sqm" after numbers
❌ DON'T: Round again
WHY: Used in area calculations and analytics
IMPACT: If text → formulas break, sorting fails
```

### Column F (Purchase_ID) - MOST CRITICAL
```
✅ DO: Client MUST define naming convention
✅ DO: Keep consistent format across all rows
✅ DO: Use something meaningful (UD_2024_001, PL_P01, etc)
❌ DON'T: Leave blank (you can, but tracking is lost)
WHY: Links purchase agreement to property
IMPACT: Without this → referral system broken, sales tracking broken
```

---

## 📋 DATA TYPE REQUIREMENTS

| Column | Type | Example | Restrictions |
|--------|------|---------|--------------|
| ParcelID | Text | P66, 1, T | Must match GeoJSON exactly |
| Status | Dropdown | Available | Only: Available, Reserved, Sold |
| Area_SqM | Number | 450.57 | No text, no units, 2 decimals |
| Price_Naira | Number | 3500000 | No commas, no currency symbols |
| Phone_Number | Text | 2347025005871 | Include country code, no spaces |
| Email_Address | Text | info@domain.com | Valid email format |
| Sale_Date | Date | 2024-02-20 | YYYY-MM-DD format |
| Service_Fee_Due | Number | 81000 | Pure number |
| Purchaser_Name | Text | John Doe | Any text format |
| Purchase_ID | Text | UD_2024_001 | Client defines (any format OK) |
| Display_Preference | Dropdown | none | Only: none, name, purchase_id |
| Payment_Status | Dropdown | pending | Only: pending, paid |

---

## 🔄 WORKFLOW REMINDER

```
Step 1: Client provides GeoJSON
         ↓
Step 2: AI validates and extracts
         ↓
Step 3: CSV generated with columns A,B,H pre-filled
        All other columns: EMPTY (client fills in)
         ↓
Step 4: Create Google Sheet named: MASTER_TEMPLATE_DATA
         ↓
Step 5: Rename tab to: ESTATE_NAME_PARCELS (exact match!)
         ↓
Step 6: Import CSV starting ROW 2 (headers already in Row 1)
         ↓
Step 7: Client fills empty columns with property data
         ↓
Step 8: Portal pulls live data from Google Sheet
```

---

## ⚠️ DATA TYPE CONVERSION - DO NOT REPEAT

### AI ALREADY DID THIS:
```
✅ Extracted Text from GeoJSON
✅ Renamed text → Text (capital T)
✅ Converted Shape_Area string → number
✅ Rounded Shape_Area to 2 decimals
✅ Built CSV with proper column order
✅ Set up column types in Google Sheet
```

### YOU SHOULD NOT:
```
❌ Extract/re-convert any data
❌ Rename properties again
❌ Round numbers again
❌ Reformat coordinates
❌ Change column order
❌ Add/remove columns
```

---

## 🎯 IMPORT CHECKLIST

```
Before importing CSV to Google Sheet:

☐ CSV file created (ESTATE_NAME_TEMPLATE.csv)
☐ File has 100+ rows (parcels)
☐ File has 12 columns (A-L)
☐ ParcelID column has values (P1, P2, etc)
☐ Area_SqM column has numbers (450.57, not "450.57 sqm")
☐ Google Sheet created: MASTER_TEMPLATE_DATA
☐ Tab renamed: ESTATE_NAME_PARCELS (case-sensitive!)
☐ Row 1 has headers
☐ Ready to import starting ROW 2
```

---

## 🛠️ POST-IMPORT (Client Responsibility)

After CSV imported, client should fill:

```
1. Purchase_ID (Column F) - MOST IMPORTANT
   Format: Whatever they use (UD_2024_001, PL_001, etc)
   
2. Purchaser_Name (Column C)
   Buyer's full name
   
3. Phone_Number (Column D)
   Contact: +234-prefixed number
   
4. Email_Address (Column E)
   Buyer's email
   
5. Price_Naira (Column I)
   Full price: 3500000 (not "3.5M")
   
6. Status (Column B) - Can update anytime
   Available → Reserved → Sold
   
7. Payment_Status (Column L)
   pending → paid
```

**All other columns are optional but recommended.**

---

## 🔍 VERIFICATION

After CSV import, verify in Google Sheet:

```
☐ 100 rows of data (check row 101)
☐ All 12 columns present (A-L)
☐ ParcelID shows correct values (T, P66, P65...)
☐ Status shows "Available" (pre-filled)
☐ Area_SqM shows numbers (450.57, not string)
☐ Empty columns ready for client data
☐ Sheet name matches code (UMUNIKO_AMOFIA_PARCELS)
☐ Sheet publicly shared ("Anyone with link")
```

---

## 📌 QUICK REFERENCE FORMULA

### If client asks: "What columns do I need to fill?"

**Answer:**
```
Pre-filled by AI:      A (ParcelID), B (Status), H (Area_SqM)
Client MUST fill:      F (Purchase_ID)
Client SHOULD fill:    C, D, E, I, J
Client CAN fill:       G, K, L (optional but recommended)

Minimum to work:       Just ParcelID (A) and Status (B)
Best practice:         All columns filled for full functionality
```

---

## 🚀 NEXT PORTAL - COPY THESE EXACT VALUES

When you start next portal, use these without modification:

```javascript
// CSV Column Order (NEVER CHANGE):
A: ParcelID
B: Status  
C: Purchaser_Name
D: Phone_Number
E: Email_Address
F: Purchase_ID
G: Display_Preference
H: Area_SqM
I: Price_Naira
J: Sale_Date
K: Service_Fee_Due
L: Payment_Status

// Data Types (NEVER CHANGE):
ParcelID: Text
Status: Dropdown (Available, Reserved, Sold)
Area_SqM: Number (2 decimals)
Price_Naira: Number (no symbols)
Phone_Number: Text (with country code)
Email: Text (valid format)
Date: Date (YYYY-MM-DD)
```

---

**Version:** 1.0  
**Last tested:** Umuniko Amofia Estate (March 15, 2026)  
**Next update:** After next portal completed
