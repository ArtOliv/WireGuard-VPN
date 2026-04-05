# System Architecture

**Languages:** [Portuguese](architecture.md) | [English](architecture.en.md)

---

## Description

The architecture of this system is a secure remote access solution based on a **hub-and-spoke model** using **WireGuard**.

The VPS acts as the central point (hub), while clients and the local network gateway (Raspberry Pi) act as spokes.

---

## Components

### 1. VPS (Public Node)

- Has a public IP address
- Acts as the central point of the VPN
- Responsible for routing traffic between peers
- Does not perform NAT (forwarding only)

---

### 2. Raspberry Pi (Gateway)

- located inside a private network
- Acts as a bridge between the VPN and the LAN
- Responsible for:
  - NAT (MASQUERADE)
  - Packet forwarding

---

### 3. Clients

- External devices (notebook, PC, etc.)
- Connect to the VPS via WireGuard
- Access LAN resources through the tunnel

---

## Network Design

- VPN subnet: 10.10.0.0/24
- LAN subnet: 192.168.0.0/24

---

## Routing Strategy

- The VPS maintains routes for:
  - Clients (individual IPs)
  - LAN (via Raspberry Pi)

- The Raspberry Pi forwards VPN traffic to the LAN using NAT

---

## Constraints

- CGNAT prevents direct incoming connections
- The VPS solves this by acting as a relay