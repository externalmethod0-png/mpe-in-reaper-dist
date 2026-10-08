# MPE Editor for REAPER — Full Commercial ReaPack Repository

This repository provides binary releases and 1-click automatic updates for the **full commercial version** of the **MPE Editor** native REAPER extension via [ReaPack](https://reapack.com) for customers who purchased the extension on [Gumroad](https://methodical4.gumroad.com/l/reaperMPE).

`MPE Editor` is a native C++ REAPER extension (`reaper_mpe_editor.dll`) that provides a dockable and inline per-note MPE expression editor for standard MIDI takes — bringing Ableton/Bitwig-style per-note **Pitch Bend**, **Channel Pressure (Aftertouch)**, and **Timbre (CC74)** curve editing directly into REAPER while writing back 100% standard MPE MIDI events.

---

## How to Install & Update via ReaPack (Full Version)

### Repository URL

```text
https://raw.githubusercontent.com/externalmethod0-png/mpe-in-reaper-dist/main/index.xml
```

### Installation Steps

1. In REAPER, navigate to:  
   **Extensions -> ReaPack -> Import repositories...**
2. Paste the repository URL above and click **OK**.
3. In REAPER, go to:  
   **Extensions -> ReaPack -> Browse packages**
4. Search for `MPE Editor`, right-click it, and select **Install**, then click **Apply**.
5. **Restart REAPER** (REAPER loads C++ extension DLLs at startup).

### Automatic Updates

Whenever a new update is released, simply run:  
**Extensions -> ReaPack -> Synchronize packages**  
Then restart REAPER.

---

## Direct Download (Without ReaPack)

If you prefer manual installation without ReaPack, download the full release package directly:
- **Full Package ZIP (v1.1.8):** [reaper-mpe-editor-1.1.8-windows-x64.zip](https://github.com/externalmethod0-png/mpe-in-reaper-dist/releases/download/v1.1.8/reaper-mpe-editor-1.1.8-windows-x64.zip)
- **Direct DLL (v1.1.8):** [reaper_mpe_editor.dll](https://github.com/externalmethod0-png/mpe-in-reaper-dist/releases/download/v1.1.8/reaper_mpe_editor.dll)

Extract and copy `reaper_mpe_editor.dll` into your REAPER `UserPlugins` folder:  
`Options -> Show REAPER resource path in explorer/finder -> UserPlugins`  
Then restart REAPER.

---

## Key Features

### 1. Per-Note Expression Lanes & Bezier Curves
- Edit **Pitch Bend** (with synth profile range presets up to $\pm 48\text{ st}$), **Channel Pressure**, and **Timbre (CC74)** inside each note body.
- Full support for **Linear**, **Bezier** (continuous midpoint tension handles), **Slow**, **Fast Start**, and **Fast End** curve segments.
- Reference (**Ghost**) lane overlay (`V`) to view inactive expression lanes behind the active curve.
- Automatic time-scaling: resizing a note stretches its expression curves proportionally.

### 2. Dedicated Tool Modes & Generative Wave Brushes
- **Note Tool (`N`):** Create, move, resize, duplicate (`Ctrl+D` / `Ctrl+Drag`), mute (`Alt+M`), adjust velocity (`Alt+Shift+Drag`), and release velocity (`Ctrl+Up/Down`).
- **Point Tool (`P`):** Add points via single-click or double-click on a curve, drag single or marquee-selected points, bend segment handles, and reset points via double-click / `Alt+Click` / `Right-Click`.
- **Modulation Brushes (`Sine`, `Triangle`, `Saw Up`, `Saw Down`):** Drag across any note to generate per-note LFO modulation; hold `Shift` while dragging to ramp modulation depth across the note.
- **One-Click Workflows & Presets:** Quick toolbar buttons for **Rnd X** (randomize active lane), **Slide** (voice-led melodic pitch glides), **Flat** (`Shift+F` reset), and a built-in/user `.mpepreset` manager.

### 3. Seamless REAPER Arrange View & Docker Integration
- **Exact Sub-Pixel Docked Timeline Parity:** When docked below the Arrange View with `Follow` enabled, timeline measures, grid lines, notes, and the playhead align in one continuous vertical line across both windows at any zoom level.
- **Bidirectional Playback & Zoom Sync:** Live horizontal zoom changes inside the MPE Editor are mirrored back to the Arrange View during continuous playback scroll without snapping back or jittering.
- **SmoothWheelScroll Support:** Native compatibility with `bobo198504`'s `SmoothWheelScroll` extension.
- **Linear Left/Right Media Item Auto-Extend:** Dragging notes or note attacks past the start or end of a MIDI item automatically extends the REAPER media item boundaries (`B_LOOPSRC` disabled) while preserving project timeline alignment.
- **Inline Editor Mode:** Run `MPE Editor: Toggle inline editor (follows selected item)` to attach a borderless expression strip directly underneath the selected MIDI item in the Arrange View.
- **Pitch-Bent Note Auditioning:** Clicking a note previews its exact Pitch Bend curve value at the clicked horizontal offset through the track's instrument chain, with automatic pitch reset on release.
- **REAPER Theme & Dark Mode Support:** Automatically adapts to REAPER's active theme and REAPER 7.81+ native Windows Dark Mode with high-contrast text legibility.

---

## Keyboard Shortcuts & Controls Quick Reference

| Action / Gesture | Shortcut / Control |
| --- | --- |
| **Switch Tool Mode** | `N` (Note Tool), `P` (Point Tool) |
| **Switch Active Lane** | `Tab` or `1` (Pitch), `2` (Pressure), `3` (Timbre CC74) |
| **Toggle Ghost Reference Lanes** | `V` |
| **Bypass Grid & Scale Snap** | Hold `Shift` while dragging notes, note edges, or curve points |
| **Insert Curve Point (Point Tool)** | Click or Double-Click on curve (`Shift` + Double-Click bypasses snap) |
| **Reset / Delete Curve Point** | `Right-Click`, `Alt+Click`, or Double-Click on point |
| **Cycle Segment Shape** | `C` / `Shift+C` |
| **Invert / Normalize Active Lane** | `I` (Invert), `M` (Normalize) |
| **Reset All Expressions on Selected Notes** | `Shift+F` |
| **Duplicate Selected Notes (Note Tool)** | `Ctrl+D` or `Ctrl+Drag` |
| **Toggle Note Mute (Note Tool)** | `Alt+M` |
| **Go to Start of Active MIDI Item** | `W` (`Home` goes to Project Start) |
| **Horizontal Zoom / Vertical Scroll** | `Mouse Wheel` (Normal Zoom mode) or `Ctrl+Wheel` |
| **Toggle Fullscreen (Floating Window)** | `F11` (`Esc` restores) |
| **Interactive Built-in Help Overlay** | `F1` |

---

## Changelog

### v1.1.8 (2026-10-06)
- **Eliminated Zero-Delta `+8191` Pitch Spikes on Back-to-Back Notes:** Fixed an exporter issue where adjacent notes sharing an MPE member channel emitted duplicate pitch bend events at the exact same tick (`delta = 0`), causing REAPER's raw CC Bezier evaluator to spike to `+8191` (`+48 st`) at the note boundary. Coincident boundary events are now deduplicated while preserving the incoming note's outgoing Bezier curve shape and tension.
- **Back-to-Back Note Duplication & Shared Boundary Curve Integrity:** When two touching notes share a single boundary CC/Pitch Bend event at `note1.end == note2.start`, the importer now assigns that boundary point to both the ending note and the starting note, preventing duplicated or back-to-back notes from losing their `0.0 st` start point and turning into a flat `+2 st` shelf.
- **Automatic Curve Recovery from Take Metadata:** Projects saved by earlier builds where a duplicated note lost its start-of-note raw CC event now automatically recover their complete authored curve and Bezier tensions from persisted `<X CODEX.MPE.NOTE>` take metadata on load.

### v1.1.7 (2026-10-05)
- **Continuous Playback Zoom Synchronization:** Live horizontal zoom changes during playback are immediately mirrored to REAPER's Arrange View without snapping back during continuous scroll.
- **Leftward Media Item Auto-Extend:** Moving notes or dragging note attack leftwards past the item start automatically extends the REAPER media item start position (`D_POSITION`) and length (`D_LENGTH`) leftward while keeping project timeline alignment (`B_LOOPSRC` disabled).
- **Point Tool `Shift+Drag` Grid Bypass:** Holding `Shift` when clicking and dragging envelope curve points immediately starts point dragging and bypasses grid snap.
- **Point Tool Double-Click Point Insertion:** Double-clicking anywhere on an expression curve in Point Tool (`P`) inserts a curve point at that position (`Shift` + double-click bypasses grid snap).

---

## Links

- **Buy on Gumroad:** [https://methodical4.gumroad.com/l/reaperMPE](https://methodical4.gumroad.com/l/reaperMPE)
- **Discussion & Feedback:** [Cockos Incorporated Forums (Thread 309540)](https://forum.cockos.com/showthread.php?t=309540)
