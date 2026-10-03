# Kasa

A quick expense log with multi-currency totals. It runs as a web app on GitHub Pages and syncs to a CSV file in your Google Drive.

## 1. Put it on GitHub Pages (about 5 minutes)

1. On GitHub, create a new **public** repository called `kasa`.
2. Click **Add file → Upload files**, drag in every file from this folder, and commit.
3. Go to **Settings → Pages**. Under "Build and deployment", choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute the app is live at `https://YOUR-USERNAME.github.io/kasa/`.

Kasa already works at this point. Your data is saved on the phone, and exchange rates update automatically once a day.

## 2. Turn on Google Drive sync (about 15 minutes, once)

1. Go to <https://console.cloud.google.com> and create a new project called **Kasa**.
2. Open **APIs & Services → Library**, search for **Google Drive API**, and click **Enable**.
3. Open **Google Auth Platform** (called "OAuth consent screen" in older layouts):
   - App name: `Kasa`, support email: your email. Audience: **External**.
   - Under **Audience → Test users**, add your own Google account.
   - Under **Data access**, add the scope `.../auth/drive.file`. Kasa can then see only the files it creates itself, nothing else in your Drive.
4. Open **Clients → Create client**:
   - Application type: **Web application**.
   - Authorized JavaScript origins: `https://YOUR-USERNAME.github.io` (no `/kasa` at the end).
   - Create it, then copy the **Client ID**. It ends in `.apps.googleusercontent.com`.
5. Back on GitHub, open `config.js`, click the pencil icon, paste the Client ID between the quotes, and commit.

The app can stay in "Testing" mode forever. You're its only user.

## 3. On your phone

1. Open `https://YOUR-USERNAME.github.io/kasa/` in Chrome. Use the menu to **Install app** or **Add to Home screen**.
2. In Kasa, open **Settings → Connect Google Drive** and sign in. Google will warn that the app isn't verified. That's expected for a personal app: tap **Continue**.
3. Two files appear in your Drive: `Kasa expenses.csv` (open it in Sheets any time) and `Kasa settings.json`.

### Moving your data in

- **From the Claude-hosted Kasa:** Settings → Export backup, then in the new Kasa: Settings → Import CSV.
- **From Monefy:** export a CSV from Monefy's settings, then Settings → Import CSV. Income and transfers are skipped.

## Good to know

- **Sign-in lasts an hour.** After that a yellow **Tap to sync** button appears at the top. Anything you log meanwhile is kept on the phone and goes up on the next sync.
- **Local storage** is shared between Chrome and the installed app, and Kasa asks Chrome to protect it from automatic cleanup. Clearing Chrome's site data still wipes it, and Drive brings it back.
- **Two devices** can edit at the same time. Kasa merges both sets of changes, including deletions.
- **Exchange rates** come from open.er-api.com, once a day. Editing a rate by hand switches automatic updates off. Turn them back on in Settings.
- **Updating the app:** after you change files, raise `VERSION` in `sw.js` (for example to `kasa-v2`) so phones pick up the new version.
- Totals convert every entry at **today's** rates, not the rate on the day you spent it.
