# IVC Records

An offline-first web app for Illiana Vision Connection: people, meetings, attendance
and pre/post surveys. Built screen-reader-first for VoiceOver on iPhone and JAWS on
Windows.

## What it is

One HTML file with everything inlined — CSS, JavaScript, and the Atkinson Hyperlegible
typeface. No build step, no framework, no backend, no accounts, no tracking. A service
worker caches the app so it runs with no signal at all.

Records are stored in `localStorage` on the device that entered them. Nothing is
transmitted anywhere.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app: people, contacts and services, meetings, attendance, surveys |
| `manifest.webmanifest` | Lets iOS and Android install it to the home screen |
| `sw.js` | Service worker — caches the shell so it works offline |
| `icon-192.png`, `icon-512.png` | Home screen icons |

## Publishing to GitHub Pages

```sh
gh repo create ivc-records --public --source=. --remote=origin --push
```

Then in the repository: **Settings → Pages → Source: Deploy from a branch →
`main` / `(root)` → Save**.

The app appears at `https://1eyebiney.github.io/ivc-records/` within a minute or two.
HTTPS is required for the service worker, and GitHub Pages provides it.

If you prefer the web UI: create the repo, upload these five files to the root, then
set Pages as above.

## Updating it

Edit `index.html`, **bump `CACHE` in `sw.js`** (`ivc-records-v1` → `v2`), commit, push.
Without the version bump, phones keep serving the copy they already cached and nobody
sees the change.

`index.template.html` is the source with `{{FONT400}}` / `{{FONT700}}` placeholders;
`index.html` is that file with the fonts base64-inlined. Edit the template and re-inline,
or edit `index.html` directly — it is a normal HTML file.

## Known limits, by design for now

- **Each phone holds its own records.** Two people taking attendance at the same meeting
  on different phones each end up with half of it. Pick one phone per meeting until
  shared storage is added.
- Meetings, surveys and contacts cannot be edited or deleted after saving.
- No duplicate-person detection.
- Reports are the six counts on the Today screen.

## Adding shared storage later

Everything that touches persistence is the `Store` object at the top of the script —
`read`, `write`, `flag`. Point those three functions at a backend (Supabase, Cloudflare
KV, anything with a REST API) and the rest of the app is unchanged. The offline cache
and the backup/restore flow stay useful either way.

## Credits and licence

Typeface: [Atkinson Hyperlegible](https://www.brailleinstitute.org/freefont) by the
Braille Institute of America, used under the SIL Open Font License 1.1 and embedded in
`index.html` as permitted by that licence.
