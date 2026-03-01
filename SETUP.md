# StoryChat — Firebase Setup Guide

## Step 1: Create a Firebase Project

1. Go to https://console.firebase.google.com
2. Click **"Add project"**
3. Name it (e.g. `storychat-app`)
4. Disable Google Analytics (optional for simplicity)
5. Click **Create project**

---

## Step 2: Enable Google Sign-In

1. In your Firebase project → **Authentication** → **Get started**
2. Click **"Sign-in method"** tab
3. Enable **Google** → Save

---

## Step 3: Create Firestore Database

1. Go to **Firestore Database** → **Create database**
2. Choose **"Start in test mode"** (for development)
3. Pick a region → Done

### Firestore Security Rules (paste these):
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /conversations/{docId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.uid;
      allow create: if request.auth != null;
    }
  }
}
```
Go to: Firestore → Rules tab → paste → Publish

---

## Step 4: Register Your Web App

1. In Firebase project → Click the **</>** (Web) icon
2. Name it `storychat-web`
3. Click **Register app**
4. You'll get a `firebaseConfig` object like this:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "storychat-xyz.firebaseapp.com",
  projectId: "storychat-xyz",
  storageBucket: "storychat-xyz.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

---

## Step 5: Paste Config into index.html

Open `index.html` and find this section near the top of the `<script>`:

```js
const FB_CONFIG = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Replace with your actual config from Step 4.

---

## Step 6: Add Authorized Domain

1. Firebase → Authentication → **Settings** tab
2. Under **Authorized domains**, add your domain
   - For local testing: `localhost` is already there
   - For production: add `yourdomain.com`

---

## Step 7: Deploy (Optional)

### Option A: Firebase Hosting (recommended)
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# Set public directory to: . (current folder)
# Configure as SPA: Yes
firebase deploy
```

### Option B: GitHub Pages
Push your files to a GitHub repo → Settings → Pages → Deploy from branch

---

## That's it!

Once configured:
- Writers sign in with Google
- All conversations auto-save to Firestore
- They can load past conversations across devices
- Each writer only sees their own conversations (secured by Firestore rules)

---

## File Structure
```
storychat-pwa/
├── index.html      ← Main app
├── manifest.json   ← PWA manifest
├── sw.js           ← Service worker (offline support)
└── SETUP.md        ← This file
```
