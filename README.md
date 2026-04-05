# VPN - Secure Remote Access Infrastructure

**Languages:** [Portuguese](README.md) | [English](README.en.md)

---

## Descrição Geral

Este projeto implementa uma infraestrutura **Peer to Peer** de acesso remoto seguro utilizando **WireGuard**, projetada para funcionar mesmo em ambientes com **CGNAT (Carrier-Grade NAT)**.

A solução utiliza um **VPS com IP público** como intermediário para permitir que clientes externos acessem dispositivos em uma rede local privada.

---

## Objetivos

* Permitir acesso remoto seguro à rede local
* Contornar limitações de CGNAT
* Implementar uma arquitetura simples, eficiente e escalável
* Demonstrar conceitos avançados de redes e segurança

---

## Arquitetura

A infraestrutura é composta por três componentes principais:

* **VPS (Servidor público)** → ponto central de conexão
* **Raspberry Pi (Gateway da LAN)** → acesso à rede local
* **Clientes (Notebook/Desktop)** → acesso remoto

---

## Topologia de rede

![Topologia de rede](Diagrams/Topologia_rede.png)

---

## Fluxo de pacotes (Resumo)

1. Cliente envia requisição para a rede local (ex: 192.168.0.X)
2. Tráfego é encapsulado via WireGuard até o VPS
3. VPS encaminha para o Raspberry Pi
4. Raspberry realiza NAT e envia para a LAN
5. Resposta retorna pelo mesmo caminho

---

## Tecnologias usadas

* WireGuard
* Linux Networking (iproute2, iptables)
* VPS (Oracle Cloud)
* SSH
* NAT / Routing

---

## Documentação

Para detalhes completos, consulte:

* [VPN architecture](Docs/architecture.md)
* [Setup VPS](Docs/setup_vps.md)
* [Setup Raspberry](Docs/setup_raspberry.md)
* [Setup Clients](Docs/setup_clients.md)

---

## Configuração rápida

### 1. Clone o repositório

```bash
git clone https://github.com/ArtOliv/WireGuard-VPN.git
cd WireGuard-VPN
```

### 2. Instale o WireGuard

Instale o wireguard em cada um dos nós, caso não esteja instalado, com:

```bash
sudo apt install wireguard
```

### 3. Configurar o VPS

Depois de conectar via ssh no VPS e clonar o reposotório, execute:

```bash
cd scripts
chmod +x setup-vps.sh
./setup-vps.sh
```

### 4. Configurar o Raspberry Pi

Depois clonar o reposotório no Raspberry Pi, execute:

```bash
cd scripts
chmod +x setup-raspberry.sh
./setup-raspberry.sh
```

### 5. Configurar o Cliente

Depois clonar o reposotório no Notebook/Desktop, execute:

```bash
cd scripts
chmod +x setup-client.sh
./setup-client.sh
```

---

## Testando

Teste de conectividade:

```bash
ping 10.10.0.2
```

Teste de acesso à LAN:

```bash
curl http://192.168.X.X
```

---

## Considerações de segurança

* Chaves privadas **não são armazenadas no repositório**
* Comunicação criptografada com WireGuard
* Uso de SSH com autenticação por chave
* Regras de firewall para comunicação aplicadas nos nós

---

## Desafios iniciais

* CGNAT bloqueia conexões de entrada
* Necessidade de PersistentKeepalive
* Configuração correta de forwarding entre peers
* Firewall do VPS pode bloquear tráfego UDP

---

## Aprendizados

* WireGuard é stateless (depende de tráfego para handshake)
* NAT traversal requer keepalive
* Debugging de rede exige ferramentas como tcpdump
* Roteamento e firewall são críticos para funcionamento correto

---

## Autor

`Arthur Carvalho Rodrigues Oliveira`

Projeto desenvolvido para uso pessoal e estudo em cybersegurança e redes.

---

## Licença

Este projeto está sob a licença MIT.
