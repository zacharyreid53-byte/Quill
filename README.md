# Quill

A little app for keeping the funny things your friends and family say, and reading one back at random.

## Using it

Open `index.html` in any browser — no install, no server.

- **Sign-in page** — the app opens on its own sign-in page (`#/login`); the rest of the app lives at `#/app` and bounces you back to sign-in if you're not signed in. Each person creates a profile with an optional password and gets their own separate quote collection. Passwords are hashed before they're stored and keep casual snooping out on a shared device — but everything lives in the browser's own storage, so treat it as a family bookshelf, not a bank vault.
- **Random quote** — the big card at the top shows one of your quotes; press **Another one** to shuffle, or the star to favorite the one on screen.
- **Clickable quotes** — click any quote in the collection to open its actions: favorite it, share it, edit, or delete. The **★ Favorites** button next to search filters the list down to your starred quotes.
- **Sharing** — from a quote's Share panel you can send a copy straight into another profile on the same device (it shows up marked "shared by you"), copy it as plain text for a group chat, or copy a Quill snippet the other person pastes into their own Backup box on any device.
- **Add a quote** — write it the way they said it, note who said it, and optionally the context ("Thanksgiving 2019, after the turkey incident").
- **The collection** — every quote you've saved, newest first, with search across quotes, people, and context. Edit or delete from each card.
- **Backup & restore** — quotes are stored in the browser you're using (localStorage), per profile, so they stay on that device. The backup section copies your collection as text you can save in a note or email; paste it into your profile on another device to bring it over. Loading a backup merges — it never deletes what's already there.

Profiles are per-browser: a profile made on the family laptop doesn't exist on your phone until you create it there and paste in a backup. Quotes saved before profiles existed are moved into the first profile created, so nothing is lost by upgrading.

## Data format

Backups are a JSON array of quotes:

```json
[
  {
    "id": "q...",
    "text": "I'm not saying it was my fault, but the smoke alarm agreed with me.",
    "by": "Uncle Ray",
    "note": "Thanksgiving 2019",
    "added": "2026-08-15T00:00:00.000Z"
  }
]
```

You can hand-edit this or bulk-add old quotes by pasting your own array into the restore box.
