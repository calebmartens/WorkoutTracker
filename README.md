# Tally Fitness

Workout tracker and food diary with calorie and macro targets. It runs entirely in the browser: no account, no server, and each person's data stays on their own device.

## Put it on GitHub Pages

1. Create a new public repository on GitHub (for example `tally`).
2. Upload every file in this folder to the repository: `index.html`, `manifest.webmanifest`, `sw.js`, the four `.png` icons and this README.
3. In the repository, open **Settings > Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**, pick the `main` branch and the `/ (root)` folder, then save.
4. After a minute or two the page shows your site address, usually `https://<your-username>.github.io/tally/`.

## Add it to your phone

- **iPhone (Safari):** open the address, tap **Share**, then **Add to Home Screen**.
- **Android (Chrome):** open the address, tap the **⋮** menu, then **Install app** or **Add to Home screen**.

Once installed it opens full screen and works offline.

## Your data

- Everything is saved on the device, in that browser. Nothing is uploaded anywhere.
- Anyone you give the address to gets their own empty copy of the app. They can't see your data and you can't see theirs.
- Use **Plan > Your data > Download backup** now and then. Use **Restore backup** to move your data to a new phone or recover it after clearing the browser.

## Updating the app

Replace `index.html` with the new version, then change `VERSION` at the top of `sw.js` (for example to `tally-v2`) and upload both. Installed copies pick up the update the next time they're opened online.
