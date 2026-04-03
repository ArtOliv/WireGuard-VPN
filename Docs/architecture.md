# System Architecture

## Descrição

A arquitetura desse sistema é de acesso remoto seguro baseada em um modelo hub-and-spoke utilizando WireGuard.

O VPS atua como ponto central (hub), enquanto os clientes e o gateway da rede local (Raspberry Pi) atuam como spokes.

---

## Componentes

### 1. VPS (nó público)

- Possui IP público
- Atua como ponto central da VPN
- Responsável por rotear tráfego entre peers
- Não realiza NAT (apenas forwarding)

---

### 2. Raspberry Pi (Gateway)

- Localizado dentro da rede privada
- Atua como ponte entre VPN e LAN
- Responsável por:
  - NAT (MASQUERADE)
  - Encaminhamento de pacotes

---

### 3. Clientes

- Dispositivos externos (notebook, PC, etc.)
- Conectam ao VPS via WireGuard
- Acessam recursos da LAN através do túnel

---

## Design da rede

- Sub-rede VPN: 10.10.0.0/24
- Sub-rede LAN: 192.168.0.0/24

---

## Estratégia de roteamento

- VPS mantém rotas para:
  - Clientes (IPs individuais)
  - LAN (via Raspberry)

- Raspberry encaminha tráfego da VPN para a LAN via NAT

---

## Restrições

- CGNAT impede conexões diretas
- VPS resolve esse problema como relay