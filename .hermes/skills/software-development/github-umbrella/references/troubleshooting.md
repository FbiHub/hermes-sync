# PITFALLS:
- **Port Conflicts:** If `pnpm dev` fails with "Port 3001 is already in use", check for existing node instances (`ss -tulpn | grep :3001` or `ps aux | grep node`) and terminate orphaned processes before restarting.
- **Out of Memory (OOM):** On constrained systems (~4GB RAM), `vite` dev servers often exceed memory limits (Error 137). Always enforce memory limits using `NODE_OPTIONS="--max-old-space-size=768"`.
- **Environment Bindings:** For cross-origin/Tailscale connectivity, ensure `HOST=0.0.0.0` is explicitly set in `.env` rather than `127.0.0.1`.
- **Credential Sync:** If automated pushes fail (403), verify `gh auth status` scopes and remote origin protocol (HTTPS vs SSH) are aligned with repository access.
- **Background Processes:** Use `terminal(background=true, notify_on_complete=true)` for long-lived development servers so they are tracked correctly and notifications are triggered upon exit (like OOM events).
