---
name: wecom-install
description: Install, configure, validate, and troubleshoot the OpenClaw WeCom plugin in Bot mode or Agent mode. Use when the user needs WeCom channel setup, credentials wiring, callback configuration, access policy setup, cron delivery, or diagnosis of WeCom connection failures.
license: MIT
metadata:
  openclaw:
    emoji: "🧩"
    category: "integration"
    tags: ["wecom", "openclaw", "plugin", "install", "troubleshooting"]
---

# WeCom Install

Set up the OpenClaw WeCom plugin from zero to working message delivery.

## Core Rules

1. Start with Bot mode unless the user explicitly needs Agent-mode callbacks or cron delivery.
2. Do not save Agent-mode callback settings in the WeCom admin console before the OpenClaw gateway is already running.
3. Validate after every major step instead of batching all changes to the end.
4. Separate installation problems from credential problems and from network problems.
5. Use the reference docs for checklists and diagnostics instead of repeating every detail in this file.

## Preflight Check

Confirm all of the following:

- `openclaw --version` is at least `2026.3.28`
- `node --version` is at least `18`
- the operator has WeCom admin access
- Agent mode has a reachable public URL if callbacks are required

If any precondition fails, stop and fix it before continuing.

## Default Workflow

### 1. Choose the mode

- Use **Bot mode** for the fastest personal setup.
- Use **Agent mode** when the user needs callback-based messaging, enterprise deployment, or cron delivery.
- Use **dual mode** only when both are intentionally required.

### 2. Install the plugin

Prefer one of these commands:

```bash
npx -y @wecom/wecom-openclaw-cli install
openclaw plugins install @wecom/wecom-openclaw-plugin
```

If installation fails, retry with:

```bash
npx -y @wecom/wecom-openclaw-cli install --force
```

### 3. Configure credentials

For Bot mode, configure:

- `channels.wecom.botId`
- `channels.wecom.secret`
- `channels.wecom.enabled`

For Agent mode, configure:

- `channels.wecom.agent.corpId`
- `channels.wecom.agent.corpSecret`
- `channels.wecom.agent.agentId`
- `channels.wecom.agent.token`
- `channels.wecom.agent.encodingAESKey`

### 4. Restart and validate

Restart the gateway after configuration changes:

```bash
openclaw gateway restart
openclaw gateway status
```

Validation is successful when WeCom shows as connected and a live test message receives a reply.

### 5. Configure policy and cron

Only after the channel is working:

- set `dmPolicy` and `groupPolicy`
- add allowlists if needed
- configure cron delivery if Agent mode is enabled

## Failure Handling

- Plugin missing: reinstall the plugin.
- WebSocket or Bot failures: verify `botId`, `secret`, and access to `openws.work.weixin.qq.com`.
- Callback verification failures: restart the gateway first, then re-save the callback URL in WeCom admin.
- No replies in DM or groups: inspect `dmPolicy`, `groupPolicy`, and allowlists.
- Cron not delivered: confirm Agent mode is enabled and outbound delivery works.

## References

Read these files when you need the detailed steps instead of re-deriving them:

- `references/installation-checklist.md`
- `references/troubleshooting.md`

Use the checklist for guided setup and the troubleshooting doc for symptoms, error codes, and recovery commands.
