# jseva
Janasevakendram customer data save system 
# 🏛️ ജനസേവകേന്ദ്രം - Phase 1 Setup Guide

## 📦 File Structure

```
janasevakendram/
├── index.html                  ← Login page
├── dashboard.html              ← Super Admin dashboard
├── css/
│   └── style.css               ← All styles
├── js/
│   ├── firebase-config.js      ← Firebase credentials (EDIT THIS)
│   ├── auth.js                 ← Login/logout/session
│   └── admin-management.js     ← Admin CRUD operations
├── php/
│   └── send-email.php          ← Email sender (EDIT THIS)
├── vendor/
│   └── phpmailer/              ← PHPMailer library (DOWNLOAD)
│       ├── PHPMailer.php
│       ├── SMTP.php
│       └── Exception.php
├── firestore-rules.txt         ← Copy to Firebase Console
└── SETUP-GUIDE.md              ← This file
```

---

## 🔧 Step 1: Firebase Project Setup

### 1.1 Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add Project"** → Enter name: `janasevakendram`
3. Disable Google Analytics (optional) → Create Project

### 1.2 Enable Authentication
1. In Firebase Console → **Authentication** → **Get Started**
2. Click **Sign-in method** tab
3. Enable **Email/Password** → Save

### 1.3 Create Super Admin Account
1. Go to **Authentication** → **Users** tab
2. Click **"Add user"**
3. Enter YOUR email and a strong password
4. Click **Add user**
5. **Copy the UID** (long string under the email)

### 1.4 Enable Firestore Database
1. Go to **Firestore Database** → **Create database**
2. Choose **"Start in production mode"**
3. Select a nearby region (e.g., `asia-south1` for India)
4. Click **Enable**

### 1.5 Add Firestore Security Rules
1. Go to **Firestore** → **Rules** tab
2. Delete existing rules
3. Copy-paste content from `firestore-rules.txt`
4. Replace `PASTE_YOUR_SUPER_ADMIN_UID_HERE` with your actual UID
5. Click **Publish**

### 1.6 Get Web App Credentials
1. Go to **Project Settings** (⚙️ icon) → **General**
2. Scroll to **"Your apps"** → Click **Web icon** `</>`
3. Enter app name: `janasevakendram-web`
4. **Don't** enable Firebase Hosting
5. Copy the `firebaseConfig` values

---

## 🔧 Step 2: Update firebase-config.js

Open `js/firebase-config.js` and replace:

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyxxxxxxxxxxxxxxxxxxxxxxxxx",
    authDomain: "janasevakendram.firebaseapp.com",
    projectId: "janasevakendram",
    storageBucket: "janasevakendram.appspot.com",
    messagingSenderId: "123456789",
    appId: "1:123456789:web:abcdef123456"
};

const SUPER_ADMIN_UID = "paste-your-uid-here";
```

---

## 🔧 Step 3: Setup PHPMailer (Email)

### 3.1 Download PHPMailer
1. Go to: https://github.com/PHPMailer/PHPMailer/releases
2. Download the latest `.zip` release
3. Extract it

### 3.2 Copy Files
From the extracted folder, copy these 3 files:
- `src/PHPMailer.php`
- `src/SMTP.php`
- `src/Exception.php`

To: `vendor/phpmailer/` folder in your project

### 3.3 Create Gmail App Password
1. Go to: https://myaccount.google.com/apppasswords
2. Sign in to your Gmail
3. Select app: **Mail**
4. Select device: **Other** → Enter "janasevakendram"
5. Click **Generate**
6. Copy the 16-character password (e.g., `abcd efgh ijkl mnop`)

> ⚠️ Note: You need 2-Factor Authentication enabled on Gmail for App Passwords to work.

### 3.4 Update send-email.php
Open `php/send-email.php` and update:

```php
define('SMTP_USERNAME', 'your-actual-gmail@gmail.com');
define('SMTP_PASSWORD', 'your-16-char-app-password');
define('SYSTEM_URL', 'https://yourdomain.com');
```

---

## 🔧 Step 4: Upload to Hostinger

### 4.1 Via File Manager
1. Login to Hostinger → **hPanel**
2. Go to **File Manager**
3. Navigate to `public_html`
4. Upload all project files maintaining the folder structure

### 4.2 Via FTP (Alternative)
1. Use FileZilla or similar FTP client
2. Connect using Hostinger FTP credentials
3. Upload to `public_html/`

---

## 🔧 Step 5: Create Firestore Index

When you first load the admin list, the console may show an error with a link to create an index. Click that link to auto-create the required index:
- Collection: `admins`
- Fields: `role` (Ascending), `createdAt` (Descending)

---

## ✅ Testing Checklist

- [ ] Open your website → Login page appears
- [ ] Login with Super Admin email/password → Dashboard loads
- [ ] Stats show 0 admins, 0 customers
- [ ] Create a test admin → Form submits
- [ ] Admin appears in Admin List tab
- [ ] Credential email received (check spam too)
- [ ] Search works by name/email/phone
- [ ] Enable/Disable admin works
- [ ] Delete admin works
- [ ] Logout works → Returns to login
- [ ] New admin can login (redirects to admin-panel.html — Phase 2)

---

## 🔜 Next Phases

| Phase | Features |
|-------|----------|
| Phase 2 | Admin panel, Customer CRUD, Status tracking |
| Phase 3 | PDF upload (registration), ID Card upload (Cloudinary/R2) |
| Phase 4 | Customer notifications (email + WhatsApp share) |
| Phase 5 | Reports, Export, Activity log |
