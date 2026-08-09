# Best Moments Gallery — Setup (100% free, no credit card)

Your invitation is a static site (no server), so a *shared* gallery — where
one guest's upload shows up for everyone — needs a small cloud backend to
store and list the photos. This uses two free services:

- **Cloudinary** — stores the actual photo files (free, no card needed)
- **Firestore** (part of Firebase) — stores the list of photos + names (free, no card needed)

> Note: Firebase *Storage* now requires a paid "Blaze" plan (Google changed
> this recently), so we're using Cloudinary for the files instead — it's
> free with no billing setup and made exactly for this kind of thing.

Takes about 10–15 minutes total, no coding required.

---

## Part A — Cloudinary (photo files)

1. Go to https://cloudinary.com/users/register/free and sign up (free plan, no card).
2. Once logged in, your **Dashboard** shows a "Cloud name" near the top — copy it.
3. In the left sidebar, go to **Settings** (gear icon) → **Upload** tab.
4. Scroll to **Upload presets** → click **Add upload preset**.
5. Set:
   - **Signing Mode: Unsigned** (important — this lets guests upload directly from the browser)
   - **Folder**: `gallery` (optional, keeps things tidy)
   - Leave everything else default
6. Click **Save**. Copy the preset's name (Cloudinary auto-generates one like `abcd1234`, or rename it to something memorable e.g. `wedding_gallery`).

You now have two values: your **Cloud name** and your **Upload preset** name.

---

## Part B — Firebase Firestore (the photo list)

1. Go to https://console.firebase.google.com and sign in.
2. **Add project** → name it → keep defaults → **Create project**.
3. Click the **`</>`** (web) icon on the overview page to register a web app (nickname anything, no Hosting needed).
4. Copy the `firebaseConfig` object it shows you.
5. In the left sidebar: **Build → Firestore Database → Create database** → **Start in production mode** → pick a region close to your guests → **Enable**.
6. Go to the **Rules** tab and replace the contents with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /gallery/{photoId} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasOnly(['imageUrl', 'uploaderName', 'createdAt'])
                    && request.resource.data.imageUrl is string
                    && request.resource.data.uploaderName is string
                    && request.resource.data.uploaderName.size() < 60;
      allow update, delete: if false; // guests can add photos, not remove others'
    }
  }
}
```

7. Click **Publish**.

---

## Part C — Paste everything into `index.html`

Open `index.html`, search for `YOUR_API_KEY` (near the bottom, in the
"BEST MOMENTS GALLERY" script). You'll find two config blocks — fill both in:

```js
const firebaseConfig = {
  apiKey: "...",            // from Part B, step 4
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};

const cloudinaryConfig = {
  cloudName: "...",         // from Part A, step 2
  uploadPreset: "..."       // from Part A, step 6
};
```

Save the file. That's it — no billing, no card, nothing else to enable.

---

## Test it

Open `index.html` (or your deployed site), scroll to **Best Moments**,
upload a photo. It should appear in the gallery within a couple of
seconds — open it on another device/browser to confirm it's really shared.

## Notes

- Photos are resized in the guest's browser before upload (a 10MB phone
  photo becomes a small, fast JPEG) — keeps things fast and well within
  free limits either way.
- Cloudinary's free tier: 25 monthly credits (roughly 25GB of storage/
  bandwidth combined) — comfortably enough for a wedding's worth of guest
  photos.
- Firestore free tier (Spark plan, no card): 1GB storage, 50k reads/day,
  20k writes/day — this app only stores small text records (URLs + names)
  here, so you'd need thousands of uploads a day to get anywhere close.
- Nobody can delete or overwrite someone else's photo entry through the
  site (the Firestore rule above blocks update/delete).
- If you redeploy to Vercel/Netlify, make sure the updated `index.html`
  (with your real config values in it) is what gets pushed.
