# Raspberry Pi Setup (Gateway da LAN)

## Objetivo

Configurar o Raspberry como gateway entre:

- VPN (10.10.0.0/28)
- LAN (192.168.100.0/24)

Ele será responsável por:
- Receber tráfego da VPN
- Traduzir (NAT)
- Encaminhar para a rede local

---

## Papel do Raspberry

- Peer do VPS
- Bridge entre VPN e LAN
- Executa NAT

---

## Configuração

Crie um arquivo **wg0.conf** em **/etc/wireguard/** e usando como base o arquivo [wg0.conf](../Configs/VPN/wg0.conf) disponibilizado, segue-se os passos:

### 1. Geração de chaves

```bash
wg genkey | tee privatekey | wg pubkey | tee publickey
```

Guarde:

- PrivateKey -> usada no Raspberry
- PublicKey -> usada nos peers

### 2. Habilitar roteamento

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Persistente:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

### 3. Interface de saída

Você deve identificar a interface de rede correta:

```bash
ip a
```

Exemplo:

- **wlan0** -> interface wi-fi
- **eth0** -> interface ethernet

E então substitua corretamente no **wg0.conf**:

```bash
-o wlan0 # ou outra
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

- Sem NAT a LAN não sabe como responder ao cliente
- Porque a LAN não reconhece 10.10.0.0/24
- MASQUERADE resolve isso fazendo parecer que o tráfego veio do Raspberry   