# Self-Hosted Minecraft Server

A self-hosted 24/7 modded Minecraft server deployed and administered on a dedicated mini PC running Ubuntu Server.

The project began as a way to replace a paid Minecraft hosting subscription with a self-managed solution for myself and my friends. I wiped the mini PC, installed Ubuntu Server, deployed the All the Mods 10 (ATM10) modpack, and configured the system to operate as a continuously running service.

The project developed into a practical Linux infrastructure and server administration project involving:

- Linux system administration
- Java and JVM configuration
- NeoForge server deployment
- SSH and SCP remote administration
- systemd service management
- Network configuration and port forwarding
- RCON remote console administration
- Resource monitoring and management
- Troubleshooting and technical problem-solving

## Key Highlights

- Migrated from third-party Minecraft hosting to self-hosted infrastructure
- Deployed a heavily modded Minecraft server on dedicated physical hardware
- Configured Java 21 and optimised JVM memory allocation
- Created a systemd service for automatic startup and restart handling
- Configured remote access through router port forwarding
- Diagnosed and resolved real-world network connectivity issues
- Implemented RCON for remote Minecraft administration
- Used Linux monitoring tools to investigate server resource usage
- Documented the deployment, networking, and server management processes

## Documentation

Detailed documentation is available in the `docs/` directory:

- [Deployment Guide](docs/deployment.md) - Server installation and deployment process
- [Networking and Troubleshooting](docs/networking.md) - Network configuration, port forwarding, and connectivity troubleshooting
- [Server Management](docs/server-management.md) - systemd, RCON, monitoring, backups, and ongoing administration

## Architecture

![Minecraft server running as a systemd service](docs/images/systemd-status.png)

```text
                         Internet
                            │
                            │ TCP :25565
                            ▼
                    ┌─────────────────┐
                    │   TP-Link       │
                    │   Router        │
                    │                 │
                    │ Port Forwarding │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Ubuntu Mini PC  │
                    │                 │
                    │    systemd      │
                    │       │         │
                    │       ▼         │
                    │  ATM10 Server   │
                    │       │         │
                    │    NeoForge     │
                    │       │         │
                    │     Java 21     │
                    └─────────────────┘

Administration:
Windows PC ──SSH──► Ubuntu Mini PC
                     │
                     └──RCON──► Minecraft Server
```

## Hardware

The server is hosted on a dedicated mini PC running continuously on my home network.

The mini PC is used as a headless server, allowing the Minecraft server to operate in the background without requiring a graphical desktop environment or an active SSH session.

This system provides sufficient system resources to run the heavily modded ATM10 server continuously alongside Ubuntu Server and its supporting services.

## Software Stack

| Component | Technology |
|---|---|
| Operating System | Ubuntu Linux |
| Minecraft Version | 1.21.1 |
| Modpack | All the Mods 10 (ATM10) |
| Mod Loader | NeoForge |
| Java | Java 21 |
| Server Management | systemd |
| Remote Administration | SSH |
| File Transfer | SCP |
| Remote Console | RCON / mcrcon |
| Network Access | TCP port forwarding |

## Deployment

The server was deployed manually on a dedicated Ubuntu Server mini PC using SSH and SCP.

The deployment involved installing Java 21 and NeoForge, transferring the ATM10 server files, configuring JVM memory allocation, and setting up systemd to manage the server as a persistent background service.

For the full deployment process, see the [Deployment Guide](docs/deployment.md).

## Resource Management

The server is configured with an 8 GB initial JVM heap and a 12 GB maximum heap.

Linux monitoring tools such as `free -h` and systemd service status were used to monitor resource usage and ensure the mini PC could support the heavily modded server environment.

See [Server Management](docs/server-management.md) for more details.

## Server Management

The Minecraft server is managed as a dedicated `systemd` service, allowing it to run independently of an SSH session, start automatically with Ubuntu, and restart automatically after unexpected failures.

The server can be remotely administered using SSH and RCON.

See [Server Management](docs/server-management.md) for the full management and monitoring process.

## Networking

The server is hosted on a private home network while allowing authorised external players to connect through router port forwarding.

The mini PC uses a reserved private IP address, while Minecraft listens on all available network interfaces.

See [Networking and Troubleshooting](docs/networking.md) for details on the network configuration and connectivity testing.

## Security

Sensitive information such as RCON passwords, SSH credentials, private keys, router credentials, and IP addresses has been excluded from this repository.

The RCON port is not exposed to the public internet and is accessed locally through SSH.

## Future Plans

Potential future uses for the mini PC include:

- Learn Docker and containerisation
- Host additional self-hosted services
- Experiment with virtualisation
- Explore personal cloud storage
- Experiment with self-hosted AI and machine learning applications

## Project Outcome

The project successfully replaced a paid Minecraft hosting service with a self-managed 24/7 server.

## Credits

This project uses the [All the Mods 10 (ATM10)](https://www.curseforge.com/minecraft/modpacks/all-the-mods-10) modpack developed by the ATMTeam.

The ATM10 modpack and its associated content are not included in this repository. The server was installed using the official server files provided through CurseForge.

The final setup allows myself and my friends to connect remotely while the server operates independently in the background.

The project also provided practical experience in Linux administration, networking, remote management, system services, and troubleshooting, while serving as a foundation for further self-hosting and server administration projects.
