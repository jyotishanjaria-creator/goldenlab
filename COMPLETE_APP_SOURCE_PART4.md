# COMPLETE_APP_SOURCE_PART4.md - Final Pages, Components & Backend

This document contains the final pages, reusable components, Firebase integration, and deployment instructions.

## src/pages/Report.tsx - PDF Report Generation

```typescript
import React, { useState } from 'react';
import { Download, FileText } from 'lucide-react';
import { generatePDFReport } from '../lib/pdf-utils';

const Report: React.FC = () => {
  const [isGenerating, setIsGenerating] = useState(false);
  const [projectName, setProjectName] = useState('Golden Ratio Experiment');
  const [studentName, setStudentName] = useState('');

  const handleGeneratePDF = async () => {
    setIsGenerating(true);
    try {
      const reportData = {
        title: projectName,
        studentName,
        date: new Date().toLocaleDateString(),
        observations: 'Phyllotaxis patterns observed in nature',
        measurements: { ratio: 1.618, accuracy: 98.5 }
      };
      await generatePDFReport(reportData);
    } finally {
      setIsGenerating(false);
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-b from-purple-50 to-white py-12">
      <div className="max-w-4xl mx-auto px-4">
        <h1 className="text-4xl font-bold mb-8">Generate Report</h1>
        <div className="bg-white rounded-lg shadow-lg p-8">
          <div className="space-y-4 mb-6">
            <div>
              <label className="block text-sm font-semibold mb-2">Project Name</label>
              <input
                type="text"
                value={projectName}
                onChange={(e) => setProjectName(e.target.value)}
                className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500"
              />
            </div>
            <div>
              <label className="block text-sm font-semibold mb-2">Student Name</label>
              <input
                type="text"
                value={studentName}
                onChange={(e) => setStudentName(e.target.value)}
                className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500"
              />
            </div>
          </div>
          <button
            onClick={handleGeneratePDF}
            disabled={isGenerating}
            className="flex items-center gap-2 px-6 py-3 bg-purple-500 text-white rounded-lg hover:bg-purple-600 disabled:bg-gray-400 transition"
          >
            <Download className="w-5 h-5" />
            {isGenerating ? 'Generating...' : 'Generate PDF Report'}
          </button>
        </div>
      </div>
    </div>
  );
};

export default Report;
```

## src/pages/Present.tsx - Full-Screen Presentation Mode

```typescript
import React, { useState } from 'react';
import { ChevronLeft, ChevronRight, Maximize2, X } from 'lucide-react';

const Present: React.FC = () => {
  const [currentSlide, setCurrentSlide] = useState(0);
  const [isFullscreen, setIsFullscreen] = useState(false);

  const slides = [
    { title: 'GoldenLab', subtitle: 'Exploring the Golden Ratio in Nature', color: 'bg-amber-500' },
    { title: 'What is the Golden Ratio?', subtitle: 'φ = 1.618033988749...', color: 'bg-blue-500' },
    { title: 'Where It Appears', subtitle: 'Sunflowers, Shells, Galaxies, Human Faces', color: 'bg-green-500' },
    { title: 'Phyllotaxis', subtitle: 'The Golden Angle: 137.5077640500378°', color: 'bg-purple-500' },
    { title: 'Our Measurements', subtitle: 'Precise ratios from nature', color: 'bg-pink-500' },
    { title: 'Conclusion', subtitle: 'Mathematics in Perfect Harmony', color: 'bg-indigo-500' }
  ];

  const slide = slides[currentSlide];

  const handleNext = () => {
    setCurrentSlide((prev) => (prev + 1) % slides.length);
  };

  const handlePrev = () => {
    setCurrentSlide((prev) => (prev - 1 + slides.length) % slides.length);
  };

  return (
    <div className={`min-h-screen ${slide.color} text-white flex items-center justify-center relative transition-colors`}>
      {/* Main Presentation Content */}
      <div className="text-center">
        <h1 className="text-7xl font-bold mb-6">{slide.title}</h1>
        <p className="text-4xl mb-12 opacity-90">{slide.subtitle}</p>
        <div className="text-lg opacity-75">Slide {currentSlide + 1} of {slides.length}</div>
      </div>

      {/* Controls */}
      <div className="absolute bottom-8 left-8 right-8 flex items-center justify-between">
        <button
          onClick={handlePrev}
          className="p-3 bg-white bg-opacity-20 hover:bg-opacity-30 rounded-full transition"
        >
          <ChevronLeft className="w-6 h-6" />
        </button>
        <button
          onClick={() => setIsFullscreen(!isFullscreen)}
          className="p-3 bg-white bg-opacity-20 hover:bg-opacity-30 rounded-full transition"
        >
          <Maximize2 className="w-6 h-6" />
        </button>
        <button
          onClick={handleNext}
          className="p-3 bg-white bg-opacity-20 hover:bg-opacity-30 rounded-full transition"
        >
          <ChevronRight className="w-6 h-6" />
        </button>
      </div>
    </div>
  );
};

export default Present;
```

## src/pages/Admin.tsx - Teacher Dashboard

```typescript
import React, { useState, useEffect } from 'react';
import { Users, FileText, Settings, LogOut } from 'lucide-react';

const Admin: React.FC = () => {
  const [students, setStudents] = useState<any[]>([]);
  const [selectedTab, setSelectedTab] = useState('students');

  useEffect(() => {
    // Fetch students from Firebase
    // setStudents(data);
  }, []);

  return (
    <div className="min-h-screen bg-gray-100">
      <div className="bg-white shadow">
        <div className="max-w-7xl mx-auto px-4 py-6 flex items-center justify-between">
          <h1 className="text-3xl font-bold">Teacher Dashboard</h1>
          <button className="flex items-center gap-2 px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600">
            <LogOut className="w-4 h-4" />
            Logout
          </button>
        </div>
      </div>

      <div className="max-w-7xl mx-auto px-4 py-8">
        <div className="flex gap-4 mb-8">
          <button
            onClick={() => setSelectedTab('students')}
            className={`flex items-center gap-2 px-4 py-2 rounded transition ${
              selectedTab === 'students' ? 'bg-blue-500 text-white' : 'bg-white text-gray-700 hover:bg-gray-100'
            }`}
          >
            <Users className="w-4 h-4" />
            Students
          </button>
          <button
            onClick={() => setSelectedTab('reports')}
            className={`flex items-center gap-2 px-4 py-2 rounded transition ${
              selectedTab === 'reports' ? 'bg-blue-500 text-white' : 'bg-white text-gray-700 hover:bg-gray-100'
            }`}
          >
            <FileText className="w-4 h-4" />
            Reports
          </button>
          <button
            onClick={() => setSelectedTab('settings')}
            className={`flex items-center gap-2 px-4 py-2 rounded transition ${
              selectedTab === 'settings' ? 'bg-blue-500 text-white' : 'bg-white text-gray-700 hover:bg-gray-100'
            }`}
          >
            <Settings className="w-4 h-4" />
            Settings
          </button>
        </div>

        <div className="bg-white rounded-lg shadow p-6">
          {selectedTab === 'students' && (
            <div>
              <h2 className="text-2xl font-bold mb-4">Manage Students</h2>
              {students.length === 0 ? (
                <p className="text-gray-600">No students enrolled yet</p>
              ) : (
                <div className="overflow-x-auto">
                  <table className="w-full">
                    <thead>
                      <tr className="border-b">
                        <th className="text-left py-2 px-4">Name</th>
                        <th className="text-left py-2 px-4">Email</th>
                        <th className="text-left py-2 px-4">Status</th>
                        <th className="text-left py-2 px-4">Actions</th>
                      </tr>
                    </thead>
                    <tbody>
                      {students.map((student) => (
                        <tr key={student.id} className="border-b hover:bg-gray-50">
                          <td className="py-2 px-4">{student.name}</td>
                          <td className="py-2 px-4">{student.email}</td>
                          <td className="py-2 px-4"><span className="px-2 py-1 bg-green-100 text-green-800 rounded">Active</span></td>
                          <td className="py-2 px-4"><button className="text-blue-600 hover:underline">View Reports</button></td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              )}
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

export default Admin;
```

## src/lib/firebase.ts - Firebase Integration

```typescript
import { initializeApp } from 'firebase/app';
import { getAuth, GoogleAuthProvider } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';
import { getStorage } from 'firebase/storage';
import { getFunctions } from 'firebase/functions';

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
export const storage = getStorage(app);
export const functions = getFunctions(app, 'asia-south1');
export const googleProvider = new GoogleAuthProvider();

export default app;
```

## Next Steps for Completion

1. **Setup Environment Variables** (.env.local)
```
VITE_FIREBASE_API_KEY=your_key
VITE_FIREBASE_AUTH_DOMAIN=your_domain
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_id
VITE_FIREBASE_APP_ID=your_app_id
```

2. **Install Dependencies**
```bash
npm install
```

3. **Run Development Server**
```bash
npm run dev
```

4. **Build for Production**
```bash
npm run build
```

5. **Deploy to Firebase**
```bash
npm run deploy
```

## Key Files Still to Create Locally
- src/components/ (all 15+ components)
- src/hooks/ (custom React hooks)
- src/__tests__/ (unit tests)
- functions/ (Cloud Functions for PDF generation)
- firestore.rules (security rules)
- storage.rules (storage security)

All core logic and pages are now documented. See SETUP_BOOTSTRAP.md for detailed setup instructions.
