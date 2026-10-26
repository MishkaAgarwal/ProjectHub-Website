Project Vault — Firebase Setup Guide
Project Vault can run in two modes:
Local mode (no Firebase): Projects saved in browser localStorage. Works immediately, no setup. Data may be lost if browser storage is cleared.
Firebase mode: Projects saved to Firestore cloud database. Images stored in Firebase Storage. Works across devices and sessions.
---
Setting Up Firebase
Step 1: Create a Firebase Project
Go to https://console.firebase.google.com
Click Add project and follow the prompts.
Disable Google Analytics if you do not need it.
Step 2: Enable Firestore Database
In your project console, go to Build ? Firestore Database.
Click Create database.
Select Start in test mode initially (you will secure it later).
Choose your preferred region.
Step 3: Enable Firebase Storage
Go to Build ? Storage.
Click Get started and follow the prompts.
Choose the same region as your Firestore database.
Step 4: Enable Authentication (optional but recommended)
Go to Build ? Authentication.
Click Get started.
Enable Email/Password under Sign-in providers.
Step 5: Register a Web App and Get Config
Go to Project settings (gear icon).
Scroll to Your apps and click the web icon `</>`.
Register the app with a name (e.g. "Project Vault Web").
Copy the `firebaseConfig` object shown.
Step 6: Add Config to Project Vault
Open `project\_vault.html` in a text editor and find:
```javascript
const FIREBASE\_CONFIG = {
  apiKey:            "REPLACE\_WITH\_YOUR\_API\_KEY",
  authDomain:        "REPLACE\_WITH\_YOUR\_AUTH\_DOMAIN",
  projectId:         "REPLACE\_WITH\_YOUR\_PROJECT\_ID",
  storageBucket:     "REPLACE\_WITH\_YOUR\_STORAGE\_BUCKET",
  messagingSenderId: "REPLACE\_WITH\_YOUR\_SENDER\_ID",
  appId:             "REPLACE\_WITH\_YOUR\_APP\_ID"
};
```
Replace each placeholder with the actual values from your Firebase console.
Step 7: Secure Firestore Rules
Replace the default rules with:
```
rules\_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /projects/{projectId} {
      allow read, write: if request.auth != null;
    }
    match /projects/{projectId}/{document=\*\*} {
      allow read, write: if request.auth != null;
    }
  }
}
```
Step 8: Secure Storage Rules
```
rules\_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /projects/{projectId}/{allPaths=\*\*} {
      allow read, write: if request.auth != null;
    }
  }
}
```
---
Free Tier Limits (as of 2025)
Resource	Free Limit
Firestore reads	50,000/day
Firestore writes	20,000/day
Firestore storage	1 GB
Storage	5 GB
Storage downloads	1 GB/day
Auth users	Unlimited (email)
These limits are sufficient for a personal project portfolio.
---
Local Development
You can run the website by opening `project\_vault.html` directly in Chrome or Edge.
No web server is required for local use. For Firebase to work, you may need to serve
from localhost due to browser security restrictions.
To serve locally:
```bash
# Python 3
python -m http.server 8080
# Then open http://localhost:8080/project\_vault.html
```
---
Environment Variables (if using a build system)
If you later move to a build system (Vite, Next.js, etc.), use these variable names:
```
VITE\_FIREBASE\_API\_KEY=your\_api\_key
VITE\_FIREBASE\_AUTH\_DOMAIN=your\_project.firebaseapp.com
VITE\_FIREBASE\_PROJECT\_ID=your\_project\_id
VITE\_FIREBASE\_STORAGE\_BUCKET=your\_project.appspot.com
VITE\_FIREBASE\_MESSAGING\_SENDER\_ID=your\_sender\_id
VITE\_FIREBASE\_APP\_ID=your\_app\_id
```
Never commit real credentials to a public repository.
