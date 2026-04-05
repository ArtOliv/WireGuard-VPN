# Client Setup

**Languages:** [Portuguese](setup_clients.md) | [English](setup_clients.en.md)

---

## Objetivo

Permitir que clientes externos acessem:

- Outros peers da VPN
- Rede local (LAN)

---

## Papel dos Clientes

- Conecta ao VPS
- Envia tráfego via túnel WireGuard
- Não precisa conhecer o Raspberry diretamente

---

## Configuração

Crie um arquivo **wg0.conf** em **/etc/wireguard/** e usando como base o arquivo [wg0.conf](../Configs/Clients/wg0.conf) disponibilizado, segue-se os passos:

### 1. Geração de chaves

```bash
wg genkey | tee privatekey | wg pubkey | tee publickey
```

Guarde:

- PrivateKey -> usada no cliente
- PublicKey -> usada nos peers

### 2. Habilitar roteamento

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Persistente:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

---

## Entendendo AllowedIPs

```
0.0.0.0/0 -> qualquer pacote entra no túnel
```

Também pode ser configurado:

```
10.10.0.0/24 -> comunicação dentro da VPN
192.168.0.0/24 -> acesso à LAN
```

Então qualquer pacote com esses destinos serão enviados via túnel.

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

- Cliente não comunica direto com Raspberry
- Tudo passa pelo VPS