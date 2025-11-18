# 🚀 GoldenLab - Quick Start (3 Steps)

## For Palash & Ananya - Ready in 5 Minutes!

### Step 1: Download & Install (2 minutes)

```bash
# Download the code
git clone https://github.com/jyotishanjaria-creator/goldenlab.git
cd goldenlab

# Install dependencies
npm install
```

### Step 2: Setup Firebase (2 minutes)

1. Go to https://console.firebase.google.com
2. Click "Create Project"
3. Enter name: `GoldenLab`
4. Follow the setup wizard
5. Go to Project Settings (gear icon)
6. Scroll to "Your apps" and add a Web app
7. Copy the config values
8. Create `.env.local` file (copy from `.env.example`)
9. Paste your Firebase values:

```bash
VITE_FIREBASE_API_KEY=paste_your_api_key
VITE_FIREBASE_AUTH_DOMAIN=paste_your_domain
VITE_FIREBASE_PROJECT_ID=paste_your_project_id
VITE_FIREBASE_STORAGE_BUCKET=paste_your_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=paste_your_id
VITE_FIREBASE_APP_ID=paste_your_app_id
```

### Step 3: Run the App (1 minute)

```bash
npm run dev
```

Open: **http://localhost:5173**

---

## That's It! 🎉

You now have a working GoldenLab app on your computer!

### What You Can Do:
- ✅ Click through all 7 pages
- ✅ Upload test photos
- ✅ Run the phyllotaxis simulator
- ✅ Generate PDF reports
- ✅ Practice your presentation
- ✅ Test offline mode (DevTools > Application > Service Workers)

---

## Next: Customize & Deploy

When ready:

```bash
# Build for production
npm run build

# Deploy to Firebase
npm run deploy
```

## Troubleshooting

**Port 5173 already in use?**
```bash
npm run dev -- --port 3000
```

**Firebase not connecting?**
- Check `.env.local` has correct values
- Restart dev server: Press `Ctrl+C`, then `npm run dev` again

**Need to update code?**
- Edit files in `src/pages/` or `src/lib/`
- Save file - app updates automatically!

---

## Links

- 📖 **Full Guide:** See `IMPLEMENTATION_GUIDE.md`
- 💻 **Source Code:** See `COMPLETE_APP_SOURCE_PART1-4.md` files
- ⚙️ **Setup Help:** See `SETUP_BOOTSTRAP.md`
- 📘 **Project Info:** See `README.md`

---

**Ready to build something amazing!** 🌟
