---
name: production-server-lifecycle
description: Use when deploying, maintaining, or troubleshooting production Node.js servers in the workspace (npm run prod). Orchestrates build → port-clearance → execution.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [production, node, server, deployment, troubleshooting]
    related_skills: [hermes-gateway-troubleshooting]
---

# Production Server Lifecycle

## Overview
Workflow for reliably starting and maintaining the production workspace server. Handles the common pitfalls of port collisions and build artifacts.

## The Standard Production Loop
1. **Clear Port**: Always ensure the target port (e.g., 3000) is free.
2. **Build**: Build the production environment.
3. **Execute**: Start the production server entry point.

## Workflow
```bash
# 1. Clear hanging processes
ss -tulpn | grep :3000
kill -9 <PID>

# 2. Build
npm run build

# 3. Start
# Use max-old-space to manage memory limits
# If non-loopback IP (0.0.0.0) is used, ensure HERMES_PASSSWORD is set or HERMES_ALLOW_INSECURE_REMOTE=1 is used
NODE_ENV=production PORT=3000 node --max-old-space-size=2048 server-entry.js
```

## Common Pitfalls
1. **Ghost Processes**: Port 3000 is still bound after `kill`. Force-kill and wait.
2. **Missing Artifacts**: The production server requires `dist/server/server.js`. If missing, `npm run build` MUST be run.
3. **Memory Limits**: `exit code 137` is an OOM. Use `--max-old-space-size=2048`.
5. Authentication/Gateway Synchronization: If the dashboard connects but shows authorization errors (blinking/no response), ensure `API_SERVER_KEY` in `WORKSPACE_DIR/.env` matches `API_SERVER_KEY` in `~/.hermes/.env`. If unauthorized, use `cat` to compare strings directly.
6. Persistent Port Conflicts: If `PORT=3001` or others continue showing as "in use" despite `kill`, the process is likely managed by a persistent manager like `pm2`, or a sub-process is holding the port. Use `ss -tulpn | grep :<port>` to identify the lingering PID and use `ps o ppid= <PID>` to identify the parent before killing. If child processes respawn immediately, verify the process manager state (e.g., `pm2 list`) and terminate the parent chain first.
6. **Workspace/Gateway Security**: Setting `HOST=0.0.0.0` or other non-loopback IPs imposes security requirements in `server-entry.js`. Ensure `HERMES_PASSWORD` is set OR `HERMES_ALLOW_INSECURE_REMOTE=1` is exported to prevent immediate process exit.

## Verification Checklist
- [ ] `curl -I http://127.0.0.1:3000/chat` returns 200.
- [ ] `ps -ef | grep node` shows the server process.
- [ ] No multiple processes bound to the same port.
---
