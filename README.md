# Quill

A little app for keeping the funny things your friends and family say, and reading one back at random.

Two versions live in this repo:

- **`index.html`** — the cloud version. Real accounts (email + password via Firebase Authentication), quotes stored in Cloud Firestore, so everything saves automatically and follows you to any device. Sharing sends a quote straight into another person's collection, wherever they are.
- **`offline.html`** — the original no-setup version. Everything stays in the browser's localStorage with device-local profiles. Open it in any browser and it just works; use its backup box to move quotes between devices. The cloud version offers a one-click import of anything saved here.

## Features (both versions)

- **Sign-in page** on its own route (`#/login`); the app lives at `#/app` and redirects you to sign-in when you're signed out.
- **Random quote** — the big card up top; press **Another one** to shuffle, or the star to favorite what's on screen.
- **Clickable quotes** — click any quote in the collection for actions: favorite, share, edit, delete. The **★ Favorites** button filters the list to starred quotes.
- **Sharing** — send a copy to another user (cloud: any family member with an account; offline: another profile on the same device), or copy the quote as plain text for a group chat.

## Setting up the cloud version

One-time setup in the [Firebase console](https://console.firebase.google.com), all on the free plan:

1. **Add project** (Analytics not needed).
2. **Build → Authentication → Get started** → Sign-in method → enable **Email/Password**.
3. **Build → Firestore Database → Create database** → production mode, pick a region near you.
4. In Firestore's **Rules** tab, paste the contents of [`firestore.rules`](firestore.rules) and **Publish**. The rules make each person's quotes private to them, while letting signed-in family members *add* a shared quote to someone's collection (never read or change it).
5. **Project settings (gear) → Your apps → Web app (`</>`)** → register the app → copy the `firebaseConfig` values into `QUILL_FIREBASE_CONFIG` near the top of `index.html`. These values aren't secrets — your security rules are what protect the data.
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
