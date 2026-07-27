# Server Management

This document outlines how the self-hosted Minecraft server is managed and maintained after deployment.

## Service Management

The Minecraft server runs as a dedicated `systemd` service named:

```text
minecraft-atm10
```

This allows the server to run independently of an SSH session and provides automatic startup and restart functionality.

### Start the Server

```bash
sudo systemctl start minecraft-atm10
```

### Stop the Server

```bash
sudo systemctl stop minecraft-atm10
```

### Restart the Server

```bash
sudo systemctl restart minecraft-atm10
```

### Check Server Status

```bash
sudo systemctl status minecraft-atm10
```

### Enable Automatic Startup

The service can be configured to start automatically when Ubuntu boots:

```bash
sudo systemctl enable minecraft-atm10
```

### Disable Automatic Startup

```bash
sudo systemctl disable minecraft-atm10
```

## Monitoring Logs

The server's output can be monitored through the systemd journal.

To view live server logs:

```bash
sudo journalctl -u minecraft-atm10 -f
```

To view recent logs:

```bash
sudo journalctl -u minecraft-atm10
```

This provides a way to monitor the server without directly attaching to the Minecraft process.

## RCON Administration

RCON is used to remotely execute Minecraft console commands.

The server is configured to use RCON on port `25575`.

RCON is not exposed to the public internet. Administrative commands are executed locally on the Ubuntu server, typically after connecting through SSH.

Example:

```bash
mcrcon -H 127.0.0.1 -P 25575 -p 'PASSWORD' "list"
```

RCON can be used for tasks including:

- Checking connected players
- Sending messages to players
- Managing players
- Executing Minecraft commands
- Performing administrative tasks without directly accessing the Minecraft server process

The RCON password is not stored in this repository.

## Resource Monitoring

The server's system resources can be monitored using standard Linux tools.

Available memory can be checked using:

```bash
free -h
```

The Java process can be inspected to determine its current memory usage and JVM configuration.

The Minecraft server's listening port can be verified using:

```bash
sudo ss -tulpn | grep 25565
```

These tools can help identify whether performance or connectivity issues are caused by the Minecraft server, Java process, or underlying operating system.

## Server Backups

Before major changes to the server environment, the existing server data is backed up.

Backups are particularly important before:

- Updating the modpack
- Changing major server configuration
- Replacing the server installation
- Making significant changes to the world
- Migrating to a new server environment

The initial migration from third-party hosting to the self-hosted server was performed only after creating a backup of the existing server.

## Updating the Server

Updates to the modpack or server software should be performed carefully.

A typical update process involves:

1. Stop the Minecraft server.
2. Create a backup of the current server data.
3. Transfer the new server files to the Ubuntu system.
4. Update or replace the required server files.
5. Verify file ownership and permissions.
6. Start the server.
7. Monitor the logs for errors.
8. Test client connectivity.

The server should not be updated while players are actively using it.

## File Ownership

The Minecraft server runs under the dedicated `minecraft` Linux user.

Server files should therefore be owned by the `minecraft` user and group.

Ownership can be checked using:

```bash
ls -la /opt/minecraft/server-atm10
```

If required, ownership can be corrected using:

```bash
sudo chown -R minecraft:minecraft /opt/minecraft/server-atm10
```

This ensures that the Minecraft process can access and modify the files it needs while avoiding unnecessary root privileges.

## Graceful Shutdown

The systemd service is configured to send `SIGINT` when stopping the Minecraft server.

This allows the server to shut down gracefully rather than terminating the Java process immediately.

A graceful shutdown gives Minecraft the opportunity to save world data and close the server correctly.

## SSH Administration

SSH provides remote access to the Ubuntu server for administration.

Typical administrative tasks performed through SSH include:

- Managing the systemd service
- Monitoring server logs
- Checking system resources
- Managing server files
- Transferring new files
- Running RCON commands
- Troubleshooting server issues

The Minecraft server itself does not require an active SSH session to remain online.

## Operational Workflow

A typical server management workflow is:

```text
SSH into Ubuntu Server
        │
        ├── Check system resources
        │
        ├── Check Minecraft service status
        │
        ├── Review logs if required
        │
        └── Use RCON for Minecraft administration
```

This separation allows the Minecraft server to operate continuously while administrative tasks can be performed remotely when required.