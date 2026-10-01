---
name: "file-upload-chooser-interceptor"
description: "Managing HTML5 file upload inputs, Android FileProvider permissions, and camera photo capture intents."
---

# File Upload Chooser Interceptor Skill

## Overview
Intercepts `<input type="file">` DOM triggers, presenting modern bottom-sheet pickers and handling camera intents.

## Workflow
1. **Permission Check**: Verify `CAMERA` and `READ_EXTERNAL_STORAGE` permissions prior to launching intents.
2. **FileProvider URI Generation**: Generate secure `content://` URIs using `AgentWebFileProvider`.
3. **Intent Dispatch**: Launch the Android system file chooser or camera intent based on user preference.
4. **Callback Resolution**: Forward selected file URIs to WebChromeClient's `ValueCallback<Uri[]>`.
