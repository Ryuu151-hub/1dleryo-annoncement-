# 1dleryu-announcement

Static JSON endpoint used by the **1dleryu X TikTok** extension popup to show
in-app announcements and to remotely block outdated versions.

`popup.js` fetches `announcement.json` on every popup open
(`ANNOUNCEMENT_URL` in `popup.js`). No auth, no server code — just static
files served by Vercel.

## Files

- **announcement.json** — the payload `popup.js` actually reads. Controls:
  - `blockedVersions` — extension versions that get the permanent
    "update required" overlay instead of the normal popup
  - `announcement` — the dismissible/non-dismissible in-app card (title,
    message, cards, buttons, theme)
- **version.json** — plain latest/minimum/blocked version pointer, for
  reference or other tooling. Not read by the extension itself.
- **changelog.json** — version history, newest first. Not read by the
  extension itself.
- **config.json** — reserved for future remote feature flags. Not read by
  the extension itself yet.
- **vercel.json** — sets CORS headers (`Access-Control-Allow-Origin: *`) so
  the popup, which runs from a `chrome-extension://` origin, can fetch these
  files, and disables caching so version-block changes take effect
  immediately.

## Deploy

```
npm i -g vercel   # skip if already installed
vercel --prod
```

Or push this folder to a GitHub repo and import it at vercel.com/new.
No framework, no build step — Vercel serves it as static files.

## Blocking a version

Edit `announcement.json`:

```json
{
  "blockedVersions": ["3.5", "3.6"]
}
```

Redeploy (`vercel --prod`). Any install running a listed version shows the
"Update required" overlay next time its popup opens.
