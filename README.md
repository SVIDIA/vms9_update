# SVIDIA VMS2020 update channel

### SVidia_VMS2020_9_1_25_291
*May 30, 2025*

## 🔧 Improvements
- **New Graphics Refresh Shortcut**: Added Ctrl+Alt+R keyboard shortcut to manually refresh Direct2D graphics display when visual issues occur
- **Enhanced Graphics Recovery**: Improved automatic recovery mechanisms for DirectX display state problems

## 🐛 Bug Fixes
- **Fixed Camera Stream Freezing**: Resolved issue where camera streams would freeze during long-running application sessions due to DirectX display state invalidation errors
- **Improved Story Image Saving**: Fixed reliability issues when saving images in story functionality with enhanced null checking and request validation
- **Enhanced Error Handling**: Added comprehensive exception handling for DirectX graphics operations to prevent application crashes

## 🔄 Technical Updates
- **Deprecated Code Updates**: Updated obsolete HTTP downloader implementation and encryption methods to current standards
- **Improved Exception Handling**: Enhanced error recovery patterns across all DirectX-related components
- **Better Logging**: Improved error logging and troubleshooting capabilities for graphics-related issues

## 📁 Components Updated
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
