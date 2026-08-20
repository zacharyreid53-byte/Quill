# Quill

A little app for keeping the funny things your friends and family say, and reading one back at random.

Three versions live in this repo:

- **`index.html`** — the Firebase version (currently the default). Real accounts (email + password via Firebase Authentication), quotes in the Firebase Realtime Database, with server-enforced privacy: each person's quotes are unreadable by anyone but them. Sharing sends a quote straight into another person's collection, wherever they are.
- **`offline.html`** — the original no-setup version. Everything stays in the browser's localStorage with device-local profiles. Open it in any browser and it just works; use its backup box to move quotes between devices.
- **`npoint.html`** — a version backed by a single free [npoint.io](https://www.npoint.io) JSON bin instead of Firebase. No accounts service to set up, but no real privacy either — see the note in that section below before using it.

Both `index.html` and `npoint.html` offer a one-click import of anything saved in `offline.html` on the same browser.

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

## A word about npoint.io (`npoint.html`) and privacy

npoint is just a JSON file with a URL — it has **no login of its own**. Anyone who has your bin's URL (which lives in the page's source, so it isn't really secret) can read or overwrite everything in it: every profile, every quote, every password hash.

Quill's profiles and optional passwords still work as a courtesy there — they stop family from casually opening someone else's collection — but they are **not real security**, and there's **no password recovery** if you forget one. Use it only if you're fine with that tradeoff in exchange for skipping Firebase's setup.

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
