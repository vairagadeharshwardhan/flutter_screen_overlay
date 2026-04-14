## 1.0.2 — 2026-04-14

- **ForegroundServiceStartNotAllowedException guard**: `OverlayService.onCreate` now wraps `startForeground()` in a try-catch. On Android 12+ the system can reject the call when the app transitions from paused to stopped between the `startForegroundService()` dispatch and the service `onCreate()` execution. Previously this threw an unhandled `RuntimeException` and crashed the process. The service now logs the error and calls `stopSelf()` instead, so the overlay silently fails to appear rather than killing the app.

## 1.0.1 — 2026-04-12

- Rename public API class `FlutterOverlayWindow` → `FlutterScreenOverlay` to match package name and eliminate developer confusion.
- Rename library file `flutter_overlay_window.dart` → `flutter_screen_overlay.dart`.
- Update all internal imports from package-prefixed to relative paths.

## 1.0.0 — 2026-04-12

Initial release as `flutter_screen_overlay` (forked from `flutter_overlay_window` v0.5.0).

### Patches included over upstream

- **Null-intent guard**: `OverlayService.onStartCommand` now safely handles `null`
  intent passed by Android on system-initiated service restarts. Previously caused
  a `NullPointerException` crash (reported via Google Play). Returns `START_NOT_STICKY`
  to prevent crash loop.
- **Missing engine guard**: Graceful `stopSelf` when Flutter engine is absent from
  cache on restart — prevents `IllegalStateException`.
- **WindowManager null guard**: Graceful `stopSelf` when `WindowManager` is
  unavailable — defensive check for edge-case device/OS behaviour.

### Inherited from flutter_overlay_window v0.5.0

- Foreground service with persistent notification
- Draggable PiP-style overlays with position gravity (snap to edge)
- Runtime resize and move
- Bidirectional data channel between main app and overlay
- Configurable overlay flags (defaultFlag, clickThrough, focusPointer)
- Android 12+ opacity enforcement
- `startPosition` parameter for initial overlay placement
- `moveOverlay` / `getOverlayPosition` API
