# Client Portal Formula Reference

**Spreadsheet:** Copy of Client Portal
**Spreadsheet ID:** `1Dz6zUlYjPvWl5pHqu0mkMUgDdxCJ7ZbFPMoI2qKnFE0`
**Last Updated:** 2026-01-10 (Rebuilt from scratch)

---

## KPI Conversion Tab

**All formulas start in Row 2** and use ARRAYFORMULA to auto-populate all rows with data.

### Data Import (from Form Intake)

| Cell | Column | Name | Formula |
|------|--------|------|---------|
| A2 | A | Submission ID | `=ARRAYFORMULA(IF('Form Intake'!A2:A="","",'Form Intake'!A2:A))` |
| B2 | B | Full Name | `=ARRAYFORMULA(IF('Form Intake'!D2:D="","",'Form Intake'!D2:D))` |
| C2 | C | Age | `=ARRAYFORMULA(IF('Form Intake'!E2:E="","",'Form Intake'!E2:E))` |
| D2 | D | Gender | `=ARRAYFORMULA(IF('Form Intake'!I2:I="","",'Form Intake'!I2:I))` |
| E2 | E | Height (cm) | `=ARRAYFORMULA(IF('Form Intake'!F2:F="","",'Form Intake'!F2:F))` |
| F2 | F | Current Weight (kg) | `=ARRAYFORMULA(IF('Form Intake'!J2:J="","",'Form Intake'!J2:J))` |
| G2 | G | Goal Weight (kg) | `=ARRAYFORMULA(IF('Form Intake'!K2:K="","",'Form Intake'!K2:K))` |
| H2 | H | Activity Level | `=ARRAYFORMULA(IF('Form Intake'!W2:W="","",'Form Intake'!W2:W))` |
| I2 | I | Training Days | `=ARRAYFORMULA(IF('Form Intake'!T2:T="","",'Form Intake'!T2:T))` |

### Program Settings

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| J2 | J | Program Weeks | `=ARRAYFORMULA(IF(A2:A="","",12))` | Hardcoded to 12 weeks |

### Phase & Goals

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| K2 | K | Phase | `=ARRAYFORMULA(IF(A2:A="","",IF(F2:F>G2:G,"CUTTING",IF(F2:F<G2:G,"BULKING","MAINTAINING"))))` | Compares current vs goal weight |
| L2 | L | 12-Week Goal | `=ARRAYFORMULA(IF(A2:A="","",IF(K2:K="CUTTING",F2:F-6.8,IF(K2:K="BULKING",F2:F+4.5,F2:F))))` | Realistic 12-week target: -6.8kg for cutting, +4.5kg for bulking |

### Body Composition

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| M2 | M | BMI | `=ARRAYFORMULA(IF(A2:A="","",ROUND(F2:F/((E2:E/100)^2),1)))` | Weight / (Height in meters)^2 |

### Protein Calculations

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| N2 | N | Protein Multiplier | `=ARRAYFORMULA(IF(A2:A="","",IF(M2:M>=35,1.6,2.0)))` | 2.0 g/kg standard, 1.6 g/kg for BMI≥35. **Format: 0.0 (1 decimal)** |
| O2 | O | Protein Target (new) | `=ARRAYFORMULA(IF(A2:A="","",ROUND(F2:F*N2:N,0)))` | Current Weight × Multiplier |

**Protein Multiplier Rules:**
- Standard (BMI < 35): **2.0 g/kg**
- Overweight (BMI ≥ 35): **1.6 g/kg**

### Energy Calculations

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| P2 | P | BMR | `=ARRAYFORMULA(IF(A2:A="","",IF(D2:D="Male",(10*F2:F)+(6.25*E2:E)-(5*C2:C)+5,(10*F2:F)+(6.25*E2:E)-(5*C2:C)-161)))` | Mifflin-St Jeor equation |
| Q2 | Q | TDEE | `=ARRAYFORMULA(IF(A2:A="","",P2:P*IF(ISNUMBER(SEARCH("Sedentary",H2:H)),1.2,IF(ISNUMBER(SEARCH("Lightly",H2:H)),1.375,IF(ISNUMBER(SEARCH("Moderately",H2:H)),1.55,IF(ISNUMBER(SEARCH("Active",H2:H)),1.725,1.55))))))` | BMR × Activity multiplier |
| R2 | R | Weight Change (kg) | `=ARRAYFORMULA(IF(A2:A="","",F2:F-G2:G))` | Current - Goal (positive = cutting needed) |
| S2 | S | Weekly Target (kg) | `=ARRAYFORMULA(IF(A2:A="","",R2:R/J2:J))` | Weight Change / Program Weeks |
| T2 | T | Daily Adjustments (cal) | `=ARRAYFORMULA(IF(A2:A="","",S2:S*1100))` | Weekly target × 1100 cal per kg |
| U2 | U | Calorie Target | `=ARRAYFORMULA(IF(A2:A="","",Q2:Q-T2:T))` | TDEE - Daily Adjustment |

**Activity Multipliers:**
- Sedentary: 1.2
- Lightly Active: 1.375
- Moderately Active: 1.55
- Active: 1.725
- Default (if not matched): 1.55

### Legacy Protein (OLD METHOD)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| V2 | V | Protein (g) OLD | `=ARRAYFORMULA(IF(A2:A="","",G2:G*2))` | **LEGACY** - Goal Weight × 2 (DO NOT USE) |

### Ideal Weight (Devine Formula)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| W2 | W | Ideal Weight (kg) | `=ARRAYFORMULA(IF(A2:A="","",IF(D2:D="Male",50+2.3*((E2:E/2.54)-60),45.5+2.3*((E2:E/2.54)-60))))` | Devine formula (height-based) |

### Heart Rate Zones (Tanaka Formula)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| X2 | X | Max HR | `=ARRAYFORMULA(IF(A2:A="","",208-(0.7*C2:C)))` | 208 - (0.7 × Age) |
| Y2 | Y | Zone 1 Low | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.5,0)))` | 50% of Max HR |
| Z2 | Z | Zone 1 High | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.6,0)))` | 60% of Max HR |
| AA2 | AA | Zone 2 Low | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.6,0)))` | 60% of Max HR |
| AB2 | AB | Zone 2 High | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.7,0)))` | 70% of Max HR |
| AC2 | AC | Zone 3 Low | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.7,0)))` | 70% of Max HR |
| AD2 | AD | Zone 3 High | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.8,0)))` | 80% of Max HR |
| AE2 | AE | Zone 4 Low | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.8,0)))` | 80% of Max HR |
| AF2 | AF | Zone 4 High | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.9,0)))` | 90% of Max HR |
| AG2 | AG | Zone 5 Low | `=ARRAYFORMULA(IF(A2:A="","",ROUND(X2:X*0.9,0)))` | 90% of Max HR |
| AH2 | AH | Zone 5 High | `=ARRAYFORMULA(IF(A2:A="","",X2:X))` | 100% of Max HR |

---

## Brackets Tab

**All formulas start in Row 2** and use ARRAYFORMULA to auto-populate all rows with data.

### Data Import (from KPI Conversion)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| A2 | A | Full Name | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!B2:B))` | |
| B2 | B | Protein Target (g) | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!O2:O))` | NEW protein (Current Weight × Multiplier) |
| G2 | G | Calorie Target | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!U2:U))` | **FIXED v2 (2026-01-10):** Actual Calorie Target (TDEE - Adjustments), not BMR |
| L2 | L | Training Days | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!I2:I))` | |

### Macro Calculations

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| C2 | C | Protein Calories | `=ARRAYFORMULA(IF(B2:B="","",B2:B*4))` | Protein grams × 4 cal/g |
| D2 | D | Remainder Calories | `=ARRAYFORMULA(IF(G2:G="","",G2:G-C2:C))` | Total Calories - Protein Calories |
| E2 | E | Carbs (g) | `=ARRAYFORMULA(IF(A2:A="","",ROUND((G2:G-B2:B*4-F2:F*9)/4,0)))` | (Calories - Protein cals - Fat cals) / 4 |
| F2 | F | Fat (g) | `=ARRAYFORMULA(IF(A2:A="","",ROUND('KPI Conversion'!P2:P*0.25/9,0)))` | 25% of BMR calories / 9 cal/g |

### Brackets & Template Assignment

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| H2 | H | Size Tier | `=ARRAYFORMULA(IF(A2:A="","",IF(G2:G<1900,"S",IF(G2:G<2500,"M",IF(G2:G<3100,"L","XL")))))` | S/M/L/XL based on calories |
| I2 | I | Protein Bracket | `=ARRAYFORMULA(IF(A2:A="","",IF(B2:B<131,"P120",IF(B2:B<151,"P140",IF(B2:B<171,"P160",IF(B2:B<191,"P180",IF(B2:B<211,"P200","P220")))))))` | P120/P140/P160/P180/P200/P220 |
| J2 | J | Calorie Bracket | `=ARRAYFORMULA(IF(G2:G="","",IF(G2:G<1650,1500,IF(G2:G<1900,1800,IF(G2:G<2100,2000,IF(G2:G<2350,2200,IF(G2:G<2650,2500,IF(G2:G<2900,2800,IF(G2:G<3125,3000,IF(G2:G<3375,3250,3500))))))))))` | Rounds to nearest standard bracket |
| K2 | K | Template Ref | `=ARRAYFORMULA(IF(A2:A="","","TPL-"&I2:I&"-"&H2:H))` | Format: TPL-P180-M |
| M2 | M | Training Split | `=ARRAYFORMULA(IF(A2:A="","",IF(L2:L>=5,"5-Day U/L/PPL","4-Day Upper/Lower")))` | ≥5 days = 5-Day, <5 days = 4-Day |

**Size Tier Thresholds:**
- S: < 1900 cal
- M: 1900-2499 cal
- L: 2500-3099 cal
- XL: ≥ 3100 cal

**Protein Bracket Thresholds:**
- P120: < 131g
- P140: 131-150g
- P160: 151-170g
- P180: 171-190g
- P200: 191-210g
- P220: ≥ 211g

**Calorie Bracket Values:**
1500, 1800, 2000, 2200, 2500, 2800, 3000, 3250, 3500

---

## Client Delivery Tab

**All formulas start in Row 2** and use ARRAYFORMULA to auto-populate all rows with data.

### Data Import (from Brackets)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| B2 | B | Client Name | `=ARRAYFORMULA(IF(Brackets!A2:A="","",Brackets!A2:A))` | |
| C2 | C | Calorie Bracket | `=ARRAYFORMULA(IF(B2:B="","","MP-"&Brackets!J2:J))` | Format: MP-2000 |
| D2 | D | Exact Calories | `=ARRAYFORMULA(IF(B2:B="","",Brackets!G2:G))` | Actual BMR value |
| E2 | E | Calorie Gap | `=ARRAYFORMULA(IF(B2:B="","",D2:D-Brackets!J2:J))` | Exact - Bracket |
| I2 | I | Training Split | `=ARRAYFORMULA(IF(B2:B="","",Brackets!M2:M))` | |

### Dietary Preferences (VLOOKUP from Form Intake)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| F2 | F | Restrictions | `=ARRAYFORMULA(IF(B2:B="","",IFNA(VLOOKUP(B2:B,'Form Intake'!$D:$M,9,FALSE),"")))` | Column M in Form Intake |
| G2 | G | Allergies | `=ARRAYFORMULA(IF(B2:B="","",IFNA(VLOOKUP(B2:B,'Form Intake'!$D:$N,10,FALSE),"")))` | Column N in Form Intake |
| H2 | H | Won't Eat | `=ARRAYFORMULA(IF(B2:B="","",IFNA(VLOOKUP(B2:B,'Form Intake'!$D:$O,11,FALSE),"")))` | Column O in Form Intake |

### Client Identification & Delivery

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| J2 | J | UUID | `=ARRAYFORMULA(IF(B2:B="","",LOWER(LEFT(B2:B,2))&TEXT(MOD(ROW(B2:B),100),"00")&MID("abcdefghijklmnopqrstuvwxyz",1+MOD(LEN(B2:B)*ROW(B2:B),26),2)&TEXT(MOD(ROW(B2:B)*7,100),"00")))` | 8-char format: xx00xx00 |
| K2 | K | Status | `=ARRAYFORMULA(IF(B2:B="","","Pending"))` | Default status |
| L2 | L | Client URL | `=ARRAYFORMULA(IF(B2:B="","","https://denis841.github.io/meal-plans/#"&J2:J))` | Client portal link |
| M2 | M | JSON Created | `=ARRAYFORMULA(IF(B2:B="","",IF(A2:A="",FALSE,TRUE)))` | Boolean check if JSON exists |

### JSON Generation (Custom Apps Script)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| A2 | A | JSON | `=ARRAYFORMULA(IF(B2:B="","",GENERATE_MEAL_JSON(B2:B,Brackets!K2:K)))` | Custom function (requires Apps Script). References Brackets!K for protein-first template names (TPL-P160-M format) |

**Old formula (before 2026-01-10):**
`'=ARRAYFORMULA(IF(B2:B="","",GENERATE_MEAL_JSON(B2:B,C2:C)))`

**UUID Format:** `xx00xx00`
- Chars 1-2: First 2 letters of name (lowercase)
- Chars 3-4: Row number mod 100
- Chars 5-6: 2 letters from name length × row
- Chars 7-8: (Row × 7) mod 100

---

## Apps Script Functions

The spreadsheet includes custom Google Apps Script functions that extend Google Sheets functionality. These are accessed via **Extensions → Apps Script** in the Google Sheets menu.

### GENERATE_MEAL_JSON()

**Purpose:** Generates a meal plan JSON for a client based on their template assignment (protein bracket + size tier).

**Usage in Sheet:**
- Client Delivery tab, Cell A2: `=ARRAYFORMULA(IF(B2:B="","",GENERATE_MEAL_JSON(B2:B,C2:C)))`
- Pulls client name from column B, template ref from column C
- Returns formatted JSON meal plan

**Function Code:**

```javascript
/**
 * Generates a meal plan JSON for a client based on their calorie bracket.
 * Usage: =GENERATE_MEAL_JSON(A2, B2) or =ARRAYFORMULA(GENERATE_MEAL_JSON(A2:A, B2:B))
 *
 * @param {string|Array} clientName - The client's name (or array of names)
 * @param {string|Array} bracket - The calorie bracket (or array of brackets)
 * @return {string|Array} The formatted JSON meal plan(s)
 * @customfunction
 */
function GENERATE_MEAL_JSON(clientName, bracket) {
  // Handle array input from ARRAYFORMULA
  if (Array.isArray(clientName)) {
    return clientName.map(function(nameRow, index) {
      var name = Array.isArray(nameRow) ? nameRow[0] : nameRow;
      var bracketRow = Array.isArray(bracket) ? bracket[index] : bracket;
      var bracketVal = Array.isArray(bracketRow) ? bracketRow[0] : bracketRow;
      return [generateSingleMealJSON(name, bracketVal)];
    });
  }

  return generateSingleMealJSON(clientName, bracket);
}

/**
 * Generates a single meal plan JSON (internal function)
 */
function generateSingleMealJSON(clientName, bracket) {
  if (!clientName || !bracket) {
    return "";
  }

  // Handle both old format (MP-2500) and new format (TPL-P160-M)
  var bracketStr = String(bracket).trim();
  var templateTab = bracketStr.startsWith("TPL-") ? bracketStr : "TPL-" + bracketStr.replace("MP-", "");

  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet;

  try {
    sheet = ss.getSheetByName(templateTab);
    if (!sheet) {
      return "ERROR: Template " + templateTab + " not found";
    }
  } catch (e) {
    return "ERROR: " + e.message;
  }

  // Get all data from template (Key in column A, Value in column B)
  var data = sheet.getRange("A2:B34").getValues();

  // Build a lookup object
  var lookup = {};
  for (var i = 0; i < data.length; i++) {
    if (data[i][0]) {
      lookup[data[i][0]] = data[i][1];
    }
  }

  // Build the meal plan object
  var mealPlan = {
    clientName: clientName,
    mealPlans: {
      day1: {
        breakfast: { name: lookup["day1_breakfast_name"] || "", details: lookup["day1_breakfast_details"] || "" },
        lunch: { name: lookup["day1_lunch_name"] || "", details: lookup["day1_lunch_details"] || "" },
        dinner: { name: lookup["day1_dinner_name"] || "", details: lookup["day1_dinner_details"] || "" },
        snack: { name: lookup["day1_snack_name"] || "", details: lookup["day1_snack_details"] || "" }
      },
      day2: {
        breakfast: { name: lookup["day2_breakfast_name"] || "", details: lookup["day2_breakfast_details"] || "" },
        lunch: { name: lookup["day2_lunch_name"] || "", details: lookup["day2_lunch_details"] || "" },
        dinner: { name: lookup["day2_dinner_name"] || "", details: lookup["day2_dinner_details"] || "" },
        snack: { name: lookup["day2_snack_name"] || "", details: lookup["day2_snack_details"] || "" }
      },
      day3: {
        breakfast: { name: lookup["day3_breakfast_name"] || "", details: lookup["day3_breakfast_details"] || "" },
        lunch: { name: lookup["day3_lunch_name"] || "", details: lookup["day3_lunch_details"] || "" },
        dinner: { name: lookup["day3_dinner_name"] || "", details: lookup["day3_dinner_details"] || "" },
        snack: { name: lookup["day3_snack_name"] || "", details: lookup["day3_snack_details"] || "" }
      }
    },
    macros: {
      calories: Number(lookup["macros_calories"]) || 0,
      protein: Number(lookup["macros_protein"]) || 0,
      fat: Number(lookup["macros_fat"]) || 0,
      carbs: Number(lookup["macros_carbs"]) || 0
    },
    shoppingList: {
      proteins: [lookup["shopping_proteins"] || ""],
      carbs: [lookup["shopping_carbs"] || ""],
      fats: [lookup["shopping_fats"] || ""],
      produce: [lookup["shopping_produce"] || ""],
      pantry: [lookup["shopping_pantry"] || ""]
    }
  };

  return JSON.stringify(mealPlan, null, 2);
}
```

**How it works:**
1. Takes client name and bracket (e.g., "Denis", "MP-2000")
2. Converts bracket format: MP-2500 → TPL-2500 (removes "MP-" prefix, adds "TPL-" prefix)
3. Reads template tab data from range A2:B34 (Key-Value pairs in columns A-B)
4. Builds a lookup object from the Key-Value pairs
5. Constructs JSON object with:
   - Client name
   - 3-day meal rotation (breakfast, lunch, dinner, snack for each day)
   - Macros (calories, protein, fat, carbs)
   - Shopping lists (proteins, carbs, fats, produce, pantry)
6. Returns formatted JSON string with 2-space indentation

**Template Lookup Range:** A2:B34 in each TPL-* tab

**Error Handling:**
- Returns empty string if client name or bracket is missing
- Returns "ERROR: Template [name] not found" if template tab doesn't exist
- Returns "ERROR: [error message]" if any exception occurs during execution

**ARRAYFORMULA Support:** The function handles both single-value and array inputs, making it compatible with ARRAYFORMULA for batch processing multiple clients.

---

## Deficit Ratio Tab

**⚠️ IMPORTANT: Formulas start in Row 11 (not Row 2)** due to reference table in rows 1-9.

**Headers are in Row 10, formulas start in Row 11.**

### Data Import (from KPI Conversion)

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| A11 | A | Client Name | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!B2:B))` | |
| B11 | B | TDEE | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!Q2:Q))` | |
| C11 | C | Calculated Deficit | `=ARRAYFORMULA(IF('KPI Conversion'!B2:B="","",'KPI Conversion'!T2:T))` | Daily calorie adjustment |

### Deficit Settings & Calculations

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| D11 | D | Intensity % | `=ARRAYFORMULA(IF(A11:A="","",75))` | Hardcoded 75% |
| E11 | E | Adjusted Deficit | `=ARRAYFORMULA(IF(A11:A="","",C11:C*(D11:D/100)))` | Calculated Deficit × Intensity % |
| F11 | F | Diet Deficit (75%) | `=ARRAYFORMULA(IF(A11:A="","",E11:E*0.75))` | 75% handled by diet |
| G11 | G | Cardio Deficit (25%) | `=ARRAYFORMULA(IF(A11:A="","",E11:E*0.25))` | 25% handled by cardio |
| H11 | H | Calorie Intake | `=ARRAYFORMULA(IF(A11:A="","",B11:B-F11:F))` | TDEE - Diet Deficit |

### Meal Plan Assignment

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| I11 | I | Meal Plan Bracket | `=ARRAYFORMULA(IF(A11:A="","",IF(H11:H>=2900,"MP-3000",IF(H11:H>=2600,"MP-2800",IF(H11:H>=2300,"MP-2500",IF(H11:H>=2100,"MP-2200",IF(H11:H>=1900,"MP-2000",IF(H11:H>=1650,"MP-1800","MP-1500"))))))))` | Based on Calorie Intake |

**Meal Plan Bracket Thresholds:**
- MP-3000: ≥ 2900 cal
- MP-2800: 2600-2899 cal
- MP-2500: 2300-2599 cal
- MP-2200: 2100-2299 cal
- MP-2000: 1900-2099 cal
- MP-1800: 1650-1899 cal
- MP-1500: < 1650 cal

### Progress Tracking

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| J11 | J | Cardio Sessions/Week | `=ARRAYFORMULA(IF((A11:A="")+(G11:G<=0),"",ROUND((G11:G*7)/250,0)))` | (Cardio Deficit × 7) / 250 cal per session |
| K11 | K | Goal Type | `=ARRAYFORMULA(IF(A11:A="","",IF(C11:C>0,"LOSS","GAIN")))` | Based on deficit direction |

### Safety Metrics

| Cell | Column | Name | Formula | Notes |
|------|--------|------|---------|-------|
| L11 | L | Deficit % of TDEE | `=ARRAYFORMULA(IFERROR(IF(A11:A="","",ROUND(E11:E/B11:B*100,1)),""))` | (Adjusted Deficit / TDEE) × 100 |
| M11 | M | Status | `=ARRAYFORMULA(IF(A11:A="","",IF(L11:L="","",IF(L11:L>30,"🚨 EXTREME",IF(L11:L>25,"⚠️ AGGRESSIVE","✅ SAFE")))))` | Safety assessment |
| N11 | N | Intake Warning | `=ARRAYFORMULA(IF((A11:A="")+(H11:H=""),"",IF(H11:H<1500,"🚨 DANGER",IF(H11:H<1800,"⚠️ LOW INTAKE","OK"))))` | Minimum intake check |

**Safety Thresholds:**

Deficit Status:
- ✅ SAFE: ≤ 25% of TDEE
- ⚠️ AGGRESSIVE: 25.1-30% of TDEE
- 🚨 EXTREME: > 30% of TDEE

Intake Warning:
- OK: ≥ 1800 cal
- ⚠️ LOW INTAKE: 1500-1799 cal
- 🚨 DANGER: < 1500 cal

---

## Quick Reference Summary

| Tab | First Formula Row | Key Dependencies |
|-----|-------------------|------------------|
| KPI Conversion | Row 2 | Form Intake tab |
| Brackets | Row 2 | KPI Conversion columns B, O, P, I |
| Client Delivery | Row 2 | Brackets A, G, J, M + Form Intake (VLOOKUP) |
| Deficit Ratio | **Row 11** | KPI Conversion columns B, Q, T |

**Form Intake Column Map:**
- A: Submission ID
- D: Full Name
- E: Age
- F: Height (cm)
- I: Gender
- J: Current Weight (kg)
- K: Goal Weight (kg)
- M: Dietary Restrictions
- N: Food Allergies
- O: Won't Eat
- T: Training Days
- W: Activity Level

---

## Changelog

| Date | Change |
|------|--------|
| 2026-01-10 | Complete rebuild - All formulas corrected with proper syntax |
| 2026-01-10 | Fixed Brackets to use NEW protein (column O) instead of OLD protein (column V) |
| 2026-01-10 | Fixed Brackets column G to reference BMR (P2:P) directly |
| 2026-01-10 | Corrected all sheet name references from `''Form Intake'` to `'Form Intake'` |
| 2026-01-10 | Documented that Deficit Ratio formulas start at Row 11, not Row 2 |
| 2026-01-10 | Added cell references for every formula for easy lookup |
| 2026-01-10 | **v2 FIX:** Brackets G2 now correctly pulls U2:U (Calorie Target) - Fixed Size Tier assignments (Denis M→XL, Corey S→XL) |
| 2026-01-10 | **v2 FIX:** KPI Conversion column N formatted to 0.0 (1 decimal) - Shows 1.6 instead of rounded "2" for BMI≥35 clients |
| 2026-01-10 | Added complete Apps Script function documentation (GENERATE_MEAL_JSON) with code, parameters, error handling, and usage examples |
| 2026-01-10 | **v3 FIX:** Client Delivery A2 now references Brackets!K2:K (Template Ref) instead of C2:C (old calorie brackets) - Fixes JSON generation for protein-first system |
| 2026-01-10 | **v3 APPS SCRIPT UPDATE:** generateSingleMealJSON now handles both old format (MP-2500) and new format (TPL-P160-M) - User must copy updated code to Apps Script editor |
