# Mariam File Manager — browser-first prototype

A responsive, static web app for selecting local files, listing Google Drive files/folders, uploading/downloading files, splitting files into numbered parts, merging part files, and running a small set of text commands.

## Run locally

Because browsers restrict file and Google OAuth behavior on `file://` URLs, serve this folder over localhost:

```bash
python -m http.server 8080
```

Then open `http://localhost:8080` in a modern browser. Or publish these files on GitHub Pages / another HTTPS static host.

## Google Drive setup

1. In Google Cloud Console, create/select a project and enable **Google Drive API**.
2. Configure the OAuth consent screen and add your account as a test user if the app is in testing.
3. Create an OAuth Client ID of type **Web application**.
4. Add the exact origin used to open the site under **Authorized JavaScript origins** (for example `http://localhost:8080` for local development or `https://YOUR-USERNAME.github.io` for GitHub Pages). Do not add a path to an origin.
5. Paste the Client ID into Connection settings and save. Then choose Connect Google Drive.

This app uses Google Identity Services and a session-only access token. It does not ask for or store a client secret. Current scopes are `drive.file` and `drive.readonly`; access to all existing Drive content may require a more carefully reviewed scope strategy and additional Google verification depending on the app's use and publishing status.

## Browser and OS limitations

- The browser cannot silently browse every folder on a device. Users must choose files/folders and grant permission.
- `showDirectoryPicker()` is not supported in every browser (notably iOS Safari); the app includes a folder-input fallback where supported.
- Browsers may block file pickers inside cross-origin iframes. Open the site directly in its own tab.
- Splitting downloads each part separately. Merging currently creates a Blob in browser memory, so extremely large merges can exhaust memory; streaming output to a writable file is a future improvement where the File System Access API is available.
- Google Drive downloads of Google Docs/Sheets/Slides require export endpoints rather than `alt=media`; those are not yet implemented.
- Command parsing is intentionally conservative and supports a small set of English/Arabic commands. It does not execute arbitrary text as code.
- The prototype has not been deployed to the user's GitHub Pages site or tested on every target device/browser.

## Commands currently recognized

- `list local files`
- `connect google drive`
- `split selected into 100 MB parts`
- `merge selected parts`
- `copy selected` — copies selected local or Drive files into the app clipboard.
- `paste selected` — uploads local files in the clipboard to the currently open Google Drive folder; Drive files in the clipboard are copied into the current Drive folder. Originals are kept.
- `upload selected`
- `download selected`
- `refresh`

## Project files

- `index.html` — application structure
- `styles.css` — responsive styling
- `app.js` — local-file operations, Drive API, split/merge and command parsing
