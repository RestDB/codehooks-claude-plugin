---
name: backend
description: Start or activate Codehooks.io backend development mode
---

You are now in Codehooks backend development mode. Follow these steps:

## 1. Check prerequisites

Run `which coho` to check if the Codehooks CLI is installed.

**If not installed**, guide the user:
```bash
npm install -g codehooks
coho login
```

## 2. Check for existing project

Look for a `config.json` file in the current directory. A Codehooks project has a config.json with `name` and `space` fields.

**If no project exists**, ask the user what they want to build, then create one:
```bash
coho create <project-name>
```

## 3. Load development context

Run `coho prompt` and read the full output — this is the complete codehooks-js API reference. You need this to write correct code.

## 4. Understand the current state

If this is an existing project, run:
```bash
coho describe
```

This shows what's already deployed — collections, schemas, queues, and files.

## 5. Ask and build

Ask the user what they want to build. Then write code, deploy with `coho deploy`, and verify with `coho log -f`.
