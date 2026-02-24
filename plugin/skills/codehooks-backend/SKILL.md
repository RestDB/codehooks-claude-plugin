---
name: codehooks-backend
description: Develop, deploy, and manage Codehooks.io serverless backends — REST APIs, webhooks, NoSQL database, scheduled jobs, queue workers, and workflows.
detection: Look for a config.json file in the project root containing "name" and "space" fields — this indicates a Codehooks project.
---

# Codehooks Backend Skill

You are working in a Codehooks.io serverless backend project. Codehooks lets you write JavaScript/TypeScript code and deploy it to a live serverless backend in ~5 seconds.

## Setup (skip if already done)

If the project is already set up (config.json exists with `name` and `space`), skip to **Core Workflow**.

```bash
# Install the CLI
npm install -g codehooks

# Login to Codehooks
coho login

# Create a new project
coho create <project-name>

# Generate an admin token for CI/CD and agent use
coho add-admintoken
```

## Core Workflow

### 1. Load the development context (ALWAYS do this first)

Before writing any code, run:

```bash
coho prompt
```

This outputs the **complete codehooks-js API reference** — routing, database operations, queues, jobs, workflows, and all available methods. Read the full output to understand the API before generating code.

### 2. Understand the project (run this for existing projects)

Before modifying an existing project, get the full picture:

```bash
# Run doctor FIRST — returns JSON with collections, stats, recent deploys, and error logs
# This is the single best command for understanding a project's current state
coho doctor

# Describe the app structure — collections, schemas, queues, files
coho describe

# Get endpoint URLs with cURL examples
coho info --examples
```

`coho doctor` is the most powerful diagnostic command. It returns structured JSON covering database collections with document counts, deployment history, queue and worker status, and recent error logs. Always run it when joining an existing project or debugging issues.

### 4. Deploy

```bash
# Deploy code (~5 seconds to live)
coho deploy
```

### 5. Monitor and debug

```bash
# Stream real-time logs
coho log -f

# Query a database collection
coho query -c <collection> -q 'field=value'

# Check queue status
coho queue-status

# Check workflow status
coho workflow-status
```

### 6. Data operations

```bash
# Import data from JSON file
coho import -c <collection> --file data.json

# Export data from a collection
coho export -c <collection>
```

## Code Patterns

All codehooks-js code follows this structure:

```javascript
import { app, Datastore } from 'codehooks-js';

// Register handlers (routes, jobs, workers, workflows)
app.get('/hello', (req, res) => {
  res.json({ message: 'Hello, world!' });
});

// MANDATORY: bind to serverless runtime
export default app.init();
```

Key patterns:
- **REST routes:** `app.get()`, `app.post()`, `app.put()`, `app.patch()`, `app.delete()`
- **Database:** `const conn = await Datastore.open()` then `conn.insertOne()`, `conn.getMany()`, `conn.updateOne()`, etc.
- **getMany() returns a stream** — use `.toArray()` when you need to manipulate data (sort, filter, map)
- **Webhooks:** Use `req.rawBody` for signature verification (not `req.body`). Bypass auth with `app.auth('/webhook/*', (req, res, next) => next())`
- **Jobs:** `app.job('0 9 * * *', async (_, { jobId }) => { ... })`
- **Workers:** `app.worker('queueName', async (req, res) => { ... })` and enqueue with `conn.enqueue('queueName', payload)`
- **Workflows:** `app.createWorkflow('name', 'description', steps, config)`
- **CRUD:** `app.crudlify({ collection: schema })` for instant REST APIs with validation
- **Static files:** `app.static({ route: '/app', directory: '/public' })` to serve static sites from deployed source
- **File storage:** `app.storage({ route: '/docs', directory: '/uploads' })` to serve uploaded files
- **Auth bypass:** `app.auth('/public/*', (req, res, next) => next())` for public endpoints
- **Environment variables:** Use `process.env.VARIABLE_NAME` for secrets and API keys
- **TypeScript:** Supported out of the box — just use `.ts` files

**Do NOT use:** `fs`, `path`, `os`, or any modules requiring file system access.

## Example Templates

Ready-to-use templates are available in the `examples/` directory of this plugin:

- `examples/webhook-handler.js` — Stripe webhook with signature verification
- `examples/daily-job.js` — Cron-scheduled background job
- `examples/queue-worker.js` — Async queue processing with task enqueue
- `examples/workflow-automation.js` — Multi-step autonomous workflow with retries and state

## Development Loop

1. Run `coho prompt` and read the full API reference
2. Run `coho describe` to understand what's already deployed
3. Write code using codehooks-js patterns
4. Run `coho deploy` (5 seconds to live)
5. Verify with `coho log -f` or test endpoints with `coho info --examples`
6. Iterate — the fast deploy loop enables rapid development

## Resources

- **Documentation:** https://codehooks.io/docs
- **CLI reference:** https://codehooks.io/docs/cli
- **Full AI prompt:** https://codehooks.io/llms.txt
- **Templates:** https://github.com/RestDB/codehooks-io-templates
- **MCP Server:** https://github.com/RestDB/codehooks-mcp-server
