# NumeroVeda Website — Complete Setup Guide

## What You Have

A complete **Progressive Web App (PWA)** website with:
- Beautiful responsive design (works on mobile + desktop)
- User registration with OTP verification
- Unique username check
- Password strength validation
- 8 service listings with individual pricing
- Booking system with payment integration
- User dashboard with order history
- Admin panel (hidden access)
- Offline support (PWA)
- "Install as App" prompt on mobile

---

## STEP 1: Free Hosting on GitHub Pages (No Cost)

### 1a. Create a GitHub account
Go to https://github.com and sign up (free).

### 1b. Create a new repository
- Click "New repository"
- Name it: `numeroveda` (or any name)
- Set to **Public**
- Click "Create repository"

### 1c. Upload your files
Upload these 3 files to the repository:
- `index.html`
- `manifest.json`
- `sw.js`

### 1d. Enable GitHub Pages
- Go to repository → Settings → Pages
- Source: "Deploy from a branch"
- Branch: `main` → `/root`
- Click Save

Your site will be live at:
`https://YOUR-GITHUB-USERNAME.github.io/numeroveda`

---

## STEP 2: Firebase Setup (Free — Required for OTP & Database)

### 2a. Create Firebase Project
1. Go to https://console.firebase.google.com
2. Click "Add project" → Name it "numeroveda"
3. Disable Google Analytics (optional) → Create project

### 2b. Enable Phone Authentication (for OTP)
1. Firebase Console → Authentication → Sign-in method
2. Click "Phone" → Enable → Save
3. Add your mobile number to "Phone numbers for testing" (for development):
   - Phone: +91XXXXXXXXXX
   - Verification code: 123456

### 2c. Create Firestore Database
1. Firebase Console → Firestore Database → Create database
2. Start in **test mode** (change to production rules later)
3. Choose region: `asia-south1` (Mumbai)

### 2d. Get your Firebase Config
1. Firebase Console → Project Settings (⚙️) → General
2. Scroll to "Your apps" → Click Web icon (</>)
3. Register app name: "NumeroVeda Web"
4. Copy the `firebaseConfig` object

### 2e. Update index.html
Replace this section in `index.html`:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",           // ← Replace
  authDomain: "YOUR_PROJECT.firebaseapp.com",  // ← Replace
  projectId: "YOUR_PROJECT_ID",     // ← Replace
  storageBucket: "YOUR_PROJECT.appspot.com",   // ← Replace
  messagingSenderId: "YOUR_SENDER_ID",         // ← Replace
  appId: "YOUR_APP_ID"              // ← Replace
};
```

### 2f. Add your domain to Firebase Auth
1. Firebase Console → Authentication → Settings → Authorized domains
2. Add: `YOUR-USERNAME.github.io`

---

## STEP 3: Razorpay Setup (Free to Register)

### 3a. Create Razorpay Account
1. Go to https://razorpay.com → Sign Up
2. Complete KYC (business/personal)
3. Dashboard → Settings → API Keys
4. Generate Key ID & Key Secret

### 3b. Update index.html
Find this line and replace:
```javascript
key: 'YOUR_RAZORPAY_KEY_ID',  // ← Replace with your actual key
```

> **Note:** Razorpay charges 2% per transaction (no monthly fee).
> For ₹999, they charge ₹19.98. You receive ₹979.

### Payment Methods Supported (already configured):
- UPI (GPay, PhonePe, Paytm, etc.)
- Credit Card / Debit Card
- Net Banking
- NEFT/IMPS
- Cash on Delivery (COD) — with ₹50 extra charge applied automatically

---

## STEP 4: Admin Panel Access

### How to Access Admin Panel
The admin link is hidden for security. To access it:
1. On the website, **click the "NumeroVeda" logo/text 5 times rapidly**
2. The admin login dialog will appear

### Your Admin Credentials
```
Username: numeroveda_admin
Password: NV@Admin#2025
```

> ⚠️ IMPORTANT: Change these credentials in the code before going live!
> Find `ADMIN_CREDS` in index.html and update.

### Admin Panel Features
- Total users, orders, revenue dashboard
- Complete user registration list
- All bookings with status management
- Payment records
- Update order status (Pending → Processing → Completed)

---

## STEP 5: Customize Your Details

Find and replace these in `index.html`:

| What to find | Replace with |
|---|---|
| `+91 98765 43210` | Your actual mobile number |
| `consult@numeroveda.com` | Your actual email |
| `NumeroVeda` | Your business name (if different) |
| Service prices (₹799, ₹999, etc.) | Your actual prices |

### To change service prices:
Find the `SERVICES` array in the script and update each `price:` value.

---

## STEP 6: Mobile App (PWA Installation)

The website IS a mobile app — it's a Progressive Web App (PWA).

### For your clients:
1. Open the website on mobile browser (Chrome/Safari)
2. Chrome: tap "Add to Home Screen" banner that appears automatically
3. Safari (iOS): tap Share → "Add to Home Screen"

The app installs like a native app with:
- Your icon on home screen
- Full-screen mode (no browser bar)
- Offline support
- Push notification capability

### For Play Store listing (optional, free):
Use **PWABuilder** (https://www.pwabuilder.com):
1. Enter your GitHub Pages URL
2. Download the Android package
3. Upload to Google Play Console ($25 one-time fee)

---

## Service Pricing (Default — Change as Needed)

| Service | Default Price |
|---|---|
| Mobile Numerology Analysis | ₹799 |
| Name Numerology & Correction | ₹999 |
| Lucky Name for Kids | ₹1,499 |
| Company Name Design | ₹1,999 |
| Logo & Visiting Card Design | ₹2,499 |
| Birth Chart Consultation | ₹1,199 |
| Vaastu Consultation | ₹2,999 |
| Remedies, Switch Words & Crystals | ₹699 |

---

## Database Structure (Firebase Firestore)

### Collection: `users`
```
users/{username}
  - firstName, middleName, lastName, fullName
  - mobile, username, passwordHash
  - registeredAt (timestamp)
```

### Collection: `orders`
```
orders/{autoId}
  - username, userFullName
  - serviceId, serviceName
  - amount, paymentMethod, paymentId
  - preferredDate, dob, notes
  - status (pending/processing/completed)
  - date (timestamp)
```

---

## Security Notes

1. **Password storage**: Currently using base64 (for demo). For production, use Firebase Authentication with email/password OR a server-side bcrypt hash.
2. **Admin credentials**: Change `ADMIN_CREDS` before going live.
3. **Firestore rules**: Set proper security rules in Firebase Console before going public.
4. **Razorpay**: Never expose your Key Secret in frontend code. Use only the Key ID.

### Recommended Firestore Security Rules:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /orders/{orderId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

---

## Quick Start Checklist

- [ ] Upload 3 files to GitHub
- [ ] Enable GitHub Pages
- [ ] Create Firebase project
- [ ] Enable Phone Auth in Firebase
- [ ] Create Firestore database
- [ ] Paste Firebase config into index.html
- [ ] Add your domain to Firebase authorized domains
- [ ] Create Razorpay account
- [ ] Add Razorpay Key ID to index.html
- [ ] Update your contact details
- [ ] Update service prices
- [ ] Change admin credentials
- [ ] Test registration with your mobile number
- [ ] Test a demo booking

---

## Support & Upgrades

When your business grows:
- **Custom domain**: Buy from GoDaddy/Namecheap (~₹500/year) → Connect to GitHub Pages free
- **Professional hosting**: Vercel or Netlify (both free tier, faster than GitHub Pages)
- **SMS OTP**: Fast2SMS.com (Indian provider, cheaper than Firebase for SMS)
- **Advanced reports**: Add PDF generation for consultation reports
- **WhatsApp integration**: Integrate WATI or Interakt for WhatsApp booking confirmations

---

*NumeroVeda Website — Built for certified numerologist use*
*All data stored locally + Firebase. No third-party data sharing.*
