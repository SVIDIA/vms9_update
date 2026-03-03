# SVIDIA VMS2020 update channel

### SVidia_VMS2020_9_1_25_298
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


### SVidia_VMS2020_9_1_25_297
*Feb 28, 2026*

## Bug Fixes

  - Fixed crash in NVR Config when switching between cameras with different layout sizes — index-out-of-range error during grid recalculation
  - Fixed NVR Config settings not saved on camera drag-and-drop; also fixed stale settings displayed after switching server configurations
  - Fixed app close hang where the main process stayed alive after the window disappeared due to a thread synchronization issue
  - Fixed NullReferenceException in DirectX rendering (RGBValuesToImage) — race condition where bitmap buffer becomes null during playback export

## Improvements

  - Enhanced single-instance check — detects hung/unresponsive previous instances and allows a new launch; brings existing window to foreground when a healthy instance is already running

### SVidia_VMS2020_9_1_25_296
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
