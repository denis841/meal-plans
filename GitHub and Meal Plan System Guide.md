# GitHub & Meal Plan System - Complete Guide

Everything learned about using GitHub for meal plan distribution.

---

## 1. What is GitHub?

### GitHub = Dropbox for Code

GitHub stores your files and tracks every change you make. Think of it like Dropbox or Google Drive, but specifically designed for code and with version history.

### GitHub Pages = Free Website Hosting

GitHub Pages is a FREE feature built into GitHub that turns your repository into a live website. This is how your meal plans are accessible on the internet.

| | GitHub | GitHub Pages |
|--|--------|--------------|
| **What it is** | Code storage & version control | Free website hosting |
| **URL** | github.com/denis841/meal-plans | denis841.github.io/meal-plans |

**GitHub** = Stores your files
**GitHub Pages** = Serves those files as a live website

### GitHub Pages vs Netlify

Both do the same thing - host static websites for free. You chose GitHub Pages because:
- Unlimited deployments (Netlify limits to ~20/month)
- Simpler setup (everything in one place)
- You accepted that the repo is public

---

## 2. How the Meal Plan System Works

### The Magic: One Website, Many Clients

Instead of creating a separate HTML file for each client, you have:
- **ONE** index.html file (the template)
- **MANY** JSON files (one per client with their meals)

The URL has a secret code after the # symbol:

| URL | What Happens |
|-----|--------------|
| `meal-plans/` | No code → shows default template |
| `meal-plans/#abc123...` | Code = John → loads John's meals |
| `meal-plans/#def456...` | Code = Sarah → loads Sarah's meals |

**Behind the scenes:** Client opens link → JavaScript reads the code after # → Fetches that client's JSON file → Displays their personalized meals.

### Why Hash URLs (#)?

The # part of the URL is never sent to servers or logged anywhere. This means:
- More private - doesn't appear in server logs
- Doesn't leak to other websites via referrer headers
- Still works perfectly for loading the right client data

---

## 3. What is a UUID?

UUID = "Universally Unique Identifier" - just a random code like:

```
d03507522d254e71b65f37182f2e1a73
```

**Why we use them:**
- 32 characters = impossible to guess (340 undecillion combinations)
- Reveals nothing about the client
- Each client gets a unique one

### Why NOT Use Client Names as Filenames?

**SECURITY RISK!** If files were named `john-smith.json`, anyone could guess:
```
meal-plans/#john-smith → see John's data
```

Random UUIDs mean nobody can guess other clients' URLs.

### How to Generate a UUID

Run this command in Terminal:
```bash
uuidgen | tr -d '-' | tr '[:upper:]' '[:lower:]'
```

---

## 4. Essential Git Commands

### The Only Commands You Need

Push changes to GitHub (run from inside the Meal-Plan folder):

```bash
git add .
git commit -m "Your message here"
git push
```

Or all in one line:
```bash
cd /path/to/Meal-Plan && git add . && git commit -m "Update" && git push
```

### What Each Command Does

| Command | What It Does |
|---------|--------------|
| `git add .` | Stages all changed files (prepares them to be saved) |
| `git commit -m "..."` | Saves the changes with a message |
| `git push` | Uploads to GitHub (site updates in ~30 seconds) |

### Where to Run Commands

You MUST be inside the Meal-Plan folder (where the .git folder lives). The `cd` command moves you there first.

---

## 5. Can I Move the Folder?

**YES!** You can move the Meal-Plan folder anywhere on your computer.

GitHub doesn't know or care where the folder is on your local machine. The `.git` folder inside contains all the connection info.

### What Stays the Same
- Live URL: denis841.github.io/meal-plans (never changes)
- How GitHub works
- How clients access their meal plans

### What You'd Update
- Any saved terminal commands with the old path
- The SOP document paths

---

## 6. How to Enable GitHub Pages

If you ever need to set this up again:

1. Go to your repo on github.com
2. Click **Settings** (top menu)
3. Click **Pages** (left sidebar)
4. Under "Source" select **Deploy from a branch**
5. Under "Branch" select **main** and **/ (root)**
6. Click **Save**
7. Wait 2-3 minutes for green box with your URL

---

## 7. Add to Home Screen (PWA)

### What is PWA?

PWA = Progressive Web App. It makes websites feel like native apps.

Your meal plan site has PWA support via manifest.json. This means:
- **Android:** Chrome automatically prompts "Add to Home Screen"
- **iPhone:** Share → Add to Home Screen works better
- Opens fullscreen (no browser bar)
- Looks and feels like a real app

---

## 8. Updating Client Meals

One of the biggest benefits of this system:

1. Edit the client's JSON file on your computer
2. Push to GitHub
3. Client refreshes their page → sees new meals

**No need to send them a new link.** The same URL always shows their latest meals.

---

## 9. Security Summary

### What's Protected
- Hash URLs don't leak to server logs or referrer headers
- 32-char UUIDs are unguessable
- Clients can only see their own meals

### Accepted Tradeoffs
- JSON files visible if someone browses the GitHub repo
- But filenames are meaningless UUIDs
- And meal plans aren't sensitive medical data

---

## 10. File Structure

```
/Meal-Plan/
├── index.html          (the website template)
├── manifest.json       (PWA support for home screen)
├── data/
│   ├── template.json   (copy this for new clients)
│   └── [uuid].json     (one per client)
```

---

## 11. Quick Reference

### Add New Client
1. Generate UUID: `uuidgen | tr -d '-' | tr '[:upper:]' '[:lower:]'`
2. Copy template: `cp data/template.json data/[UUID].json`
3. Edit the JSON with their meals
4. Push: `git add . && git commit -m "Add client" && git push`
5. Send URL: `https://denis841.github.io/meal-plans/#[UUID]`

### Update Meals
1. Edit their JSON file
2. Push: `git add . && git commit -m "Update meals" && git push`
3. Client refreshes to see changes

### Your Live Site
https://denis841.github.io/meal-plans/
