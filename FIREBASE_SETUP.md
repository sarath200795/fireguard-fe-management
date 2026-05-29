# Firebase Setup Guide

## 1. Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project**, give it a name (e.g. `fireguard-prod`)
3. Disable Google Analytics (optional)
4. Click **Create project**

---

## 2. Enable Authentication

1. In your project → **Build → Authentication → Get started**
2. Click **Sign-in method → Email/Password → Enable → Save**
3. Go to **Users → Add user** and create accounts:

| Email | Password | Notes |
|-------|----------|-------|
| admin@yourcompany.com | (strong password) | Will be admin role |
| manager@yourcompany.com | (strong password) | Site manager |
| vendor@yourcompany.com | (strong password) | Vendor access |

---

## 3. Create Firestore Database

1. In your project → **Build → Firestore Database → Create database**
2. Choose **Production mode**
3. Select your region (e.g. `asia-south1` for India)
4. Click **Enable**

---

## 4. Deploy Security Rules

Copy the contents of `firestore.rules` into **Firestore → Rules** tab and click **Publish**.

Or install Firebase CLI and run:
```bash
npm install -g firebase-tools
firebase login
firebase init firestore
firebase deploy --only firestore:rules
```

---

## 5. Set User Roles

After creating users in Authentication, you need to set their roles in Firestore.

In **Firestore → Data**, create a collection called `users` and add a document for each user:

- **Document ID**: the user's `uid` (copy from Authentication → Users tab)
- **Fields**:
  ```
  name: "Admin User"     (string)
  role: "admin"          (string — admin | manager | vendor | viewer)
  ```

Do this for every user. Without a `users` document, the user defaults to `viewer` role.

---

## 6. Get your Firebase config

1. In your project → **Settings (gear icon) → Project settings**
2. Scroll to **Your apps → Web app**
3. If no web app exists, click **Add app → Web → Register app**
4. Copy the `firebaseConfig` object values

---

## 7. Connect the app

1. Open `index.html` in your browser
2. The **Firebase Setup screen** appears on first load
3. Paste each config value into the fields
4. Click **Save & Connect**
5. Sign in with the email/password you created

---

## Firestore Collections

The app uses these collections:

| Collection | Description |
|-----------|-------------|
| `extinguishers` | All FE asset records |
| `issues` | Defect/issue reports |
| `users` | User role profiles (uid → {name, role}) |

---

## Role Permissions

| Role | Dashboard | Assets | Alerts | Vendor | Issues | Admin | Register | Bulk |
|------|-----------|--------|--------|--------|--------|-------|----------|------|
| admin | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| manager | ✓ | ✓ | ✓ | ✓ | ✓ | — | — | — |
| vendor | — | ✓ | ✓ | ✓ | — | — | — | — |
| viewer | ✓ | ✓ | ✓ | — | — | — | — | — |

---

## Indexes (if needed)

If you see Firestore index errors in the browser console, click the link in the error to auto-create the required composite index.

Common indexes needed:
- `extinguishers` → `status ASC, updatedAt DESC`
- `issues` → `status ASC, date DESC`
