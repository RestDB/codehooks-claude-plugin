# Codehooks Claude Plugin

A Claude Code plugin for developing, deploying, and managing [Codehooks.io](https://codehooks.io) serverless backends.

## What it does

This plugin teaches Claude Code how to work with the Codehooks platform. It provides:

- **Auto-detection** — Claude activates Codehooks mode when it detects a `config.json` with `name` and `space` fields
- **CLI integration** — Claude uses `coho prompt`, `coho deploy`, `coho doctor`, and other CLI commands to develop and manage backends
- **Ready-to-use templates** — Example files for common patterns (webhooks, cron jobs, queues, workflows)

## Prerequisites

```bash
npm install -g codehooks
coho login
```

## Installation

Load the plugin locally:

```bash
claude --plugin-dir ./codehooks-claude-plugin
```

Or install from the repository:

```bash
claude plugin install https://github.com/RestDB/codehooks-claude-plugin
```

## Usage

### Automatic (existing project)

Open a directory with a Codehooks `config.json` — Claude will automatically activate the backend skill when you ask it to work on backend tasks.

### Explicit

Run the `/codehooks:backend` command to activate Codehooks mode manually. This is useful for starting new projects or when auto-detection hasn't kicked in.

## Plugin Structure

```
.claude-plugin/plugin.json          Plugin manifest
commands/backend.md                 /codehooks:backend user command
skills/codehooks-backend/SKILL.md   Agent skill (auto-detected)
examples/
  webhook-handler.js                Stripe webhook with signature verification
  daily-job.js                      Cron-scheduled background job
  queue-worker.js                   Async queue processing
  workflow-automation.js            Multi-step workflow with state
```

## License

MIT
