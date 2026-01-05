# Project Context

**Project**: Groovy 🥁
**Repository**: https://github.com/AdarBahar/groovy
**Local Path**: `/Users/adar.bahar/Code/groovy`
**Status**: Active Development (v0.1.0)

---

## What is Groovy?

Groovy is a **modern drum notation editor and player** built with strict separation between core logic and UI. It allows users to create, edit, and play drum patterns with an interactive grid interface.

### Core Architecture Principles

1. **Framework-Agnostic Core** (`src/core/`)
   - Zero dependencies on React or any UI framework
   - Event-based communication (observer pattern)
   - Can be used with React, Vue, Svelte, or vanilla JS

2. **React UI Layer** (`src/components/`, `src/pages/`)
   - Consumes core engine through adapter hooks
   - Completely replaceable without touching core logic

3. **Adapter Layer** (`src/hooks/`)
   - Bridges core logic and React
   - `useGrooveEngine` hook is the ONLY React-specific integration point

---

## Current System State (2026-01-05)

### ✅ What's Working

**Core Engine** (Framework-agnostic)
- `GrooveEngine` - Playback engine with precise timing and scheduling
- `DrumSynth` - Web Audio API integration with sample loading
- Event-based API: `playbackStateChange`, `positionChange`, `grooveChange`
- Swing calculation and sync modes (start/middle/end)
- Loop management with pending groove updates

**POC Testing Interface** (`/poc`)
- Full drum grid (hihat, snare, kick)
- Playback controls (play/stop with visual feedback)
- Tempo control (40-240 BPM)
- Swing control (0-100%)
- Pattern presets (Basic Rock, Disco, Funk, Reggae, Bossa Nova, Samba, Shuffle, Waltz)
- Sync mode selection
- Real-time position indicator
- Sound preview on voice labels
- 30 drum samples loaded from `/public/sounds/`

**Production UI** (`/`)
- Placeholder page with navigation
- Ready for development

**Infrastructure**
- React Router for dual-page setup
- TypeScript with strict type checking
- Vite for fast development and builds
- ESLint for code quality

### 🚧 In Progress

- Production UI design and implementation
- Additional drum voices (toms, cymbals, percussion)
- Advanced features (velocity, articulations, multi-measure)

### 📋 Planned

- Notation rendering (ABC notation, sheet music)
- MIDI import/export
- Audio export
- Save/load patterns
- Cloud storage and user accounts

---

## Recent Changes (Newest First)

### 2026-01-05: CodeAgent Kit Integration
- Embedded codeagent-kit v1.2.0 at `.codeagent/`
- Added project documentation structure
- See `.codeagent/current/project_history.md` for detailed history

### 2025-12-XX: Dual-Page Setup
- Split into POC testing page (`/poc`) and production UI (`/`)
- Added React Router for navigation
- Moved POC components to `src/poc/` directory
- Created placeholder production page

### 2025-12-XX: Initial Implementation
- Core engine architecture (GrooveEngine, DrumSynth)
- React adapter hook (useGrooveEngine)
- POC interface with full functionality
- 30 drum samples loaded

---

## Tech Stack

- **React 18** - UI framework
- **TypeScript 5.6** - Type safety
- **Vite 6** - Build tool and dev server
- **Web Audio API** - Audio synthesis and playback
- **React Router 7** - Client-side routing
- **ESLint** - Code linting

---

## Key Files

### Core Logic (Framework-Agnostic)
- `src/core/GrooveEngine.ts` - Main playback engine
- `src/core/DrumSynth.ts` - Audio synthesis
- `src/core/index.ts` - Public API exports

### React Integration
- `src/hooks/useGrooveEngine.ts` - React adapter hook
- `src/App.tsx` - Router setup
- `src/main.tsx` - React entry point

### POC Testing
- `src/poc/PocApp.tsx` - POC application
- `src/poc/components/` - POC UI components
- `src/pages/PocPage.tsx` - POC route wrapper

### Production UI
- `src/pages/ProductionPage.tsx` - Production route (placeholder)
- `src/components/` - Shared/production components

### Types & Config
- `src/types.ts` - Shared TypeScript types
- `package.json` - Dependencies and scripts
- `tsconfig.json` - TypeScript configuration
- `vite.config.ts` - Vite configuration

---

## Development Workflow

1. **Start dev server**: `npm run dev`
2. **Test on POC**: http://localhost:5174/poc
3. **Build production**: http://localhost:5174/
4. **Type check**: `npm run type-check`
5. **Lint**: `npm run lint`

---

## Links

- **GitHub**: https://github.com/AdarBahar/groovy
- **POC Reference**: `~/Code/GrooveScribe-1/groove-poc/`
- **Documentation**: See `README.md`, `GETTING_STARTED.md`, `DUAL_PAGE_SETUP.md`
