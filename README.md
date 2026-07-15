# SVIDIA VMS2020 update channel

## SVidia_VMS2020_9_1_26_302
*Jul 15, 2026*

## New Features

### Event Playback — Configurable Play Window & Event-Only Timelines
- **Play window ±s:** a spinner in the Add/Remove Events dialog (1–10s, default 5) sets the half-width of the window played around each event. It replaces the "Events play time" toolbar button and its fixed 5/10/20s menu, and the value is remembered across sessions.
- **Event-only timeline:** with "Play by Events" on, the timeline collapses to just the rows defined in the dialog (NVR Events / Bookmarks / Motion Search) and hides the per-camera rows, restoring them when it is switched off. Off by default and remembered per playback tab.

### Events Panel — Detection Tooltip
- **Hover tooltip** over the camera image shows the selected event's parsed detection properties — detector, label or plate, the attribute pair, and confidence. It stays out of the way while zoom-dragging, and non-ADATA events yield no tooltip.

### View Tab Tree — Drag a Parent Node to Open Its View
- Single-child views — Alarm Log, Bookmarks, Events, Export, History, Messages, NVR Configuration, Search Results, Settings and Spot Monitor — can now be opened by dragging either the child node or its parent. Multi-child parents (Live View, Playback, Map, Browser, Story) stay non-draggable, since the view to open would be ambiguous.

### About Dialog
- **Release notes:** a small info (i) icon next to the version number opens the release notes, mirroring the update dialog's info button
- **Show Logs** (was "Show Log") now opens the log folder instead of a single per-exe log file

## Improvements

### Events Panel
- **Auto-reload on event changes:** turning on Show Events or changing the NVR Events checkmarks in the Add/Remove Events dialog now reloads the archive automatically, preserving playback position — no manual Reload. Motion Search and Bookmarks changes only re-filter the loaded timeline, as before.
- **Parsed/raw toggle keeps its context:** the ⓘ toggle in the "Data" column header no longer clears the search, deselects the row, or blanks the camera image — it re-applies the current filter in place
- **Data column** label "LPR:" is now "LP:"

### Camera Dialogs
- The Add New Camera dialog now labels cameras with the same index-plus-name prefix as the NVR tree (e.g. `C4 Corridor`)

## Changes

- **V9 NVR support is temporarily disabled in this build.** Connections probe V6 only, scanner discovery skips the V9 port, and the Group Users / Group Settings / Cam Manager configuration tabs are not built. Export V9 is dropped from the add-page menu, but existing saved layouts containing an Export V9 page still load.

## Bug Fixes

- **Event playback:** Playback no longer jumps backwards behind a clicked position — the window around the first event of a play session is clamped to the requested position, while auto-advancing from event to event keeps the full preroll
- **Event playback:** Fixed three paths that could leave a hidden timeline row selected, which pointed the status line and play marker at a row with no rectangle


## SVidia_VMS2020_9_1_26_301
*Jul 11, 2026*

## New Features

### Events Panel — Image Review & Smarter Filtering
- **Camera image panel:** a collapsible panel shows the full-resolution archive frame for the selected event, fetched headlessly. Wheel/drag-ROI zoom, detection box, prev/next, save, copy-to-clipboard, and sync-to-archive.
- **Dynamic value filter:** a dropdown lists distinct values in the current slice (object / attribute / camera) with counts — single-select, case-insensitive, combined with the text search.

## Improvements

### Events Panel
- **Richer parsing:** the Data column now shows the detected object plus its attributes (vehicle/person color, gender) and ONVIF events that were previously dropped
- **Type column** now shows the event's detector id
- **Parsed/raw toggle** moved into an info glyph in the "Data" column header
- **Server-local event times** — shown in the NVR's own zone

## Bug Fixes

- **Events:** Fixed a time-zone drift when seeking/fetching an event's image on cross-zone NVRs — seek/image time now uses timestamp in the record's own time base
- **Events:** Deselecting a row no longer throws; the image panel and header clear when the table empties or the row is deselected

### SVidia_VMS2020_9_1_26_300
*Jul 05, 2026*

## New Features

### R-CAD Editor for V6 NVRs — Full Editing
Building on the V6 R-CAD editor introduced in 299, this release brings the remaining advanced-editor capabilities to V6 NVRs, matching the V6 wire protocol:
- **Clipboard:** Copy / Cut / Paste / Clone modules via the canvas context menu or Ctrl+C/X/V/D, with client-side re-UID and connection remapping replayed to the server
- **Import / Export:** "Copy to / Paste from file" reads and writes `.rcd` files
- **Multi-select:** Ctrl+click and Shift+drag marquee selection; the full selection now survives live canvas rebuilds and drives Copy / Cut / Delete on every selected module
- **Pic scripting:** Run / Compile / Restart Pics from the editor, with a built-in server-side script editor (Pic / Code Unit / Graphics Program) featuring inline Run/Stop and a live debug pane
- **Live Pic state sync:** a running Pic's server-side self-changes — run/stop color, renamed Pic or pins — now reflect on the canvas in real time
- **Script password lock:** viewing or editing a locked Pic's script now prompts for its password, matching V6 behavior
- **"Select Users" picker:** the Select Users pin/device action now opens a proper user-picker dialog and writes the assignment back to the server
- **Alarm Panel live push:** dynamic `<DPN#>` pin names and active-state changes pushed from the server now refresh the on-canvas Alarm Panel object, the I/O tab pins, and the Alarm Panel view
- **Pin properties:** pin selection now spreads across the full lower panel, with the owning module's name shown in the breadcrumb (e.g. `External Devices > ADATA > Output > out_2 > Pin settings`)
- Zoom and pan are now remembered per NVR

## Improvements

### R-CAD (V6 and V9)
- Smooth, eased mouse-wheel zoom — the view now glides toward the point under the cursor instead of snapping in fixed steps
- Objects render with their own back/text colors from the server instead of a fixed color scheme

### Camera Trees
- Camera nodes now combine the server index label with the explicitly-defined name (e.g. `C1 Corridor`), so both are always visible in the NVR tree, the Cameras tree, and group hover-previews

## Bug Fixes

- **R-CAD V6:** Fixed "Set output" not driving alarm-panel output pins (wrong desktop opcode) and inverting the chosen state
- **R-CAD V6:** Fixed drifted module colors — black text and system-colored backgrounds (e.g. Alarm Panel) no longer fall back to white-on-dark
- **R-CAD V6:** Fixed multi-selection collapsing to a single module after a live update, which caused Copy / Cut / Delete to act on only one object
- **I/O Tab:** Fixed crash when right-clicking a V6 inputs/outputs branch
- **Camera Settings:** Restored the wait-cursor / window-freeze release on a failed camera load, and allowed re-dragging the same camera to retry
- **Dialogs:** Fixed title text shift


### SVidia_VMS2020_9_1_26_299
*Jun 25, 2026*

## New Features

### R-CAD Graphical Editor for V6 NVRs
- Brought the R-CAD event/automation editor (NVR Configuration → R-Cad) to V6 NVRs — previously available on V9 only
- Single editor now drives both NVR generations through a capability-flag backend seam, with no change to existing V9 behavior
- Full editing workflow: read-only graph rendering → property editing → wiring (move / connect / disconnect) → palette add/remove, including external/DLL modules enumerated from the server
- New property editor types: Date/Time, and File pickers, plus pushed-state button toggles
- **UserBlock drill-down:** navigate into composite blocks with boundary and repeater terminals rendered, and VClient-style selection breadcrumbs to track your position
- **Save & persist parity:** per-operation apply with a cooperative server edit lock — the editor drops to read-only when another supervisor holds the lock
- R-CAD alarm-zone events from V6 NVRs surface as timeline markers and seek correctly during playback
- Hovering a module on the canvas shows its memo text as a tooltip, so long notes are readable without selecting the module (V6 and V9)

### SVidia_VMS2020_9_1_26_298
*Mar 04, 2026*

## New Features

### PTZ Control Toggle in Camera Settings
- Enable or disable PTZ control per camera directly from the Camera Settings panel
- Settings panel updates in real-time to reflect the new PTZ state

### Bulk PTZ Control Editing
- Added PTZ Control enable/disable toggle to the bulk Camera Settings dialog
- Batch-edit PTZ settings across multiple cameras in a single operation
- Cameras without PTZ support are automatically skipped

## Improvements

### Application Lifecycle
- Enhanced single-instance detection: hung or unresponsive instances are now bypassed, allowing a new instance to launch
- When a healthy instance is already running, its window is brought to the foreground instead of showing an error

### Build & Deployment
- Updated installer build script and project configuration
- Code cleanup in timeline playback controls

## Bug Fixes

- **NVR Config:** Fixed crash when switching between cameras with different sizes
- **NVR Config:** Fixed settings not being saved after drag-and-drop camera rearrangement; fixed stale settings displayed after switching servers
- **Application:** Fixed close hang where the process remained alive after the window disappeared
- **Playback:** Fixed crash during bitmap export caused by a race condition in the DirectX rendering pipeline
- **PTZ Settings:** Fixed state corruption when PTZ protocol enable/disable failed; fixed duplicate event handler and UI freeze during property updates
- **SpotMonitor:** Fixed crash when closing panels due to stale camera index references
- **Bulk Settings:** Fixed Apply button being incorrectly enabled on dialog open across Camera, Motion, and Delta tabs


### SVidia_VMS2020_9_1_26_297
*Feb 28, 2026*

## Bug Fixes

  - Fixed crash in NVR Config when switching between cameras with different layout sizes — index-out-of-range error during grid recalculation
  - Fixed NVR Config settings not saved on camera drag-and-drop; also fixed stale settings displayed after switching server configurations
  - Fixed app close hang where the main process stayed alive after the window disappeared due to a thread synchronization issue
  - Fixed NullReferenceException in DirectX rendering (RGBValuesToImage) — race condition where bitmap buffer becomes null during playback export

## Improvements

  - Enhanced single-instance check — detects hung/unresponsive previous instances and allows a new launch; brings existing window to foreground when a healthy instance is already running

### SVidia_VMS2020_9_1_26_296
*Feb 10, 2026*

## New Features

### I/O Panel Support for V6 Devices
- Full I/O panel implementation for V6 hardware
- New "Assign to Group" dialog for organizing I/O pins
- Drag-and-drop support for pins into groups
- Visual indicators for input/output states

### Enhanced Layout Selection
- Tabbed layout selection organized by aspect ratio
- Support for well-known display ratios (16:9, 4:3, 21:9, etc.)
- Improved layout preview and selection experience

## Improvements

### Performance
- Optimized UI thread update operations for better responsiveness
- Improved NVR control performance

### Network Layer
- Refactored client communication code

### Auto-Update
- Added error handling and validation for update process
- Improved update reliability

## Bug Fixes

- **Archive Explorer:** Fixed time selection bugs in archive explorer dialog
- **Alarm Panel:** Fixed pin drag-and-drop functionality
- **I/O Tree:** Fixed drag-and-drop into groups
- **Application:** Fixed graceful application termination issues


### SVidia_VMS2020_9_1_25_292
*May 30, 2025*

## Improvements
- **New Graphics Refresh Shortcut**: Added Ctrl+Alt+R keyboard shortcut to manually refresh Direct2D graphics display when visual issues occur
- **Enhanced Graphics Recovery**: Improved automatic recovery mechanisms for DirectX display state problems

## Bug Fixes
- **Fixed Camera Stream Freezing**: Resolved issue where camera streams would freeze during long-running application sessions due to DirectX display state invalidation errors
- **Improved Story Image Saving**: Fixed reliability issues when saving images in story functionality with enhanced null checking and request validation
- **Enhanced Error Handling**: Added comprehensive exception handling for DirectX graphics operations to prevent application crashes

## Technical Updates
- **Deprecated Code Updates**: Updated obsolete HTTP downloader implementation and encryption methods to current standards
- **Improved Exception Handling**: Enhanced error recovery patterns across all DirectX-related components
- **Better Logging**: Improved error logging and troubleshooting capabilities for graphics-related issues

## Components Updated
- Main VMS Application (graphics display, keyboard shortcuts)
- DirectX Graphics System (display recovery, exception handling) 
- Client Library (image saving, network operations)
- Update System (HTTP implementation)
- Encryption Components (deprecated method updates)

---
*This release focuses on stability improvements for long-running surveillance operations and enhanced user control over graphics display issues.*


### SVidia_VMS2020_9_1_23_289
*May 09, 2024*
- fixed bug in NVR configuration when camera has no valid source
- fixed fault frames handling 
- fixed set PTZ preset function 
- fixed crash while changing some camera settings in NVR configuration

### SVidia_VMS2020_9_1_23_286
*Apr 20, 2024*
- added QV2M archive feature 

### SVidia_VMS2020_9_1_23_284
*Jan 11, 2024*
- frozen display on long run fix

### SVidia_VMS2020_9_1_23_283
*Dec 28, 2023*
- improved UI for video export
- video export will be resumed if interrupted during download phase 
- fixed Motion detection mask settings

### SVidia_VMS2020_9_1_23_281
*Dec 18, 2023*
- fixed UI appearance when display scaled
- displaying key frames on TimeLine if zoomed
- sync playback by events with Events window

### SVidia_VMS2020_9_1_23_280
*Dec 12, 2023*
- improved performance via redesigned internal codec
- build on new dotnet 8 LTS platform 
- removed dependencies on runtime packages from Microsoft
- updated Camera OD configuration user interface 
- improved drag-n-drop handling in Story user interface 
- other performance, security and UI improvements

### SVidia_VMS2020_9_0_19_277
*Nov 09, 2023*
- fixed: export item has not been updated after editing

### SVidia_VMS2020_9_0_19_276
*Nov 02, 2023*
- functionality synced with VMS version 251 
