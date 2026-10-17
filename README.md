# Tally Fitness

Workout tracker and food diary with calorie and macro targets. It runs in the browser, works offline, and can back up each person's data to their own account (Firebase).

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

- Without an account, everything is saved on the device, in that browser.
- **Plan > Your data > Create account or sign in** creates a free account. Everything already logged on that phone moves into the account, and from then on it's backed up to the cloud and available on any device you sign in on.
- The app still works offline. Changes are saved on the phone and upload when you're back online.
- Anyone you give the address to gets their own account. They can't see your data and you can't see theirs.
- **Download backup** still works as an extra safety copy.

## Firebase setup (already done for this app)

The app uses the Firebase project `tally-82514`: Authentication with Email/Password, and a Firestore database. Each user's data is stored at `users/{uid}` (settings, workouts, foods, weigh-ins) with one document per day under `users/{uid}/days/{date}`.

Scanned products that people enter from the label are shared in `barcodes/{barcode}`, so anyone signed in can scan them.

Firestore rules (Firebase console > Firestore Database > Rules): each user reaches only their own data, everyone signed in can read shared barcodes, and only the person who added a barcode can change it:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    match /barcodes/{code} {
      allow read: if request.auth != null;
      allow create: if request.auth != null && code.matches('^[0-9]{6,14}$') && validFood();
      allow update: if request.auth != null && resource.data.by == request.auth.uid && validFood();
      function validFood() {
        let d = request.resource.data;
        return d.by == request.auth.uid
          && d.keys().hasOnly(['n','kcal','p','c','f','fib','sl','sg','by','at'])
          && d.n is string && d.n.size() > 0 && d.n.size() <= 60
          && d.kcal is number && d.kcal >= 0 && d.kcal <= 950
          && d.p is number && d.p >= 0 && d.c is number && d.c >= 0 && d.f is number && d.f >= 0
          && d.p + d.c + d.f <= 101;
      }
    }
  }
}
```

## Updating the app

Replace `index.html` with the new version, then change `VERSION` at the top of `sw.js` (for example to `tally-v10`) and upload both. Installed copies pick up the update the next time they're opened online.
