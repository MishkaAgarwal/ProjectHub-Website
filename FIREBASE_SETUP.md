# Project Vault — Firebase Setup Guide

Project Vault can run in two modes:

1. **Local mode** (no Firebase): Projects saved in browser localStorage. Works immediately, no setup. Data may be lost if browser storage is cleared.
2. **Firebase mode**: Projects saved to Firestore cloud database. Images stored in Firebase Storage. Works across devices and sessions.

---

## Setting Up Firebase

### Step 1: Create a Firebase Project

1. Go to https://console.firebase.google.com
2. Click **Add project** and follow the prompts.
3. Disable Google Analytics if you do not need it.

### Step 2: Enable Firestore Database

1. In your project console, go to **Build ? Firestore Database**.
2. Click **Create database**.
3. Select **Start in test mode** initially (you will secure it later).
4. Choose your preferred region.

### Step 3: Enable Firebase Storage

1. Go to **Build ? Storage**.
2. Click **Get started** and follow the prompts.
3. Choose the same region as your Firestore database.

### Step 4: Enable Authentication (optional but recommended)

1. Go to **Build ? Authentication**.
2. Click **Get started**.
3. Enable **Email/Password** under Sign-in providers.

### Step 5: Register a Web App and Get Config

1. Go to **Project settings** (gear icon).
2. Scroll to **Your apps** and click the web icon `</>`.
3. Register the app with a name (e.g. "Project Vault Web").
4. Copy the `firebaseConfig` object shown.

### Step 6: Add Config to Project Vault

Open `project_vault.html` in a text editor and find:

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "REPLACE_WITH_YOUR_API_KEY",
  authDomain:        "REPLACE_WITH_YOUR_AUTH_DOMAIN",
  projectId:         "REPLACE_WITH_YOUR_PROJECT_ID",
  storageBucket:     "REPLACE_WITH_YOUR_STORAGE_BUCKET",
  messagingSenderId: "REPLACE_WITH_YOUR_SENDER_ID",
  appId:             "REPLACE_WITH_YOUR_APP_ID"
};
```

Replace each placeholder with the actual values from your Firebase console.

### Step 7: Secure Firestore Rules

Replace the default rules with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /projects/{projectId} {
      allow read, write: if request.auth != null;
    }
    match /projects/{projectId}/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Step 8: Secure Storage Rules

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /projects/{projectId}/{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

---

## Free Tier Limits (as of 2025)

| Resource         | Free Limit      |
|-----------------|-----------------|
| Firestore reads  | 50,000/day      |
| Firestore writes | 20,000/day      |
| Firestore storage| 1 GB            |
| Storage          | 5 GB            |
| Storage downloads| 1 GB/day        |
| Auth users       | Unlimited (email)|

These limits are sufficient for a personal project portfolio.

---

## Local Development

You can run the website by opening `project_vault.html` directly in Chrome or Edge.
No web server is required for local use. For Firebase to work, you may need to serve
from localhost due to browser security restrictions.

To serve locally:
```bash
# Python 3
python -m http.server 8080
# Then open http://localhost:8080/project_vault.html
```

---

## Environment Variables (if using a build system)

If you later move to a build system (Vite, Next.js, etc.), use these variable names:

```
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

**Never commit real credentials to a public repository.**
