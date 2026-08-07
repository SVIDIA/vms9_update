# SVIDIA VMS2020 update channel

## SVidia_VMS2020_9_1_26_309
*Aug 7, 2026*

## Bug Fixes

- **The Alarm Zones tab could be missing from NVR Configuration.** The tab was only ever brought out when picking something in the NVR or Camera tree. Which tabs belong on screen is now settled when the view is built and checked again every time NVR Configuration is opened, so Alarm Zones is simply always there.


## SVidia_VMS2020_9_1_26_308
*Aug 6, 2026*

## New Features

### Export the Events Table to a Spreadsheet
- A new button in the **Events** view's camera-image bar saves the events you are looking at to a **CSV file**. The search text and the value filter currently applied are respected, so the file holds exactly the rows on screen, in the same order — not the whole unfiltered load.
- Each row carries the full date and time (the table itself omits the year to fit the column), the event type and data, the camera number *and* its name, and which NVR it came from. The last two matter as soon as one view is showing events merged from several servers, where the table can only show a bare camera number.
- The ⓘ toggle over the *Data* column still decides what that column contains, so exporting with it on gives the full raw event payload and with it off the friendly one-line reading.
- The file opens straight into Excel, non-English camera names, labels and plate text included. The button is available whether or not the camera image is showing.


## SVidia_VMS2020_9_1_26_307
*Aug 4, 2026*

## Bug Fixes

- **Cameras could not be added to an R-CAD scheme.** The R-CAD editor released in 9.1.26.300/301 offered every element type except the camera: a scheme kept showing and running the camera blocks it already had, but no new one could be placed. Cameras are now their own category in *Add New Device*, listed by number and name and limited to those not already on the scheme — one block per camera, as the NVR requires. On an NVR whose firmware cannot create camera blocks, the client now says so and leaves the scheme untouched instead of adding an empty one.

## SVidia_VMS2020_9_1_26_306
*Jul 30, 2026*

## New Features

### The NVR Now Tells the Client Its Time Zone
- **NVR time zone reported by the server.** Where 9.1.26.305 needed the zone to be named by hand — legacy NVRs report only their current offset from UTC, which cannot place a timestamp from the other side of a daylight-saving change — a capable NVR now states its own zone, rules and all. **Auto** stops being an inference: archive times, timeline seek, export and download ranges, motion search and event times convert exactly across a DST boundary with nothing to configure.
- The per-NVR **NVR time zone** setting still wins when it is set. It remains the escape hatch for a server whose own clock configuration is wrong, and overriding it with a value from the machine being corrected for would make it useless.
- The **Auto** entry now names the zone it actually resolved to for that NVR, instead of describing the rule it uses to pick one.

### License Status on the NVR Info Panels
- The **NVR Info** box and the hover **preview panel** gain a license block: status, channels in use against channels licensed, serial number (and whether it is bound to that hardware) and expiry date.
- A license that is not valid — expiring, expired, or over its channel count — is also raised in the system-messages list rather than sitting quietly in a panel nobody has open.
- Supervisor sign-in only, and only on NVRs that report a license at all.

### Time Zone and Clock Health at a Glance
- Both the **NVR Info** box and the hover **preview panel** gain **Timezone** and **Time diff** rows, plus a line stating how the zone was established — reported by the server, configured for this NVR, inferred, or offset-only. Only the first two convert exactly across a daylight-saving change; the line says which one you have.
- A **Clock skew** row appears, in red, only when the NVR's clock is genuinely wrong — more than a minute out — as opposed to simply sitting in another time zone. This is a separate fault from a zone gap, it quietly shifts every archive seek, and it was previously invisible everywhere in the client.
- Both panels now measure their content and grow to fit it. They were fixed-size and silently cut off anything that ran past the edge, so the license and time-zone rows would not have been readable on either.

### System Messages Carry a Real Severity
- Status messages from the NVR now arrive with a severity attached. Anything critical — an imminent license expiry, for instance — is filed as an exception rather than sharing one flat channel with routine operator chatter. License changes prompt the client to re-read the license from the server rather than guess at the new state.

### Extended Capabilities — One Handshake
- The 32-camera, 8K and full-permission support introduced in 9.1.26.305 is now negotiated through the NVR's single extended-capability handshake. The features themselves are unchanged; they require a server build that offers the new handshake.

*All of the above appear only when the NVR reports them. Against existing NVRs the client behaves exactly as before.*

## Bug Fixes

- **Archive shown as hours old when it was recording right now.** The "ago" line under *Archive from* / *Archive to* was measured against this PC's clock, while the timestamps themselves are the NVR's own wall clock — so the whole time-zone gap was charged as age. On an NVR ten hours behind, an archive whose end *is* the present moment read "10.0 hours ago". Both ends are now converted before the subtraction, and the timestamp itself is labelled **NVR time**, staying server-local so it continues to match the timeline and the NVR's own logs.
- **"43.2166666666667 mins ago".** The minutes form of the age text printed at full precision. It now reads one decimal place, like the hours and days forms beside it. (It had never been seen before, because the time-zone error above always pushed the result into the hours branch.)
- **Daylight-saving indicator beside the NVR offset could be wrong for hours after a clock change.** It was evaluated at the wrong instant and applied the offset a second time, so an NVR an hour into a spring-forward could be marked incorrectly for the rest of the day. The client now prefers the server's own daylight-saving flag, captured together with the offset printed next to it, so the two cannot contradict each other.


## SVidia_VMS2020_9_1_26_305
*Jul 29, 2026*

## New Features

### NVR Connections — Time Zone & Settings While Enrolling
- **NVR time zone:** the Add / Edit NVR dialog gains an **NVR time zone** selector. Legacy NVRs report only their current offset from UTC, which is not enough to convert times that fall on the other side of a daylight-saving change; naming the NVR's zone makes those conversions exact. Left on **Auto**, the client infers the zone only when the NVR is in this PC's own zone and otherwise keeps the previous behaviour, so nothing changes until a zone is set.
- **Camera settings available when adding an NVR:** the Resolution / FPS / Quality / Time zone dropdowns are now shown while enrolling a new NVR.

### Extended NVR Capabilities — 32 Cameras and 8K
- **Up to 32 cameras per NVR** on server builds that support it. The camera trees, live and playback grids, PTZ, QuickView and the monitor-status feed all follow the connection's real camera count.
- **Frames up to 7680×4320 (8K).** Native-resolution stills and streams above the previous ceiling now decode instead of being rejected, and the decoder releases the memory again when a stream drops back to HD.
- **Camera access for cameras 17–32:** the Users dialog reads and writes view / sound grants for all 32 cameras, along with the full permission set, over the NVR's canonical user protocol.
- These are negotiated per connection and only when the NVR advertises them. Against existing NVRs the client behaves exactly as before.

## Improvements

### Events Panel
- **Camera image panel restyled** to match the archive view — the same dark button bar, 36×36 icon buttons and hover behavior, with the camera name, frame time and time difference moved to their own label row directly beneath the bar.

### Users & Roles — Permission Grids
- **Fixed permissions now read as fixed.** Permission and camera-access cells that cannot be changed — a supervisor's camera access, or a permission the NVR does not support — were drawn as ordinary checkboxes, so the only way to discover they were locked was to click one. They now render as a plain tick (granted), a dash (partially granted) or nothing at all (not granted).
- The **select-all header checkbox** is dropped from columns whose cells are all fixed, since it could not change anything.

## Bug Fixes

- **Detection boxes landing away from the object, differently on each PC:** the same event, camera and recording could put the detection box on the moving object on one machine and feet away on another. The box was never misplaced — each client was resolving a *different archive frame*, because time conversions ran off a clock measurement taken at connect, which carries the PC's own clock error and network round-trip noise in addition to the time-zone difference. Conversions now use the NVR's reported time-zone offset, re-evaluated as the session runs, so every client resolves the same frame regardless of how accurate its own clock is. This affects all archive time handling — timeline seek, export and download ranges, motion search, event seek and the live overlay clock — not only detection boxes.
- **Times across a daylight-saving boundary were an hour out:** viewing a January recording in July (or vice versa) converted with the current offset rather than the offset in force at that moment. With an NVR time zone configured, both ends are now evaluated at the instant being converted.
- **Instant replay** now anchors "the last N seconds" on the NVR's clock rather than the local PC's, so a PC whose clock is off no longer replays the wrong window.
- **Dialog titles fixed:** dialogs captions now span the title bar and stay vertically centered.
- **Time conversion performance:** the corrected conversions are called from timeline are computed at most once a second and reused. Connections in the PC's own zone — the common case — pay essentially nothing, and a live change to the NVR's reported offset or configured zone still takes effect immediately.


## SVidia_VMS2020_9_1_26_304
*Jul 22, 2026*

## New Features

### Playback — Frame Stepping (V6 archives)
- **Next / Previous frame:** in archive mode the Next-frame and Previous-frame buttons now step one frame at a time on V6 NVRs.

## Improvements

### Playback — Reverse Playback
- **Faster play-backward:** a dedicated faster reverse-play button in archive mode for quicker review when scrubbing backwards.

## Bug Fixes

- **Green band along the bottom of some cameras:** Some cameras showed a thin green strip across the very bottom of live and playback images after the recent codec migration. The decoder now seeds its working frame correctly. 
- **Crash when changing camera Quality:** Changing a camera's Quality setting could crash the application. Decoding and decoder teardown are now serialized, so the change applies cleanly.


## SVidia_VMS2020_9_1_26_303
*Jul 19, 2026*

## New Features

### Playback — Motion Heatmap (Delta Statistics)
- **Motion heatmap:** right-click a camera in archive mode → **Motion heatmap** opens a dialog that scans the archive and renders where motion occurred, over the same interval Motion Search uses (playhead → last recorded frame on the timeline). Motion is drawn as a blue→red heatmap normalised to the busiest block, and the status bar reports the peak change count and percentage so the scale stays interpretable.
- **Pause / resume / regenerate:** the scan can be paused and resumed without losing accumulated counts, and cancelled cleanly. Results are cached per (server, camera, interval) and re-shown instead of rescanning; **Regenerate** discards the cache and rescans.

## Improvements

### Camera Source Properties
- **Long values now wrap:** long server-supplied string properties that were clipped to a single unreadable line now wrap into a taller, compact box that grows to the content and then scrolls — mirroring the Hint field. Short fields (IP, username, URL) keep their normal single-line control. Applies to both the editable "Edit source properties" modal and the read-only display.

## Changes

- **Video codec migrated to svdcodec.** Live decode, archive playback, and the export / download-save YUY2→BGR32 conversion all move off the legacy Intel-toolchain native libraries to the new `svdcodec`.
- **Logs are now UTF-8.** Log files are written as UTF-8 (no BOM) and gain a `.txt` extension. The stale UTF-16 files (and their `.bak` backups) are deleted automatically on the first start after upgrade.

## Bug Fixes

- **Export crash on high-resolution cameras:** Fixed a destination-stride error in the BGR32 image resizer that corrupted the heap when downscaling, crashing the timelapse and Playback Export paths — most reliably on the 3840×1755 (6.7 MP) camera. Standard ~1080p cameras were rarely affected.
- **Encoder tab diagnostics restored:** In NVR Configuration → **Encoder**, the changed-block overlay and the running **Frame size** histogram were both blank; they now render again (the overlay in the same bright magenta as the Motion/Alarm tabs).


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
