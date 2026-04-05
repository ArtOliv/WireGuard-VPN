# Client Setup

**Languages:** [Portuguese](setup_clients.md) | [English](setup_clients.en.md)

---

## Objective

Allow external clients to access:

- Other VPN peers
- Local network (LAN)

---

## Client Role

- Connects to the VPS
- Sends traffic through the WireGuard tunnel
- Does not need to directly know or communicate with the Raspberry Pi

---

## Configuration

Create a **wg0.conf** file in **/etc/wireguard/** using the provided template [wg0.conf](../Configs/Clients/wg0.conf) as a reference, and follow the steps below:

### 1. Key Generation

```bash
wg genkey | tee privatekey | wg pubkey | tee publickey
```

Store:

- PrivateKey → used on the client
- PublicKey → used by peers

### 2. Enable IP Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Make it persistent:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

## Understanding AllowedIPs

```
0.0.0.0/0 -> all traffic is routed through the tunnel
```

It can also be configured as:

```
10.10.0.0/24 -> communication within the VPN
192.168.0.0/24 -> access to the LAN
```

Therefore, any packet destined for these networks will be routed through the tunnel.

---

## Bring Up the Interface

```bash
sudo wg-quick up wg0
```

---

## Verification

```bash
sudo wg
```

You should see:

- Connected peers
- Active handshake

## Notes

- The client does not communicate directly with the Raspberry Pi
- All traffic flows through the VPS