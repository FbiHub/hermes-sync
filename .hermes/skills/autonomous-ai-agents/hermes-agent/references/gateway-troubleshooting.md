### Gateway Connection Failures (Cross-Origin/Routing)
If the front-end shows "Failed to send message" or "No models available" despite the gateway being active:
1. Verify `API_SERVER_KEY` match: Ensure the key in `~/.hermes/config.yaml` (`gateway.api_server_key`) is identical to the one in the workspace's `.env` (`API_SERVER_KEY`).
2. Verify Host Binding: Check `HOST` in the workspace `.env`. For cross-network or containerized access, it must be `HOST=0.0.0.0` (not `127.0.0.1`).
3. Port Conflicts: Ensure ports 3000, 3001, and 9119 are not blocked.
   - Use `netstat -tulpn | grep -E ":3000|:3001|:9119"` to check.
   - Kill lingering processes if necessary.
4. Gateway Cycle: Restart the middleware to pick up the new bind: `sudo systemctl restart hermes-gateway`.

2. **Synchronize Config:**
   - In `~/.hermes/config.yaml`:
     ```yaml
     gateway:
       api_server_enabled: true
       api_server_host: 127.0.0.1
       api_server_port: 8642
     ```
   - In `~/.hermes/.env`:
     ```bash
     API_SERVER_ENABLED=true
     API_SERVER_KEY=secret_key_12345
     ```
3. **Restart Service:** `sudo systemctl restart hermes-gateway`.
4. **Verify Binding:** `ss -tulpn | grep :8642`.

## Workspace Integration
Ensure your `hermes-workspace/.env` has:
`HERMES_API_URL=http://127.0.0.1:8642`
`API_SERVER_KEY=secret_key_12345`
