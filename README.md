# Quill

A little app for keeping the funny things your friends and family say, and reading one back at random.

This repo also hosts **[`pit.html`](pit.html)** — a Kalshi-style prediction market for a single yes/no question. See [its section below](#pit-a-one-market-prediction-app) for details.

Two versions are active in this repo:

- **`index.html`** — the Firebase version (the default). Real accounts (email + password via Firebase Authentication), quotes in the Firebase Realtime Database, with server-enforced privacy: each person's quotes are unreadable by anyone but them. Sharing sends a quote straight into another person's collection, wherever they are.
- **`offline.html`** — the original no-setup version. Everything stays in the browser's localStorage with device-local profiles. Open it in any browser and it just works; use its backup box to move quotes between devices.

`index.html` offers a one-click import of anything saved in `offline.html` on the same browser.

An earlier variant backed by a free [npoint.io](https://www.npoint.io) JSON bin (no accounts service, but no real privacy — a bin has no login of its own) is kept for reference at [`archive/npoint.html`](archive/npoint.html), not actively used.

## Features (all versions)

- **Sign-in page** on its own route (`#/login`); the app lives at `#/app` and redirects you to sign-in when you're signed out.
- **Random quote** — the big card up top; press **Another one** to shuffle, or the star to favorite what's on screen.
- **Clickable quotes** — click any quote in the collection for actions: favorite, share, edit, delete. The **★ Favorites** button filters the list to starred quotes.
- **Sharing** — send a copy straight to another profile/account, or copy the quote as plain text for a group chat.

## Setting up the Firebase version (`index.html`)

One-time setup in the [Firebase console](https://console.firebase.google.com), all on the free plan:

1. **Add project** (Analytics not needed).
2. **Build → Authentication → Get started** → Sign-in method → enable **Email/Password**.
3. **Build → Realtime Database → Create database** → locked mode.
4. In the database's **Rules** tab, paste the contents of [`database.rules.json`](database.rules.json) and **Publish**. The rules make each person's quotes private to them, while letting signed-in family members *add* a shared quote to someone's collection (never read or change what's there).
5. **Project settings (gear) → Your apps → Web app (`</>`)** → register the app → copy the `firebaseConfig` values into `QUILL_FIREBASE_CONFIG` near the top of `index.html` (including `databaseURL`). These values aren't secrets — your security rules are what protect the data.
6. Host the file somewhere with a real URL (Firebase auth won't run from a double-clicked file). Easiest: GitHub Pages — repo **Settings → Pages → Deploy from a branch**. Then add that domain (e.g. `yourname.github.io`) in Firebase **Authentication → Settings → Authorized domains**.

Then everyone creates an account on the sign-in page, and their names appear in each other's Share panels automatically.

## Data format

Bulk import/export (and offline backups) use a JSON array of quotes:

```json
[
  {
    "text": "I'm not saying it was my fault, but the smoke alarm agreed with me.",
    "by": "Uncle Ray",
    "note": "Thanksgiving 2019",
    "added": "2026-08-15T00:00:00.000Z",
    "fav": true,
    "sharedBy": ""
  }
]
```

Paste an array like this into the app's backup box to bulk-add old quotes you have written down elsewhere.

## Pit — a one-market prediction app

[`pit.html`](pit.html) is a self-contained knock-off of Kalshi/Polymarket-style event trading, scoped to **one** yes/no question at a time. Everyone signs in with a username/password (Firebase Auth, same pattern as `index.html`), starts with $1,000 in play money, and buys or sells "Yes" or "No" contracts on the current question. A contract pays $1 if it resolves your way, $0 otherwise.

- **Pricing** is automatic — there's no order book to fill. Trades go through an [LMSR](https://en.wikipedia.org/wiki/Prediction_market#Automated_market_makers) automated market maker, so there's always a live price and always a counterparty (the market itself). Buying pushes the price up, selling pushes it down.
- **Leaderboard** shows everyone's cash + mark-to-market position value, live.
- **Admin** — the first person to ever open the app becomes the market admin (stored at `pit/admin` in the database). They can edit the question, resolve it Yes or No (which pays out every position and locks trading), and start a new round afterward with a fresh question and reset prices — cash balances carry over.

### Setup

`pit.html` can reuse the same Firebase project as `index.html`, or its own — it only touches its own `pit/` node in the Realtime Database, so it can't collide with Quill's data:

1. Fill in `window.PIT_FIREBASE_CONFIG` near the top of `pit.html` (same values as `QUILL_FIREBASE_CONFIG` if reusing the project).
2. Make sure **Authentication → Sign-in method → Email/Password** is enabled (same as Quill's setup).
3. In the Realtime Database **Rules** tab, publish the contents of [`database.rules.json`](database.rules.json) — it now includes both Quill's `directory`/`users` rules and Pit's `pit` rules.
4. Host it somewhere with a real URL, same as Quill (Firebase Auth won't run from a double-clicked file).

Trading here is trust-based, the same spirit as Quill's family-scale design: the security rules stop one player from editing another player's cash or shares directly, but they can't independently verify that a submitted trade's LMSR math was computed correctly client-side. Fine for friends; not a substitute for a real exchange.
