# VPS Setup (WireGuard Hub)

## Objetivo

Configurar o VPS como nó central (hub) da VPN.

Ele será responsável por:
- Receber conexões dos clientes
- Receber conexão do Raspberry
- Rotear tráfego entre peers

---

## Papel do VPS na arquitetura

- Possui IP público
- Atua como relay entre clientes e Raspberry
- NÃO realiza NAT
- Apenas encaminha pacotes

---

## Configuração

Eu utilizei uma máquina virtual da **Oracle Cloud** pois possui IP público, essa configuração foi feita concetando na máquina via SSH.

Crie um arquivo **wg0.conf** em **/etc/wireguard/** e usando como base o arquivo [wg0.conf](../Configs/VPS/wg0.conf) disponibilizado, segue-se os passos:

### 1. Geração de chaves

```bash
wg genkey | tee privatekey | wg pubkey | tee publickey
```

Guarde:

- PrivateKey -> usada no VPS
- PublicKey -> usada nos peers

### 2. Habilitar roteamento

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Persistente:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

### 3. Firewall do Oracle Cloud

Na plataforma, configure o firewall da máquina para permitir a entrada:

```
- Protocolo: UDP
- Porta: 65100
- Origem: 0.0.0.0/0
```

---

## Subir a Interface

```bash
sudo wg-quick up wg0
```

---

## Verificação

```bash
sudo wg
```

Você deve ver:

- Peers conectados
- Handshake ativo

---

## Observações

- VPS apenas decide para qual peer enviar pacotes
- AllowedIPs funciona como tabela de roteamento interno no WireGuard