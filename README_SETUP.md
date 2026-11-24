# AgriSetu Full-Stack Website (Firebase)

This keeps the existing AgriSetu farmer/consumer interface and connects login, profiles, products, requirements, orders, messages, complaints and feedback to Firebase Authentication + Cloud Firestore. Data is shared between phones and computers signed into the same Firebase project.

## Before testing

1. Open Firebase Console: https://console.firebase.google.com/ and select project `agrisetu-da72e`.
2. In **Authentication → Sign-in method**, enable **Phone**.
3. In **Authentication → Settings → Authorized domains**, add your eventual hosting domain (for example the Netlify or Firebase Hosting domain). `localhost` may be used for local development.
4. In **Firestore Database**, create the database if it does not exist.
5. Open the **Rules** tab and paste the contents of `firestore.rules`, then Publish. These are starter prototype rules: they require sign-in but allow any signed-in account to read/write the prototype collections. Do NOT use these broad rules for a public commercial launch; implement owner/role rules first.
6. Host the whole folder over HTTPS (Netlify Drop or Firebase Hosting). Phone auth reCAPTCHA and SMS verification need a supported web domain and internet connection.

## Run locally

Do not double-click `index.html` as a `file://` page for phone authentication. Use a local web server, e.g. from this folder:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`. Add `localhost` to Firebase authorized domains if needed.

## Deploy with Firebase Hosting

Install Firebase CLI, then from this folder run:

```bash
firebase login
firebase use agrisetu-da72e
firebase deploy --only hosting
```

The included `firebase.json` uses this folder as the Hosting public directory.

## Demo records

The website has 10 sample farmer profiles, 10 sample consumer profiles, product listings, requirements, orders and sample feedback. On the first successful real phone sign-in, it copies this starter dataset into Firestore if the `users` collection is empty. Sample profiles are demonstration marketplace profiles, NOT login accounts; use a real phone number to receive an OTP.

## Current scope / limitations

- Real SMS OTP: Firebase Phone Authentication (requires Phone provider enabled, authorized domain, reCAPTCHA and SMS availability/quotas).
- Shared data: Cloud Firestore live listeners.
- Product images: compressed and stored in Firestore document data for this small prototype; for production, move images to Firebase Storage.
- Cash/UPI: the existing UI records a selected payment method only. It does not transfer money. Add a verified payment gateway and server-side payment verification for real payments.
- Market prices/AI: demo rule-based suggestions; no live mandi API or trained AI model.
- Security: starter Firestore rules are intentionally broad for prototype testing. Tighten access rules before public/real user launch. Never expose service-account keys in frontend code.

## Firebase project config

The existing web app config for `agrisetu-da72e` is already placed in `index.html`. Firebase web API keys are public client identifiers; access control must be enforced by Authentication and Firestore Security Rules.
