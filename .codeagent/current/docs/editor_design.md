# Groovy Drum Editor - Complete Specification

## 1. Overview

The Drum Editor is an interactive grid-based sequencer for creating drum patterns across multiple measures. Users can click cells to add/remove drum hits, manage measures, and adjust note divisions to create complex rhythmic patterns.

---

## 2. Visual Structure

### 2.1 Editor Container

**Layout**:
- **Display**: Flex container with wrap
- **Gap**: 24px (6 units) between measures
- **Flex Wrap**: Enabled - measures flow horizontally then wrap to next row
- **Margin Top**: 24px from previous section
- **Background**: Inherits from parent (slate-900)

**Behavior**:
- Measures display inline, wrapping when reaching container width
- Each measure card is self-contained and independent
- Consistent spacing maintained both horizontally and vertically between cards

---

## 3. Measure Card Structure

### 3.1 Measure Card Container

**Visual Properties**:
- **Display**: `inline-block` (width based on content)
- **Background**: Slate-800 (`#1e293b`)
- **Border**: 1px solid slate-700 (`#334155`)
- **Border Radius**: 12px (rounded-lg)
- **Padding**: 16px (4 units)
- **Box Shadow**: None (flat design)

**Sizing**:
- Width automatically adjusts to content (grid width + padding)
- Width = (drum label column width) + (beat columns × 48px) + (32px padding)
- Height = header (48px) + grid height + padding

**Example Widths**:
- 1/8 Notes: ~520px (96px label + 8×48px + 32px)
- 1/16 Notes: ~904px (96px label + 16×48px + 32px)
- 1/32 Notes: ~1672px (96px label + 32×48px + 32px)

---

### 3.2 Measure Header

**Container**:
- **Display**: Flex row with space-between
- **Align Items**: Center
- **Margin Bottom**: 16px (4 units)
- **Height**: ~32px

#### Measure Title

**Visual**:
- **Text**: "Measure [number]" (e.g., "Measure 1", "Measure 2")
- **Font Size**: 18px (lg)
- **Font Weight**: Semibold (600)
- **Color**: Purple-400 (`#c084fc`)
- **Line Height**: 1.5

**Behavior**:
- Number increments sequentially (1, 2, 3...)
- Updates automatically when measures are added/removed
- Non-interactive (display only)

#### Action Buttons Container

**Layout**:
- **Display**: Flex row
- **Gap**: 8px (2 units)
- **Align Items**: Center

**Contains 4 buttons**: Clear, Duplicate, Add, Delete

---

### 3.3 Measure Action Buttons

Each button shares base styling with specific icons and behaviors:

#### Base Button Style

**Visual**:
- **Size**: 32px × 32px (8 units square)
- **Background**: Transparent
- **Border**: None
- **Border Radius**: 6px (rounded)
- **Cursor**: Pointer
- **Padding**: 0 (icon centered)
- **Transition**: All 150ms ease

**States**:
- **Default**: Transparent background
- **Hover**: Slate-700 (`#334155`) background
- **Active**: Slate-600 (`#475569`) background (brief flash on click)

**Icon Properties**:
- **Size**: 16px (4 units)
- **Stroke Width**: 2px
- **Default Color**: Slate-400 (`#94a3b8`)
- **Hover Color**: White (`#ffffff`)

---

#### Clear Measure Button

**Icon**: Trash2 (trash can)
**Tooltip**: "Clear measure"

**Behavior**:
- **Action**: Removes all active drum hits from this measure
- **Confirmation**: None (instant action)
- **Effect**: All cells in measure change from active to inactive state
- **Visual Feedback**: All purple cells instantly become slate-800
- **Undo**: This action is added to undo history
- **Disabled State**: Never disabled (always can clear)

**When Clicked**:
1. Iterate through all drum parts (6 rows)
2. Set all beat cells to `{ active: false }`
3. Clear any sound selections
4. Push state to undo history
5. Re-render measure with all inactive cells

---

#### Duplicate Measure Button

**Icon**: Copy (two overlapping squares)
**Tooltip**: "Duplicate measure"

**Behavior**:
- **Action**: Creates a new measure immediately after the current one
- **Content**: New measure is an exact copy of current measure's pattern
- **Position**: Inserted directly after current measure in sequence
- **Effect**: All drum hits and sound selections are copied
- **Numbering**: All subsequent measures renumber automatically

**When Clicked**:
1. Clone current measure data (all drum parts, beat states, sounds)
2. Generate new unique ID for cloned measure
3. Insert cloned measure after current measure in array
4. Push action to undo history
5. Re-render editor with new measure appearing with 24px gap
6. Scroll slightly to show new measure if needed

**Visual Flow**:
```
Before: [Measure 1] [Measure 2] [Measure 3]
Click duplicate on Measure 1
After:  [Measure 1] [Measure 2 - copy of 1] [Measure 3 - was 2] [Measure 4 - was 3]
```

---

#### Add Measure Button

**Icon**: Plus (+ symbol)
**Tooltip**: "Add measure"

**Behavior**:
- **Action**: Creates a new empty measure after the current one
- **Content**: All cells start inactive (empty pattern)
- **Position**: Inserted directly after current measure
- **Default State**: All drum parts have all beats set to inactive

**When Clicked**:
1. Create new measure object with empty pattern:
   ```javascript
   {
     id: generateUniqueId(),
     beats: {
       "Hi-Hat": Array(columnCount).fill({ active: false }),
       "Tom 1": Array(columnCount).fill({ active: false }),
       "Snare": Array(columnCount).fill({ active: false }),
       "Tom 2": Array(columnCount).fill({ active: false }),
       "Floor Tom": Array(columnCount).fill({ active: false }),
       "Kick": Array(columnCount).fill({ active: false })
     }
   }
   ```
2. Insert after current measure
3. Push to undo history
4. Re-render with new empty measure

---

#### Delete Measure Button

**Icon**: X (close/delete symbol)
**Tooltip**: "Delete measure"
**Icon Color**: Red-400 (`#ef4444`) - different from other buttons
**Hover Icon Color**: Red-300 (`#fca5a5`)

**Behavior**:
- **Action**: Removes this measure from the sequence
- **Confirmation**: None (instant action)
- **Constraint**: Cannot delete if only 1 measure remains
- **Effect**: Measure disappears, subsequent measures reflow and renumber

**When Clicked**:
1. Check if more than 1 measure exists
2. If only 1 measure: Do nothing (or show tooltip/disable)
3. If multiple measures:
   - Remove measure from measures array
   - Push deletion to undo history
   - Re-render editor
   - Remaining measures flow left to fill gap

**Disabled State** (when only 1 measure):
- **Cursor**: not-allowed
- **Opacity**: 0.5
- **Icon Color**: Red-400 (but muted)
- **No hover effect**
- **Click does nothing**

---

## 4. Grid Structure

### 4.1 Grid Container

**Layout**:
- **Display**: Inline-block
- **Background**: Transparent
- **Padding**: 0

**Contains**:
1. Beat labels row (header)
2. 6 drum part rows (one per drum instrument)

---

### 4.2 Beat Labels Row

**Container**:
- **Display**: Flex row
- **Margin Bottom**: 8px (2 units)
- **Align Items**: Center

#### Empty Space (Left Column)

**Purpose**: Aligns with drum name labels
**Dimensions**:
- **Width**: 96px (24 units)
- **Flex Shrink**: 0 (fixed width)
- **Content**: Empty

#### Beat Label Cells

**Visual**:
- **Width**: 48px (12 units) per beat label
- **Height**: Auto (text height)
- **Text Align**: Center
- **Font Size**: 12px (xs)
- **Font Weight**: Medium (500)
- **Color**: Slate-400 (`#94a3b8`)
- **Line Height**: 1.5

**Content Pattern**:
- Labels show beat numbers: "1", "2", "3", "4"
- Pattern repeats based on note division subdivisions
- For 1/16 notes: "1 1 1 1 | 2 2 2 2 | 3 3 3 3 | 4 4 4 4" (16 total)
- For 1/8 notes: "1 1 | 2 2 | 3 3 | 4 4" (8 total)
- For triplets: Each beat divided into 3 or 6 parts

**Example Label Sequences**:

*1/8 Notes (8 columns)*:
```
1  1  |  2  2  |  3  3  |  4  4
```

*1/16 Notes (16 columns)*:
```
1  1  1  1  |  2  2  2  2  |  3  3  3  3  |  4  4  4  4
```

*1/32 Notes (32 columns)*:
```
1  1  1  1  1  1  1  1  |  2  2  2  2  2  2  2  2  |  3... |  4...
```

*Triplets 1/8 (12 columns)*:
```
1  1  1  |  2  2  2  |  3  3  3  |  4  4  4
```

*Triplets 1/16 (24 columns)*:
```
1  1  1  1  1  1  |  2  2  2  2  2  2  |  3... |  4...
```

*Mixed (28 columns)*:
```
1  1  1  1  1  1  1  |  2  2  2  2  2  2  2  |  3... |  4...
(varies per beat - combination of divisions)
```

---

## 5. Drum Rows

### 5.1 Drum Parts Order

The 6 drum parts appear in this exact order (top to bottom):

1. **Hi-Hat** - Top row
2. **Tom 1** - Second row
3. **Snare** - Third row (center)
4. **Tom 2** - Fourth row
5. **Floor Tom** - Fifth row
6. **Kick** - Bottom row

### 5.2 Drum Row Container

**Per Row**:
- **Display**: Flex row
- **Align Items**: Center
- **Margin Bottom**: 4px (1 unit)
- **Height**: 40px (10 units) - matches cell height

**Last Row**:
- **Margin Bottom**: 0 (no extra space after Kick row)

---

### 5.3 Drum Name Label Column

**Visual**:
- **Width**: 96px (24 units)
- **Flex Shrink**: 0 (fixed width)
- **Padding**: 8px 12px (2 units vertical, 3 units horizontal)
- **Display**: Flex
- **Align Items**: Center
- **Justify Content**: Flex-end (right-aligned text)
- **Background**: None (transparent)
- **Border**: None
- **Font Size**: 14px (sm)
- **Font Weight**: Medium (500)
- **Color**: Slate-300 (`#cbd5e1`)
- **Text Align**: Right
- **Line Height**: 1.5

**Behavior**:
- Non-interactive (label only)
- No hover state
- No click interaction
- Simply identifies the drum row

**Content**:
- Displays drum part name as plain text
- Names: "Hi-Hat", "Tom 1", "Snare", "Tom 2", "Floor Tom", "Kick"

---

## 6. Drum Cells

### 6.1 Cell Structure

**Dimensions**:
- **Width**: 48px (12 units)
- **Height**: 40px (10 units)
- **Border**: 1px solid slate-600 (`#475569`)
- **Border Radius**: 0 (sharp corners)
- **Cursor**: Pointer
- **Transition**: All 150ms ease

**Layout in Row**:
- Cells placed consecutively after drum label
- No gap between cells (borders touch)
- Number of cells = number of beat columns for current note division

---

### 6.2 Cell States & Colors

#### Inactive State (Default)

**Visual**:
- **Background**: Slate-800 (`#1e293b`)
- **Border**: 1px solid slate-600 (`#475569`)
- **Content**: Empty (no indicator)

**Hover**:
- **Background**: Slate-700 (`#334155`)
- **Border**: Same (slate-600)
- **Cursor**: Pointer
- **Transition**: Smooth background change (150ms)

**Active (Clicking)**:
- **Background**: Briefly flashes slate-600 during click
- **Duration**: ~100ms

---

#### Active State (Drum Hit Placed)

**Visual**:
- **Background**: Purple-600 (`#9333ea`)
- **Border**: 1px solid slate-600 (same as inactive)
- **Content**: Solid color fill (no icon or text)

**Hover**:
- **Background**: Purple-700 (`#7e22ce`)
- **Border**: Same
- **Cursor**: Pointer
- **Effect**: Darker purple on hover

**Active (Clicking)**:
- **Background**: Briefly flashes purple-800 during click

---

### 6.3 Cell Interactions

#### Left Click (Primary Interaction)

**Action**: Toggle cell state (inactive ↔ active)

**Behavior**:

*When Inactive Cell is Clicked*:
1. Cell immediately changes to active state (purple-600)
2. State updates in measure data: `{ active: true }`
3. Visual feedback is instant (no delay)
4. Action added to undo history
5. Future: Play drum sound sample

*When Active Cell is Clicked*:
1. Cell immediately changes to inactive state (slate-800)
2. State updates: `{ active: false }`
3. If cell had sound selection, it's cleared
4. Visual feedback is instant
5. Action added to undo history

**State Updates**:
```javascript
// Toggle logic
onCellClick(drumPart, beatIndex) {
  const currentState = measure.beats[drumPart][beatIndex].active;
  measure.beats[drumPart][beatIndex] = {
    active: !currentState,
    sound: currentState ? undefined : previousSound
  };
  updateMeasure(measure);
  addToUndoHistory();
}
```

**Visual Transition**:
- Duration: 150ms
- Easing: ease
- Properties: background-color

---

#### Right Click (Context Menu - Hi-Hat Only)

**Trigger**: Right-click on any Hi-Hat cell (active or inactive)

**Behavior**:
1. Prevent default browser context menu (`e.preventDefault()`)
2. Check if drum part has sound options (only Hi-Hat does)
3. If sound options exist:
   - Calculate cursor position (clientX, clientY)
   - Open context menu at cursor location
   - Menu appears instantly (no animation)
4. If no sound options: Do nothing (right-click behaves like left-click)

**Other Drum Parts**:
- Tom 1, Snare, Tom 2, Floor Tom, Kick: Right-click does nothing
- No context menu appears
- Could implement sound options in future

---

## 7. Context Menu (Hi-Hat Sound Selection)

### 7.1 Menu Container

**Position**:
- **Type**: Fixed positioning
- **Left**: Cursor X position (`e.clientX`)
- **Top**: Cursor Y position (`e.clientY`)
- **Z-Index**: 50 (above all grid content)

**Visual**:
- **Background**: Slate-700 (`#334155`)
- **Border**: 1px solid slate-600 (`#475569`)
- **Border Radius**: 8px (rounded-md)
- **Box Shadow**: `0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05)` (large shadow)
- **Padding**: 8px 0 (2 units vertical)
- **Min Width**: 200px
- **Max Width**: 300px

**Behavior**:
- Appears instantly on right-click
- Positioned to avoid viewport edges (if near edge, adjust position)
- Remains visible until: menu item clicked, or click outside, or Escape pressed

---

### 7.2 Menu Header

**Visual**:
- **Padding**: 4px 12px (1 unit vertical, 3 units horizontal)
- **Font Size**: 12px (xs)
- **Font Weight**: Semibold (600)
- **Color**: Slate-300 (`#cbd5e1`)
- **Border Bottom**: 1px solid slate-600
- **Margin Bottom**: 4px (1 unit)
- **Background**: Transparent

**Content**:
- Text: "[Drum Part] - Select Sound"
- Example: "Hi-Hat - Select Sound"
- Static (non-interactive)

---

### 7.3 Menu Items

Each sound option is a clickable menu item:

**Container**:
- **Padding**: 8px 12px (2 units vertical, 3 units horizontal)
- **Font Size**: 14px (sm)
- **Font Weight**: Normal (400)
- **Cursor**: Pointer
- **Display**: Flex row with space-between
- **Align Items**: Center
- **Transition**: All 150ms ease
- **Background**: Transparent (default)

**States**:

*Default (Not Selected)*:
- **Background**: Transparent
- **Text Color**: Slate-200 (`#e2e8f0`)
- **No indicator**: Checkmark hidden

*Default (Currently Selected)*:
- **Background**: Transparent
- **Text Color**: Purple-400 (`#c084fc`)
- **Indicator**: White checkmark ("✓") on left

*Hover (Not Selected)*:
- **Background**: Slate-600 (`#475569`)
- **Text Color**: Slate-200
- **Transition**: Smooth background fade-in

*Hover (Currently Selected)*:
- **Background**: Slate-600
- **Text Color**: Purple-400
- **Checkmark**: Remains visible

*Active/Clicking*:
- **Background**: Slate-500 (`#64748b`) - lighter flash
- **Duration**: ~100ms
- **Then**: Menu closes and state updates

---

### 7.4 Menu Item Content

**Layout**: Two parts - left and right

#### Left Side (Name + Checkmark)

**Container**:
- **Display**: Flex row
- **Gap**: 8px (2 units)
- **Align Items**: Center

**Checkmark Indicator**:
- **Content**: "✓" (Unicode check mark)
- **Font Size**: 14px (sm)
- **Color**: Purple-400 (`#c084fc`)
- **Display**: Only when this option is currently selected
- **Position**: Before sound name

**Sound Name**:
- **Font Size**: 14px (sm)
- **Color**: Slate-200 (or purple-400 if selected)
- **Text**: Option name (e.g., "Open Hi-Hat")

#### Right Side (Key Number)

**Visual**:
- **Font Size**: 12px (xs)
- **Color**: Slate-400 (`#94a3b8`)
- **Text**: Numeric key (e.g., "1", "2", "3")
- **Purpose**: Shows keyboard shortcut or MIDI note number

---

### 7.5 Hi-Hat Sound Options

The three sound options available:

| Option | Key | Description |
|--------|-----|-------------|
| Open Hi-Hat | 1 | Fully open cymbal sound (bright, sustained) |
| Closed Hi-Hat | 2 | Closed cymbal sound (tight, short) - DEFAULT |
| Pedal Hi-Hat | 3 | Foot pedal press sound (chick sound) |

**Default Selection**:
- When first activating a Hi-Hat cell: "Closed Hi-Hat" (key 2)
- Most common in drum patterns
- Users can change via right-click menu

---

### 7.6 Menu Interactions

#### Selecting a Sound

**Click on Menu Item**:
1. Update cell state with selected sound key:
   ```javascript
   measure.beats["Hi-Hat"][beatIndex] = {
     active: true, // ensure cell is active
     sound: selectedKey // 1, 2, or 3
   }
   ```
2. Close context menu (fade out)
3. Update visual: Selected item shows checkmark next time
4. Add to undo history
5. Future: Play sound preview on selection

**Visual Feedback**:
- Clicked item flashes slate-500 background briefly
- Menu disappears (instant, no animation)
- Cell remains purple (active state)
- No visible change to cell appearance (sound stored internally)

---

#### Closing the Menu

**Methods to Close**:

1. **Click Menu Item**: Selects sound and closes
2. **Click Outside Menu**: Closes without change
3. **Click on Another Cell**: Closes and handles that cell's click
4. **Press Escape**: Closes without change (future enhancement)
5. **Scroll Page**: Menu remains fixed to viewport (doesn't scroll with content)

**Close Behavior**:
- Menu disappears instantly
- No fade-out animation
- State returns to normal (showContextMenu = false)
- Re-clicking same cell shows menu again with updated selection

---

## 8. Note Division Impact on Grid

### 8.1 Column Count by Division

When note division changes, entire grid regenerates:

| Division | Columns | Beat Subdivision |
|----------|---------|------------------|
| 1/8 Notes | 8 | 2 per beat (4 beats) |
| 1/16 Notes | 16 | 4 per beat |
| 1/32 Notes | 32 | 8 per beat |
| Triplets (1/8) | 12 | 3 per beat |
| Triplets (1/16) | 24 | 6 per beat |
| Mixed | 28 | Variable per beat |

### 8.2 Grid Regeneration Behavior

**When Note Division Changes**:

1. **All Measures Clear**: Existing patterns are lost (warn user in future)
2. **Column Count Updates**: Grid recalculates number of cells per row
3. **Beat Labels Update**: Label pattern changes to match new division
4. **Cell Array Regenerates**: Each drum part gets new array of cells
5. **Width Adjusts**: Measure cards resize to fit new column count
6. **Layout Reflows**: Measures may wrap differently based on new widths

**Initialization Per Measure**:
```javascript
// Generate empty beat array for new division
const emptyBeats = {};
drumParts.forEach(drumPart => {
  emptyBeats[drumPart] = Array(newColumnCount).fill({ active: false });
});
```

**Visual Transition**:
- Instant change (no animation)
- All cells reset to inactive (slate-800)
- Grid width changes immediately
- Measures reflow to fit new widths

---

### 8.3 Mixed Division Pattern

**Special Case**: "Mixed (1/8 + 1/16)"

**Beat Structure** (28 columns total):
- Beat 1: 8 subdivisions (1/32 spacing)
- Beat 2: 8 subdivisions
- Beat 3: 6 subdivisions (1/16 triplet spacing)
- Beat 4: 6 subdivisions

**Purpose**: Allows complex polyrhythmic patterns within one measure

**Beat Labels**:
```
1  1  1  1  1  1  1  1  |  2  2  2  2  2  2  2  2  |  3  3  3  3  3  3  |  4  4  4  4  4  4
   (beat 1 - 8 cols)           (beat 2 - 8 cols)        (beat 3 - 6)        (beat 4 - 6)
```

---

## 9. Multi-Measure Behavior

### 9.1 Initial State

**Default on Load**:
- **Measure Count**: 1 measure
- **Content**: Empty (all cells inactive)
- **Note Division**: 1/8 Notes (8 columns)
- **Position**: Single measure card centered

### 9.2 Adding Measures

**Methods to Add**:
1. Click "Add Measure" button on any existing measure
2. Click "Duplicate Measure" to add with copied pattern

**Positioning Logic**:
- New measure appears immediately after source measure
- Example: Add from Measure 2 → new measure becomes Measure 3
- All subsequent measures renumber (3→4, 4→5, etc.)
- Measures flow left-to-right, wrapping when container width exceeded

### 9.3 Measure Flow & Wrapping

**Horizontal Flow**:
- Measures display inline-block with 24px gaps
- Flow left-to-right until container edge reached
- When measure would overflow: Wraps to new row
- New row starts flush left with same alignment

**Example Layout** (1/16 notes, ~900px per measure, 1400px container):
```
[Measure 1]  24px gap  [Measure 2]
[Measure 3]  24px gap  [Measure 4]
[Measure 5]
```

**Vertical Spacing**:
- 24px gap between rows (same as horizontal gap)
- Consistent spacing in all directions

### 9.4 Measure Deletion

**Behavior**:
- Click delete on any measure (if > 1 measure exists)
- Measure immediately disappears
- Remaining measures shift left to fill gap
- All measures renumber sequentially
- Cannot delete if only 1 measure remains (delete button disabled)

**Example**:
```
Before: [Measure 1] [Measure 2] [Measure 3] [Measure 4]
Delete Measure 2
After:  [Measure 1] [Measure 2 - was 3] [Measure 3 - was 4]
```

---

## 10. State Management & Data Flow

### 10.1 Data Structure

**Measure Object**:
```javascript
{
  id: "measure-uuid-12345",
  beats: {
    "Hi-Hat": [
      { active: true, sound: 2 },   // Beat 1, closed hi-hat
      { active: false },              // Beat 2, no hit
      { active: true, sound: 1 },   // Beat 3, open hi-hat
      // ... continues for all columns
    ],
    "Tom 1": [
      { active: false },
      { active: false },
      // ... all beats for Tom 1
    ],
    "Snare": [ /* ... */ ],
    "Tom 2": [ /* ... */ ],
    "Floor Tom": [ /* ... */ ],
    "Kick": [ /* ... */ ]
  }
}
```

**Sound Key Values**:
- `undefined` or not present: Default sound
- `1`: Open Hi-Hat
- `2`: Closed Hi-Hat
- `3`: Pedal Hi-Hat

### 10.2 Event Flow

**Cell Toggle**:
```
User clicks cell
  ↓
DrumCell: onClick event
  ↓
DrumMeasure: onCellToggle handler
  ↓
DrumEditor: handleCellToggle(measureId, drumPart, beatIndex)
  ↓
Update measures state array
  ↓
Push to undo history
  ↓
React re-renders affected measure
  ↓
Cell displays new state (purple or slate)
```

**Sound Selection**:
```
User right-clicks Hi-Hat cell
  ↓
DrumCell: onContextMenu event
  ↓
Context menu displays at cursor
  ↓
User clicks sound option
  ↓
DrumCell: handleSoundSelect(soundKey)
  ↓
DrumMeasure: onSoundChange handler
  ↓
DrumEditor: handleSoundChange(measureId, drumPart, beatIndex, soundKey)
  ↓
Update cell with { active: true, sound: soundKey }
  ↓
Close context menu
  ↓
Push to undo history
```

**Measure Actions**:
```
User clicks measure action button
  ↓
DrumMeasure: on[Action]Measure handler
  ↓
DrumEditor: handle[Action]Measure(measureId)
  ↓
Modify measures array (add/remove/duplicate)
  ↓
Push to undo history
  ↓
React re-renders entire editor
  ↓
Measures reflow with updated list
```

---

## 11. Keyboard Interactions (Future)

### 11.1 Planned Shortcuts

**Cell Manipulation**:
- Arrow Keys: Navigate between cells
- Space: Toggle selected cell
- Number Keys (1-3): Set Hi-Hat sound on selected cell
- Delete/Backspace: Clear selected cell

**Selection**:
- Click + Shift: Multi-select cells
- Cmd/Ctrl + A: Select all in measure
- Escape: Clear selection

**Measure Operations**:
- Cmd/Ctrl + D: Duplicate selected measure
- Cmd/Ctrl + N: Add new measure
- Delete (with measure selected): Delete measure

---

## 12. Accessibility Features

### 12.1 Current Implementation

**Keyboard Access**:
- Tab: Navigate between measure action buttons
- Enter/Space: Activate focused button
- Tab through all interactive elements in logical order

**Screen Reader Support**:
- Measure titles announced (e.g., "Measure 1")
- Button labels with aria-labels or title attributes
- Drum part names announced for each row

**Visual Indicators**:
- Focus visible on all interactive elements
- High contrast between active/inactive states
- Clear hover states for all clickable elements

### 12.2 Future Enhancements

- Keyboard navigation within grid
- Screen reader announcements for cell state changes
- ARIA live regions for dynamic updates
- Keyboard shortcuts help dialog
- High contrast mode support

---

## 13. Performance Considerations

### 13.1 Optimization Strategies

**Render Optimization**:
- React.memo on DrumCell component (pure presentational)
- Memoize drum part arrays to prevent recreation
- Use stable keys (measure.id, beatIndex) for list rendering
- Avoid inline function definitions in render (use useCallback)

**State Updates**:
- Batch state updates when possible
- Use immer for immutable state updates
- Debounce rapid cell toggles (prevent double-click issues)

**Large Grids**:
- 1/32 notes = 32 columns × 6 rows × N measures = 192N cells
- For 10 measures: 1,920 cells rendered
- Virtualization not needed until 20+ measures
- Consider lazy loading measures if > 50

---

## 14. Responsive Behavior

### 14.1 Desktop (> 1024px)

**Layout**:
- Measures flow horizontally with wrapping
- Multiple measures per row (depending on note division)
- Full 48px cell width
- 24px gaps between measures

**Interaction**:
- Hover states fully functional
- Right-click context menus
- Smooth transitions on all interactions

### 14.2 Tablet (640px - 1024px)

**Layout**:
- Fewer measures per row (more wrapping)
- 1/32 notes may require horizontal scroll per measure
- Same cell sizes and gaps
- Measure cards may reduce padding slightly

**Interaction**:
- Touch-friendly button sizes (32px minimum)
- Long-press for context menu (instead of right-click)
- Hover states replaced with active states

### 14.3 Mobile (< 640px)

**Layout**:
- One measure per row (full width)
- Horizontal scroll within each measure for wide grids
- Reduced cell width: 40px instead of 48px
- Reduced gaps: 16px instead of 24px
- Drum label column: 80px instead of 96px

**Interaction**:
- Larger touch targets for buttons (40px minimum)
- Long-press for context menu
- Simplified transitions (fewer animations)
- Sticky measure headers when scrolling within measure

**Font Adjustments**:
- Measure title: 16px (from 18px)
- Drum labels: 12px (from 14px)
- Beat labels: 11px (from 12px)

---

## 15. Error States & Edge Cases

### 15.1 Single Measure

**Behavior**:
- Delete button disabled (opacity 0.5, cursor not-allowed)
- Clear button still functional
- Add/Duplicate buttons always functional
- No error message shown (disabled state is clear)

### 15.2 Maximum Measures

**Limit**: None (theoretically unlimited)
**Practical Limit**: ~50 measures before performance impact
**Future**: Consider pagination or virtual scrolling

### 15.3 Grid Width Overflow

**1/32 Notes on Small Screens**:
- Measure width: ~1672px
- If container < 1672px: Horizontal scroll within measure
- Measure remains inline-block
- Scroll bar appears at measure level (not page level)

**Solution**:
- Add `overflow-x: auto` to measure container
- Sticky drum label column (remains visible while scrolling beats)

---

## 16. Visual Examples

### 16.1 Color Coding Summary

**Inactive Cell**: `#1e293b` (slate-800) → Hover: `#334155` (slate-700)
**Active Cell**: `#9333ea` (purple-600) → Hover: `#7e22ce` (purple-700)
**Borders**: `#475569` (slate-600)
**Measure Card**: `#1e293b` (slate-800) with `#334155` (slate-700) border
**Measure Title**: `#c084fc` (purple-400)
**Drum Labels**: `#cbd5e1` (slate-300)
**Beat Labels**: `#94a3b8` (slate-400)
**Action Buttons**: Slate-400 icons → White on hover
**Delete Button**: Red-400 icon → Red-300 on hover
**Context Menu**: Slate-700 background, slate-600 border

### 16.2 Spacing Summary

**Between Measures**: 24px (6 units)
**Cell Size**: 48px × 40px (W × H)
**Cell Borders**: 1px
**Measure Padding**: 16px (4 units)
**Header Margin**: 16px (4 units) below
**Row Gap**: 4px (1 unit) between drum rows
**Button Gap**: 8px (2 units) between action buttons
**Drum Label Width**: 96px (24 units)

---

## 17. Future Enhancements

### 17.1 Planned Features

**Grid Enhancements**:
- Drag selection (select multiple cells)
- Copy/paste patterns between measures
- Velocity control (cell color intensity)
- Ghost notes (low velocity indicators)
- Accents (strong hit indicators)
- Flams and other rudiments

**Measure Features**:
- Measure length control (3/4, 5/4, 6/8, etc.)
- Measure labels/names
- Color coding measures
- Loop selected measures
- Swing per measure

**Interaction Improvements**:
- Undo/redo with visual history
- Pattern presets library
- Randomize pattern
- Humanize (add timing variation)
- Quantize (snap to grid)

**Visualization**:
- Playhead indicator during playback
- Visual metronome flash
- Waveform display per drum part
- Mini-map for long sequences

---

This specification provides complete details for recreating the drum editor component with all visual properties, interactions, behaviors, and state management patterns.
