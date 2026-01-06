# Implementation Plan: Issue #10 – ABC Notation Transcoding & Rendering

## 1. Scope

### In Scope
- **Core ABC transcoding** – Convert `GrooveData` to valid ABC notation string
- **ABC rendering to SVG** – Display sheet music notation in the UI
- **URL encoding** – Compact drum-tab format for sharing patterns
- **Bi-directional support** – Parse URL/ABC back to `GrooveData`
- **UI component** – Sheet music display panel in POC

### Out of Scope
- MIDI export (future issue)
- Audio export (future issue)
- Printing/PDF generation (can be added later)
- Multi-measure patterns (single measure only in Phase 1)

### Assumptions
- ABC notation library (abcjs) will be used for SVG rendering
- Sheet music display is read-only initially (no clicking on notation to edit)
- Focus on hi-hat, snare, kick first; toms/cymbals can follow

---

## 2. Design

### 2.1 Architecture (Separation of Concerns)

```
┌─────────────────────────────────────────────────────────────────┐
│                         CORE LAYER                              │
│                    (src/core/, no React)                        │
├─────────────────────────────────────────────────────────────────┤
│  ABCTranscoder.ts          │  GrooveURLCodec.ts                 │
│  • GrooveData → ABC string │  • GrooveData → URL params         │
│  • ABC constants/mappings  │  • URL params → GrooveData         │
├─────────────────────────────────────────────────────────────────┤
│                     ABCRenderer.ts                               │
│  • ABC string → SVG (via abcjs)                                 │
│  • Error handling, render options                               │
└─────────────────────────────────────────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        REACT LAYER                               │
│                     (src/poc/components/)                        │
├─────────────────────────────────────────────────────────────────┤
│  SheetMusicDisplay.tsx     │  useABCNotation.ts (optional)      │
│  • React component for SVG │  • Debounced ABC generation        │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 ABC Symbol Mapping

| Drum Voice | URL Tab | ABC Symbol |
|------------|---------|------------|
| hihat-closed | `x` | `^g` |
| hihat-open | `o` | `!open!^g` |
| hihat-accent | `X` | `!accent!^g` |
| snare-normal | `o` | `c` |
| snare-accent | `O` | `!accent!c` |
| snare-ghost | `g` | `_c` |
| snare-cross-stick | `x` | `^c` |
| kick | `o` | `F` |

---

## 3. Task Breakdown

### Phase 1: Core ABC Transcoding
- **Task 1.1**: `ABCConstants.ts` – DrumVoice → ABC/URL mappings
- **Task 1.2**: `ABCTranscoder.ts` – `grooveToABC()` function
- **Task 1.3**: Triplet division support
- **Task 1.4**: `ABCTranscoder.test.ts` – Unit tests

### Phase 2: ABC Rendering
- **Task 2.1**: Install `abcjs` dependency
- **Task 2.2**: `ABCRenderer.ts` – Wrapper for abcjs
- **Task 2.3**: `ABCRenderer.test.ts` – Tests

### Phase 3: React UI
- **Task 3.1**: `SheetMusicDisplay.tsx` component
- **Task 3.2**: Integrate into `PocApp.tsx`

### Phase 4: URL Encoding (Optional)
- **Task 4.1**: `GrooveURLCodec.ts` for sharing

---

## 4. Rollout Plan
- No breaking changes to existing GrooveData or UI
- Sheet music display is additive (new panel, toggleable)
- Feature can be disabled via toggle

---

## 5. Testing Plan
- Unit tests for ABCTranscoder (all voices, time sigs, divisions)
- Unit tests for ABCRenderer (valid/invalid ABC)
- Integration: Full pipeline GrooveData → ABC → SVG
- Manual: Grid changes reflect in sheet music

---

## 6. Open Questions
1. **abcjs bundle size** – ~300KB, acceptable?
2. **Multi-voice vs single-staff** – Kick stems down?
3. **Click-to-edit notation** – Defer to future?
4. **Legacy URL format** – Match GrooveScribe exactly?

---

## 7. Dependencies
| Package | Version | Purpose |
|---------|---------|---------|
| `abcjs` | `^6.x` | ABC to SVG rendering |

---

## 8. Estimated Effort
| Phase | Estimate |
|-------|----------|
| Core transcoding | 2-3 hours |
| abcjs integration | 1-2 hours |
| React UI | 1-2 hours |
| URL codec | 1 hour (optional) |
| **Total** | **5-8 hours** |

