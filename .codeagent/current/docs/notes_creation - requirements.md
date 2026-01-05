# Note Creation and Drum Part Mapping – Requirements

## 1. Overview

This document specifies how GrooveScribe creates and manages **notes on the editor grid**, including:

- How user actions (clicks, drags, menus) create or modify notes.
- How notes are **visually represented** in the editor for each drum part.
- How visual states map to **logical note states** that drive playback, export (ABC), and URL encoding.

This spec is based on the current implementation in:

- `js/groove_writer.js` – editor grid HTML, event handlers, and state readers/writers.
- `js/groove_utils.js` – scaling note arrays and generating MIDI/ABC.

---

## 2. Editor Grid Model

### 2.1 Measures and Note Indices

- Each measure is rendered via `HTMLforStaffContainer(...)` in `js/groove_writer.js`.
- Internally, each **clickable column** in the editor has a 1‑based index `i`:
  - `i` ranges from `indexStartForNotes` up to `indexStartForNotes + class_notes_per_measure - 1` per measure.
  - Across multiple measures, `i` continues to increase (no reset per measure).

### 2.2 Instrument Rows

For each measure, the following rows are rendered (from top to bottom):

- **Hi-hat / Cymbals** – `.hi-hat-container`
- **Tom 1** – `.toms-container` with `id="tom1-container"`
- **Tom 2** – `.toms-container` with `id="tom2-container"`
- **Snare** – `.snare-container`
- **Floor Tom (Tom 4)** – `.toms-container` with `id="tom4-container"`
- **Kick** – `.kick-container`

Each row has **one clickable cell per note index**. IDs follow a predictable pattern, e.g.:

- Hi-hat cell: `div#hi-hat{i}` with class `"hi-hat"`.
- Tom 1 cell: `div#tom1-{i}` with class `"tom"`.
- Tom 2 cell: `div#tom2-{i}` with class `"tom"`.
- Tom 4 cell: `div#tom4-{i}` with class `"tom"`.
- Snare cell: `div#snare{i}` with class `"snare"`.
- Kick cell: `div#kick{i}` with class `"kick"`.

Each cell is wired with event handlers:

- `onClick="myGrooveWriter.noteLeftClick(event, '<instrument>', i)"`
- `oncontextmenu="event.preventDefault(); myGrooveWriter.noteRightClick(event, '<instrument>', i)"`
- `onmouseenter="myGrooveWriter.noteOnMouseEnter(event, '<instrument>', i)"`

### 2.3 Per‑Cell Sub‑Elements (note parts)

Inside each clickable cell are one or more `.note_part` elements representing **specific articulations** for that drum part. Examples:

- **Hi-hat / Cymbals** (`div#hi-hat{i}`):
  - `.hh_crash` – `<i class="fa fa-asterisk"></i>`
  - `.hh_ride` – `<i class="fa fa-dot-circle-o"></i>`
  - `.hh_ride_bell` – `<i class="fa fa-bell-o"></i>`
  - `.hh_cow_bell` – `<i class="fa fa-plus-square-o"></i>`
  - `.hh_stacker` – `<i class="fa fa-bars"></i>`
  - `.hh_metronome_normal` – `<i class="fa fa-neuter"></i>`
  - `.hh_metronome_accent` – `<i class="fa fa-map-pin"></i>`
  - `.hh_cross` – `<i class="fa fa-times"></i>`
  - `.hh_open` – `<i class="fa fa-circle-o"></i>`
  - `.hh_close` – `<i class="fa fa-plus"></i>`
  - `.hh_accent` – `<i class="fa fa-angle-right"></i>`

- **Tom rows** (`div#tom1-{i}`, `div#tom2-{i}`, `div#tom4-{i}`):
  - `.tom_circle` – a filled circle representing a tom hit.

- **Snare row** (`div#snare{i}`):
  - `.snare_ghost` – `(<i class="fa fa-circle dot_in_snare_ghost_note"></i>)`
  - `.snare_circle` – main snare hit.
  - `.snare_xstick` – `<i class="fa fa-times"></i>` (cross‑stick).
  - `.snare_buzz` – `<i class="fa fa-bars"></i>` (buzz/roll).
  - `.snare_flam` – inline SVG flam glyph.
  - `.snare_drag` – inline SVG drag glyph.
  - `.snare_accent` – `<i class="fa fa-chevron-right"></i>` accent indicator.

- **Kick row** (`div#kick{i}`):
  - `.kick_splash` – `<i class="fa fa-times"></i>` (hi‑hat foot splash marker).
  - `.kick_circle` – main kick hit.

**Requirement:**  
Each instrument cell must maintain a **single visual representation at a time** (or “off”), determined by which `.note_part` elements are styled as “on”.

---

## 3. Note Creation and Editing Interactions

### 3.1 Basic Left‑Click (non‑advanced mode)

Handler: `root.noteLeftClick(event, type, id)` in `js/groove_writer.js`.

- If `class_advancedEditIsOn === false`, left‑click is a **simple toggle** for that instrument at index `id`:

  ```js
  switch (type) {
    case "hh":
      set_hh_state(id, is_hh_on(id) ? "off" : "normal", true);
      break;
    case "snare":
      set_snare_state(id, is_snare_on(id) ? "off" : "accent", true);
      break;
    case "tom1":
      set_tom_state(id, 1, is_tom_on(id, 1) ? "off" : "normal", true);
      break;
    case "tom2":
      set_tom_state(id, 2, is_tom_on(id, 2) ? "off" : "normal", true);
      break;
    case "tom4":
      set_tom_state(id, 4, is_tom_on(id, 4) ? "off" : "normal", true);
      break;
    case "kick":
      set_kick_state(id, is_kick_on(id) ? "off" : "normal", true);
      break;
  }
  ```

- **Behavior by instrument:**
  - Hi‑hat (`"hh"`): OFF ⇄ **“normal” hi-hat/cymbal** (default state).
  - Snare (`"snare"`): OFF ⇄ **accented snare**.
  - Toms (`"tom1"`, `"tom2"`, `"tom4"`): OFF ⇄ **normal tom**.
  - Kick (`"kick"`): OFF ⇄ **normal kick**.

- After each left‑click, `updateSheetMusic()` is called to regenerate ABC/staff notation.

**Requirement:**  
- In non‑advanced mode, a single click on an empty cell must create a **sensible default hit** for that row; clicking again must turn it off.

### 3.2 Advanced Mode and Right‑Click Context Menus

- If `class_advancedEditIsOn === true`, left‑click **delegates to right‑click behavior**:

  ```js
  if (class_advancedEditIsOn === true) {
    root.noteRightClick(event, type, id);
  }
  ```

- Right‑click handler: `root.noteRightClick(event, type, id)`:
  - Records `class_which_index_last_clicked = id`.
  - Selects an instrument‑specific context menu:

    ```js
    switch (type) {
      case "hh":   contextMenu = document.getElementById("hhContextMenu");   break;
      case "tom1": contextMenu = document.getElementById("tom1ContextMenu"); break;
      case "tom2": contextMenu = document.getElementById("tom2ContextMenu"); break;
      case "tom4": contextMenu = document.getElementById("tom4ContextMenu"); break;
      case "snare":contextMenu = document.getElementById("snareContextMenu");break;
      case "kick": contextMenu = document.getElementById("kickContextMenu"); break;
    }
    ```

  - Positions the menu near the click point (`event.clientX/Y`) and shows it via `myGrooveUtils.showContextMenu(contextMenu)`.

- The context menu items are wired to various **set\_*_state** calls (e.g., snare ghost, flam, drag, buzz, x‑stick; hi‑hat open/closed/ride/crash, etc.).

**Requirement:**

- In advanced mode, both **left‑click and right‑click must open a context menu** for the chosen instrument at the clicked index.
- The menu must allow selecting all supported note variants for that part (see Section 4), and must update both:
  - The **visual icon / style** in the grid.
  - The **logical state** returned by `get_*_state(id, ...)`.

### 3.3 Drag‑to‑Paint (Ctrl/Alt + Mouse Enter)

Handler: `root.noteOnMouseEnter(event, instrument, id)`.

- When the mouse enters a cell with a modifier:
  - `Ctrl` → force **ON**.
  - `Alt` → force **OFF**.

- Example logic:

  ```js
  if (event.ctrlKey) action = "on";
  if (event.altKey)  action = "off";

  if (action) {
    switch (instrument) {
      case "hh":
        set_hh_state(id, action == "off" ? "off" : "normal", true);
        break;
      case "snare":
        set_snare_state(id, action == "off" ? "off" : "accent", true);
        break;
      case "kick":
        set_kick_state(id, action == "off" ? "off" : "normal", true);
        break;
    }
    updateSheetMusic();
  }
  ```

**Requirement:**

- Holding `Ctrl` and dragging over cells must **paint hits** in default states (normal/ accent).
- Holding `Alt` and dragging must **erase hits** along the drag path.
- This behavior should work regardless of `class_advancedEditIsOn`.

### 3.4 Line‑Label Bulk Operations

- Each row’s text label (e.g., “Hi-hat”, “Snare”, “Kick”) calls:

  ```html
  <div class="snare-label"
       onClick="myGrooveWriter.noteLabelClick(event, 'snare', baseindex)"
       oncontextmenu="event.preventDefault(); myGrooveWriter.noteLabelClick(event, 'snare', baseindex)">
    Snare
  </div>
  ```

- The handler (`noteLabelClick`) opens a row‑level popup that can apply patterns across the entire measure, e.g.:
  - **All snare notes on** (accent/normal/ghost).
  - **Hi‑hat upbeats only**.
  - **Hi‑hat foot on beats or “&”s** via kick row’s splash states.

- The popup actions are implemented by iterating `i` over the measure and calling `set_*_state(...)` with mode strings like:
  - `"all_on"`, `"all_on_normal"`, `"all_on_ghost"` for snare.
  - `"upbeats"` for hi‑hat.
  - `"hh_foot_nums_on"`, `"hh_foot_ands_on"` for kick (foot splash).

**Requirement:**

- Each row label must support **bulk pattern operations** that:
  - Set or clear notes in all columns belonging to that measure.
  - Use instrument‑appropriate default or specialized states.

---

## 4. Visual Representation per Drum Part

### 4.1 Hi‑Hat / Cymbals

Rendered by the **hi-hat row** (`.hi-hat-container`):

- Each cell (`#hi-hat{i}`) contains multiple `.hh_*` elements with distinct icons (crash, ride, ride bell, cowbell, stacker, metronome click, cross, open, closed, accent arrow).
- Only one or zero hi‑hat/cymbal/metronome state should be visually “on” at a time:
  - “On” is detected by style, e.g. `style.color == constant_note_on_color_rgb`.

**Visual semantics (high‑level):**

- **Crash / Ride / Ride Bell / Cowbell / Stacker:**  
  Represent different cymbals for that subdivision; used in `get_hh_state` to choose appropriate ABC/MIDI cymbal sound.
- **Metronome Normal / Metronome Accent:**  
  Used when the hi‑hat row is configured as a metronome track.
- **Cross / Open / Close / Accent arrow:**  
  Represent variants of hi‑hat behavior (e.g., foot “chick”, open vs closed, accented strokes).

**Requirement:**  
The hi‑hat/cymbal cell must clearly show which cymbal/hi‑hat state is active based on icon and color, and this must be in sync with `get_hh_state(id, ...)`.

### 4.2 Toms (Tom 1, Tom 2, Floor Tom)

Each tom row (`tom1`, `tom2`, `tom4`) uses:

- `.tom_circle` inside each `#tomX-{i}` cell.
- “On” is indicated by background color: `backgroundColor == constant_note_on_color_hex`.

**Requirement:**  
Tom notes must be visually represented as filled circles; tom type (1/2/4) is determined by the row, not by the icon itself.

### 4.3 Snare

The snare row uses multiple layered elements to represent **different snare articulations**:

- `.snare_circle` – normal snare hit.
- `.snare_accent` – accent indicator.
- `.snare_ghost` – ghost note.
- `.snare_xstick` – cross‑stick (side‑stick).
- `.snare_buzz` – buzz/roll.
- `.snare_flam` – flam glyph (SVG).
- `.snare_drag` – drag/ruff glyph (SVG).

**Requirement:**  

- At any time, the visual combination of these elements must correspond to **one logical snare state** (normal, accent, ghost, flam, drag, buzz, x‑stick, or off).
- Visual color and icon must match the mode chosen via `set_snare_state(id, mode, ...)`.

### 4.4 Kick

The kick row uses:

- `.kick_circle` – main bass drum hit.
- `.kick_splash` – hi‑hat foot splash marker.

These can be combined (both on) to represent **kick + foot splash**.

**Requirement:**  

- Kick-only, foot-only, and kick+foot combinations must be visually distinct and must stay in sync with the logical kick state (`get_kick_state`).

---

## 5. Logical Note States and Drum Mapping

Each drum part has **two main responsibilities**:

1. Provide a **visual state** within the editor grid.
2. Provide a **logical encoding** for playback/export via `get_*_state(id, returnType)`.

### 5.1 General Pattern

For all instruments:

- `get_*_state(id, returnType)`:
  - Returns `false` when the note is “off”.
  - Returns a non‑false value when a note is “on”, with two encoding modes:
    - `"ABC"` – ABC drum notation (e.g., `c`, `!accent!c`, `_c`).
    - `"URL"` – compact single‑character code for use in shareable URLs.

- `set_*_state(id, mode, make_sound)`:
  - Updates DOM styles for relevant `.note_part` elements at index `id`.
  - Optionally plays a preview MIDI note (`make_sound === true`).

These helpers are used by:

- `get32NoteArrayFromClickableUI(...)` – converts clickable UI into a full‑size 32/48‑step per measure representation.
- `MIDI_from_HH_Snare_Kick_Arrays(...)` – generates MIDI based on those arrays.
- `generate_ABC(...)` – builds the ABC notation for the written groove.

### 5.2 Snare Mapping

From comments in `get_snare_state(id, returnType)`:

- `false` – no snare hit.
- `"c"` – **normal snare** (ABC).
- `"!accent!c"` – **accented snare**.
- `"_c"` – **ghost note**.
- `"^c"` – **cross‑stick**.

Additional modes (determined by `set_snare_state` and corresponding DOM elements):

- `"flam"` – flam glyph and matching MIDI note.
- `"drag"` – drag/ruff glyph.
- `"buzz"` – buzz/roll icon and sound.

**Requirement:**  
For any index `id`, the snare cell’s visual state must match the value returned by `get_snare_state(id, "ABC")` and `"URL"`. Changing one via `set_snare_state` must update the other.

### 5.3 Toms Mapping

From `get_tom_state(id, tom_num, returnType)`:

- `false` – no tom hit.
- For toms, the only logical state is “normal”:
  - `"x"` (URL code) – normal tom hit.
  - ABC values differ per tom number (T1, T2, T4 constants), but conceptually they are **normal tom hits** pitched on different staff lines.

**Requirement:**  
Tom cells have a single hit type per row (no ghost/flam variants here); `get_tom_state` and `set_tom_state` must be consistent with that.

### 5.4 Kick Mapping

From `get_kick_state(id, returnType)`:

- `false` – off.
- `"F"` – normal kick (ABC).
- `"^d,"` – splash (hi‑hat foot) without kick.
- `"F^d,"` – kick + splash combination.

Dom mapping (from `set_kick_state`):

- **off** – `.kick_circle` background off; `.kick_splash` color off.
- **normal** – `.kick_circle` background on; splash off.
- **splash** – `.kick_splash` color on; `.kick_circle` border dimmed.
- **kick_and_splash** – both `.kick_circle` background and `.kick_splash` color on.

**Requirement:**  
Kick visual combinations must track whether the logical state represents: off, kick only, foot splash only, or both.

### 5.5 Hi‑Hat / Cymbals Mapping

From `get_hh_state(id, returnType)`:

- `false` – off.
- Each `.hh_*` element corresponds to a distinct ABC and URL code (constants in `groove_writer.js`), e.g.:
  - ride vs ride bell vs crash vs cowbell vs stacker, etc.
- `get_hh_state` checks DOM color on each `.hh_*` element in priority order and returns:
  - An ABC drum symbol when `returnType == "ABC"`.
  - A short letter when `returnType == "URL"` (e.g., `"r"` for ride).

**Requirement:**  
Exactly one high‑hat/cymbal/metronome logical state must be active per index when “on”. The active state is determined by which `.hh_*` element has the “on” color, and must match the ABC/URL encodings.

---

## 6. From Editor to Playback / Export

### 6.1 Scaling to Full‑Size Arrays

Function: `get32NoteArrayFromClickableUI(Sticking_Array, HH_Array, Snare_Array, Kick_Array, Toms_Array, startIndexForClickableUI)`.

- Reads clickable UI across one measure:
  - Uses `class_notes_per_measure`, `class_num_beats_per_measure`, and `class_note_value_per_measure`.
  - Uses `root.myGrooveUtils.getNoteScaler(...)` to compute a scaler between UI positions and full-resolution positions.
- For each UI index:
  - Calls `get_hh_state`, `get_snare_state`, `get_kick_state`, `get_tom_state` (and stickings).
  - Writes results into **full-size arrays** (`HH_Array`, `Snare_Array`, etc.), scaled to:
    - 32 positions per measure (non‑triplets).
    - 48 positions per measure (triplets).

### 6.2 Playback and Export

- **Playback:**
  - `MIDI_from_HH_Snare_Kick_Arrays(...)` iterates over full-size arrays and:
    - Maps each logical state to a specific MIDI note (percussion instrument).
    - Applies durations and swing.
- **ABC notation:**
  - `generate_ABC(...)` uses the same arrays and `get_*_state` logic to build drum staff notation.

**Requirement:**  
The **only source of truth** for playback and export is the logical state derived from the DOM via `get_*_state`. Any change in visual state (through clicking, dragging, or menus) must be reflected in those functions so the audio and notation remain in sync with what the user sees.

---

## 7. Summary of Behavioral Requirements

- **Creation & toggling:**
  - Non‑advanced left‑click toggles between OFF and a **default note type** for that row.
  - Advanced mode and right‑click show per‑instrument context menus for detailed articulations.

- **Editing over ranges:**
  - Ctrl/drag paints ON, Alt/drag paints OFF across cells.
  - Row labels open bulk‑operation dialogs (all on, upbeats only, foot splash positions, etc.).

- **Display:**
  - Each instrument row’s icons and circles must reflect a single, well‑defined note state (or off).
  - Visual grouping and background highlights must stay aligned with `class_notes_per_measure` and note grouping size.

- **Mapping to drum parts:**
  - Snare: normal, accent, ghost, flam, drag, buzz, x‑stick.
  - Kick: off, kick only, foot splash only, kick + splash.
  - Toms: off / normal hit, differentiated by row (T1, T2, T4).
  - Hi‑hat/cymbals: off or one of several cymbal/hi‑hat/metronome states.

- **Playback & export:**
  - `get_*_state` must always return a value consistent with the visual state for every index.
  - `get32NoteArrayFromClickableUI` is the bridge between editor grid and playback/export arrays.

This document should be kept in sync whenever the set of `.note_part` elements, modes in `set_*_state`, or the encodings used in `get_*_state` are changed.