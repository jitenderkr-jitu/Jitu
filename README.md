Jitu Creative Studio — Website Setup Guide
(English + Hindi steps — dono zabaan mein)
Ye website customer login/signup ke sath ek real, free Firebase database
use karti hai — taaki customer ka data hamesha ke liye save rahe aur aap
kisi bhi device se dekh sako. Site GitHub Pages par free host hogi.
Files in this folder
File
Kya hai
index.html
Poori website (design, forms, admin dashboard)
firebase-config.js
Yahan apni Firebase project ki keys daalni hain
README.md
Ye guide
Step 1 — Firebase Project Banayein (Free)
Jaayein: https://console.firebase.google.com
Google account se login karein
"Add project" par click karein → naam dein (e.g. jitu-creative-studio)
Google Analytics ka prompt aaye to "Not now" / skip kar sakte hain
Project create hone do (~30 seconds)
Step 2 — Authentication (Login System) Enable Karein
Left sidebar mein Build → Authentication par jaayein
"Get started" click karein
"Email/Password" provider par click karein → Enable karein → Save
Step 3 — Apna Admin Account Banayein
Wahi Authentication section mein → "Users" tab → "Add user"
Email: jitender8804@gmail.com (ya jo bhi aap chahte hain)
Password: apni marzi ka strong password set karein
Add user click karein
⚠️ Ye email firebase-config.js ke ADMIN_EMAILS list mein bhi match hona chahiye.
Step 4 — Firestore Database Banayein
Left sidebar mein Build → Firestore Database → "Create database"
Location choose karein (e.g. asia-south1 — Mumbai, India ke liye sabse tez)
"Start in production mode" select karein → Next → Enable
Step 5 — Security Rules Set Karein (Important — Data Suraksha ke liye)
Firestore Database ke andar "Rules" tab par jaayein
Neeche di gayi rules ko poora copy-paste karke replace kar dein:
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    match /bookings/{bookingId} {
      allow create: if request.auth != null &&
        (request.resource.data.uid == request.auth.uid ||
         request.auth.token.email in ["jitender8804@gmail.com"]);

      allow read: if request.auth != null &&
        (resource.data.uid == request.auth.uid ||
         request.auth.token.email in ["jitender8804@gmail.com"]);

      allow update, delete: if request.auth != null &&
        request.auth.token.email in ["jitender8804@gmail.com"];
    }

    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
Yaad rahe: "jitender8804@gmail.com" ko apni admin email se replace karein
(dono jagah — bookings aur agar aap aur admin add karein).
"Publish" button click karein
Ye rules kya karti hain (samjhein):
Har customer sirf apni khud ki bookings dekh sakta hai
Sirf admin email hi sabki bookings dekh/edit/delete kar sakta hai
Koi bhi doosre ka data nahi dekh sakta — data surakshit rehta hai
Step 6 — Web App Register Karein aur Config Copy Karein
Project ke ⚙️ Settings → Project settings mein jaayein
Neeche scroll karein "Your apps" tak → </> (Web) icon click karein
App ka naam dein (e.g. jitu-website) → Register app
Jo firebaseConfig = {...} code dikhega, usse copy karein
Is folder ki firebase-config.js file kholein, aur PASTE_YOUR_...
waali sab lines ko apni real values se replace karein
Usi file mein ADMIN_EMAILS list mein apni admin email daalein
Step 7 — GitHub Par Free Publish Karein
https://github.com par account banayein (agar nahi hai)
New repository banayein (e.g. jitu-creative-studio) → Public rakhein
In teeno files ko upload karein: index.html, firebase-config.js, README.md
(GitHub website par "Add file → Upload files" se seedha drag-drop kar sakte hain)
Repository ke Settings → Pages mein jaayein
"Branch" mein main select karein, folder / (root) rakhein → Save
1-2 minute wait karein — aapko URL milega jaisa:
https://your-username.github.io/jitu-creative-studio/
Bas! Aapki website ab live hai, free mein, aur real login system ke sath.
Kaise Test Karein
Apni site kholiye (GitHub Pages link se)
Booking section mein "Create Account" se ek test account banayein
Booking form fill karein aur submit karein
Footer mein "Director / Staff Access" click karein → apni admin
email/password se login karein → dashboard mein wo booking dikhni chahiye
Password Bhool Jaayein To
Firebase Console → Authentication → Users mein jaake password reset kar sakte hain.
Kuch Bhi Dikkat Aaye To
Browser mein F12 dabakar Console tab dekhein — error message dikhega
Sabse common galti: firebase-config.js mein values sahi se paste nahi hui,
ya Firestore Rules publish nahi hui
