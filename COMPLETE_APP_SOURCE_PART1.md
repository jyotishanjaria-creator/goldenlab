# GoldenLab - Complete Application Source Code (Part 1)

## Master File Index
This document contains all production-ready source code for the GoldenLab application.
Copy each code block and create the file in the specified directory.

---

## CORE APPLICATION FILES

### FILE: `src/App.tsx`
```typescript
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import { useEffect, useState } from 'react';
import { onAuthStateChanged } from 'firebase/auth';
import { auth } from './lib/firebaseConfig';

// Pages
import Home from './pages/Home';
import Learn from './pages/Learn';
import Measure from './pages/Measure';
import Simulate from './pages/Simulate';
import Report from './pages/Report';
import Present from './pages/Present';
import Admin from './pages/Admin';

// Components
import Navigation from './components/shared/Navigation';
import Loading from './components/shared/Loading';

export default function App() {
  const [user, setUser] = useState<any>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
      setLoading(false);
    });
    return () => unsubscribe();
  }, []);

  if (loading) return <Loading />;

  return (
    <Router>
      <div className="min-h-screen bg-gradient-to-br from-yellow-50 to-amber-50">
        {/* Navigation - hidden on /present route */}
        <Routes>
          <Route path="/present" element={null} />
          <Route path="*" element={<Navigation user={user} />} />
        </Routes>

        <Routes>
          <Route path="/" element={<Home user={user} />} />
          <Route path="/learn" element={<Learn />} />
          <Route path="/measure" element={user ? <Measure user={user} /> : <Navigate to="/" />} />
          <Route path="/simulate" element={<Simulate />} />
          <Route path="/report" element={user ? <Report user={user} /> : <Navigate to="/" />} />
          <Route path="/present" element={<Present />} />
          <Route path="/admin" element={user ? <Admin user={user} /> : <Navigate to="/" />} />
        </Routes>
      </div>
    </Router>
  );
}
```

### FILE: `src/lib/phyllotaxis.ts`
```typescript
/**
 * Phyllotaxis Simulation Engine
 * Implements the golden angle formula for natural pattern generation
 */

const GOLDEN_ANGLE = 137.5077640500378; // degrees
const PHI = (1 + Math.sqrt(5)) / 2; // golden ratio

export interface Seed {
  x: number;
  y: number;
  angle: number;
  radius: number;
  index: number;
}

export function generatePhyllotaxisSeeds(
  count: number,
  angleOffset: number = 0,
  scale: number = 1
): Seed[] {
  const seeds: Seed[] = [];
  const c = 4 * scale; // scaling constant

  for (let n = 0; n < count; n++) {
    const radius = c * Math.sqrt(n);
    const angle = (n * GOLDEN_ANGLE + angleOffset) * (Math.PI / 180);

    seeds.push({
      x: radius * Math.cos(angle),
      y: radius * Math.sin(angle),
      angle: angle,
      radius: radius,
      index: n,
    });
  }

  return seeds;
}

export function calculatePackingMetrics(seeds: Seed[]): number {
  if (seeds.length < 2) return 0;

  let totalDistance = 0;
  let count = 0;

  // Calculate nearest neighbor distances
  for (let i = 0; i < Math.min(seeds.length, 100); i++) {
    let minDistance = Infinity;

    for (let j = 0; j < seeds.length; j++) {
      if (i === j) continue;

      const dx = seeds[i].x - seeds[j].x;
      const dy = seeds[i].y - seeds[j].y;
      const distance = Math.sqrt(dx * dx + dy * dy);

      if (distance < minDistance && distance > 0) {
        minDistance = distance;
      }
    }

    if (minDistance !== Infinity) {
      totalDistance += minDistance;
      count++;
    }
  }

  return count > 0 ? totalDistance / count : 0;
}

export function drawPhyllotaxis(
  ctx: CanvasRenderingContext2D,
  seeds: Seed[],
  radius: number = 4,
  color: string = '#ffd700'
): void {
  ctx.fillStyle = color;
  ctx.strokeStyle = '#ff9100';
  ctx.lineWidth = 0.5;

  seeds.forEach((seed) => {
    const screenX = seed.x + ctx.canvas.width / 2;
    const screenY = seed.y + ctx.canvas.height / 2;

    // Draw circle
    ctx.beginPath();
    ctx.arc(screenX, screenY, radius, 0, Math.PI * 2);
    ctx.fill();
    ctx.stroke();
  });
}
```

### FILE: `src/lib/pdf-utils.ts`
```typescript
/**
 * PDF Generation Utilities
 * Helper functions for PDF creation
 */

export interface ReportData {
  title: string;
  studentNames: string[];
  images: Array<{ url: string; label: string }>;
  data: {
    cwCount?: number;
    ccwCount?: number;
    ratio?: number;
    angle?: number;
    seeds?: number;
    packingScore?: number;
  };
  timestamp: Date;
}

export function generateHTMLReport(data: ReportData): string {
  const ratio = data.data.ratio?.toFixed(4) || 'N/A';
  const packingScore = data.data.packingScore?.toFixed(2) || 'N/A';

  return `
    <!DOCTYPE html>
    <html>
    <head>
      <title>${data.title}</title>
      <style>
        body { font-family: Arial, sans-serif; margin: 20px; background: #fafafa; }
        .header { text-align: center; margin-bottom: 30px; border-bottom: 3px solid #ffd700; padding-bottom: 20px; }
        h1 { color: #1a1a1a; margin: 0; }
        .students { color: #666; font-size: 14px; margin-top: 10px; }
        .section { margin-bottom: 30px; page-break-inside: avoid; }
        .section h2 { color: #ff9100; border-left: 4px solid #ffd700; padding-left: 10px; }
        .data-table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        .data-table td { padding: 8px; border: 1px solid #ddd; }
        .data-table td:first-child { font-weight: bold; background: #fffde1; width: 40%; }
        .images { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 15px; }
        .image-container { border: 2px solid #ffd700; padding: 10px; border-radius: 8px; }
        .image-container img { width: 100%; height: auto; border-radius: 4px; }
        .image-label { font-size: 12px; color: #666; margin-top: 8px; text-align: center; font-weight: bold; }
        .footer { text-align: center; color: #999; font-size: 12px; margin-top: 40px; border-top: 1px solid #ddd; padding-top: 20px; }
      </style>
    </head>
    <body>
      <div class="header">
        <h1>${data.title}</h1>
        <div class="students">Students: ${data.studentNames.join(', ')}</div>
        <div class="students">Date: ${data.timestamp.toLocaleDateString()}</div>
      </div>

      ${data.images.length > 0 ? `
      <div class="section">
        <h2>Experiment Images</h2>
        <div class="images">
          ${data.images.map(img => `
            <div class="image-container">
              <img src="${img.url}" alt="${img.label}" />
              <div class="image-label">${img.label}</div>
            </div>
          `).join('')}
        </div>
      </div>
      ` : ''}

      <div class="section">
        <h2>Analysis Results</h2>
        <table class="data-table">
          ${data.data.ratio ? `<tr><td>Golden Ratio (φ)</td><td>${ratio}</td></tr>` : ''}
          ${data.data.cwCount ? `<tr><td>Clockwise Count</td><td>${data.data.cwCount}</td></tr>` : ''}
          ${data.data.ccwCount ? `<tr><td>Counter-Clockwise Count</td><td>${data.data.ccwCount}</td></tr>` : ''}
          ${data.data.angle ? `<tr><td>Angle Measured (°)</td><td>${data.data.angle.toFixed(2)}</td></tr>` : ''}
          ${data.data.seeds ? `<tr><td>Seeds Simulated</td><td>${data.data.seeds}</td></tr>` : ''}
          ${data.data.packingScore ? `<tr><td>Packing Efficiency</td><td>${packingScore}</td></tr>` : ''}
        </table>
      </div>

      <div class="footer">
        <p>Generated by GoldenLab - Golden Ratio Explorer</p>
        <p>© 2025 | Palash Anjaria & Ananya Tiwari | Rangoli International School</p>
      </div>
    </body>
    </html>
  `;
}
```

## CONTINUE TO PART 2
Due to file size limits, the complete component code (pages, components, etc.) continues in:
- `COMPLETE_APP_SOURCE_PART2.md` (Pages)
- `COMPLETE_APP_SOURCE_PART3.md` (Components)
- `COMPLETE_APP_SOURCE_PART4.md` (Cloud Functions & Utilities)

See the repository for all parts.
