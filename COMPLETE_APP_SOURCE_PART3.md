# COMPLETE_APP_SOURCE_PART3.md - Advanced Pages

This document contains the remaining React page components and essential utilities for GoldenLab.

## src/pages/Measure.tsx - Image Upload & Measurement

```typescript
import React, { useState, useRef } from 'react';
import { Upload, Trash2, Download } from 'lucide-react';
import { calculateRatio } from '../lib/phyllotaxis';

const Measure: React.FC = () => {
  const [image, setImage] = useState<string | null>(null);
  const [measurements, setMeasurements] = useState<Array<{ x1: number; y1: number; x2: number; y2: number; ratio: number }>>([]);
  const [drawingMode, setDrawingMode] = useState(false);
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const fileInputRef = useRef<HTMLInputElement>(null);

  const handleImageUpload = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = (event) => {
      setImage(event.target?.result as string);
      setMeasurements([]);
    };
    reader.readAsDataURL(file);
  };

  const handleCanvasClick = (e: React.MouseEvent<HTMLCanvasElement>) => {
    if (!drawingMode || !canvasRef.current) return;

    const rect = canvasRef.current.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;

    // This would need more complete implementation for actual measurement
    console.log('Clicked at:', x, y);
  };

  const handleClearImage = () => {
    setImage(null);
    setMeasurements([]);
  };

  return (
    <div className="min-h-screen bg-gradient-to-b from-blue-50 to-white py-12">
      <div className="max-w-6xl mx-auto px-4">
        <h1 className="text-4xl font-bold mb-8">Measure Golden Ratios</h1>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          {/* Upload Section */}
          <div className="lg:col-span-2">
            <div className="bg-white rounded-lg shadow-lg p-8">
              {!image ? (
                <div
                  className="border-2 border-dashed border-blue-300 rounded-lg p-12 text-center cursor-pointer hover:border-blue-500 transition"
                  onClick={() => fileInputRef.current?.click()}
                >
                  <Upload className="w-16 h-16 mx-auto text-blue-400 mb-4" />
                  <h3 className="text-xl font-semibold mb-2">Upload a Photo</h3>
                  <p className="text-gray-600">Click to select an image (sunflower, face, shell, etc.)</p>
                  <input
                    ref={fileInputRef}
                    type="file"
                    accept="image/*"
                    onChange={handleImageUpload}
                    className="hidden"
                  />
                </div>
              ) : (
                <div>
                  <img src={image} alt="Measurement" className="w-full rounded-lg mb-4" />
                  <button
                    onClick={handleClearImage}
                    className="flex items-center gap-2 px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600 transition"
                  >
                    <Trash2 className="w-4 h-4" />
                    Clear Image
                  </button>
                </div>
              )}
            </div>
          </div>

          {/* Stats Section */}
          <div className="bg-white rounded-lg shadow-lg p-8">
            <h3 className="text-xl font-bold mb-4">Measurements</h3>
            {measurements.length > 0 ? (
              <div className="space-y-2">
                {measurements.map((m, i) => (
                  <div key={i} className="p-2 bg-blue-50 rounded">
                    <p className="text-sm">Ratio {i + 1}</p>
                    <p className="text-lg font-bold text-blue-600">{m.ratio.toFixed(3)}</p>
                  </div>
                ))}
                <button className="w-full px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600 transition mt-4">
                  <Download className="w-4 h-4 inline mr-2" />
                  Save Measurements
                </button>
              </div>
            ) : (
              <p className="text-gray-500">No measurements yet</p>
            )}
          </div>
        </div>
      </div>
    </div>
  );
};

export default Measure;
```

## src/pages/Simulate.tsx - Phyllotaxis Simulation

```typescript
import React, { useState, useRef, useEffect } from 'react';
import { Slider } from '../components/Slider';
import { generatePhyllotaxis } from '../lib/phyllotaxis';

const Simulate: React.FC = () => {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const [seedCount, setSeedCount] = useState(200);
  const [angle, setAngle] = useState(137.5077640500378);
  const [scale, setScale] = useState(5);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const seeds = generatePhyllotaxis(seedCount, angle, scale);
    const centerX = canvas.width / 2;
    const centerY = canvas.height / 2;

    ctx.fillStyle = '#f59e0b';
    seeds.forEach((seed, index) => {
      const hue = (index / seedCount) * 360;
      ctx.fillStyle = `hsl(${hue}, 70%, 50%)`;
      ctx.beginPath();
      ctx.arc(centerX + seed.x, centerY + seed.y, 3, 0, Math.PI * 2);
      ctx.fill();
    });
  }, [seedCount, angle, scale]);

  return (
    <div className="min-h-screen bg-gradient-to-b from-green-50 to-white py-12">
      <div className="max-w-6xl mx-auto px-4">
        <h1 className="text-4xl font-bold mb-8">Phyllotaxis Simulation</h1>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          <div className="lg:col-span-2">
            <canvas
              ref={canvasRef}
              width={600}
              height={600}
              className="bg-white rounded-lg shadow-lg w-full border-2 border-green-200"
            />
          </div>

          <div className="bg-white rounded-lg shadow-lg p-6">
            <h3 className="text-xl font-bold mb-6">Controls</h3>

            <div className="space-y-6">
              <div>
                <label className="block text-sm font-semibold mb-2">
                  Seed Count: {seedCount}
                </label>
                <input
                  type="range"
                  min="10"
                  max="1000"
                  value={seedCount}
                  onChange={(e) => setSeedCount(parseInt(e.target.value))}
                  className="w-full"
                />
              </div>

              <div>
                <label className="block text-sm font-semibold mb-2">
                  Angle: {angle.toFixed(2)}°
                </label>
                <input
                  type="range"
                  min="130"
                  max="145"
                  step="0.01"
                  value={angle}
                  onChange={(e) => setAngle(parseFloat(e.target.value))}
                  className="w-full"
                />
                <button
                  onClick={() => setAngle(137.5077640500378)}
                  className="text-xs text-blue-600 hover:underline mt-2"
                >
                  Reset to Golden Angle
                </button>
              </div>

              <div>
                <label className="block text-sm font-semibold mb-2">
                  Scale: {scale}x
                </label>
                <input
                  type="range"
                  min="1"
                  max="10"
                  value={scale}
                  onChange={(e) => setScale(parseInt(e.target.value))}
                  className="w-full"
                />
              </div>

              <div className="p-4 bg-green-50 rounded border-l-4 border-green-500">
                <p className="text-sm text-gray-700">
                  <strong>Golden Angle:</strong> 137.5077640500378°
                </p>
                <p className="text-xs text-gray-600 mt-2">
                  This angle produces optimal leaf/seed spacing in nature.
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Simulate;
```

## CONTINUE TO PART 4

Due to file size limits, the following components are documented in PART 4:
- `src/pages/Report.tsx` (PDF generation interface)
- `src/pages/Present.tsx` (Full-screen presentation)
- `src/pages/Admin.tsx` (Teacher dashboard)
- `src/components/` (All reusable components)
- `src/lib/firebase.ts` (Firebase integration)
- Unit tests and Cloud Functions
