# Groovy POC - UI Elements Reference

Complete inventory of UI elements for the final UI design.

---

## Layout Structure

The page is organized into:
1. **Header** - Title and subtitle (top, centered)
2. **Main Content Area** - White card containing:
   - Controls Section (top)
   - Grid Section (middle)
   - Shortcuts Footer (bottom)

---

## Element Catalog

### 1. App Header

| Property | Value |
|----------|-------|
| **Description** | Displays app title "🥁 GrooveScribe POC" and subtitle |
| **Location** | Top of page, centered, full width |
| **Size** | Title: 3rem (2rem mobile), Subtitle: 1.2rem |
| **Can be hidden** | ❌ No (but can be simplified/minimized) |

---

### 2. TimeSignatureSelector

| Property | Value |
|----------|-------|
| **Description** | Button showing current time signature (e.g., 4/4). Opens popup with beat/note-value selectors |
| **Location** | Controls section, row 1, left side |
| **Size** | Button: ~60px wide. Popup: ~200px wide, auto height |
| **Can be hidden** | ⚠️ Optional - could be in settings |

---

### 3. PresetSelector

| Property | Value |
|----------|-------|
| **Description** | Dropdown to select built-in groove presets (Basic Rock, Disco, Funk, etc.) |
| **Location** | Controls section, row 1, middle |
| **Size** | Min-width: 200px |
| **Can be hidden** | ✅ Yes - could collapse into menu |

---

### 4. PlaybackControls

| Property | Value |
|----------|-------|
| **Description** | Play/Pause button + Play with Speed Up button + Config gear button |
| **Location** | Controls section, row 1, right side |
| **Size** | Play button: min-width 140px, height ~50px. Speed Up button: similar. Config: 24px |
| **Can be hidden** | ❌ No (core functionality) |

**Sub-elements:**
- **Play/Pause Button**: Purple gradient, 16px padding, shows "▶ Play" or "⏸ Pause"
- **Speed Up Button**: Green gradient, shows "▶+ Speed Up" or "⏸ Stop"
- **Config Gear Button**: Small ⚙ button to open speed-up config

---

### 5. AutoSpeedUpConfig

| Property | Value |
|----------|-------|
| **Description** | Expandable panel with sliders: Step BPM (1-20), Interval (1-10 min), Keep Going checkbox |
| **Location** | Controls section, row 2 (conditional - only shown when config button clicked) |
| **Size** | Full width of controls row, height: ~180px |
| **Can be hidden** | ✅ Yes - toggled by config button |

---

### 6. AutoSpeedUpIndicator

| Property | Value |
|----------|-------|
| **Description** | Shows speed-up status: current increment, interval, countdown to next, total increased |
| **Location** | Controls section, row 2/3 (conditional - only shown when auto speed up active) |
| **Size** | Full width, height: ~40px |
| **Can be hidden** | ✅ Yes - only shows during auto speed up playback |

---

### 7. DivisionSelector

| Property | Value |
|----------|-------|
| **Description** | Row of buttons for note divisions (8ths, 16ths, 32nds, Triplets). Shows info about current selection |
| **Location** | Controls section, row 3 |
| **Size** | Full width, buttons ~60-80px each. Info panel: ~200px wide |
| **Can be hidden** | ⚠️ Optional - could be in settings, but useful for quick changes |

---

### 8. TempoControl

| Property | Value |
|----------|-------|
| **Description** | Two sliders: Tempo (40-240 BPM) and Swing (0-60%). Shows labels with current values |
| **Location** | Controls section, row 4, left side |
| **Size** | Min-width: 250px, flexible width |
| **Can be hidden** | ❌ No (core functionality) |

---

### 9. SyncControl (Visual Sync Mode)

| Property | Value |
|----------|-------|
| **Description** | Three buttons: Start/Middle/End - controls when visual highlight appears relative to sound |
| **Location** | Controls section, row 4, middle |
| **Size** | ~180px wide, ~120px tall |
| **Can be hidden** | ✅ Yes - advanced setting, most users won't need |

---

### 10. SyncOffsetControl (A/V Sync)

| Property | Value |
|----------|-------|
| **Description** | Slider (-200ms to +200ms) to adjust visual timing. Has enable/disable toggle and reset button |
| **Location** | Controls section, row 4, right side |
| **Size** | ~200px wide, ~80px tall |
| **Can be hidden** | ✅ Yes - advanced setting for latency compensation |

---

### 11. EditModeToggle

| Property | Value |
|----------|-------|
| **Description** | Toggle switch between Simple Mode and Advanced Mode. Shows mode description and keyboard hint |
| **Location** | Grid section header, left side |
| **Size** | ~200px wide, ~60px tall |
| **Can be hidden** | ⚠️ Optional - could be in settings |

---

### 12. UndoRedoControls

| Property | Value |
|----------|-------|
| **Description** | Two buttons with Font Awesome icons: Undo (fa-undo) and Redo (fa-repeat). Disabled when no actions |
| **Location** | Grid section header, after edit mode toggle |
| **Size** | ~120px wide (two buttons ~60px each), ~36px tall |
| **Can be hidden** | ❌ No (essential for editing) |

---

### 13. Sheet Music Toggle Button

| Property | Value |
|----------|-------|
| **Description** | Button "🎼 Show/Hide Notation" to toggle sheet music display |
| **Location** | Grid section header, middle-right |
| **Size** | ~140px wide, ~36px tall |
| **Can be hidden** | ❌ No (controls major feature) |

---

### 14. ShareButton

| Property | Value |
|----------|-------|
| **Description** | Button "🔗 Share" that copies URL to clipboard. Shows "✓ Copied!" for 2 seconds after click |
| **Location** | Grid section header, right side |
| **Size** | ~80px wide, ~36px tall |
| **Can be hidden** | ⚠️ Optional - nice to have visible |

---

### 15. MetadataEditor

| Property | Value |
|----------|-------|
| **Description** | Three input fields: Title (100 chars), Author (50 chars), Notes (500 chars textarea) |
| **Location** | Grid section, below header, above sheet music |
| **Size** | Full width, ~80-100px tall. Responsive: stacks on mobile |
| **Can be hidden** | ✅ Yes - can be collapsed or in a modal |

---

### 16. SheetMusicDisplay

| Property | Value |
|----------|-------|
| **Description** | Renders ABC notation as SVG. Shows playback cursor during playback. Multi-line for longer grooves (3 measures per line) |
| **Location** | Grid section, above drum grid |
| **Size** | Max-width: 780px, min-height: 100px, auto height based on measures |
| **Can be hidden** | ✅ Yes - toggled by sheet music button |

**Sub-elements:**
- **Playback Cursor**: 2px wide vertical line, moves horizontally during playback

---

### 17. DrumGrid (Main Editor)

| Property | Value |
|----------|-------|
| **Description** | Interactive grid with 6 rows (Hi-Hat, Tom 1, Snare, Tom 2, Floor Tom, Kick) × N positions per measure. Multiple measures displayed horizontally |
| **Location** | Grid section, main content area |
| **Size** | Min-width per measure: 600px. Cell size: 30×30px (desktop), 40-44×44px (mobile/touch) |
| **Can be hidden** | ❌ No (core UI element) |

**Sub-elements:**

#### Measure Container
- Header with measure number and action buttons
- **Measure Actions** (per measure):
  - 🗑️ Clear (24×24px)
  - 📋 Duplicate (24×24px)
  - ➕ Add measure (24×24px)
  - ❌ Remove (24×24px)

#### Grid Header Row
- Voice label header ("Drum")
- Count labels (1, e, &, a, 2, e, &, a, etc.)
- Downbeat labels highlighted

#### Drum Rows (6 total)
- **Voice Label Button**: 80×30px, clickable for bulk operations
- **Note Cells**: 30×30px min, flex-grow. States: empty, active, current (playing)

#### Context Menu (on right-click/advanced mode)
- Min-width: 140px
- Header with row name
- List of articulation options with keyboard shortcuts

#### Bulk Operations Dialog (on voice label click)
- Modal overlay
- ~300px wide, auto height
- Pattern buttons, custom patterns section

---

### 18. NoteIcon

| Property | Value |
|----------|-------|
| **Description** | Font Awesome icons showing articulation type (normal=circle, accent=exclamation, ghost=circle-o, etc.) |
| **Location** | Inside active note cells in DrumGrid |
| **Size** | Inherits from cell, typically 0.7-1rem |
| **Can be hidden** | ❌ No (shows note articulations) |

---

### 19. ShortcutsFooter

| Property | Value |
|----------|-------|
| **Description** | Horizontal bar showing all keyboard shortcuts: Space, E, Ctrl+drag, Alt+drag, ⌘Z, ⌘⇧Z |
| **Location** | Bottom of main content area |
| **Size** | Full width, ~50px tall, uses `<kbd>` styled elements |
| **Can be hidden** | ✅ Yes - could be in a help modal instead |

---

## Summary Table

| # | Component | Location | Essential | Can Hide | Mobile Considerations |
|---|-----------|----------|-----------|----------|----------------------|
| 1 | Header | Top | ✅ | ❌ | Reduce to single line |
| 2 | TimeSignatureSelector | Controls R1 | ⚠️ | ✅ | Keep visible |
| 3 | PresetSelector | Controls R1 | ⚠️ | ✅ | Move to menu |
| 4 | PlaybackControls | Controls R1 | ✅ | ❌ | Simplify |
| 5 | AutoSpeedUpConfig | Controls R2 | ⚠️ | ✅ | Fullscreen modal |
| 6 | AutoSpeedUpIndicator | Controls R2/3 | ⚠️ | ✅ | Compact version |
| 7 | DivisionSelector | Controls R3 | ⚠️ | ✅ | Dropdown instead |
| 8 | TempoControl | Controls R4 | ✅ | ❌ | Keep visible |
| 9 | SyncControl | Controls R4 | ⚠️ | ✅ | Hide in settings |
| 10 | SyncOffsetControl | Controls R4 | ⚠️ | ✅ | Hide in settings |
| 11 | EditModeToggle | Grid header | ⚠️ | ✅ | Keep visible |
| 12 | UndoRedoControls | Grid header | ✅ | ❌ | Icons only |
| 13 | SheetMusicToggle | Grid header | ⚠️ | ❌ | Icon only |
| 14 | ShareButton | Grid header | ⚠️ | ⚠️ | Icon only |
| 15 | MetadataEditor | Above sheet | ⚠️ | ✅ | Collapse by default |
| 16 | SheetMusicDisplay | Above grid | ⚠️ | ✅ | Horizontal scroll |
| 17 | DrumGrid | Main | ✅ | ❌ | Horizontal scroll, larger cells |
| 18 | NoteIcon | In cells | ✅ | ❌ | Scale with cells |
| 19 | ShortcutsFooter | Bottom | ⚠️ | ✅ | Help button instead |

---

## Sizing Constraints

| Element Type | Min Width | Max Width | Min Height | Touch Target |
|--------------|-----------|-----------|------------|--------------|
| Button (action) | 36px | — | 36px | 44×44px |
| Input (text) | 100px | 300px | 32px | 44×44px |
| Slider | 150px | 300px | 32px | Track: 8px tall |
| DrumGrid cell | 30px | — | 30px | 44×44px (mobile) |
| Measure | 600px | — | — | — |
| Sheet Music | 300px | 780px | 100px | — |
| Controls row | — | 1200px | — | — |

---

## Color Palette (Current)

| Element | Color |
|---------|-------|
| Primary gradient | `linear-gradient(135deg, #667eea 0%, #764ba2 100%)` |
| Play button active | `linear-gradient(135deg, #f093fb 0%, #f5576c 100%)` |
| Speed Up button | `linear-gradient(135deg, #11998e 0%, #38ef7d 100%)` |
| Background | `linear-gradient(135deg, #667eea 0%, #764ba2 100%)` |
| Card background | `#ffffff` |
| Downbeat highlight | `#e8eeff` |
| Active note cell | Purple gradient |
| Disabled state | 40% opacity |

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Space` | Play/Pause |
| `E` | Toggle Edit Mode |
| `Ctrl/⌘` + drag | Paint notes |
| `Shift/Alt` + drag | Erase notes |
| `Ctrl/⌘` + `Z` | Undo |
| `Ctrl/⌘` + `Shift` + `Z` | Redo |

---

## Responsive Breakpoints

| Breakpoint | Width | Behavior |
|------------|-------|----------|
| Desktop | > 1200px | Full layout, horizontal measures |
| Tablet | 768-1200px | Stacked measures |
| Mobile | < 768px | Simplified controls, larger touch targets (44×44px) |
| Touch devices | hover: none | 44×44px min touch targets |

