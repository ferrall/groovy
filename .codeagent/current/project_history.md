# Project History

Entries are added in reverse chronological order.

Format:
- Date
- Summary
- Key changes
- Impact
- Deployment / ops notes
- Follow-ups

---

## 2026-01-05: Production Deployment Configuration for /scribe2/

**Summary**: Configured production build and deployment for www.bahar.co.il/scribe2/ subdirectory with comprehensive documentation and configurable base path.

**Key Changes**:
- **Vite Configuration** (`vite.config.ts`):
  - Added `PRODUCTION_BASE_PATH` constant for easy subdirectory configuration
  - Set base path to `/scribe2/` for production builds
  - Configured manual chunk splitting (React vendor code separate for better caching)
  - Build optimizations: minification, tree shaking, content hashing

- **React Router** (`src/App.tsx`):
  - Added `basename` prop to `BrowserRouter` using `import.meta.env.BASE_URL`
  - Fixes routing to maintain `/scribe2/` prefix in URLs

- **Sound Loading** (`src/core/DrumSynth.ts`):
  - Updated to use `import.meta.env.BASE_URL` for dynamic sound paths
  - Loads sounds from `/scribe2/sounds/` in production, `/sounds/` in dev
  - Added better error logging for sound loading failures

- **Build Scripts** (`package.json`):
  - Added `build:prod` - Production build with `/scribe2/` base path
  - Added `preview:prod` - Preview production build locally

- **Apache Configuration** (`.htaccess`):
  - Created subdirectory `.htaccess` for React Router support
  - Gzip compression, asset caching, security headers
  - MIME type configuration for audio files

- **Documentation**:
  - `DEPLOYMENT.md` - Comprehensive deployment guide
  - `CHANGE_BASE_PATH.md` - How to change deployment subdirectory
  - `DEPLOY_README.md` - Quick upload instructions
  - `FINAL_DEPLOYMENT_STEPS.md` - Complete deployment checklist
  - `ROOT_HTACCESS_CHANGES.md` - Root .htaccess modifications
  - `ROUTER_FIX_UPDATE.md` - Router basename fix details
  - `UPDATE_DEPLOYMENT.md` - Sound loading fix details
  - Updated `README.md` with deployment instructions

**Commits**:
- `2295b2e` - Make base path configurable via PRODUCTION_BASE_PATH constant
- `4565bc9` - Add router basename fix deployment guide
- `5a03d29` - Fix React Router basename for /scribe2/ subdirectory
- `2ef3498` - Add comprehensive final deployment guide
- `c8d8a80` - Fix vite.svg 404 error and add root .htaccess exclusion guide
- `a018dba` - Fix sound loading paths for production deployment
- `1b88308` - Add production build configuration for www.bahar.co.il/scribe2/

**Impact**:
- ✅ App can now be deployed to subdirectories (not just root)
- ✅ Single `PRODUCTION_BASE_PATH` constant controls all path configuration
- ✅ Comprehensive deployment documentation for manual deployment
- ✅ Build optimizations: ~480KB code + 272KB sounds = ~752KB total
- ✅ Manual chunk splitting improves caching (React vendor separate)
- ✅ Easy to change deployment path for different environments

**Deployment Notes**:
- **Current deployment**: www.bahar.co.il/scribe2/
- **Build command**: `npm run build:prod`
- **Preview command**: `npm run preview:prod`
- **Upload**: `dist/` contents to `/scribe2/` directory on server
- **Server config**: Upload `.htaccess` to `/scribe2/` directory
- **Root .htaccess**: Add `RewriteCond %{REQUEST_URI} !^/scribe2/` exclusion

**Testing**:
- ✅ Tested locally with `npm run preview:prod`
- ✅ Verified sound loading from `/scribe2/sounds/`
- ✅ Verified React Router maintains `/scribe2/` prefix
- ✅ Verified no console errors (vite.svg 404 fixed)
- ✅ Build size optimized with chunk splitting

**Follow-ups**:
- Deploy to production server (manual upload)
- Consider automated deployment with GitHub Actions
- Monitor bundle size (currently ~480KB, target <500KB)
- Consider adding favicon (currently removed to avoid 404)

---

## 2026-01-05: CodeAgent Kit Integration

**Summary**: Embedded codeagent-kit v1.2.0 for project documentation and AI-assisted development workflows.

**Key Changes**:
- Added `.codeagent/` directory via git subtree
- Flattened nested structure (`.codeagent/.codeagent/` → `.codeagent/`)
- Created documentation structure:
  - `current/` - Living documentation (project_context.md, project_history.md, memory-log.md, deployment.md, security.md, design_system.md, database_schema.sql)
  - `prompts/` - Reusable AI prompts (plan-feature.md, implement-feature.md, document-changes.md, cleanup-and-order.md)

**Commits**:
- `7b2acba` - Flatten CodeAgent Kit structure
- `1588c92` - Merge commit for subtree add
- `847f437` - Squashed codeagent-kit content

**Impact**:
- Established documentation conventions for the project
- Created workflow for AI-assisted development
- Improved project context for future development

**Deployment Notes**:
- No deployment changes
- Documentation-only update

**Follow-ups**:
- Keep documentation updated as features are added
- Use prompts for structured development workflow

---

## 2025-12-XX: Dual-Page Setup (POC + Production)

**Summary**: Split application into POC testing page and production UI to enable parallel development and testing.

**Key Changes**:
- Added `react-router-dom` dependency
- Created dual-page structure:
  - `/poc` - POC testing interface (fully functional)
  - `/` - Production UI (placeholder)
- Reorganized file structure:
  - Moved POC components to `src/poc/`
  - Created `src/pages/` for route components
  - Added `src/components/Navigation.tsx` for page switching
- Created documentation: `DUAL_PAGE_SETUP.md`

**Files Added**:
- `src/App.tsx` - Router setup
- `src/pages/PocPage.tsx` - POC route wrapper
- `src/pages/ProductionPage.tsx` - Production route
- `src/components/Navigation.tsx` - Navigation component
- `DUAL_PAGE_SETUP.md` - Documentation

**Files Moved**:
- `src/App.tsx` → `src/poc/PocApp.tsx`
- `src/App.css` → `src/poc/PocApp.css`
- `src/components/*` → `src/poc/components/*`

**Commits**:
- `a97e429` - Add dual-page setup documentation
- `0d688c4` - Add dual-page structure: POC testing page + production UI placeholder

**Impact**:
- Enables testing core logic on POC while building production UI
- Separates concerns between testing and production
- Provides clear development workflow

**Deployment Notes**:
- Dev server runs on http://localhost:5174
- POC available at `/poc`
- Production UI at `/`

**Follow-ups**:
- Build production UI components
- Design production interface
- Maintain POC for core logic testing

---

## 2025-12-XX: Initial Project Setup

**Summary**: Created Groovy project with clean architecture and separated core logic from UI.

**Key Changes**:
- Initialized React + TypeScript + Vite project
- Implemented framework-agnostic core engine:
  - `GrooveEngine` - Playback, timing, scheduling, loop management
  - `DrumSynth` - Web Audio API integration, sample loading
- Created React adapter:
  - `useGrooveEngine` hook - Bridge between core and React
- Built POC interface:
  - DrumGrid - Interactive drum pattern editor
  - PlaybackControls - Play/stop with visual feedback
  - TempoControl - Tempo (40-240 BPM) and swing (0-100%)
  - PresetSelector - 8 pattern presets
  - SyncControl - Sync mode selection (start/middle/end)
- Loaded 30 drum samples in `/public/sounds/`
- Set up TypeScript, ESLint, and Vite configuration
- Created documentation: `README.md`, `GETTING_STARTED.md`

**Core Architecture**:
- **Separation of Concerns**: Core logic has ZERO React dependencies
- **Event-Based Communication**: Core emits events, UI listens
- **Framework Agnostic**: Core can work with any framework
- **Test-Driven**: Core logic can be tested without UI

**Files Created**:
- `src/core/GrooveEngine.ts` - Main playback engine
- `src/core/DrumSynth.ts` - Audio synthesis
- `src/core/index.ts` - Public API
- `src/hooks/useGrooveEngine.ts` - React adapter
- `src/types.ts` - Shared TypeScript types
- `src/App.tsx` - Main React app (later moved to POC)
- `src/components/` - UI components (later moved to POC)
- `package.json` - Dependencies and scripts
- `tsconfig.json` - TypeScript config
- `vite.config.ts` - Vite config

**Commits**:
- `d2d9406` - Initial commit: Clean architecture with separated core logic
- `cc21e62` - Add Vite environment types
- `8f61a15` - Add getting started guide

**Impact**:
- Established clean architecture foundation
- Validated core engine with POC interface
- Proved separation of concerns works

**Deployment Notes**:
- Dev server: `npm run dev`
- Build: `npm run build`
- Type check: `npm run type-check`
- Lint: `npm run lint`

**Follow-ups**:
- Add more drum voices (toms, cymbals, percussion)
- Implement notation rendering
- Add MIDI import/export
- Build production UI

---

## Project Origin

**Background**: Groovy is a modern rewrite of GrooveScribe, built with clean architecture principles and framework-agnostic core logic.

**Related Projects**:
- **groove-poc** - Original proof-of-concept that validated the architecture
- **GrooveScribe** - Original implementation (legacy codebase)

**Repository**: https://github.com/AdarBahar/groovy
**Local Path**: `/Users/adar.bahar/Code/groovy`
