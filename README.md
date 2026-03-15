# BAS99 — Setup & Deployment Guide

## What's included
| File | Purpose |
|------|---------|
| `index.html` | Customer store — product page + 3-step checkout |
| `admin.html` | Admin dashboard — manage items, orders, pricing, settings |

---

## Step 1 — Create a Free Firebase Project (5 minutes)

1. Go to **https://console.firebase.google.com**
2. Click **"Add project"** → name it `bas99` → Continue (disable Google Analytics is fine)
3. Once created, click **"Web"** (the `</>` icon) to add a web app
4. Register app name `bas99` → Copy the `firebaseConfig` object shown

### Enable Realtime Database
1. In Firebase console, go to **Build → Realtime Database**
2. Click **"Create Database"** → choose your region → start in **Test mode** (you can tighten security later)

---

## Step 2 — Add Your Firebase Config

Open **both** `index.html` and `admin.html`, find this block near the bottom, and replace the placeholder values:

```js
const FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY",               // ← paste your values here
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

---

## Step 3 — Deploy to Netlify (2 minutes, free)

### Option A — Drag & Drop (easiest)
1. Go to **https://netlify.com** → Sign up free
2. Click **"Add new site" → "Deploy manually"**
3. Drag the entire `BAS99` folder into the browser
4. Your site goes live instantly at a URL like `https://amazing-name-123.netlify.app`
5. Optionally set a custom domain in Netlify settings

### Option B — GitHub Pages
1. Push this folder to a GitHub repo
2. Go to repo Settings → Pages → Deploy from `main` branch
3. Live at `https://yourusername.github.io/bas99`

---

## Step 4 — First Login & Setup

1. Open `https://your-site.netlify.app/admin.html`
2. Enter any password on first login — this becomes your admin password (stored in Firebase)
3. Go to **New Item** → fill in item details → **Publish Item**
4. Go to **Settings** → add your Instagram handle (e.g. `@bas99.store`)
5. Visit your store URL — the item will be live!

---

## How the 3-Step Purchase Works

| Step | What happens |
|------|-------------|
| **1 — Confirm Item** | Customer sees the product, price, qty remaining |
| **2 — Delivery Details** | Fills name, phone, email, address (browser autofills on return visits) |
| **3 — Review & Pay** | Order summary + COD notice → Place Order |

After placing an order, qty automatically decrements in real-time. Pricing rules apply instantly.

---

## Pricing Rules

Example: Item starts at $100, qty = 50

| Rule | When qty ≤ | Price becomes |
|------|------------|---------------|
| Rule 1 | 20 | $110 (+10%) |
| Rule 2 | 10 | $120 (+20%) |
| Rule 3 | 5  | $150 (+50%) |

Rules are set per-item in **Admin → Pricing Rules**. The highest matching increase wins.

---

## Admin Dashboard Quick Reference

| Section | What you can do |
|---------|-----------------|
| Overview | Live stats, current item status |
| New Item | Upload item (image URL, name, desc, price, qty) |
| Pricing Rules | Add/remove automatic price-increase rules |
| Orders | See all orders, update status (Pending/Confirmed/Shipped/Cancelled) |
| Settings | Instagram handle, currency symbol, change admin password |

---

## Security Note

The current setup uses Firebase in Test mode (open read/write). Before going fully live:
1. In Firebase Console → Realtime Database → Rules, restrict writes:
```json
{
  "rules": {
    "currentItem": { ".read": true, ".write": false },
    "settings": { ".read": true, ".write": false },
    "orders": {
      ".read": false,
      "$orderId": { ".write": "!data.exists()" }
    }
  }
}
```
2. Admin writes should go through Firebase Authentication — contact your developer to implement.

---

## Support
For issues or improvements, check the code comments in `index.html` and `admin.html`.
