---
name: "scheme-security-guardrail"
description: "Filtering URL navigation, validating deeplink schemes, and preventing intent redirection vulnerabilities."
---

# Scheme Security Guardrail Skill

## Overview
Protects mobile users from malicious intent redirects, unauthorized app launching, and SSL spoofing.

## Security Sequence
1. **URL Inspection**: Inspect every outgoing request in `shouldOverrideUrlLoading`.
2. **Scheme Validation**: Whitelist standard schemes (`http`, `https`, `file`) and verified apps (`alipays`, `weixin`).
3. **Intent Sanitization**: Disallow raw `intent://` schemes that expose internal unexported components.
4. **SSL Error Gating**: Prompt users with explicit security warnings before proceeding on certificate errors.
