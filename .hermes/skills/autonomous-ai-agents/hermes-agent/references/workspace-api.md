# Workspace Environment Configuration

When integrating external applications (like the Hermes Workspace dashboard) with the Hermes Gateway's API Server:

1. **Server Configuration (`~/.hermes/config.yaml`)**:
   Ensure `gateway.api_server_enabled` is set to `true` and `gateway.api_server_key` is established.
   Example:
   ```yaml
   gateway:
     api_server_enabled: true
     api_server_key: YOUR_CHOSEN_SECRET_KEY
   ```

2. **Client Configuration (Workspace `.env`)**:
   The external application must store the identical key in its local `.env`.
   Example:
   ```env
   API_SERVER_KEY=YOUR_CHOSEN_SECRET_KEY
   ```

**Important**: If you encounter connection failures, verify that both the server-side configuration file and the client-side `.env` contain the exact same key. After changing `config.yaml`, run `/restart` within an active Hermes session (or restart the service) to apply the changes.
