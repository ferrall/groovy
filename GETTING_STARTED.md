# Getting Started with Groovy

## ✅ Project Setup Complete!

Your new Groovy project is ready to go with clean architecture and separated concerns.

---

## 📁 What's Been Created

### Repository
- **GitHub**: https://github.com/AdarBahar/groovy
- **Local**: `/Users/adar.bahar/Code/groovy`
- **Branch**: `main`

### Project Structure
```
groovy/
├── src/
│   ├── core/                   # ✅ Framework-agnostic core logic
│   │   ├── GrooveEngine.ts    # Playback engine
│   │   ├── DrumSynth.ts       # Audio synthesis
│   │   └── index.ts           # Public API
│   ├── types.ts               # Shared TypeScript types
│   ├── App.tsx                # Main React app (placeholder)
│   ├── App.css
│   ├── main.tsx               # React entry point
│   └── index.css
├── package.json
├── tsconfig.json
├── vite.config.ts
├── index.html
├── .gitignore
└── README.md
```

---

## 🚀 Quick Start

### 1. Development Server (Already Running!)
```bash
npm run dev
```
**URL**: http://localhost:5174

### 2. Type Check
```bash
npm run type-check
```
✅ **Status**: Passing (no errors)

### 3. Build for Production
```bash
npm run build
```

---

## ✅ What's Working

### Core Engine
- ✅ `GrooveEngine` - Complete playback engine with event-based API
- ✅ `DrumSynth` - Audio sample loading and playback
- ✅ TypeScript types and interfaces
- ✅ Zero UI dependencies in core

### Development Setup
- ✅ Vite + React + TypeScript configured
- ✅ TypeScript strict mode enabled
- ✅ ESLint configured
- ✅ Git repository initialized
- ✅ Pushed to GitHub

---

## 🎯 Next Steps

### Phase 1: React Adapter (Immediate)
Create the React hook to bridge core and UI:

```bash
# Create the hook
touch src/hooks/useGrooveEngine.ts
```

Copy the hook implementation from the POC:
- Location: `~/Code/GrooveScribe-1/groove-poc/src/hooks/useGrooveEngine.ts`

### Phase 2: Basic UI Components
1. **DrumGrid** - Interactive drum pattern editor
2. **PlaybackControls** - Play/stop buttons
3. **TempoControl** - Tempo and swing sliders
4. **PresetSelector** - Pattern presets

### Phase 3: Copy Drum Samples
```bash
# Copy sounds from POC
cp -r ~/Code/GrooveScribe-1/groove-poc/public/sounds public/
```

### Phase 4: Test & Iterate
- Test playback functionality
- Verify timing accuracy
- Test swing calculation
- Verify sound quality

---

## 📚 Key Files to Know

### Core Logic (Framework-Agnostic)
- **`src/core/GrooveEngine.ts`** - Main playback engine
  - Event-based API
  - Timing and scheduling
  - Loop management
  
- **`src/core/DrumSynth.ts`** - Audio synthesis
  - Sample loading
  - Web Audio API integration
  
- **`src/core/index.ts`** - Public API exports
  - Clean interface for consumers

### Types
- **`src/types.ts`** - Shared type definitions
  - `GrooveData`, `DrumVoice`, `TimeSignature`, etc.

### Configuration
- **`package.json`** - Dependencies and scripts
- **`tsconfig.json`** - TypeScript configuration
- **`vite.config.ts`** - Vite build configuration

---

## 🔍 Architecture Principles

### 1. Separation of Concerns
- **Core** (`src/core/`) = Business logic (framework-agnostic)
- **Hooks** (`src/hooks/`) = React adapters
- **Components** (`src/components/`) = UI presentation

### 2. Event-Based Communication
- Core emits events (`positionChange`, `playbackStateChange`)
- UI listens to events via hooks
- No tight coupling

### 3. Framework Independence
- Core has ZERO React dependencies
- Can be used with Vue, Svelte, or vanilla JS
- Easy to test without UI

---

## 🧪 Testing the Core (Without UI)

You can test the core engine in the browser console:

```javascript
import { GrooveEngine } from './src/core/index.ts';

const engine = new GrooveEngine();

engine.on('positionChange', (pos) => {
  console.log('Position:', pos);
});

engine.on('playbackStateChange', (playing) => {
  console.log('Playing:', playing);
});

const groove = {
  timeSignature: { beats: 4, noteValue: 4 },
  division: 16,
  tempo: 120,
  swing: 0,
  notes: {
    hihat: Array(16).fill(true),
    snare: Array(16).fill(false),
    kick: Array(16).fill(false),
  }
};

await engine.play(groove);
```

---

## 📖 Documentation

- **README.md** - Project overview and roadmap
- **GETTING_STARTED.md** - This file
- **POC Reference**: `~/Code/GrooveScribe-1/groove-poc/`
  - `ARCHITECTURE.md` - Detailed architecture
  - `SEPARATION_OF_CONCERNS.md` - Design principles
  - `REFACTORING_SUMMARY.md` - What changed and why

---

## 🎉 You're All Set!

The project is initialized with:
- ✅ Clean architecture
- ✅ Separated core logic
- ✅ TypeScript configured
- ✅ Dev server running
- ✅ Git repository set up
- ✅ Pushed to GitHub

**Next**: Start building the React adapter and UI components!


