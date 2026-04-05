# Raspberry Pi Setup (LAN Gateway)

**Languages:** [Portuguese](setup_raspberry.md) | [English](setup_raspberry.en.md)

---

## Objective

Configure the Raspberry Pi as a gateway between:

- VPN (10.10.0.0/28)
- LAN (192.168.100.0/24)

It will be responsible for:
- Receiving traffic from the VPN
- Translating it (NAT)
- Forwarding it to the local network

---

## Raspberry Pi Role

- Acts as a peer of the VPS
- Serves as a bridge between the VPN and the LAN
- Performs NAT

---

## Configuration

Create a **wg0.conf** file in **/etc/wireguard/** using the provided template [wg0.conf](../Configs/VPN/wg0.conf) as a reference, and follow the steps below:

### 1. Key Generation

```bash
wg genkey | tee privatekey | wg pubkey | tee publickey
```

Store:

- PrivateKey → used on the Raspberry Pi
- PublicKey → used by peers

### 2. Enable IP Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Make it persistent:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

### 3. Outgoing Interface

You must identify the correct network interface:

```bash
ip a
```

Example:

- wlan0 → Wi-Fi interface
- eth0 → Ethernet interface

Then replace it accordingly in the wg0.conf:

```bash
-o wlan0 # or another interface
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

- Without NAT, the LAN does not know how to respond to VPN clients
- This is because the LAN does not recognize the 10.10.0.0/24 network
- MASQUERADE solves this by making the traffic appear as if it originated from the Raspberry Pi