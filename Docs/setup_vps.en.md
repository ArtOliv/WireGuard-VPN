# VPS Setup (WireGuard Hub)

**Languages:** [Portuguese](setup_vps.md) | [English](setup_vps.en.md)

---

## Objective

Configure the VPS as the central node (hub) of the VPN.

It will be responsible for:
- Receiving connections from clients
- Receiving the connection from the Raspberry Pi
- Routing traffic between peers

---

## VPS Role in the Architecture

- Has a public IP address
- Acts as a relay between clients and the Raspberry Pi
- Does NOT perform NAT
- Only forwards packets

---

## Configuration

A virtual machine from **Oracle Cloud** was used, as it provides a public IP. The configuration is performed by connecting to the machine via SSH.

Create a **wg0.conf** file in **/etc/wireguard/** using the provided template [wg0.conf](../Configs/VPS/wg0.conf) as a reference, and follow the steps below:

### 1. Key Generation

```bash
wg genkey | tee privatekey | wg pubkey | tee publickey
```

Store:

- PrivateKey → used on the VPS
- PublicKey → used by peers

### 2. Enable IP Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Make it persistent:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

### 3. Oracle Cloud Firewall

On the platform, configure the instance firewall to allow inbound traffic:

```
- Protocol: UDP
- Port: 65100
- Source: 0.0.0.0/0
```

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

---

## Notes

- The VPS only determines which peer should receive packets
- AllowedIPs acts as an internal routing table within WireGuard