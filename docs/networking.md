# Network Configuration and Troubleshooting

This document outlines how the self-hosted Minecraft server was made accessible to players outside the local network, as well as the troubleshooting process used to diagnose initial connectivity issues.

## Network Architecture

The Minecraft server operates on a private home network.

The general network path for an external player is:

```text
External Player
      │
      │ TCP :25565
      ▼
Internet
      │
      ▼
TP-Link Router
      │
      │ Port Forwarding
      ▼
Ubuntu Mini PC
      │
      ▼
Minecraft Server
```

Administration is performed separately through SSH:

```text
Windows PC
    │
    │ SSH
    ▼
Ubuntu Mini PC
    │
    │ Local RCON
    ▼
Minecraft Server
```

## Private IP Address Reservation

The Ubuntu mini PC was assigned a reserved private IP address through the TP-Link router.

This ensures that the mini PC continues to receive the same local IP address, allowing the port forwarding configuration to consistently target the correct device.

Without a reserved address, the mini PC could potentially receive a different private IP address after a network restart, causing the port forwarding rule to stop working.

## Port Forwarding

The TP-Link router was configured with a port forwarding rule for the Minecraft server.

The configuration forwards:

```text
Protocol: TCP
External Port: 25565
Internal Port: 25565
Destination: Ubuntu Mini PC
```

This allows incoming Minecraft connections from outside the local network to reach the Minecraft server.

The Minecraft server itself is configured to listen on port `25565`.

The `server-ip` setting is intentionally left blank so that Minecraft can listen on the available network interfaces.

## Verifying the Minecraft Server

Before troubleshooting external connectivity, the server was checked to confirm that Minecraft was actively listening on the expected port.

The following command was used:

```bash
sudo ss -tulpn | grep 25565
```

The server returned a listening socket on port `25565`, confirming that the Minecraft process was accepting connections.

The server was listening on all available network interfaces rather than being restricted to a specific private IP address.

## Firewall Investigation

The Ubuntu firewall status was checked using:

```bash
sudo ufw status
```

The firewall was inactive, meaning Ubuntu's UFW configuration was not blocking incoming connections.

This helped narrow down the problem to the network configuration between the internet and the Ubuntu server.

## Diagnosing `getsockopt` Connection Errors

When external players initially attempted to connect, they encountered a Minecraft `getsockopt` connection error.

The issue was investigated systematically rather than assuming the Minecraft server itself was at fault.

The following areas were checked:

1. Whether the Minecraft server was running.
2. Whether the server was listening on TCP port `25565`.
3. Whether the server was listening on all network interfaces.
4. Whether the Ubuntu firewall was blocking connections.
5. Whether the mini PC had a stable private IP address.
6. Whether the router had a correct port forwarding rule.
7. Whether external connectivity could be established.

The Minecraft server was confirmed to be listening correctly and the Ubuntu firewall was inactive.

The issue was ultimately resolved by configuring port forwarding on the TP-Link router to direct incoming TCP traffic on port `25565` to the reserved private IP address of the Ubuntu mini PC.

## Testing

After configuring port forwarding, external players were able to connect successfully to the Minecraft server.

The network configuration was tested from both the local network and external networks to verify that the server was accessible through the intended connection path.

This confirmed that:

- The Minecraft server was running correctly.
- The server was listening on the correct port.
- The Ubuntu system was not blocking the connection.
- The router was correctly forwarding incoming traffic.
- The server was accessible from outside the local network.

## Security Considerations

Only the Minecraft server port is exposed to the public internet.

The RCON port is not exposed externally and is instead accessed locally through the Ubuntu server.

SSH is used for remote administration of the Ubuntu system.

Sensitive network information, including private IP addresses, public IP addresses, router credentials, and authentication credentials, is intentionally excluded from this repository.

## Key Learning Outcomes

This process provided practical experience with:

- Private and public IP addressing
- TCP ports
- Router port forwarding
- Network address translation
- Linux network diagnostics
- Firewall investigation
- SSH administration
- Systematic network troubleshooting
- Diagnosing connectivity issues across multiple network layers