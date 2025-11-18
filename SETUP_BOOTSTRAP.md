# GoldenLab - Complete Setup & Build Guide

## Overview
This document guides you through building the complete GoldenLab application locally. Due to the size of the project (100+ files), we provide comprehensive instructions and code that you can implement step-by-step.

## Phase 1: Local Setup (15 minutes)

### Step 1a: Clone & Install
```bash
git clone https://github.com/jyotishanjaria-creator/goldenlab.git
cd goldenlab
npm install
```

### Step 1b: Create Directory Structure
```bash
# Create all necessary directories
mkdir -p src/{pages,components,lib,types,hooks,store}
mkdir -p src/pages
mkdir -p src/components/{shared,home,learn,measure,simulate,report,present,admin}
mkdir -p functions/src
mkdir -p public/icons
```

## Phase 2: Firebase Setup (10 minutes)

### Step 2a: Create Firebase Project
1. Go to https://console.firebase.google.com/
2. Click "Add project" → Name it "goldenlab"
3. Enable Google Analytics (optional)
4. Wait for project creation

### Step 2b: Enable Required Services
1. Go to Build → Authentication → Enable "Google" and "Email/Password" providers
2. Go to Build → Firestore Database → Create database (Start in test mode initially, then secure it)
3. Go to Build → Storage → Create bucket (Start in test mode)
4. Go to Build → Functions → Enable Cloud Functions

### Step 2c: Get Service Account & Config
1. Go to Project Settings (gear icon)
2. Click "Service Accounts" → "Generate new private key" → Save as `firebase-service-account.json` (KEEP SECRET)
3. Copy your Firebase config from "Your apps" section (web app config)
4. Create `.env.local` file in project root:
```
VITE_FIREBASE_API_KEY=YOUR_API_KEY
VITE_FIREBASE_AUTH_DOMAIN=YOUR_AUTH_DOMAIN
VITE_FIREBASE_PROJECT_ID=YOUR_PROJECT_ID
VITE_FIREBASE_STORAGE_BUCKET=YOUR_STORAGE_BUCKET
VITE_FIREBASE_MESSAGING_SENDER_ID=YOUR_MESSAGING_SENDER_ID
VITE_FIREBASE_APP_ID=YOUR_APP_ID
```

## Phase 3: Create Core Files (30 minutes)

Follow the detailed code blocks below. Create each file in the corresponding directory.

### 3.1: TypeScript Configuration

**File: `tsconfig.json`**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "resolveJsonModule": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolvePackageJsonExports": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

**File: `tsconfig.node.json`**
```json
{
  "compilerOptions": {
    "composite": true,
    "skipLibCheck": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true
  },
  "include": ["vite.config.ts"]
}
```

### 3.2: Tailwind Configuration

**File: `tailwind.config.js`**
```javascript
export default {
  content: [
    './index.html',
    './src/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      colors: {
        gold: {
          50: '#fffef0',
          100: '#fffde1',
          500: '#ffd700',
          600: '#ffb700',
          700: '#ff9100',
          900: '#cc7000',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        display: ['Playfair Display', 'serif'],
      },
    },
  },
  plugins: [],
}
```

**File: `postcss.config.js`**
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### 3.3: Main Application Files

**File: `index.html`**
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="GoldenLab - Explore the Golden Ratio through interactive simulations and image analysis" />
    <meta name="theme-color" content="#ffd700" />
    <title>GoldenLab - Golden Ratio Explorer</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

**File: `src/main.tsx`**
```typescript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

**File: `src/index.css`**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', system-ui, sans-serif;
  background-color: #fafafa;
  color: #1a1a1a;
}

::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #ffd700;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #ffb700;
}

/* Golden ratio visual elements */
.golden-spiral {
  animation: spin 20s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
```

## Phase 4: Firebase Integration Files (20 minutes)

**File: `src/lib/firebaseConfig.ts`**
```typescript
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';
import { getStorage } from 'firebase/storage';
import { getFunctions } from 'firebase/functions';

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
export const storage = getStorage(app);
export const functions = getFunctions(app);

export default app;
```

**File: `firebase.json`**
```json
{
  "hosting": {
    "public": "dist",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  },
  "functions": {
    "source": "functions",
    "runtime": "node18"
  }
}
```

**File: `.firebaserc`**
```json
{
  "projects": {
    "default": "goldenlab-PROJECT_ID"
  }
}
```

## Phase 5: Core Components (45 minutes)

### See FULL_APP_SOURCE.md for all 50+ React components and pages

The code is provided in a separate companion file for space efficiency. Extract that file to create all components.

## Phase 6: Run Locally

```bash
npm run dev
```
App will run at: http://localhost:5173

## Phase 7: GitHub Secrets Setup (for CI/CD)

1. Go to: GitHub Repo → Settings → Secrets and variables → Actions
2. Create these secrets:
   - `FIREBASE_SERVICE_ACCOUNT`: Paste entire JSON from firebase-service-account.json
   - `FIREBASE_PROJECT_ID`: Your Firebase project ID

## Phase 8: Deploy to Firebase

```bash
npm run build
npm run deploy
```

## Project Structure

```
goldenlab/
├── src/
│   ├── pages/
│   │   ├── Home.tsx
│   │   ├── Learn.tsx
│   │   ├── Measure.tsx
│   │   ├── Simulate.tsx
│   │   ├── Report.tsx
│   │   ├── Present.tsx
│   │   └── Admin.tsx
│   ├── components/
│   │   ├── shared/
│   │   ├── home/
│   │   ├── learn/
│   │   ├── measure/
│   │   ├── simulate/
│   │   └── report/
│   ├── lib/
│   │   ├── firebaseConfig.ts
│   │   ├── phyllotaxis.ts
│   │   └── pdf-utils.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── functions/
│   └── src/
│       └── generateReport.ts
├── .github/
│   └── workflows/
│       └── deploy.yml
├── firebase.json
├── vite.config.ts
├── package.json
└── README.md
```

## Next Steps

1. Check `FULL_APP_SOURCE.md` for all React components code
2. Create each file following the structure
3. Update environment variables
4. Run `npm run dev` to test locally
5. Deploy via `npm run deploy`

---

**Note**: This is a comprehensive project. Take time to build it step-by-step locally. All source code is provided in accompanying documents.
