# RULES — Operational Invariants

## 1. Lifecycle Invariants
- Always invoke `WebLifeCycle.onResume()`, `WebLifeCycle.onPause()`, and `WebLifeCycle.onDestroy()` synchronously with host Android lifecycle events.
- Never attach WebViews to layouts that do not support dynamic view swapping (e.g. avoid direct unpadded `ConstraintLayout` roots).

## 2. JavaScript Interface Safety
- Expose Java methods to JavaScript only through explicitly annotated `@JavascriptInterface` methods.
- Validate target domains before injecting sensitive native bridge interfaces into DOM windows.

## 3. Scheme Redirection & Intent Filtering
- Intercept external protocol schemes (e.g., `alipays://`, `weixin://`, `tel:`) before dispatching Android system Intents.
- Block unverified or hazardous schemes (e.g., `intent://` with unsafe reflection components) to prevent intent redirection attacks.

## 4. Permission & File Chooser Safety
- Check and request `CAMERA` and `READ_EXTERNAL_STORAGE` permissions dynamically before launching file picker sheets.
- Delete temporary camera capture files after successful multipart upload completion.
