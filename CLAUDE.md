# Quill — working agreement

## Deployment rule (important)

Two repos:

- `/home/user/quill-preview` → `zacharyreid53-byte/quill-preview`, branch `main`.
  The test site. **Push freely** — this is where Zach reviews changes on his phone.
- `/home/user/Quill` → `zacharyreid53-byte/Quill`, branch `claude/random-quotes-app-lpln8j`.
  The **live family-facing site** (GitHub Pages serves this branch; there is no `main`).

**Never commit or push to the live `Quill` repo without Zach's explicit approval for
that specific change.** An approval like "push it live" covers only the batch of work
being discussed at that moment — it is never standing permission for later changes.

Default workflow for any change:

1. Edit in `/home/user/Quill`.
2. Copy the changed files to `/home/user/quill-preview` and push there.
3. Tell Zach what to check, and **stop**. Leave the live repo untouched — not even a
   local commit — until he approves.

## Verification before pushing anywhere

No build step; verify statically:

- Extract the `<script type="module">` body from `index.html` (plain `<script>` for
  `offline.html`) and run `node --check` on it.
- Check `<div>`/`</div>` balance.
- Cross-check every `$('id')` reference in the script against real `id="..."` attributes.

## Known recurring bug

An element given `display: flex|grid|block` unconditionally will **not** hide when its
`hidden` attribute is set — author CSS beats the UA `[hidden] { display: none }` rule.
Every toggleable element needs its own `.selector[hidden] { display: none; }` override.
This has bitten `.wrap`, `.panel`, `.account-menu`, and `.share-panel`.

## Firebase

`database.rules.json` is **not** deployed by pushing. Zach must paste it into the
Firebase console (Realtime Database → Rules → Publish) by hand. Whenever a new subtree
is added under `users/$uid` (so far: `collections`, `friends`, `shares`), remind him —
otherwise the feature fails with `PERMISSION_DENIED`.

This sandbox has no network access to Firebase, so data-side fixes must be built as
in-app tools Zach runs himself.
