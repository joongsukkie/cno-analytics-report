# CNO Reports — desktop app

The report generator can be packaged as a real installable app for **Windows** and **macOS**,
in addition to running in a browser or as an installable web app (PWA). Same single `index.html`
inside a native window, powered by [Electron](https://www.electronjs.org/).

You do **not** need a Mac to build the Mac version, and you do not need to install anything to
get the installers. GitHub builds them for you.

## The easy way: let GitHub build the installers

A workflow at `.github/workflows/build-desktop.yml` builds both installers on GitHub's own
Windows and macOS machines.

**Option A — tag a release (recommended).** From the repo:

```bash
git tag v1.0.0
git push origin v1.0.0
```

In a few minutes a **Release** appears on GitHub with two files attached:

- `CNO Reports Setup 1.0.0.exe` — Windows installer
- `CNO Reports-1.0.0.dmg` — macOS disk image

**Option B — run it on demand.** On GitHub: **Actions → "Build desktop installers" → Run workflow.**
When it finishes, download the installers from the run's **Artifacts** section.

Bump the `"version"` in `package.json` for each new release so the tag and the installer match.

## Installing the app

- **Windows:** double-click the `.exe`, choose a folder, done. Because the app is not code-signed,
  Windows SmartScreen may say "Windows protected your PC." Click **More info → Run anyway**.
- **macOS:** open the `.dmg`, drag **CNO Reports** to Applications. Because the app is not signed
  by an Apple Developer account, the first launch needs **right-click → Open → Open** (a normal
  double-click will refuse). After that it opens normally.

Removing those two friction points requires paid code-signing certificates (an Apple Developer
account at 99 USD per year for macOS, and a Windows code-signing certificate). For an internal
team tool this is optional; the right-click-open step is a one-time thing per machine.

## What the app does

- Opens the full report generator in its own window, offline. No server, no account.
- Everything works exactly like the web version: upload CSVs, load the demo, customize, AI
  narrative (with your own API key), print/PDF.
- **Share links created inside the desktop app point to the hosted site**
  (`https://cno-analytics-report.onrender.com/`), so a client can open the link in a normal
  browser. If you move the hosted site, update `HOSTED_BASE` near the top of the script in
  `index.html`.

## Building locally (optional, for developers)

Requires [Node.js](https://nodejs.org/) 18+.

```bash
npm install
npm start          # run the app in a dev window
npm run dist:win   # build a Windows installer   (only works on Windows)
npm run dist:mac   # build a macOS installer      (only works on macOS)
```

Installers land in `dist-desktop/`. A machine can only build its own platform's installer, which
is exactly why the GitHub workflow above exists.

## Minimum OS

Windows 10 (1809+) or Windows 11, and macOS 11 (Big Sur) or newer. Older Windows 10 without the
Edge WebView runtime is handled automatically by the installer.

## Which should we ship — desktop app, or the web app?

Both are maintained from the same `index.html`, so it is not either/or:

- **Web app / PWA** (already live on Render): nothing to install, always the latest version,
  works on phones. Best for clients opening share links and for quick access.
- **Desktop app** (this file): a real icon in the dock/Start menu, opens offline, feels like
  owned software. Best for CNO staff who build reports every month.
