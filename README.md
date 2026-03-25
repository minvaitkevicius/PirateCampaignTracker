# ⚓ DnD Party Tracker

Mobile-friendly web app with **real-time sync** — multiple players see changes instantly.

---

## Files

```
dnd-tracker/
├── index.html            ← App (don't edit unless you know HTML/JS)
├── README.md             ← This file
└── data/
    ├── config.json       ← ✏ YOUR FIREBASE KEYS GO HERE
    ├── weapons.json      ← ✏ Edit weapon types, dice, ranges
    ├── crew.json         ← ✏ Edit crew roles and abilities
    ├── motes.json        ← ✏ Edit elemental mote spells/tooltips
    ├── ship.json         ← ✏ Edit ship default stats
    └── supplies.json     ← ✏ Edit supply rule descriptions
```

---

## Step 1 — Set up Firebase (Free, ~5 minutes)

Firebase is Google's free database service. The free tier handles ~50,000 reads/day,
which is far more than you'll ever use in a weekly campaign.

### Create the project

1. Go to **console.firebase.google.com** and sign in with a Google account
2. Click **"Add project"**, name it (e.g. `my-party-tracker`), click through the wizard
3. On the project homepage, click the **"Web"** icon (`</>`) to add a web app
4. Give it a nickname (anything), click **"Register app"**
5. You'll see a block of code like this — **copy these values:**

```js
const firebaseConfig = {
  apiKey:            "AIzaSy...",
  authDomain:        "my-party-tracker.firebaseapp.com",
  projectId:         "my-party-tracker",
  storageBucket:     "my-party-tracker.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abc123"
};
```

### Enable Firestore database

1. In the left sidebar click **"Firestore Database"**
2. Click **"Create database"**
3. Choose **"Start in test mode"** (allows anyone with your party code to read/write — fine for a private game)
4. Pick any region, click **"Enable"**

### Paste your keys into config.json

Open `data/config.json` and replace the placeholder values:

```json
{
  "firebase": {
    "apiKey":            "AIzaSy...",
    "authDomain":        "my-party-tracker.firebaseapp.com",
    "projectId":         "my-party-tracker",
    "storageBucket":     "my-party-tracker.appspot.com",
    "messagingSenderId": "123456789",
    "appId":             "1:123456789:web:abc123"
  },
  "defaultPartyCode": "pirates"
}
```

Change `defaultPartyCode` to whatever your party wants to use as their code.

---

## Step 2 — Host on GitHub Pages (Free)

### First time

1. Go to **github.com**, create a free account
2. Click **+** → **New repository**, name it `dnd-tracker`, set to **Public**, click **Create**
3. In the new repo, click **"uploading an existing file"**
4. Drag the whole `dnd-tracker` folder contents (index.html + data/ folder)
5. Click **Commit changes**

### Enable Pages

1. Go to repo **Settings** → **Pages** (left sidebar)
2. Source: **Deploy from a branch** → branch: **main**, folder: **/ (root)**
3. Click **Save**
4. Wait 2 minutes → your app is live at:
   `https://YOUR-USERNAME.github.io/dnd-tracker/`

### Share with your party

There's no login screen — the URL determines the session.

**Everyone bookmarks the same link:**
```
https://YOUR-USERNAME.github.io/dnd-tracker/?party=YOURCODE
```

Open the link → app loads instantly → everyone's in the same session.
Changes sync in real-time for anyone with that URL.

The party code is set in `data/config.json` as `defaultPartyCode`, used when
no `?party=` param is in the URL. The URL param always takes priority.

---

## Updating rules later

All game content is in the `data/` folder. To edit on GitHub:

1. Go to your repo, open `data/weapons.json` (or any file)
2. Click the **pencil ✏ icon**
3. Edit and click **Commit changes**
4. Live site updates within ~1 minute

---

## Alternative: Netlify Drop (Even Faster)

1. Go to **app.netlify.com/drop**
2. Drag your `dnd-tracker` folder onto the page
3. Free URL instantly (create account to keep it permanent)

---

## How sync works

- All game state is stored in **Firebase Firestore** under your party code
- Every player's changes push to the database within ~700ms
- All other players receive updates via a live listener
- If Firebase is unavailable, the app still works locally (localStorage)
- The sync indicator in the top right shows: 🟢 Synced / 🔴 Offline / ⚠️ Error

## Notes on the ship image

The ship image is stored as base64 in Firestore. If the image is very large (>1MB),
it may hit Firestore's document size limit (1MB). Use a reasonably sized image —
phone photos should be resized first if needed.
