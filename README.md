# Katete Agrodealers — setup guide

This folder is your whole app. Follow these steps in order. None of it costs money.

## Part 1 — Firebase (your cloud database)

1. Go to https://console.firebase.google.com and sign in with a Google account.
2. Click **Add project** → name it `katete-agrodealers` → you can turn off Google Analytics (not needed) → **Create project**.
3. In the left menu, go to **Build → Firestore Database** → **Create database** → choose a location close to Malawi (e.g. `europe-west1`) → start in **Production mode** (we'll apply our own rules in step 6).
4. In the left menu, click the **gear icon → Project settings**. Scroll down to "Your apps" and click the **Web icon (`</>`)**. Give it a nickname like "Katete App" → **Register app**. Firebase will show you a `firebaseConfig` object that looks like this:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "katete-agrodealers.firebaseapp.com",
     projectId: "katete-agrodealers",
     storageBucket: "katete-agrodealers.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```

5. Open `index.html` in this folder, find the `firebaseConfig` block near the top of the `<script type="module">` section, and replace the `PASTE_..._HERE` placeholders with your real values from step 4.

6. Back in the Firebase console, go to **Firestore Database → Rules**, delete what's there, and paste in the contents of `firestore.rules` from this folder. Click **Publish**.

## Part 2 — Put the app online (GitHub Pages, free)

1. Go to https://github.com and create a free account if you don't have one.
2. Click **New repository** → name it `katete-agrodealers` → make it **Public** → **Create repository**.
3. On the repo page, click **Add file → Upload files**, then drag in every file from this folder (`index.html`, `manifest.json`, `sw.js`, `icon-192.jpeg`, `icon-512.jpg`). Commit the changes.
4. Go to the repo's **Settings → Pages**. Under "Build and deployment", set Source to **Deploy from a branch**, branch **main**, folder **/ (root)** → **Save**.
5. Wait about a minute, then refresh that page — GitHub will show you your live URL, something like:
   `https://yourusername.github.io/katete-agrodealers/`
6. Open that URL on your phone or computer and test it — add a shop, log in as that shop, report a sale. This is now a real, permanent, working web app.

## Part 3 — Turn it into an APK

1. Go to https://www.pwabuilder.com
2. Paste in your GitHub Pages URL from Part 2 and press Start.
3. PWABuilder will scan the site and show scores for Manifest, Service Worker, and Security — all three should be green since this app already includes them.
4. Click **Package for Stores** → choose **Android** → keep the default options → **Generate**.
5. Download the `.zip` it gives you and unzip it — inside is your `.apk` file (or `.aab` if you chose that option).

## Part 4 — Install it on your phone

1. Transfer the `.apk` file to your phone (Google Drive, WhatsApp to yourself, USB cable — any way works).
2. Open the file on your phone. Android will warn about "installing from unknown sources" — allow it for this file.
3. It installs like any other app, with the Katete Agrodealers icon on your home screen.
4. Share the GitHub Pages link (or the same `.apk` file) with your shopkeepers so they can install it too — everyone's app talks to the same Firebase database, so it all stays in sync.

## Notes for later

- **Making changes:** if you ever want to tweak the app, edit `index.html`, re-upload it to your GitHub repo (Part 2, step 3), and it updates automatically at the same URL — no need to repackage the APK unless you want a fresh install file.
- **Security:** the current Firestore rules (`firestore.rules`) allow anyone with your app's link and Firebase config to read/write the database directly — fine for a small trusted team, but worth tightening later with Firebase Authentication if the business grows or the app becomes public.
- **Costs:** Firebase's free Spark plan gives you 1 GiB storage and 50,000 reads / 20,000 writes / 20,000 deletes per day, at no cost and with no card required. A handful of shops won't come close to that.
