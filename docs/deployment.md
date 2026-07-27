## Server Deployment

The server was deployed manually using SSH and SCP.

The general deployment process involved:

1. Preparing the Ubuntu mini PC.
2. Creating a dedicated `minecraft` Linux user.
3. Creating a dedicated server directory.
4. Transferring the ATM10 server files to the mini PC using SCP.
5. Installing and configuring Java 21.
6. Installing and configuring NeoForge.
7. Configuring JVM memory allocation.
8. Configuring Minecraft server properties.
9. Setting up RCON for remote console management.
10. Configuring router port forwarding.
11. Testing remote connectivity.
12. Creating a systemd service to manage the server as a background process.

# Server Deployment Guide

This document outlines the process used to deploy the self-hosted All the Mods 10 (ATM10) Minecraft server on a dedicated Ubuntu Server mini PC.

## 1. Prepare the Server

The mini PC was wiped and configured with Ubuntu Server to provide a dedicated environment for hosting the Minecraft server.

The system was configured to operate as a headless server, allowing it to run continuously without requiring a graphical desktop environment.

## 2. Create a Dedicated Minecraft User

A dedicated Linux user was created to run the Minecraft server.

This prevents the server from running with root privileges and provides a clear separation between the Minecraft server and the rest of the operating system.

The server files and processes are owned by the `minecraft` user.

## 3. Create the Server Directory

A dedicated directory was created to contain the Minecraft server installation.

The final server installation is located at:

```text
/opt/minecraft/server-atm10
```

The directory contains the NeoForge installation, Minecraft server configuration, mods, and server startup scripts.

## 4. Install Java

ATM10 requires Java 21.

Java 21 was installed and verified on the Ubuntu server before starting the Minecraft server.

The Java version can be checked using:

```bash
java -version
```

## 5. Transfer the Server Files

The ATM10 server files were transferred from a Windows PC to the Ubuntu server using SCP.

The server files were initially transferred to `/tmp` before being extracted and installed into the dedicated server directory.

SCP provided a convenient way to securely transfer large server files over SSH without requiring additional file-sharing software.

## 6. Install NeoForge

The ATM10 server uses NeoForge as its mod loader.

The server startup script automatically installs the required NeoForge server environment if the required libraries are not already present.

The server was configured to use NeoForge version `21.1.241`.

The startup script was then used to initialise the server environment and generate the required libraries and configuration files.

## 7. Configure the Server

The Minecraft server was configured using `server.properties`.

Key configuration decisions included:

- Minecraft port: `25565`
- View distance: 12 chunks
- Simulation distance: 8 chunks
- Maximum players: 20
- Difficulty: Hard
- Game mode: Survival
- RCON enabled for remote administration
- RCON port: `25575`
- Flight enabled to support the modded environment

The server IP was intentionally left blank, allowing Minecraft to listen on the available network interfaces.

A sanitised example configuration is available in:

```text
config/server.properties.example
```

## 8. Configure JVM Memory

The Java Virtual Machine was configured to allocate:

```text
-Xms8G
-Xmx12G
```

This gives the Minecraft server an initial Java heap allocation of 8 GB and allows it to use up to 12 GB when required.

Additional JVM options were configured to use the G1 garbage collector and tune garbage collection behaviour for the heavily modded server environment.

A sanitised example configuration is available in:

```text
config/user_jvm_args.txt.example
```

## 9. Configure RCON

RCON was enabled to allow remote Minecraft console administration.

The RCON configuration uses:

- RCON enabled: `true`
- RCON port: `25575`

The RCON password is not stored in this repository.

RCON administration is performed locally on the server through SSH rather than exposing the RCON port to the public internet.

## 10. Create a systemd Service

A dedicated systemd service was created to manage the Minecraft server as a background process.

The service:

- Runs under the dedicated `minecraft` user
- Starts the server automatically
- Restarts the server if it exits unexpectedly
- Waits 10 seconds before restarting
- Allows the server to shut down gracefully
- Starts automatically as part of the normal system boot process

The sanitised service configuration is available in:

```text
systemd/minecraft-atm10.service
```

## 11. Configure Automatic Startup

The systemd service was enabled so that the Minecraft server starts automatically when the Ubuntu system boots.

This allows the server to operate continuously without requiring an administrator to manually start it after every reboot.

## 12. Configure Networking

The mini PC was assigned a reserved private IP address through the router.

The TP-Link router was configured with a port forwarding rule to forward TCP traffic on port `25565` to the mini PC.

This allows players outside the local network to connect to the Minecraft server.

The network configuration was tested using both local and external connections.

## 13. Verify the Server

The server can be checked using systemd:

```bash
sudo systemctl status minecraft-atm10
```

The Minecraft listening port can be verified using:

```bash
sudo ss -tulpn | grep 25565
```

Available system memory can be checked using:

```bash
free -h
```

Live server logs can be monitored using:

```bash
sudo journalctl -u minecraft-atm10 -f
```

These checks help verify that the server is running correctly, listening for connections, and has sufficient system resources.

## 14. Final Deployment

Once the server was configured and tested, the system was able to operate independently as a 24/7 self-hosted Minecraft server.

Players can connect remotely while the server runs in the background as a systemd service, without requiring an active SSH session.

The resulting architecture provides a self-managed alternative to third-party Minecraft hosting while providing practical experience with Linux administration, networking, Java configuration, service management, and troubleshooting.
