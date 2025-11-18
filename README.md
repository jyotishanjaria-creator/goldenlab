# GoldenLab — Golden Ratio Explorer

An interactive web app (PWA) that teaches the Golden Ratio, lets students measure and annotate photos (sunflowers, ear, face), runs phyllotaxis simulations, computes packing metrics, creates exportable PDF experiment reports, and provides a full-screen presentation (kiosk) mode for judges.

**Student Owners:** Palash Anjaria, Ananya Tiwari

## Features

- 🎓 **Learn**: Interactive cards explaining the Golden Ratio, Fibonacci sequence, and Golden Angle
- 📸 **Measure**: Upload images, overlay golden spirals/rectangles, use protractor tool to measure angles
- 🌻 **Simulate**: Client-side phyllotaxis simulation with adjustable parameters and packing metrics
- 📊 **Report**: Generate beautiful PDF experiment reports with images, data, and graphs
- 🎬 **Present**: Full-screen presentation mode for judges with autoplay demo and QR code
- 📱 **PWA**: Works offline, installable on phones and tablets

## Quick Start (Local Development)

### Prerequisites
- Node.js 18+
- npm
- Git
- Firebase CLI (for deployment)

### Setup

```bash
# Clone the repository
git clone https://github.com/jyotishanjaria-creator/goldenlab.git
cd goldenlab

# Install dependencies
npm install

# Start development server
npm run dev
```

The app will open at `http://localhost:5173`

## Project Structure

```
goldenlab/
├── src/
│   ├── pages/              # Route pages
│   │   ├── Home.tsx
│   │   ├── Learn.tsx
│   │   ├── Measure.tsx
│   │   ├── Simulate.tsx
│   │   ├── Report.tsx
│   │   ├── Present.tsx
│   │   └── Admin.tsx
│   ├── components/         # Reusable components
│   │   ├── PhyllotaxisCanvas.tsx
│   │   ├── ImageOverlay.tsx
│   │   ├── Protractor.tsx
│   │   └── PDFGenerator.tsx
│   ├── lib/                # Utilities and helpers
│   │   ├── firebaseConfig.ts
│   │   ├── phyllotaxis.ts
│   │   └── pdf-utils.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── functions/              # Firebase Cloud Functions
│   └── generateReport/
├── .github/workflows/      # CI/CD
│   └── deploy.yml
├── public/                 # PWA icons & manifest
├── vite.config.ts
├── package.json
└── README.md
```

## Firebase Setup

### 1. Create a Firebase Project
- Go to [Firebase Console](https://console.firebase.google.com/)
- Create new project: `goldenlab`
- Enable Authentication, Firestore, Storage, Cloud Functions

### 2. Get Service Account
- Project Settings → Service Accounts → Generate new private key
- Save as `service-account.json` (keep it secret!)

### 3. Set Up GitHub Secrets
Add these to your repository Secrets (Settings → Secrets → Actions):
- `FIREBASE_SERVICE_ACCOUNT`: Paste the entire JSON file content
- `FIREBASE_PROJECT_ID`: Your Firebase project ID

## Build & Deploy

### Build for Production
```bash
npm run build
```

### Deploy to Firebase
```bash
# Requires Firebase login and project setup
npm run deploy
```

## Technology Stack

- **Frontend**: React 18 + TypeScript + Vite
- **Styling**: Tailwind CSS
- **Routing**: React Router v6
- **Database**: Firebase Firestore
- **Storage**: Firebase Storage
- **Auth**: Firebase Authentication
- **Functions**: Firebase Cloud Functions (Node.js)
- **PDF**: html2pdf.js or pdfkit
- **Charts**: Recharts
- **QR Codes**: qrcode.react
- **PWA**: vite-plugin-pwa

## Routes

- `/` — Home page
- `/learn` — Educational content
- `/measure` — Image upload & measurement
- `/simulate` — Phyllotaxis simulator
- `/report` — Generated reports
- `/present` — Presentation mode (full-screen)
- `/admin` — Teacher dashboard

## Key Components

### PhyllotaxisCanvas
Generates sunflower seed patterns using the golden angle. Renders seeds on canvas with adjustable parameters.

### ImageOverlay
Allows overlaying golden spirals and golden rectangles on uploaded images for measurement and analysis.

### Protractor
Click-to-measure angle tool. Users click center point and another point to compute angle in degrees.

### PDFGenerator (Cloud Function)
Converts experiment data + images to beautiful PDF report with:
- Cover page (title, students, synopsis)
- Image gallery with annotations
- Data tables
- Analysis graphs

## Development Workflow

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Make changes, commit with conventional commits
3. Push to GitHub
4. CI/CD automatically builds and deploys on merge to main

## Contributing

Feel free to submit PRs for:
- New educational content
- Improved image processing
- Additional simulations
- UI/UX improvements

## License

MIT License - See LICENSE file

## Support

For questions or issues:
- Check GitHub Issues
- Contact: palash@example.com, ananya@example.com

---

**Last Updated**: November 2025
