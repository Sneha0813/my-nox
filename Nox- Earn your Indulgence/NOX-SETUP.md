# NOX — Setup & Deploy Guide

## Files in this package
```
index.html       ← Full app (open this)
netlify.toml     ← Netlify config (auto SPA routing)
manifest.json    ← PWA manifest (install as app)
firestore.rules  ← Firebase security rules
SETUP.md         ← This file
```

---

## STEP 1 — Firebase Setup (10 min)

### Create Project
1. Go to https://console.firebase.google.com
2. New Project → name it `nox-app` → Create

### Enable Authentication
1. Build → Authentication → Get started
2. Sign-in method → **Email/Password** → Enable → Save

### Enable Firestore
1. Build → Firestore Database → Create database
2. Start in **test mode** → Choose region → Done

### Get Your Config
1. Project Settings (⚙️) → General → Your apps → Web (</>)
2. Register app → Copy the `firebaseConfig` object

### Paste Config into index.html
Find this block near line 700 of index.html:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",          ← replace
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### Add Security Rules
1. Firestore → Rules tab
2. Replace contents with `firestore.rules`
3. Publish

---

## STEP 2 — Deploy to Netlify (2 min)

### Option A: Drag & Drop (Fastest)
1. Go to https://app.netlify.com/drop
2. Drag the entire `nox/` folder onto the page
3. Get your live URL instantly → e.g. `https://radiant-nox-abc123.netlify.app`

### Option B: CLI Deploy
```bash
npm install -g netlify-cli
cd /path/to/nox
netlify login
netlify deploy --dir=. --prod
```

### Option C: GitHub Auto-Deploy
```bash
git init && git add . && git commit -m "NOX launch"
# Push to GitHub → Connect to Netlify → Auto-deploys on every push
```

---

## STEP 3 — AI Features (Optional)

1. Open the deployed app
2. Login/Register an account
3. Go to **Customize** → scroll to API Key section
4. Enter your Anthropic API key (get it at https://console.anthropic.com)
5. Click Save Key

AI features enabled:
- ✧ **AI Theme Studio** — Describe an aesthetic, get a custom color theme
- 🏆 **Generate Achievements** — AI creates personalized achievements from your goals
- 💡 **Goal suggestions** via AI

---

## Features Overview

| Feature | Description |
|---------|-------------|
| Email Auth | Register/login with email & password |
| Per-user data | Each account has isolated data in Firestore |
| Daily goals | Checklist with point rewards, resets at midnight |
| Fun Shop | 2× per item per week, resets Saturday 23:59 |
| Streak pet | Animated companion that grows every 115 streak days |
| Pet stages | Egg → Hatchling → Juvenile → Teen → Adult → Elder → Legend |
| Pet interactions | Feed (5pts), Pet (3pts), Play (8pts) |
| Pet customization | 9 animal types + upload custom image |
| Streak bonuses | +10 pts every 10-day streak milestone |
| Achievements | AI-generated based on your custom goals |
| Referral system | Unique code per user, +50 pts on successful referral |
| Leaderboard | Top users by total points (Firebase) |
| Push notifications | Daily/streak/reset reminders |
| AI theme | Claude generates custom CSS themes from text descriptions |
| PWA | Install as native app on phone/desktop |

---

## Customization

Everything is configurable inside the app:
- Goals: Add/edit/delete with custom point values
- Shop items: Add/edit/delete with custom costs & icons
- Threshold: Min points to unlock Fun Shop (default 60)
- Routine: Day-by-day time blocks
- Theme: AI-generated from text or choose presets
- Pet: Name, choose animal, upload custom image

---

## Referral System

Each user gets a unique code like `NOX-A1B2C3`.

When someone registers using your code:
- **You** get +50 points
- The referral is tracked in your profile

Share via:
- Direct code (Social → copy)
- Invite link (auto-generated URL with `?ref=YOUR-CODE`)

---

## Streak Pet Stages

| Stage | Streak | Description |
|-------|--------|-------------|
| 🥚 Egg | 0 | Just starting out |
| 🐣 Hatchling | 5 | First hatch! |
| 🐥 Juvenile | 20 | Growing up |
| 🐦 Teen | 50 | Finding personality |
| 🦅 Adult | 115 | Fully grown |
| 🦋 Elder | 230 | Wisdom achieved |
| 🐉 Legend | 500 | Ultimate form |

Every 10-day streak milestone grants bonus points equal to (streak/10) × 10.

---

## Notes

- **Offline mode**: App works with localStorage even without Firebase
- **No Firebase**: Just open index.html — everything still works locally
- **API key security**: Stored in localStorage only, never sent to any server except Anthropic
- **CORS**: Anthropic API calls work from browser with your key
