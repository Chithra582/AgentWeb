# EXPLAINABILITY — AgentWeb Runtime Engine

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* AgentWeb Runtime Engine (`agentweb-runtime-engine`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Android WebView Agent Runtime & Framework  

---

## 1. Overview & Operational Purpose

The **AgentWeb Runtime Engine** (`agentweb-runtime-engine`) is an autonomous, high-performance Android WebView agent runtime library. Embedded web views are essential to modern mobile applications, yet developers routinely battle critical issues: severe memory leaks, fragile JavaScript-to-Java bridges, broken file upload pickers, video fullscreen crashes, and hijacked navigation schemes.

AgentWeb resolves these challenges through a modular, chain-of-responsibility middleware architecture. The agent manages the entire WebView lifecycle in lockstep with Android Activities and Fragments, injects secure bidirectional JavaScript interfaces, intercepts and manages HTML5 file/camera uploads, and guards against malicious URL schemes and intent redirection vulnerabilities.

---

## 2. How the Agent Decides (Decision-Making Logic)

AgentWeb Runtime Engine operates across a deterministic, multi-stage decision pipeline:

```
[Navigation Request / Intent] ──> [Scheme & SSL Security Gate] ──> [Middleware Filter Chain]
                                                                                │
                                                                                ▼
[Rendered DOM / Native Action] <── [Bridge & File Interceptor] <── [WebView Lifecycle State]
```

### 2.1 Lifecycle State Synchronization & Memory Management
- **Decision:** Determines when to pause rendering, suspend JavaScript execution, or release native WebView memory.
- **Rules:**
  - Evaluates host Activity/Fragment lifecycle events; pauses web timers and video playback during `onPause()`.
  - Resumes active timers and WebSocket streams immediately upon `onResume()`.
  - Automatically unbinds parent ViewGroup references and invokes `destroy()` during `onDestroy()` to prevent memory leaks.

### 2.2 JavaScript Bridge Injection & Origin Validation
- **Decision:** Decides which native Java/Kotlin capabilities are exposed to DOM scripts based on target URL origins.
- **Rules:**
  - Injects native interfaces only into web pages originating from whitelisted HTTPS domains.
  - Requires explicit `@JavascriptInterface` method annotations before permitting JavaScript invocation.
  - Executes asynchronous JavaScript callbacks via `evaluateJavascript` without freezing Android's main looper.

### 2.3 File Chooser & Camera Upload Interception
- **Decision:** Determines whether an HTML5 file request requires gallery selection, file browsing, or camera capture.
- **Rules:**
  - Inspects MIME types (`image/*`, `video/*`, `*/*`); prompts the user with an intuitive native selection sheet.
  - Verifies runtime permissions (`CAMERA`, `READ_EXTERNAL_STORAGE`) before launching Android system intents.
  - Generates secure `content://` URIs via `AgentWebFileProvider` and returns results to `ValueCallback<Uri[]>`.

### 2.4 URL Scheme Filtering & Deep Link Redirection
- **Decision:** Evaluates whether an outbound navigation link should be loaded in WebView or delegated to native apps.
- **Rules:**
  - Standard `http://` and `https://` URLs load within the embedded WebView container.
  - Authorized payment schemes (`alipays://`, `weixin://`) dispatch validated native Android Intents.
  - Blocks unknown, unverified, or reflection-based `intent://` schemes to prevent intent hijacking vulnerabilities.

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
| :--- | :--- | :--- | :--- |
| Navigation URLs & DOM Requests | Host Android application / Web server | Directs page loading and web asset rendering | Loaded locally or over encrypted HTTPS |
| User Uploaded Files & Photos | Native camera / Android gallery | Fulfills `<input type="file">` HTML5 upload requests | Handled via secure FileProvider, temporary files cleared |
| JavaScript Bridge RPC Payloads | Web application running inside WebView | Bridges web events to native mobile device features | Sanitized in memory, validated against schema |
| WebView Telemetry & Errors | Android WebChromeClient / WebViewClient | Monitors page loading progress and SSL error states | Processed locally on device, no PII retained |

AgentWeb Runtime Engine complies with operational security and privacy standards:
- **No Cloud Data Exfiltration:** All WebView state transitions, file selections, and JavaScript bridge dispatches execute strictly on the local device.
- **Epistemic Isolation:** Web storage, cookies, and cache partitions are isolated per application sandbox.
- **Sanitized Model Payloads:** Bridge responses and telemetry exclude personal identifiers and sensitive camera file paths.
- **Data Minimization:** Only requested file URIs and necessary parameter arguments are transferred across the native-web boundary.

---

## 4. Known Limitations & Failure Modes

Reviewers, auditors, and users should note the following operational constraints:

1. **Unpadded ConstraintLayout Parent Restrictions:**
   - *Limitation:* Placing AgentWeb directly inside unconstrained `ConstraintLayout` roots can cause layout measurement glitches.
   - *Mitigation:* Explicit recommendation to wrap AgentWeb containers inside standard `FrameLayout` or `LinearLayout` viewgroups.

2. **Android OS FileProvider Incompatibilities:**
   - *Limitation:* Different Android API levels (Android 7.0 vs. Android 11+ Scoped Storage) handle file URIs differently.
   - *Mitigation:* Unified `AgentWebFileProvider` abstracts Scoped Storage APIs, providing consistent `content://` URIs.

3. **Global WebView Pause Side Effects:**
   - *Limitation:* Calling `onPause()` on older Android versions pauses all WebViews within the application process.
   - *Mitigation:* Scoped lifecycle management ensuring individual view instances manage their own render threads.

4. **SSL Certificate Error Interception:**
   - *Limitation:* Invalid or self-signed certificates on external URLs can halt navigation.
   - *Mitigation:* Configurable SSL error handler alerting users with actionable security dialogs.

---

## 5. Verification, Safety & Human Oversight

- **Real-Time Human Approval Gate:** Launching external payment applications or accessing camera hardware mandates user permission.
- **Emergency Session Interrupt:** Users and host applications can instantly abort loading, destroy WebViews, or clear session cookies.
- **Step Quota Guardrails:** Strict page load timeouts and redirection depth counters prevent infinite URL redirect loops.
- **Structured Audit Logging:** Every navigation override, JavaScript invocation, file selection, and SSL event is logged via Android Logcat.
