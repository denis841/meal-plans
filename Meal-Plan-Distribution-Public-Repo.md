# Meal Plan Distribution System \- Private Repo Plan

**Status:** Archived \- User chose GitHub Pages \+ Public Repo instead **Date:** 2025-12-31

## Current State

- Single `index.html` file with embedded meal data (JavaScript object with breakfast/lunch/dinner/snack for each day)  
- Beautiful dark-themed UI with interactive tabs  
- Currently hardcoded \- would require duplicating the entire HTML for each client  
- **Current system is actually secure** \- localStorage keeps data client-side only

## User Requirements

1. Reuse one template across multiple clients  
2. Easy updates AFTER sending (client sees changes without resending)  
3. Free solutions only  
4. Actionable immediately (no complex setup)

---

## Google Sheets Approach \- REJECTED

After security/viability analysis, the public Google Sheets approach is **not viable**:

| Issue | Why It Matters |
| :---- | :---- |
| **All clients exposed** | Public sheet \= anyone with URL sees ALL client data |
| **GDPR violation** | Dietary data is "special category" \- public access \= legal liability |
| **CORS blocked** | Modern Google Sheets API doesn't allow cross-origin requests from static hosts |
| **Undocumented API** | The gviz endpoint isn't officially supported, could break anytime |
| **No row-level security** | Can't show only one client's data from a shared sheet |

---

## Chosen Approach: Per-Client JSON Files \+ Netlify

This approach gives you:

- One HTML template (no duplication)  
- Easy updates (edit JSON, push, done)  
- Client isolation (each only sees their data)  
- Secure (UUID-based URLs, no guessing)  
- Live updates (client refreshes \= sees new meals)

---

## Implementation Plan

### Step 1: Restructure Project

/meal-plan/

├── index.html          \# Template (reads from URL param)

├── data/

│   ├── template.json   \# Example structure for new clients

│   ├── a1b2c3d4.json   \# Client 1's meals (UUID filename)

│   └── e5f6g7h8.json   \# Client 2's meals

└── generate-client.sh  \# Script to create new client

### Step 2: Create JSON Structure

Each client gets a JSON file like `/data/a1b2c3d4.json`:

{

  "clientName": "John",

  "mealData": {

    "monday": {

      "breakfast": { "name": "Scrambled Eggs on Toast", "details": "3 eggs, 2 slices wholemeal toast" },

      "lunch": { "name": "Chicken Fillet Roll", "details": "From Spar/Centra \- no butter/mayo" },

      "dinner": { "name": "Chicken Stir Fry", "details": "Chicken breast, mixed veg, rice" },

      "snack": { "name": "Protein Shake \+ Banana", "details": "Post-gym" }

    },

    "tuesday": { ... },

    ...

  }

}

### Step 3: Modify HTML to Fetch Client Data

Replace hardcoded `mealData` with dynamic fetch using **hash URLs** (more secure):

// Get client ID from hash (e.g., /\#a1b2c3d4...)

const clientId \= window.location.hash.slice(1); // Remove the \#

// Validate UUID format (32 hex chars)

const UUID\_REGEX \= /^\[a-f0-9\]{32}$/i;

if (\!clientId || \!UUID\_REGEX.test(clientId)) {

    document.body.innerHTML \= '\<div style="text-align:center;padding:50px;"\>\<h1\>Invalid link\</h1\>\<p\>Please use the link your coach sent you.\</p\>\</div\>';

} else {

    // Fetch this client's JSON

    fetch(\`/data/${clientId}.json\`)

        .then(res \=\> {

            if (\!res.ok) throw new Error('Not found');

            return res.json();

        })

        .then(data \=\> {

            mealData \= data.mealData;

            initializeApp(); // Existing render logic

        })

        .catch(() \=\> {

            document.body.innerHTML \= '\<div style="text-align:center;padding:50px;"\>\<h1\>Meal plan not found\</h1\>\<p\>Your coach is preparing your plan. Check back soon\!\</p\>\</div\>';

        });

}

**Why hash URLs?**

- `/#uuid` is NOT sent to server (no logging)  
- NOT included in referrer header (no leaks)  
- Still works for client-side routing

### Step 4: Deploy Public Repo

**Why Public GitHub Pages:**

- GitHub Pages requires PUBLIC repo on free tier  
- Private repo \= nobody can browse your JSON files  
- For now we will use a public repo

**Setup:**

1. Make your GitHub repo  
2. Click "Add new site" → "Import an existing project"  
3. Connect to GitHub → Select your private repo  
4. Deploy settings: leave defaults (Netlify auto-detects static site)  
5. Click "Deploy" → Get URL like: `your-site.netlify.app`

**Security achieved:**

- Source files: Hidden (private repo)  
- Website: Public but no directory listing  
- Client URLs: Only accessible if you know the UUID

### Step 5: Generate New Clients

\# Generate 32-char UUID for new client (128-bit entropy)

NEW\_ID=$(uuidgen | tr \-d '-' | tr '\[:upper:\]' '\[:lower:\]')

\# Copy template

cp data/template.json data/$NEW\_ID.json

\# Edit with client's meals

\# Then push to git

echo "Client URL: https://yoursite.netlify.app/\#$NEW\_ID"

---

## Stress Test Findings & Mitigations

### Netlify Constraints

| Constraint | Limit | Impact | Mitigation |
| :---- | :---- | :---- | :---- |
| Build minutes | \~20 deploys/month | Can't deploy daily | Batch updates (2x/week max) |
| CORS headers | Not configured by default | Fetch may fail | Add `netlify.toml` |
| GitHub OAuth | Access to all repos | Security risk | Use GitHub App instead |

### Security Hardening Required

| Vulnerability | Risk | Fix |
| :---- | :---- | :---- |
| **Short UUIDs (8 char)** | Brute-forceable in \~50 days | Use 32-char UUIDs |
| **UUID in query string** | Leaks via referrer, logs, history | Use hash URL: `/#uuid` |
| **No referrer policy** | Third-party sites see client UUID | Add header: `Referrer-Policy: no-referrer` |
| **No rate limiting** | Brute force enumeration | Optional: Add Cloudflare free tier |

### Required Configuration Files

**`netlify.toml`** (create in repo root):

\[build\]

  publish \= "."

\[\[headers\]\]

  for \= "/\*"

  \[headers.values\]

    Referrer-Policy \= "no-referrer"

    X-Content-Type-Options \= "nosniff"

    X-Frame-Options \= "DENY"

\[\[headers\]\]

  for \= "/index.html"

  \[headers.values\]

    Cache-Control \= "no-cache, no-store, must-revalidate"

\[\[headers\]\]

  for \= "/data/\*"

  \[headers.values\]

    Cache-Control \= "max-age=60"  
