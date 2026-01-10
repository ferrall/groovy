# Groovy - Music Sequencer Application
## Complete Style Guide & UI Specification

---

## 1. Overview

**Groovy** is a drum/rhythm pattern sequencer web application designed for creating and editing drum patterns. The application features an interactive grid-based editor where users can compose rhythmic patterns across multiple measures with various note divisions.

### Core Features
- Interactive drum sequencer grid with 6 drum parts
- Multiple note division options (1/8, 1/16, 1/32, triplets, mixed)
- Multi-measure editing with add/duplicate/delete/clear operations
- Playback controls with tempo and swing adjustment
- Metronome configuration
- Pattern metadata (title, author, comments)
- Undo/redo functionality
- View modes (full editor vs. notes-only)
- Right-click context menus for drum sound selection
- Keyboard shortcuts

---

## 2. Color Palette

### Dark Theme (Primary)

#### Background Colors
- **Primary Background**: `#0f172a` (slate-900)
- **Secondary Background**: `#1e293b` (slate-800)
- **Tertiary Background**: `#334155` (slate-700)
- **Border Color**: `#475569` (slate-600)
- **Divider Color**: `#64748b` (slate-500)

#### Accent Colors
- **Primary Accent**: `#9333ea` (purple-600)
- **Primary Accent Hover**: `#7e22ce` (purple-700)
- **Primary Accent Light**: `#c084fc` (purple-400)
- **Primary Accent Lighter**: `#e9d5ff` (purple-200)

#### Text Colors
- **Primary Text**: `#ffffff` (white)
- **Secondary Text**: `#cbd5e1` (slate-300)
- **Tertiary Text**: `#94a3b8` (slate-400)
- **Muted Text**: `#64748b` (slate-500)

#### Status Colors
- **Success**: `#22c55e` (green-500)
- **Warning**: `#eab308` (yellow-500)
- **Error/Destructive**: `#ef4444` (red-400)
- **Error Hover**: `#fca5a5` (red-300)

#### Interactive States
- **Hover Background**: `#334155` (slate-700)
- **Active Background**: `#475569` (slate-600)
- **Focus Ring**: `#9333ea` with opacity

### Light Theme (Alternative)

#### Background Colors
- **Primary Background**: `#ffffff` (white)
- **Secondary Background**: `#f8fafc` (slate-50)
- **Tertiary Background**: `#f1f5f9` (slate-100)
- **Border Color**: `#e2e8f0` (slate-200)
- **Divider Color**: `#cbd5e1` (slate-300)

#### Accent Colors
- **Primary Accent**: `#9333ea` (purple-600)
- **Primary Accent Hover**: `#7e22ce` (purple-700)
- **Primary Accent Light**: `#a855f7` (purple-500)
- **Primary Accent Lighter**: `#c084fc` (purple-400)

#### Text Colors
- **Primary Text**: `#0f172a` (slate-900)
- **Secondary Text**: `#334155` (slate-700)
- **Tertiary Text**: `#64748b` (slate-500)
- **Muted Text**: `#94a3b8` (slate-400)

#### Status Colors
- **Success**: `#16a34a` (green-600)
- **Warning**: `#ca8a04` (yellow-600)
- **Error/Destructive**: `#dc2626` (red-600)
- **Error Hover**: `#ef4444` (red-500)

#### Interactive States
- **Hover Background**: `#f1f5f9` (slate-100)
- **Active Background**: `#e2e8f0` (slate-200)
- **Focus Ring**: `#9333ea` with opacity

---

## 3. Layout Structure

### Application Container
- **Width**: Full viewport width (100vw)
- **Height**: Full viewport height (100vh)
- **Background**: Dark theme primary background
- **Overflow**: Auto scroll

### Main Content Area
- **Max Width**: 1400px
- **Padding**: 24px (6 units)
- **Margin**: 0 auto (centered)

### Layout Sections (Top to Bottom)
1. **Header**: Logo and title
2. **Playback Controls**: Play/pause, tempo, swing, metronome
3. **Left Toolbar**: Time signature, note divisions, actions
4. **Metadata Section**: Collapsible title/author/comment fields
5. **Action Buttons Row**: Clear, Stickings, Toggle View
6. **Sheet Music Area**: Reserved for future notation display
7. **Drum Editor**: Multi-measure sequencer grid
8. **Keyboard Shortcuts Footer**: Help reference

---

## 4. Component Specifications

### 4.1 Header

#### Logo and Title
- **Layout**: Horizontal flex with gap of 12px (3 units)
- **Logo**: 
  - Music note icon (Lucide `Music4`)
  - Size: 32px (8 units)
  - Color: Purple-400 (`#c084fc`)
- **Title "Groovy"**:
  - Font size: 32px (2xl)
  - Font weight: Bold
  - Color: White
  - Letter spacing: Tight
- **Margin Bottom**: 24px (6 units)

---

### 4.2 Playback Controls

#### Container
- **Background**: Slate-800 (`#1e293b`)
- **Border**: 1px solid slate-700 (`#334155`)
- **Border Radius**: 12px (rounded-xl)
- **Padding**: 24px (6 units)
- **Margin Bottom**: 24px (6 units)

#### Play/Pause Button
- **Size**: 48px × 48px (12 units)
- **Background**: Purple-600 (`#9333ea`)
- **Hover Background**: Purple-700 (`#7e22ce`)
- **Border Radius**: 8px (rounded-lg)
- **Icon Size**: 24px (6 units)
- **Icon Color**: White
- **Transition**: All 150ms ease

#### Tempo Control
- **Label**: "Tempo (BPM)"
- **Layout**: Vertical stack
- **Slider**:
  - Width: 200px
  - Height: 6px
  - Track color: Slate-700
  - Thumb color: Purple-600
  - Thumb size: 16px × 16px
  - Hover thumb: Purple-500
- **Value Display**: 
  - Font size: 18px
  - Font weight: Semibold
  - Color: White
  - Position: Below slider
- **Range**: 40-240 BPM
- **Default**: 120 BPM

#### Swing Control
- **Label**: "Swing"
- **Slider**:
  - Width: 200px
  - Height: 6px
  - Track color: Slate-700
  - Thumb color: Purple-600
- **Value Display**: Percentage (0%-75%)
- **Default**: 0%

#### Metronome Settings
- **Container Background**: Slate-700 (`#334155`)
- **Border Radius**: 8px (rounded-lg)
- **Padding**: 16px (4 units)

**Enable Toggle**:
- **Type**: Checkbox
- **Label**: "Enable Metronome"
- **Unchecked**: Slate-600 border, transparent fill
- **Checked**: Purple-600 background, white checkmark
- **Size**: 20px × 20px

**Volume Slider**:
- **Width**: 120px
- **Same styling as tempo/swing sliders**
- **Range**: 0-100
- **Default**: 70

**Sound Dropdown**:
- **Width**: 140px
- **Background**: Slate-800
- **Border**: 1px slate-600
- **Border Radius**: 6px
- **Padding**: 8px 12px
- **Hover**: Slate-700 background
- **Options**: Beep, Click, Cowbell, Wood Block

---

### 4.3 Left Toolbar

#### Container
- **Position**: Floating on the left side
- **Background**: Slate-800 (`#1e293b`)
- **Border**: 1px solid slate-700
- **Border Radius**: 12px (rounded-xl)
- **Padding**: 24px (6 units)
- **Width**: Auto (content-based)
- **Margin Bottom**: 24px (6 units)

#### Time Signature Display
- **Background**: Slate-700 (`#334155`)
- **Border Radius**: 8px (rounded-lg)
- **Padding**: 12px (3 units)
- **Text**: "4/4"
- **Font Size**: 24px (2xl)
- **Font Weight**: Bold
- **Color**: Purple-400 (`#c084fc`)
- **Text Align**: Center
- **Margin Bottom**: 16px (4 units)

#### Section Divider
- **Height**: 1px
- **Background**: Slate-700
- **Margin**: 16px 0 (4 units vertical)

#### Note Division Section
**Label**:
- **Text**: "NOTE DIVISION"
- **Font Size**: 12px (xs)
- **Font Weight**: Semibold
- **Color**: Slate-400
- **Letter Spacing**: Wide
- **Margin Bottom**: 12px (3 units)

**Radio Buttons**:
- **Layout**: Vertical stack with 8px gap (2 units)
- **Each Option**:
  - Display: Flex row with 8px gap
  - Align: Center
  - Padding: 8px
  - Border Radius: 6px
  - Cursor: Pointer
  - Hover background: Slate-700
  - Transition: All 150ms

**Radio Circle**:
- **Size**: 18px × 18px
- **Border**: 2px solid slate-500
- **Border Radius**: 50% (full circle)
- **Unchecked**: Transparent fill
- **Checked**: Purple-600 border, white inner circle (8px)
- **Transition**: All 150ms

**Radio Labels**:
- **Font Size**: 14px (sm)
- **Color**: Slate-300
- **Options**: 
  - 1/8 Notes
  - 1/16 Notes
  - 1/32 Notes
  - Triplets (1/8)
  - Triplets (1/16)
  - Mixed (1/8 + 1/16)

#### Actions Section
**Label**: "ACTIONS"
- Same styling as "NOTE DIVISION" label

**Undo Button**:
- **Width**: Full width
- **Background**: Slate-700
- **Hover Background**: Slate-600
- **Disabled Background**: Slate-800
- **Border**: 1px slate-600
- **Border Radius**: 6px
- **Padding**: 8px 16px (2 units, 4 units)
- **Display**: Flex row with 8px gap
- **Icon**: Undo (Lucide `Undo2`) - 16px
- **Text**: "Undo"
- **Font Size**: 14px (sm)
- **Color**: White (slate-400 when disabled)
- **Cursor**: Pointer (not-allowed when disabled)
- **Transition**: All 150ms

**Redo Button**:
- Same styling as Undo button
- **Icon**: Redo (Lucide `Redo2`)
- **Text**: "Redo"

**View Mode Toggle**:
- **Background**: Purple-600 (`#9333ea`)
- **Hover Background**: Purple-700
- **Icon**: Eye (Lucide `Eye`)
- **Text**: "Switch to Notes Only"
- **Color**: White
- Same size/padding as Undo/Redo

---

### 4.4 Metadata Section

#### Container
- **Background**: Slate-800
- **Border**: 1px solid slate-700
- **Border Radius**: 12px
- **Padding**: 20px (5 units)
- **Margin Bottom**: 16px (4 units)

#### Toggle Button
- **Display**: Flex row with space-between
- **Width**: Full width
- **Align Items**: Center
- **Cursor**: Pointer
- **Hover**: Slight slate-700 background

**Title Text**:
- **Font Size**: 18px (lg)
- **Font Weight**: Semibold
- **Color**: White

**Chevron Icon**:
- **Size**: 20px (5 units)
- **Color**: Slate-400
- **Rotation**: 0deg (collapsed), 180deg (expanded)
- **Transition**: Transform 200ms ease

#### Expanded Content
- **Padding Top**: 16px (4 units)
- **Layout**: Vertical stack with 16px gaps

**Input Fields** (Title, Author):
- **Width**: Full width
- **Background**: Slate-700
- **Border**: 1px slate-600
- **Hover Border**: Slate-500
- **Focus Border**: Purple-600
- **Border Radius**: 8px
- **Padding**: 12px
- **Font Size**: 14px (sm)
- **Color**: White
- **Placeholder Color**: Slate-500
- **Transition**: Border 150ms

**Textarea** (Comment):
- Same styling as input fields
- **Height**: 80px (20 units)
- **Resize**: Vertical only

**Labels**:
- **Font Size**: 14px (sm)
- **Font Weight**: Medium
- **Color**: Slate-300
- **Margin Bottom**: 6px

---

### 4.5 Action Buttons Row

#### Container
- **Display**: Flex row
- **Gap**: 12px (3 units)
- **Margin Bottom**: 16px (4 units)
- **Flex Wrap**: Wrap

#### Clear Button
- **Background**: Red-400 (`#ef4444`)
- **Hover Background**: Red-300 (`#fca5a5`)
- **Border Radius**: 8px
- **Padding**: 10px 20px
- **Display**: Flex row with 8px gap
- **Align Items**: Center
- **Icon**: Trash (Lucide `Trash2`) - 18px
- **Text**: "Clear All Patterns"
- **Font Size**: 14px (sm)
- **Font Weight**: Medium
- **Color**: White
- **Cursor**: Pointer
- **Transition**: All 150ms

#### Stickings Button
- **Background**: Slate-700
- **Hover Background**: Slate-600
- **Same padding, border radius, icon size as Clear**
- **Icon**: Hand (Lucide `Hand`)
- **Text**: "Stickings"
- **Color**: White

#### Toggle View Button
- **Background**: Purple-600
- **Hover Background**: Purple-700
- **Icon**: Eye (Lucide `Eye`)
- **Text**: "Switch to Notes Only"
- **Same styling pattern as other buttons**

---

### 4.6 Sheet Music Area

#### Container
- **Background**: Slate-800
- **Border**: 2px dashed slate-600
- **Border Radius**: 12px
- **Padding**: 48px
- **Margin Bottom**: 24px (6 units)
- **Text Align**: Center

**Placeholder Content**:
- **Icon**: Music (Lucide `Music`) - 48px
- **Color**: Slate-600
- **Text**: "Sheet Music Area (Future Implementation)"
- **Font Size**: 16px
- **Color**: Slate-500
- **Margin Top**: 16px

---

### 4.7 Drum Editor - Measure Cards

#### Measures Container
- **Display**: Flex
- **Flex Wrap**: Wrap
- **Gap**: 24px (6 units) - space between measures

#### Individual Measure Card

**Container**:
- **Display**: Inline-block (width based on content)
- **Background**: Slate-800 (`#1e293b`)
- **Border**: 1px solid slate-700
- **Border Radius**: 12px (rounded-lg)
- **Padding**: 16px (4 units)

**Header Section**:
- **Display**: Flex row with space-between
- **Align Items**: Center
- **Margin Bottom**: 16px (4 units)

**Measure Title**:
- **Text**: "Measure [number]"
- **Font Size**: 18px (lg)
- **Font Weight**: Semibold
- **Color**: Purple-400 (`#c084fc`)

**Action Buttons Container**:
- **Display**: Flex row
- **Gap**: 8px (2 units)

**Action Buttons** (Clear, Duplicate, Add, Delete):
- **Size**: 32px × 32px (8 units)
- **Background**: Transparent
- **Hover Background**: Slate-700
- **Border Radius**: 6px (rounded)
- **Icon Size**: 16px (4 units)
- **Icon Color**: Slate-400 (Delete: red-400)
- **Hover Icon Color**: White (Delete: red-300)
- **Cursor**: Pointer
- **Transition**: Colors 150ms
- **Icons**:
  - Clear: Trash2
  - Duplicate: Copy
  - Add: Plus
  - Delete: X

#### Grid Structure

**Beat Labels Row**:
- **Display**: Flex row
- **Margin Bottom**: 8px (2 units)

**Empty Space** (for drum names column):
- **Width**: 96px (24 units)
- **Flex Shrink**: 0

**Beat Labels**:
- **Width**: 48px (12 units) per beat
- **Text Align**: Center
- **Font Size**: 12px (xs)
- **Color**: Slate-400
- **Font Weight**: Medium
- **Text**: "1", "2", "3", "4" (repeats based on note division)

**Beat Columns Calculation**:
- 1/8 Notes: 8 columns (beats 1-4, each divided into 2)
- 1/16 Notes: 16 columns (beats 1-4, each divided into 4)
- 1/32 Notes: 32 columns (beats 1-4, each divided into 8)
- Triplets (1/8): 12 columns (beats 1-4, each divided into 3)
- Triplets (1/16): 24 columns (beats 1-4, each divided into 6)
- Mixed (1/8 + 1/16): 28 columns (varies per beat)

#### Drum Rows

**Drum Parts** (in order):
1. Hi-Hat
2. Tom 1
3. Snare
4. Tom 2
5. Floor Tom
6. Kick

**Row Container**:
- **Display**: Flex row
- **Margin Bottom**: 4px (1 unit)

**Drum Name Label**:
- **Width**: 96px (24 units)
- **Flex Shrink**: 0
- **Padding**: 8px 12px (2 units, 3 units)
- **Display**: Flex
- **Align Items**: Center
- **Justify Content**: Flex-end (right-aligned)
- **Font Size**: 14px (sm)
- **Font Weight**: Medium
- **Color**: Slate-300 (`#cbd5e1`)
- **Background**: None
- **Border**: None

#### Drum Cells

**Cell Container**:
- **Width**: 48px (12 units)
- **Height**: 40px (10 units)
- **Border**: 1px solid slate-600
- **Cursor**: Pointer
- **Transition**: Colors 150ms

**Cell States**:

*Inactive (Default)*:
- **Background**: Slate-800 (`#1e293b`)
- **Hover Background**: Slate-700 (`#334155`)

*Active*:
- **Background**: Purple-600 (`#9333ea`)
- **Hover Background**: Purple-700 (`#7e22ce`)

**Cell Interactions**:
- **Left Click**: Toggle active/inactive
- **Right Click**: Open context menu (Hi-Hat only)

---

### 4.8 Context Menu (Right-Click)

#### Container
- **Position**: Fixed (at cursor position)
- **Background**: Slate-700 (`#334155`)
- **Border**: 1px solid slate-600
- **Border Radius**: 8px (rounded-md)
- **Box Shadow**: Large shadow (lg)
- **Padding**: 8px 0 (2 units vertical)
- **Min Width**: 200px
- **Z-Index**: 50

#### Header
- **Padding**: 4px 12px (1 unit, 3 units)
- **Font Size**: 12px (xs)
- **Font Weight**: Semibold
- **Color**: Slate-300
- **Border Bottom**: 1px solid slate-600
- **Margin Bottom**: 4px (1 unit)
- **Text**: "[Drum Part] - Select Sound"

#### Menu Items

**Container**:
- **Padding**: 8px 12px (2 units, 3 units)
- **Font Size**: 14px (sm)
- **Cursor**: Pointer
- **Display**: Flex row with space-between
- **Align Items**: Center
- **Transition**: All 150ms

**States**:
- **Default**: 
  - Background: Transparent
  - Text Color: Slate-200 (selected: purple-400)
- **Hover**:
  - Background: Slate-600 (`#475569`)
  - Text Color: Slate-200 (selected: purple-400)
- **Active** (clicking):
  - Background: Slate-500 (`#64748b`)

**Selected Indicator**:
- **Position**: Left side of text
- **Content**: "✓" checkmark
- **Color**: Purple-400
- **Font Size**: 14px

**Sound Key Display**:
- **Position**: Right side
- **Font Size**: 12px (xs)
- **Color**: Slate-400
- **Text**: Numeric key (e.g., "1", "2", "3")

**Hi-Hat Sound Options**:
- Open Hi-Hat (1)
- Closed Hi-Hat (2)
- Pedal Hi-Hat (3)

---

### 4.9 Clear Confirmation Dialog

#### Overlay
- **Position**: Fixed, full viewport
- **Background**: `rgba(0, 0, 0, 0.5)` (black with 50% opacity)
- **Z-Index**: 50
- **Display**: Flex
- **Justify Content**: Center
- **Align Items**: Center

#### Dialog Box
- **Background**: Slate-800
- **Border**: 1px solid slate-700
- **Border Radius**: 12px (rounded-xl)
- **Padding**: 24px (6 units)
- **Max Width**: 400px
- **Box Shadow**: Extra large (2xl)

**Title**:
- **Font Size**: 20px (xl)
- **Font Weight**: Semibold
- **Color**: White
- **Margin Bottom**: 12px (3 units)
- **Text**: "Clear All Patterns?"

**Message**:
- **Font Size**: 14px (sm)
- **Color**: Slate-300
- **Margin Bottom**: 24px (6 units)
- **Text**: "This will remove all drum patterns from all measures. This action cannot be undone."

**Buttons Container**:
- **Display**: Flex row
- **Gap**: 12px (3 units)
- **Justify Content**: Flex-end

**Cancel Button**:
- **Background**: Slate-700
- **Hover Background**: Slate-600
- **Border Radius**: 8px
- **Padding**: 10px 20px
- **Font Size**: 14px (sm)
- **Font Weight**: Medium
- **Color**: White
- **Cursor**: Pointer
- **Transition**: All 150ms

**Confirm Button**:
- **Background**: Red-400
- **Hover Background**: Red-300
- **Same styling as Cancel**
- **Text**: "Clear All"

---

### 4.10 Keyboard Shortcuts Footer

#### Container
- **Margin Top**: 32px (8 units)
- **Padding Top**: 24px (6 units)
- **Border Top**: 1px solid slate-700

**Title**:
- **Font Size**: 16px
- **Font Weight**: Semibold
- **Color**: Slate-300
- **Margin Bottom**: 12px (3 units)
- **Text**: "⌨️ Keyboard Shortcuts"

**Shortcuts Grid**:
- **Display**: Grid
- **Columns**: 2 columns (auto-fit, min 250px)
- **Gap**: 8px (2 units)

**Shortcut Item**:
- **Display**: Flex row with space-between
- **Padding**: 8px 0
- **Border Bottom**: 1px solid slate-700 (last item: none)

**Shortcut Key**:
- **Background**: Slate-700
- **Border**: 1px solid slate-600
- **Border Radius**: 4px (rounded)
- **Padding**: 4px 8px (1 unit, 2 units)
- **Font Size**: 12px (xs)
- **Font Family**: Monospace
- **Color**: Purple-300
- **Font Weight**: Medium

**Shortcut Description**:
- **Font Size**: 14px (sm)
- **Color**: Slate-400

**Shortcuts List**:
- Space: Play/Pause
- Cmd+Z / Ctrl+Z: Undo
- Cmd+Shift+Z / Ctrl+Y: Redo
- Cmd+K / Ctrl+K: Clear All
- Delete / Backspace: Clear Selected
- 1-6: Select Note Division
- V: Toggle View Mode

---

## 5. Responsive Behavior

### Breakpoints
- **Mobile**: < 640px
- **Tablet**: 640px - 1024px
- **Desktop**: > 1024px

### Mobile Adjustments (< 640px)
- Left toolbar moves below playback controls
- Metadata section starts collapsed
- Measure cards stack vertically (full width)
- Playback controls stack vertically
- Tempo/swing sliders reduce to 160px width
- Drum cell width reduces to 40px
- Font sizes reduce by 10-15%

### Tablet Adjustments (640px - 1024px)
- Measure cards may wrap to 2 per row
- Left toolbar remains in original position
- Playback controls remain horizontal

---

## 6. Interactions & Behaviors

### 6.1 Drum Cell Interactions

**Left Click**:
- Action: Toggle cell active/inactive state
- Visual: Immediate color change (slate-800 ↔ purple-600)
- Sound: Play drum sound (future implementation)
- State persistence: Updates measure data

**Right Click** (Hi-Hat only):
- Action: Open context menu
- Position: Menu appears at cursor position
- Visual: Context menu with 3 sound options
- Click outside: Closes menu

### 6.2 Measure Actions

**Clear Measure**:
- Icon: Trash2
- Action: Remove all active cells in measure
- Confirmation: None (quick action)
- Visual: All cells revert to inactive state

**Duplicate Measure**:
- Icon: Copy
- Action: Create new measure after current with same pattern
- Position: New measure appears immediately after current
- Visual: Smooth appearance with same pattern

**Add Measure**:
- Icon: Plus
- Action: Create new empty measure after current
- Position: Appears after current measure
- Visual: Empty grid with default 4/4 structure

**Delete Measure**:
- Icon: X
- Action: Remove measure from sequence
- Constraint: Cannot delete if only 1 measure remains
- Visual: Measure disappears, remaining measures reflow

### 6.3 Note Division Changes

**Action**: Select different note division radio button
**Effect**: 
- All measures regenerate grids with new column count
- Existing patterns are cleared
- Beat labels update to match new division
- Grid width adjusts automatically

**Column Counts**:
- 1/8 Notes: 8 columns
- 1/16 Notes: 16 columns
- 1/32 Notes: 32 columns
- Triplets (1/8): 12 columns
- Triplets (1/16): 24 columns
- Mixed: 28 columns

### 6.4 Playback Controls

**Play/Pause Button**:
- Click: Toggle between play and pause states
- Icon: Play (triangle) / Pause (two bars)
- Visual: Icon switches, button remains purple
- Behavior: Starts/stops pattern playback

**Tempo Slider**:
- Drag: Adjust BPM from 40 to 240
- Visual: Value updates in real-time below slider
- Effect: Changes playback speed

**Swing Slider**:
- Drag: Adjust swing from 0% to 75%
- Visual: Percentage displays below slider
- Effect: Adds rhythmic swing to playback

**Metronome Toggle**:
- Click: Enable/disable metronome
- Visual: Checkbox checked/unchecked
- Effect: Metronome plays during playback when enabled

### 6.5 Undo/Redo

**Undo (Cmd+Z / Ctrl+Z)**:
- Action: Revert last change
- States: Enabled when history exists, disabled when at start
- Visual: Button grays out when disabled
- History: Cell toggles, measure additions/deletions, clears

**Redo (Cmd+Shift+Z / Ctrl+Y)**:
- Action: Reapply undone change
- States: Enabled after undo, disabled when at latest state
- Visual: Button grays out when disabled

### 6.6 View Modes

**Full Editor Mode**:
- Shows: All controls, metadata, sheet music area, drum editor
- Button text: "Switch to Notes Only"

**Notes Only Mode**:
- Shows: Only drum editor measures
- Hides: Playback controls, metadata, left toolbar, sheet music
- Button text: "Switch to Full Editor"
- Purpose: Focus on pattern editing

### 6.7 Clear All

**Trigger**: Click "Clear All Patterns" button
**Flow**:
1. Confirmation dialog appears with overlay
2. User must click "Clear All" to confirm
3. All cells in all measures set to inactive
4. Dialog closes
5. Undo history records this action

**Cancel**: Click "Cancel" or click outside dialog to abort

---

## 7. Typography

### Font Family
- **Primary**: System font stack (default sans-serif)
- **Monospace**: Used for keyboard shortcut keys

### Font Sizes
- **3xl**: 32px - Logo, main title
- **2xl**: 24px - Time signature
- **xl**: 20px - Dialog titles
- **lg**: 18px - Section headers, measure titles
- **base**: 16px - Body text, buttons
- **sm**: 14px - Labels, input fields
- **xs**: 12px - Hints, helper text, beat labels

### Font Weights
- **Bold**: 700 - Logo, time signature
- **Semibold**: 600 - Section titles, measure numbers
- **Medium**: 500 - Button text, labels
- **Normal**: 400 - Body text, placeholders

### Line Heights
- **Tight**: 1.25 - Headers
- **Normal**: 1.5 - Body text
- **Relaxed**: 1.625 - Long-form text

---

## 8. Spacing System

### Base Unit: 4px (0.25rem)

**Spacing Scale**:
- 0.5: 2px
- 1: 4px
- 2: 8px
- 3: 12px
- 4: 16px
- 5: 20px
- 6: 24px
- 8: 32px
- 10: 40px
- 12: 48px
- 16: 64px
- 20: 80px
- 24: 96px

**Common Applications**:
- **Gaps between elements**: 8px (2), 12px (3), 16px (4), 24px (6)
- **Padding - Small**: 8px 12px
- **Padding - Medium**: 12px 16px
- **Padding - Large**: 16px 24px
- **Margin - Section**: 24px (6)
- **Margin - Component**: 16px (4)

---

## 9. Border Radius

- **sm**: 4px - Small elements, keyboard shortcuts
- **md**: 6px - Buttons, radio buttons, inputs
- **lg**: 8px - Cards, panels, action buttons
- **xl**: 12px - Major sections, measure cards
- **full**: 50% - Radio buttons, circular elements

---

## 10. Shadows

### Box Shadows
- **sm**: `0 1px 2px 0 rgba(0, 0, 0, 0.05)` - Subtle elevation
- **md**: `0 4px 6px -1px rgba(0, 0, 0, 0.1)` - Default cards
- **lg**: `0 10px 15px -3px rgba(0, 0, 0, 0.1)` - Context menus
- **xl**: `0 20px 25px -5px rgba(0, 0, 0, 0.1)` - Dialogs
- **2xl**: `0 25px 50px -12px rgba(0, 0, 0, 0.25)` - Modal overlays

### Focus States
- **Focus Ring**: `0 0 0 3px rgba(147, 51, 234, 0.5)` - Purple-600 with 50% opacity

---

## 11. Transitions

### Default Timing
- **Duration**: 150ms
- **Easing**: ease (cubic-bezier(0.25, 0.1, 0.25, 1.0))

### Specific Transitions
- **Colors/Background**: `all 150ms ease`
- **Transform**: `transform 200ms ease` (chevron rotations)
- **Opacity**: `opacity 200ms ease`

---

## 12. Z-Index Layers

- **Base Content**: 0
- **Sticky Elements**: 10
- **Dropdown Menus**: 20
- **Context Menus**: 50
- **Modal Overlays**: 50
- **Tooltips**: 60

---

## 13. Icons

### Library: Lucide React

**Icon Sizes**:
- **Small**: 16px (4 units) - Action buttons in measures
- **Medium**: 18px (4.5 units) - Text-adjacent icons
- **Large**: 20px (5 units) - Chevrons, larger buttons
- **XLarge**: 24px (6 units) - Play/pause button
- **2XL**: 32px (8 units) - Logo
- **3XL**: 48px (12 units) - Placeholder areas

**Icons Used**:
- Music4: Logo
- Play: Playback
- Pause: Pause playback
- Trash2: Clear/delete actions
- Copy: Duplicate measure
- Plus: Add measure
- X: Close/delete
- Undo2: Undo action
- Redo2: Redo action
- Eye: Toggle view
- Hand: Stickings
- ChevronDown: Collapsible sections
- Music: Sheet music placeholder

---

## 14. States & Animations

### Button States

**Default → Hover → Active**:
```
Slate button:
  slate-700 → slate-600 → slate-500

Purple button:
  purple-600 → purple-700 → purple-800

Red button:
  red-400 → red-300 → red-200
```

### Input States

**Default → Hover → Focus**:
```
Border:
  slate-600 → slate-500 → purple-600

Background:
  slate-700 (no change on hover/focus)
```

### Cell States

**Inactive → Hover → Active → Active Hover**:
```
slate-800 → slate-700 → purple-600 → purple-700
```

### Disabled States
- **Opacity**: 0.5
- **Cursor**: not-allowed
- **Color**: slate-400 (from slate-300)
- **Background**: slate-800 (from slate-700)

---

## 15. Accessibility

### Keyboard Navigation
- All interactive elements are keyboard accessible (Tab, Enter, Space)
- Focus visible with purple ring
- Keyboard shortcuts for common actions

### ARIA Labels
- Buttons have descriptive labels
- Icons have sr-only text for screen readers
- Form inputs have associated labels

### Color Contrast
- All text meets WCAG AA standards
- Minimum contrast ratio: 4.5:1 (normal text), 3:1 (large text)
- Purple accent has sufficient contrast against dark backgrounds

### Focus Management
- Clear focus indicators
- Logical tab order
- Focus trap in dialogs

---

## 16. Data Structure

### Measure Data Structure
```javascript
{
  id: string (unique identifier),
  beats: {
    "Hi-Hat": [
      { active: boolean, sound?: number },
      { active: boolean, sound?: number },
      // ... one per beat column
    ],
    "Tom 1": [...],
    "Snare": [...],
    "Tom 2": [...],
    "Floor Tom": [...],
    "Kick": [...]
  }
}
```

### Note Division Mapping
```javascript
{
  "1/8": 8 columns,
  "1/16": 16 columns,
  "1/32": 32 columns,
  "triplets-8": 12 columns,
  "triplets-16": 24 columns,
  "mixed": 28 columns
}
```

---

## 17. Animation Details

### Collapsible Sections
- **Property**: max-height
- **Duration**: 200ms
- **Easing**: ease-in-out
- **Behavior**: Smooth expand/collapse

### Context Menu
- **Entry**: Fade in (opacity 0 → 1, 150ms)
- **Exit**: Fade out (opacity 1 → 0, 100ms)
- **Position**: Instant at cursor location

### Dialog
- **Overlay**: Fade in (200ms)
- **Box**: Scale + fade (0.95 → 1.0, opacity 0 → 1, 200ms)
- **Exit**: Reverse of entry

### Button Presses
- **Active State**: 150ms transition
- **Visual feedback**: Background darkens immediately

---

## 18. Performance Considerations

### Render Optimization
- Use React.memo for drum cells (prevent unnecessary rerenders)
- Virtualize long measure lists if > 20 measures
- Debounce slider changes (tempo/swing)

### State Management
- Local state for UI interactions
- Context/props for measure data
- Separate undo/redo history stack

### Event Handling
- Event delegation for drum grid cells
- Prevent default on right-click
- Click outside handlers for menus/dialogs

---

## 19. Future Enhancements

### Planned Features
- Sheet music notation display
- Audio playback engine
- Save/load patterns
- Export to MIDI
- Pattern library/presets
- Stickings notation
- Multiple pattern tracks
- Real-time collaboration

### Technical Debt
- Implement actual Web Audio API playback
- Add unit tests for state management
- Improve mobile touch interactions
- Add drag selection for multiple cells
- Implement copy/paste between measures

---

## 20. Implementation Notes

### Technology Stack
- **Framework**: React 18+
- **Styling**: Tailwind CSS v4
- **Icons**: Lucide React
- **Build Tool**: Vite
- **Language**: TypeScript

### Key Libraries
- React hooks for state management
- No external audio library yet (future: Tone.js or Web Audio API)
- No animation library (using CSS transitions)

### File Structure
```
/src
  /app
    App.tsx (main component)
    /components
      DrumEditor.tsx
      DrumMeasure.tsx
      DrumCell.tsx
      PlaybackControls.tsx
      LeftToolbar.tsx
      MetadataSection.tsx
      ClearConfirmDialog.tsx
      KeyboardShortcuts.tsx
  /styles
    theme.css
    fonts.css
```

### Browser Support
- Modern browsers (Chrome, Firefox, Safari, Edge)
- ES6+ features required
- No IE11 support

---

## 21. Color Usage Summary

### Primary Interactions
- **Purple-600**: Primary actions, selected states, accents
- **Purple-400**: Headers, highlights, checked indicators
- **Slate-800**: Card backgrounds, input backgrounds
- **Slate-700**: Secondary backgrounds, hover states

### Semantic Colors
- **Red-400**: Destructive actions (clear, delete)
- **Green-500**: Success states (future)
- **Yellow-500**: Warning states (future)

### Neutrals
- **White**: Primary text, icons
- **Slate-300**: Secondary text, labels
- **Slate-400**: Tertiary text, placeholders
- **Slate-500**: Muted text, borders
- **Slate-600**: Dividers, inactive borders

---

## 22. Component State Management

### DrumEditor (Parent)
- Manages array of measures
- Handles add/delete/duplicate measure
- Passes note division to children
- Manages undo/redo history

### DrumMeasure (Child)
- Receives measure data as props
- Handles cell toggle events
- Manages local context menu state
- Passes events up to parent

### DrumCell (Grandchild)
- Pure presentational component
- Handles click/right-click events
- Shows context menu locally
- Emits events to parent

---

This comprehensive style guide provides all necessary information to recreate the Groovy music sequencer application. It covers design specifications, interaction patterns, color schemes for both dark and light themes, component structures, and implementation details.
