# Memory Log

Durable knowledge: decisions, patterns, "how we do things here", gotchas.

---

## Architecture Decisions

### Core Logic Separation (2025-12-XX)
**Decision**: Keep core engine (GrooveEngine, DrumSynth) completely framework-agnostic with ZERO React dependencies.

**Reasoning**:
- Makes core logic testable without UI
- Allows UI framework to be replaced without touching core
- Enables reuse across different frameworks (React, Vue, Svelte, vanilla JS)
- Future-proof architecture

**Pattern**:
- Core emits events (`playbackStateChange`, `positionChange`, `grooveChange`)
- UI listens to events and updates state
- UI calls core methods (`play()`, `stop()`, `updateGroove()`)
- Adapter layer (`useGrooveEngine` hook) bridges core and React

**Gotcha**: Never import React or any UI framework in `src/core/`. If you need to, you're doing it wrong.

---

### Dual-Page Setup (2025-12-XX)
**Decision**: Maintain separate POC testing page (`/poc`) and production UI (`/`) using React Router.

**Reasoning**:
- POC provides lightweight interface for testing core logic changes
- Production UI can be built incrementally without breaking testing workflow
- Clear separation between testing and production concerns
- POC serves as reference implementation

**Pattern**:
1. Make changes to core logic (`src/core/`)
2. Test on POC page (`/poc`) to verify functionality
3. Once validated, implement in production UI (`/`)

**Gotcha**: Don't delete the POC page - it's valuable for testing and validation.

---

### Event-Based Communication (2025-12-XX)
**Decision**: Use observer pattern for core-to-UI communication.

**Reasoning**:
- Decouples core from UI
- Allows multiple listeners (useful for debugging, analytics, etc.)
- Standard pattern for framework-agnostic code

**Pattern**:
```typescript
// Core emits events
engine.on('playbackStateChange', (isPlaying) => { ... });
engine.on('positionChange', (position) => { ... });
engine.on('grooveChange', (groove) => { ... });

// UI listens and updates state
const [isPlaying, setIsPlaying] = useState(false);
engine.on('playbackStateChange', setIsPlaying);
```

**Gotcha**: Always clean up listeners on unmount to prevent memory leaks.

---

## Development Patterns

### How We Handle Groove Updates During Playback
**Pattern**: Changes apply on next loop, not immediately.

**Implementation**:
- User edits groove → UI calls `engine.updateGroove(newGroove)`
- Engine stores as `pendingGroove`
- On loop completion, `pendingGroove` becomes `currentGroove`
- UI shows "pending changes" indicator during playback

**Reasoning**: Prevents jarring mid-loop changes, maintains musical timing.

---

### How We Load Drum Samples
**Pattern**: DrumSynth loads samples on construction, stores in Map.

**Implementation**:
```typescript
private sampleFiles: Record<DrumVoice, string> = {
  kick: 'Kick.mp3',
  snare: 'Snare Normal.mp3',
  hihat: 'Hi Hat Normal.mp3',
};
```

**Location**: `/public/sounds/` (30 samples available)

**Gotcha**: Web Audio API requires user interaction before playing audio. Always call `synth.resume()` on first user action.

---

### How We Calculate Swing
**Pattern**: Swing delays every other note by a percentage of the note duration.

**Implementation**:
- Swing 0% = straight timing
- Swing 50% = triplet feel
- Swing 100% = maximum delay
- Formula: `swingOffset = (position % 2 === 1) ? (swing / 100) : 0`

**Gotcha**: Swing only affects odd-numbered positions (1, 3, 5, etc.).

---

## Tech Stack Decisions

### Why Vite?
- Fast dev server with HMR
- Modern build tool optimized for React + TypeScript
- Better than Create React App (CRA is deprecated)

### Why TypeScript?
- Type safety prevents bugs
- Better IDE support and autocomplete
- Self-documenting code

### Why Web Audio API?
- Native browser support (no external dependencies)
- Precise timing for musical applications
- Sample-based playback with gain control

---

## Gotchas & Lessons Learned

### Web Audio Context Timing
**Issue**: `audioContext.currentTime` is in seconds, not milliseconds.

**Solution**: Always use seconds for timing calculations.

---

### React Router Basename
**Issue**: If deploying to subdirectory (e.g., GitHub Pages), routes may break.

**Solution**: Set `basename` prop on `<BrowserRouter>` if needed.

---

### Sample Loading Race Condition
**Issue**: User might click play before samples are loaded.

**Solution**: DrumSynth checks `isLoaded` flag before playing. Could add loading indicator in UI.

---

### Pending Groove Updates
**Issue**: User might forget changes are pending during playback.

**Solution**: Show visual indicator (`hasPendingChanges` state) in UI.

---

## File Organization

### Core Logic (`src/core/`)
- Framework-agnostic
- No React imports
- Pure TypeScript/JavaScript
- Event-based API

### Hooks (`src/hooks/`)
- React-specific adapters
- Bridge between core and UI
- Only place where React meets core

### POC (`src/poc/`)
- Testing interface
- Reference implementation
- Lightweight components

### Pages (`src/pages/`)
- Route components
- Minimal logic (delegate to components)

### Components (`src/components/`)
- Shared/production components
- Reusable UI elements

---

## Naming Conventions

- **Core classes**: PascalCase (GrooveEngine, DrumSynth)
- **React components**: PascalCase (DrumGrid, PlaybackControls)
- **Hooks**: camelCase with `use` prefix (useGrooveEngine)
- **Types**: PascalCase (GrooveData, DrumVoice)
- **Files**: Match export name (GrooveEngine.ts, useGrooveEngine.ts)

---

## Testing Strategy (Future)

### Core Logic
- Unit tests for GrooveEngine (timing, scheduling, loop management)
- Unit tests for DrumSynth (sample loading, playback)
- No UI dependencies needed

### UI Components
- Integration tests with React Testing Library
- Test user interactions (click, input changes)
- Mock core engine for predictable tests

### E2E
- Playwright or Cypress for full user flows
- Test on POC page first, then production

---

## Performance Notes

### Audio Scheduling
- Schedule notes 100ms ahead (`scheduleAheadTime = 0.1`)
- Use `setTimeout` for visual updates (less precise, but fine for UI)
- Use Web Audio API scheduling for audio (precise timing)

### React Rendering
- Use `useState` for UI state
- Use `useRef` for engine instance (doesn't trigger re-renders)
- Minimize re-renders by keeping state minimal

---

## Future Considerations

### Multi-Measure Support
- Will need to extend `GrooveData` type
- May need to refactor scheduling logic
- POC can test core changes first

### MIDI Export
- Core logic should generate MIDI events
- UI provides download button
- Keep MIDI generation in core (framework-agnostic)

### Notation Rendering
- Consider separate library (ABC.js, VexFlow)
- Keep rendering logic separate from core
- Core provides data, UI renders notation
