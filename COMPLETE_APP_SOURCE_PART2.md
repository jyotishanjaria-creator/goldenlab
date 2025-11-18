# COMPLETE_APP_SOURCE_PART2.md - React Pages

This document contains all React page components for the GoldenLab application.

## Directory Structure
```
src/
  pages/
    Home.tsx
    Learn.tsx
    Measure.tsx
    Simulate.tsx
    Report.tsx
    Present.tsx
    Admin.tsx
```

## src/pages/Home.tsx - Landing Page

```typescript
import React from 'react';
import { Link } from 'react-router-dom';
import { Flower, Ruler, Activity, FileText, Presentation, Settings } from 'lucide-react';

const Home: React.FC = () => {
  const features = [
    {
      icon: Flower,
      title: 'Learn',
      description: 'Understand the Golden Ratio in nature',
      path: '/learn',
      color: 'from-yellow-400 to-yellow-600'
    },
    {
      icon: Ruler,
      title: 'Measure',
      description: 'Upload photos and measure ratios',
      path: '/measure',
      color: 'from-blue-400 to-blue-600'
    },
    {
      icon: Activity,
      title: 'Simulate',
      description: 'Explore phyllotaxis patterns',
      path: '/simulate',
      color: 'from-green-400 to-green-600'
    },
    {
      icon: FileText,
      title: 'Report',
      description: 'Generate PDF experiment reports',
      path: '/report',
      color: 'from-purple-400 to-purple-600'
    },
    {
      icon: Presentation,
      title: 'Present',
      description: 'Full-screen presentation mode',
      path: '/present',
      color: 'from-pink-400 to-pink-600'
    },
    {
      icon: Settings,
      title: 'Admin',
      description: 'Teacher dashboard & controls',
      path: '/admin',
      color: 'from-gray-400 to-gray-600'
    }
  ];

  return (
    <div className="min-h-screen bg-gradient-to-b from-amber-50 to-white">
      {/* Hero Section */}
      <div className="max-w-7xl mx-auto px-4 py-16 sm:py-24">
        <div className="text-center">
          <h1 className="text-5xl sm:text-6xl font-bold text-gray-900 mb-4">
            GoldenLab
          </h1>
          <p className="text-xl sm:text-2xl text-gray-600 mb-8">
            Explore the Golden Ratio in Nature
          </p>
          <p className="text-gray-600 max-w-2xl mx-auto mb-12">
            An interactive learning platform that teaches the Golden Ratio through photography,
            simulation, and hands-on experimentation. Perfect for science projects and exhibitions.
          </p>
          <div className="flex gap-4 justify-center">
            <Link
              to="/learn"
              className="px-8 py-3 bg-amber-500 text-white rounded-lg font-semibold hover:bg-amber-600 transition"
            >
              Get Started
            </Link>
            <Link
              to="/present"
              className="px-8 py-3 border-2 border-amber-500 text-amber-600 rounded-lg font-semibold hover:bg-amber-50 transition"
            >
              View Presentation
            </Link>
          </div>
        </div>
      </div>

      {/* Features Grid */}
      <div className="max-w-7xl mx-auto px-4 py-16">
        <h2 className="text-3xl font-bold text-center mb-12">Features</h2>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          {features.map((feature) => {
            const IconComponent = feature.icon;
            return (
              <Link
                key={feature.path}
                to={feature.path}
                className="group"
              >
                <div className="bg-white rounded-lg shadow-lg p-8 hover:shadow-xl transition h-full border-t-4 border-amber-500">
                  <div className={`w-12 h-12 rounded-lg bg-gradient-to-br ${feature.color} flex items-center justify-center mb-4 group-hover:scale-110 transition`}>
                    <IconComponent className="w-6 h-6 text-white" />
                  </div>
                  <h3 className="text-xl font-bold mb-2">{feature.title}</h3>
                  <p className="text-gray-600">{feature.description}</p>
                </div>
              </Link>
            );
          })}
        </div>
      </div>

      {/* Footer */}
      <div className="bg-gray-900 text-white py-12 mt-16">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <p className="text-gray-400">GoldenLab © 2025 | Palash Anjaria & Ananya Tiwari</p>
          <p className="text-gray-500 text-sm mt-2">Class 9 Project | Rangoli International School</p>
        </div>
      </div>
    </div>
  );
};

export default Home;
```

## src/pages/Learn.tsx - Educational Content

```typescript
import React, { useState } from 'react';
import { ChevronDown } from 'lucide-react';

const Learn: React.FC = () => {
  const [expandedCard, setExpandedCard] = useState<number | null>(0);

  const lessons = [
    {
      id: 1,
      title: 'What is the Golden Ratio?',
      content: 'The Golden Ratio (φ ≈ 1.618) is a mathematical constant that appears frequently in nature. It describes a proportion that is aesthetically pleasing and mathematically elegant.'
    },
    {
      id: 2,
      title: 'Where Does It Appear?',
      content: 'The golden ratio appears in: sunflower seed spirals, pinecones, shells, galaxies, human faces, and many other natural patterns.'
    },
    {
      id: 3,
      title: 'The Mathematics',
      content: 'φ = (1 + √5) / 2 ≈ 1.618033988749... It satisfies the equation: φ² = φ + 1'
    },
    {
      id: 4,
      title: 'Fibonacci Sequence',
      content: 'The ratio of consecutive Fibonacci numbers approaches the golden ratio. Example: 55/34 ≈ 1.618'
    },
    {
      id: 5,
      title: 'Phyllotaxis',
      content: 'Plants arrange leaves/seeds using the golden angle (137.5°) for optimal sunlight exposure and spacing.'
    },
    {
      id: 6,
      title: 'Measuring Ratios',
      content: 'To find the golden ratio in photos: measure a longer segment (a) and shorter segment (b), calculate a/b.'
    }
  ];

  return (
    <div className="min-h-screen bg-gradient-to-b from-blue-50 to-white py-12">
      <div className="max-w-4xl mx-auto px-4">
        <h1 className="text-4xl font-bold mb-4">Learn About the Golden Ratio</h1>
        <p className="text-gray-600 mb-12">Expand each card to learn more</p>

        <div className="space-y-4">
          {lessons.map((lesson) => (
            <div
              key={lesson.id}
              className="bg-white rounded-lg shadow-md border-l-4 border-blue-500 overflow-hidden"
            >
              <button
                onClick={() => setExpandedCard(expandedCard === lesson.id ? null : lesson.id)}
                className="w-full px-6 py-4 flex items-center justify-between hover:bg-blue-50 transition text-left"
              >
                <h3 className="font-semibold text-lg text-gray-900">{lesson.title}</h3>
                <ChevronDown
                  className={`w-5 h-5 text-blue-500 transition-transform ${
                    expandedCard === lesson.id ? 'rotate-180' : ''
                  }`}
                />
              </button>
              {expandedCard === lesson.id && (
                <div className="px-6 py-4 bg-blue-50 border-t border-blue-200">
                  <p className="text-gray-700">{lesson.content}</p>
                </div>
              )}
            </div>
          ))}
        </div>

        {/* Key Facts Box */}
        <div className="mt-12 bg-gradient-to-r from-blue-400 to-blue-600 text-white rounded-lg p-8">
          <h2 className="text-2xl font-bold mb-4">Key Facts</h2>
          <ul className="space-y-2">
            <li>✓ φ (phi) = 1.618033988749...</li>
            <li>✓ φ² = φ + 1</li>
            <li>✓ The golden angle = 137.5077640500378°</li>
            <li>✓ Found in spirals, buildings, art, and nature</li>
            <li>✓ Present in human anatomy (face proportions, bone ratios)</li>
          </ul>
        </div>
      </div>
    </div>
  );
};

export default Learn;
```

## CONTINUE TO PART 3

Due to file size limits, the following pages are documented in PART 3:
- `src/pages/Measure.tsx` (Image upload, overlay, measurements)
- `src/pages/Simulate.tsx` (Phyllotaxis simulation with canvas)
- `src/pages/Report.tsx` (PDF report generation)
- `src/pages/Present.tsx` (Full-screen presentation mode)
- `src/pages/Admin.tsx` (Teacher dashboard)
