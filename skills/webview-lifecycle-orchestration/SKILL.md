---
name: "webview-lifecycle-orchestration"
description: "Synchronizing WebView runtime state with Android Activity and Fragment lifecycles to prevent leaks."
---

# WebView Lifecycle Orchestration Skill

## Overview
Manages the orderly startup, pause, resume, and destruction of Android WebViews to eliminate memory leaks and background audio/video playback.

## Operational Workflow
1. **Lifecycle Binding**: Connect `WebLifeCycle` callbacks to host `onPause`, `onResume`, and `onDestroy`.
2. **State Suspension**: Pause JavaScript timers and hardware-accelerated video rendering during `onPause()`.
3. **Cache Ingestion**: Set cache policies (`LOAD_DEFAULT`, `LOAD_CACHE_ELSE_NETWORK`) dynamically.
4. **Clean Disposal**: Detach the WebView from the parent hierarchy and invoke `destroy()` cleanly.
