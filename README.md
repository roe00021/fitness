# Gym Tracker

A mobile-first gym log. No build step, no dependencies: plain HTML, CSS and JavaScript.

- Log sets with a visual barbell / dumbbell loader and colored plate buttons
- History with PR badges, edit/delete, and a chart of top weight vs. estimated 1RM
- Installs to your home screen and opens offline (PWA)
- **Your log is saved to a file in a GitHub repo (the cloud)**, so it follows you across devices

Your existing log (52 sets from the Claude version) is built into `index.html` as starting data. It is
uploaded to your data repo the first time you connect, and the cloud copy wins after that. To keep your
lifting history out of a public site repo, set `var SEED=[];` in `index.html` before publishing and use
Settings > Restore from backup instead.

## 1. Host the site on GitHub Pages

1. Create a repository (for example `gym-tracker`) and upload every file in this folder to the root.
2. **Settings > Pages**: choose **Deploy from a branch**, pick `main` and `/ (root)`, then Save.
3. After a minute it is live at `https://YOUR-USERNAME.github.io/gym-tracker/`.
4. On your phone open the link. iPhone: Share > Add to Home Screen. Android: menu > Install app.

## 2. Turn on cloud saving (once per device)

A website on GitHub Pages cannot write to GitHub by itself, so the app needs a token. Do this once on each
phone or browser you use.

1. Create a **separate private repo** for your data, for example `gym-data`. Do not use the public site
   repo: anything in a public repo is visible to everyone.
2. GitHub > Settings > Developer settings > Personal access tokens > Fine-grained tokens > Generate new
   token. Under Repository access choose **Only select repositories** and pick your data repo. Under
   Permissions > Repository permissions set **Contents: Read and write**.
3. Open the app > **Settings**. Enter `yourname/gym-data`, branch `main`, file `gym-log.json`, paste the
   token, and tap **Save & sync**.

## How saving works

- Tap Add set (or edit or delete) and the change is committed to `gym-log.json` in your data repo within
  about a second. The bar at the top turns green and says **Saved to GitHub** once it is stored.
- If you are offline or GitHub is unreachable, the bar turns red, the change is kept in the browser, and
  it uploads automatically when the connection returns or when you next open the app. Nothing is lost
  unless you clear the browser's site data before it uploads.
- Opening the app (or switching back to it) pulls the newest copy from GitHub. If two devices disagree,
  the most recently saved one wins, so avoid logging on two devices at the same moment.
- The token is stored in that browser's local storage. Anyone with access to your unlocked phone or
  browser profile could read it, so limit it to the one data repo and set an expiry date.
- Settings > Download backup saves a copy of your log as a file; Restore loads one.

## If you clear your browser history or data

Your log is safe: the real copy is in your GitHub data repo, and every save is a commit, so GitHub also
keeps the full version history of the file. Clearing browser data only removes the local cache and the
saved token on that device. To get back in:

1. Open the app and go to Settings, then re-enter the repo, branch, file and token and tap Save & sync. Or
   open your **private setup link** (Settings > Copy private setup link) and it does this for you.
2. Your whole log reloads from GitHub.

Do this now, before you need it: in Settings tap **Copy private setup link** and store it in a password
manager. The link contains your token, so don't share it.

Other safeguards built in:
- The app asks the browser to treat its storage as persistent, so it is not removed automatically.
- If a device has sets that never reached GitHub (for example you logged offline), they are merged into
  the cloud copy instead of overwriting it, even on a freshly cleared device.
- Settings > Download backup gives you a file copy as well.

## Updating the app

After editing any file, change `VERSION` in `sw.js` (for example `gym-v3`) so phones pick up the new
version instead of the cached one.

## Files

- `index.html` the whole app
- `manifest.json`, `sw.js`, `icon*.png`, `apple-touch-icon.png` installable app and offline support
