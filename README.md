# John Matthew Rey — Portfolio

Personal portfolio site for John Matthew Rey — IT graduate seeking remote
**Customer Support**, **Data Entry**, and **Service Desk** roles.

**Live site:** _add your deployed URL here once live_

## What's in this repo

| File | Description |
|---|---|
| `index.html` | Main portfolio page — about, skills, experience, projects, and a working contact form |
| `fitquest.html` | FitQuest — a gamified fitness tracker built as a project sample, linked from the portfolio |

No build step, no dependencies, no framework. Both pages are plain HTML, CSS,
and vanilla JavaScript — open `index.html` directly in a browser and
everything works.

## Features

**Portfolio (`index.html`)**
- Responsive layout, mobile nav, scroll-triggered animations
- Skills shown as proficiency "signal bars" across Support/Productivity and
  Technical/Web categories
- Experience timeline and education section
- Contact form wired for [Netlify Forms](https://docs.netlify.com/forms/setup/)
  (works automatically once deployed on Netlify — no backend required)

**FitQuest (`fitquest.html`)**
- Daily rotating quests (8 exercises drawn from a larger pool, refreshes
  every day) plus a separate "Daily Challenge" bonus quest
- XP bar, levels, and rank titles (Rookie → Legend)
- Day-streak tracking and an achievements/badge system
- Custom quest creation
- Optional cloud sync via Google Sign-In (Firebase Auth + Firestore) — see setup below
- All progress saved locally in the browser via `localStorage`

## Running it locally

No install needed:

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
cd YOUR-REPO-NAME
open index.html   # or just double-click it
```

## Deployment

### Option A — GitHub Pages (simplest, pure GitHub)
1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, choose the `main` branch and `/ (root)` folder, then save.
4. Your site will be live at `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`.

⚠️ **Contact form limitation:** GitHub Pages has no server, so the Netlify
Forms attribute on the contact form will not actually send anything if
deployed this way. Use Option B if you want the form to work.

### Option B — Netlify, connected to this GitHub repo (recommended)
1. Push this repo to GitHub first.
2. In [Netlify](https://app.netlify.com), choose **Add new site → Import an
   existing project → GitHub**, and select this repo.
3. Leave the build settings empty (no build command, publish directory `/`).
4. Deploy. Every future `git push` to `main` auto-deploys the new version.
5. The contact form works automatically — submissions appear under your
   Netlify site's **Forms** tab.

## Cloud sync setup (FitQuest, optional but recommended)

FitQuest can save your XP, streak, and quests to the cloud instead of just
this browser — sign in with Google on your phone and see the same progress
you built on your laptop. This uses **Firebase** (Google's free
backend-as-a-service): **Firebase Authentication** for sign-in and
**Firestore** as the database. There's no server to write or host — the
Firebase SDK is called directly from the browser, and it works the same on
GitHub Pages or Netlify.

Without this setup, FitQuest still works completely fine — it just falls
back to saving locally in that one browser ("guest mode"), and the sign-in
area will say "Cloud sync not set up yet."

### 1. Create a Firebase project
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
   and click **Add project**. It's free (Spark plan).
2. Once created, click the **Web** icon (`</>`) to register a web app. Give
   it any nickname.
3. Firebase will show you a `firebaseConfig` object with your `apiKey`,
   `authDomain`, `projectId`, etc. Keep this tab open — you'll need it in
   step 4.

### 2. Enable Google Sign-In
1. In the Firebase console, go to **Build → Authentication → Get started**.
2. Under **Sign-in method**, enable **Google**, and set a support email.

### 3. Create the database
1. Go to **Build → Firestore Database → Create database**.
2. Choose **Start in production mode**, pick any region close to you.
3. Once created, go to the **Rules** tab and replace the default rules with:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /fitquest_users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```
   This ensures each signed-in user can only read and write their own
   progress document — nobody else's.
4. Click **Publish**.

### 4. Add your web app's domain
1. Back in **Authentication → Settings → Authorized domains**, add your
   live domain (e.g. `your-site-name.netlify.app` or
   `YOUR-USERNAME.github.io`). `localhost` is already included by default
   for local testing.

### 5. Paste your config into the code
In `fitquest.html`, find the `firebaseConfig` object near the bottom of the
`<script type="module">` block and replace the placeholder values with the
real ones from step 1:

```js
var firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

That's it — reload the page and "Sign in with Google" will actually
authenticate and sync your progress to Firestore.

## Before this goes live

A few placeholders in `index.html` still need your real details (marked in
orange on the page):
- Email, phone number, and CV/resume link
- Graduation year
- Exact employment dates for the Derive role

## License

MIT — see [LICENSE](LICENSE).
