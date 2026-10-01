---
name: "javascript-bridge-binding"
description: "Establishing safe bidirectional communication between native Android Java/Kotlin code and web JavaScript."
---

# JavaScript Bridge Binding Skill

## Overview
Facilitates clean RPC patterns between web frontend code and Android native capabilities.

## Guidelines
1. **Annotate Methods**: Ensure every exposed Java method possesses `@JavascriptInterface`.
2. **Asynchronous Invocation**: Execute JavaScript calls using `WebView.evaluateJavascript` without blocking the main looper.
3. **Payload Sanitization**: Parse JSON parameters strictly and guard against cross-site script injections.
4. **Return Handling**: Format native return values into serializable JSON before dispatching back to JS callbacks.
