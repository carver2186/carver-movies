# Setup — one-time, ~10 minutes

This turns your movie tracker into a phone app that writes straight back to GitHub —
no copy/paste into chat, ever again. You'll do three things: create a repo, upload these
files, then create a personal access token and enter it in the app's Settings.

## 1. Create the repo
1. Go to github.com, log in, click **+** (top right) → **New repository**.
2. Name it whatever you like (e.g. `carver-movies`). Keep it **Public** — GitHub Pages
   on the free tier needs a public repo, and the only thing exposed is your movie list
   (no token, no secrets, ever lives in the repo).
3. Click **Create repository**.

## 2. Upload the app files
1. On the new repo's page, click **Add file → Upload files**.
2. Drag in everything from this `movie-app` folder: `index.html`, `manifest.json`,
   `sw.js`, `data.json`, and the `icons` folder (`icon-192.png`, `icon-512.png`).
3. Click **Commit changes**.

## 3. Turn on GitHub Pages
1. In the repo, go to **Settings → Pages**.
2. Under "Build and deployment," set **Source** to "Deploy from a branch."
3. Set **Branch** to `main` and folder to `/ (root)`, then **Save**.
4. Wait about a minute. Your app's URL will be:
   `https://<your-github-username>.github.io/<repo-name>/`
   (GitHub shows this URL at the top of the Pages settings page once it's live.)

## 4. Create your access token
This lets your phone/browser write directly to the repo. It's scoped to only this one
repo, so it can't touch anything else in your GitHub account.

1. Go to **github.com/settings/tokens?type=beta** (Settings → Developer settings →
   Personal access tokens → Fine-grained tokens).
2. Click **Generate new token**.
3. Name it anything (e.g. "Movie tracker").
4. **Repository access** → "Only select repositories" → pick your new repo.
5. **Permissions** → **Repository permissions** → **Contents** → set to **Read and write**.
   Leave everything else as "No access."
6. Click **Generate token**, then **copy it immediately** — GitHub only shows it once.

## 5. Open the app and connect it
1. On your phone, visit the Pages URL from step 3 in Safari (iPhone) or Chrome (Android).
2. The app will prompt you for Settings on first load. Enter:
   - **GitHub username** — your username (the one in the URL).
   - **Repo name** — what you named it in step 1.
   - **Personal access token** — what you copied in step 4.
3. Tap **Save & reload**. Your movie list should load.

## 6. Add it to your home screen (so it feels like a real app)

**iPhone (Safari only — Chrome/other browsers can't install PWAs on iOS):**
1. Open the app URL in Safari.
2. Tap the **Share** icon (square with an arrow) → **Add to Home Screen**.
3. Tap **Add**. A movie icon appears on your home screen — tapping it opens the app
   full-screen, no browser bar.

**Android (Chrome):**
1. Open the app URL in Chrome.
2. Tap the **⋮** menu → **Add to Home screen** (or you'll see an "Install app" banner).

**Desktop (Chrome/Edge):**
- Click the install icon in the address bar, or just bookmark the URL.

## How it works day to day
Tap a rating, tap "Queue next," tap the reorder arrows — every tap commits straight to
`data.json` in your GitHub repo. The sync line at the top of the app always shows
whether it's saved, saving, or hit an error (with a Retry button if so). No exporting,
no pasting into chat.

Separately, Claude pulls the latest `data.json` from the repo on a recurring schedule
and folds any changes into `movies.csv` and `ranked-watchlist.md` automatically — that
part needs no action from you once the schedule is set up.

## If something breaks
- **"data.json not found"** in the sync line → double check the repo name in Settings
  matches exactly (case-sensitive).
- **Save failed: 401** → your token is wrong or expired. Generate a new one (step 4)
  and re-enter it in Settings.
- **Save failed: 409** → two devices saved at nearly the same time. Just reload the
  app — it'll re-fetch the latest version — and redo the last tap.
