---
name: openclaw-gateway
description: Install and operate the OpenClaw personal AI assistant: run the Gateway, send messages through WhatsApp, Telegram, Slack, Discord and other channels, run agent turns and approve DM pairings from the openclaw CLI. Use when a user wants a self-hosted multi-channel assistant.
---

# OpenClaw gateway and CLI

OpenClaw is a personal AI assistant you run on your own devices. A local Gateway is the control plane; the `openclaw` CLI starts it, sends messages through connected channels and runs agent turns. This repo is a copy of the OpenClaw project (npm package `openclaw`, MIT).

## Install

Needs Node 22 or newer.

```
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

`onboard` is the setup wizard (gateway, workspace, channels, skills). `--install-daemon` installs the Gateway as a user service (launchd or systemd). Run `openclaw doctor` after installing or upgrading to catch misconfiguration, including risky DM policies.

Model access: an Anthropic or OpenAI subscription (OAuth) or provider API keys. For `--local` runs the keys must be exported in your own shell. Never paste keys into the repo.

## Run and use

```
openclaw gateway --port 18789 --verbose
openclaw message send --target +15555550123 --message "Hello from OpenClaw"
openclaw agent --message "Ship checklist" --thinking high
```

- `message send` options: `-t, --target <dest>` (required), `-m, --message <text>`, `--channel <channel>`, `--account <id>`, `--media <path-or-url>`, `--dry-run` (print the payload without sending), `--json`, `--verbose`. Use `--dry-run` first when the target is a real person.
- `agent` options: `-m, --message <text>` (required), `-t, --to <number>`, `--session-id <id>`, `--agent <id>`, `--thinking <off|minimal|low|medium|high>`, `--channel <channel>`, `--deliver` (send the reply to the channel), `--local` (embedded agent, needs provider keys in your shell), `--json`, `--timeout <seconds>`.
- `openclaw update --channel stable|beta|dev` switches release channel.

## DM pairing and security

Inbound DMs are untrusted input. By default an unknown sender gets a pairing code and the bot ignores the message until you approve it:

```
openclaw pairing approve <channel> <code>
```

Public inbound DMs need an explicit opt-in (`dmPolicy="open"` plus `"*"` in the channel allowlist). Do not enable that without the user's explicit instruction.

## Notes

- Sending a message through a channel contacts a real person or group. Confirm the target and text with the user first.
- The repo also bundles skills under `skills/` (for example `github`, `weather`, `tmux`, `summarize`, `coding-agent`) that the assistant loads itself; they are not installed by this plugin.
- Full docs: https://docs.openclaw.ai and `docs/` in the repo.
