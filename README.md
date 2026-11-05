# ML Study Workbook

A single-page, no-build static site. Everything lives in `index.html`
(HTML, CSS and JavaScript). Fonts load from Google Fonts.

- `index.html`: the site
- `firebase-config.js`: your Firebase web config (empty by default)
- `firestore.rules`: security rules for the progress data
- `vercel.json`: Vercel settings

## Deploy to Vercel

**Vercel CLI**

```bash
npm i -g vercel
cd ~/Desktop/Projects/ml-workbook
vercel          # preview deployment
vercel --prod   # production deployment
```

Accept the defaults: framework preset **Other**, no build command, empty output directory.

**Or Git:** push this folder to GitHub and import it in Vercel with **Add New → Project**.

## Google sign-in and synced progress (Firebase)

Without Firebase the site still works and saves ticks and solved questions in the
browser only. To sign in with Google and have progress follow you across devices:

1. Go to https://console.firebase.google.com and **Add project** (Analytics can stay off).
2. **Build → Authentication → Get started → Sign-in method → Google → Enable**, choose a support email, **Save**.
3. **Build → Firestore Database → Create database**, pick a region, start in **production mode**.
4. In Firestore open the **Rules** tab, paste the contents of `firestore.rules`, **Publish**.
5. **Project settings (gear) → General → Your apps → Web (`</>`)**, register an app, and copy the `firebaseConfig` values.
6. Paste them into `firebase-config.js` as `window.FIREBASE_CONFIG = { ... };`
7. **Authentication → Settings → Authorized domains → Add domain**: add your Vercel domain
   (for example `ml-workbook.vercel.app`, and your custom domain if you have one). `localhost` is allowed by default.
8. Redeploy (`vercel --prod`), open the site, and press **Sign in with Google**.

Data layout: one document per user at `users/<uid>` with `{ l: {lessonId: true}, q: {questionId: true}, t: timestamp }`.
The first time you sign in on a browser, progress already saved there is merged with your account; after that your account is the source of truth.

## Run locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```
