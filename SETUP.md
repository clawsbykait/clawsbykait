# Claws by Kait — Setup Guide (Firebase)

Hosting: GitHub Pages (free)
Auth + Database + Storage: Firebase (free Spark plan)

---

## Step 1 — Create a Firebase project

1. Go to https://console.firebase.google.com
2. Click **Add project**
3. Name it `claws-by-kait` → Continue
4. Disable Google Analytics (not needed) → Create project

---

## Step 2 — Enable Authentication

1. In the left sidebar click **Build → Authentication**
2. Click **Get started**
3. Under **Sign-in method**, click **Email/Password** → Enable it → Save
4. Go to the **Users** tab → **Add user**
   - Email: Kait's email address
   - Password: her chosen password
   - Click **Add user**

---

## Step 3 — Create Firestore Database

1. In the sidebar click **Build → Firestore Database**
2. Click **Create database** → Start in production mode → Enable
3. Go to the **Rules** tab and replace with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /gallery/{doc} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

Click **Publish**.

---

## Step 4 — Enable Firebase Storage

1. In the sidebar click **Build → Storage**
2. Click **Get started** → Start in production mode → Done
3. Go to the **Rules** tab and replace with:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /gallery/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

Click **Publish**.

---

## Step 5 — Get your config values

1. Gear icon → **Project settings**
2. Scroll to **Your apps** → click **</>** → Register app → name it `claws-web`
3. Copy the firebaseConfig block and paste the values into **firebase-config.js**

---

## Step 6 — Deploy to GitHub Pages

1. Create a Public repo on github.com
2. Upload all site files
3. Settings → Pages → Branch: main → Save
4. Live at `https://YOUR_USERNAME.github.io/claws-by-kait/`

**Important:** In Firebase → Authentication → Settings → Authorized domains
→ Add your GitHub Pages domain (e.g. `YOUR_USERNAME.github.io`)
Without this, login will be blocked.

---

## Free tier limits (Firebase Spark plan)

| Feature | Free limit |
|---|---|
| Authentication | 10,000 users/month |
| Firestore reads | 50,000/day |
| Firestore writes | 20,000/day |
| Storage | 5GB stored, 1GB/day download |
