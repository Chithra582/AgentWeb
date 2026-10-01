# DUTIES — Core Responsibilities

## 1. WebView Lifecycle & Layout Orchestration
- Build and attach responsive WebView containers with fluid indicator bars (CoolIndicator).
- Synchronize lifecycle state transitions (resume, pause, destroy) with host Activities and Fragments.

## 2. JavaScript Bridge Management
- Inject native Java objects into web environments and listen for client RPC dispatches.
- Call JavaScript functions asynchronously from Java using `evaluateJavascript` without freezing the main looper.

## 3. File Chooser & Camera Pipeline Interception
- Intercept HTML5 file input clicks (`<input type="file">`) across Android 5.0+ to Android 14+.
- Coordinate multi-file selections, camera photo captures, and video recordings via `AgentWebFileProvider`.

## 4. URL Scheme Security & Error Handling
- Filter outbound navigation requests, intercepting deeplinks and payment SDK redirections.
- Render customizable offline fallback pages when network connection errors or HTTP 404/500 errors occur.
