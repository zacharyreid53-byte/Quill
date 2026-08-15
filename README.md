# Quill

A little app for keeping the funny things your friends and family say, and reading one back at random.

## Using it

Open `index.html` in any browser — no install, no server, no account.

- **Random quote** — the big card at the top shows one of your quotes; press **Another one** to shuffle.
- **Add a quote** — write it the way they said it, note who said it, and optionally the context ("Thanksgiving 2019, after the turkey incident").
- **The collection** — every quote you've saved, newest first, with search across quotes, people, and context. Edit or delete from each card.
- **Backup & restore** — your quotes are stored in the browser you're using (localStorage), so they stay on that device. The backup section copies everything as text you can save in a note or email; paste it into Quill on another device to bring your collection over. Loading a backup merges — it never deletes what's already there.

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
