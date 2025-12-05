# Configurando Máquinas Virtuais para Cybersecurity

## Ferramentas Recomendadas

### Hypervisors Tipo 2 (Desktop)
- **VirtualBox** - Gratuito, multiplataforma
- **VMware Workstation** - Pago, mais rápido
- **Hyper-V** - Windows, integrado

### Hypervisors Tipo 1 (Servidores)
- **Proxmox** - Gratuito, profissional
- **ESXi** - VMware enterprise
- **KVM** - Linux nativo

---

## Passos Básicos para Criar uma VM

### 1. Preparar a ISO
- Baixar ISO (Ubuntu, Kali Linux, Windows)
- Verificar integridade (SHA256)
- Armazenar localmente

### 2. Criar a Máquina Virtual

**Recomendações de Hardware**:
- **CPU**: 2-4 cores
- **RAM**: 2-4 GB (Kali: 4GB+)
- **Disco**: 20-50 GB
- **Rede**: NAT ou Bridge

**Passos no VirtualBox**:
```
1. Nova → Máquina Virtual
2. Nome e Sistema Operacional
3. Memória: 2-4 GB
4. Disco Rígido: 20-50 GB
5. Configurações → Rede → Adaptador 1 → NAT
6. Adicionar ISO no CD/DVD
7. Iniciar
```

### 3. Instalar o Sistema Operacional
- Boot da ISO
- Seguir instalador
- Aceitar particionamento automático

### 4. Após Instalação

#### Atualizações Iniciais (Linux)
```bash
sudo apt update
sudo apt upgrade
sudo apt install -y build-essential linux-headers-$(uname -r)
```

#### Instalar Guest Additions (VirtualBox)
```bash
# Ubuntu/Debian
sudo apt install virtualbox-guest-additions-iso
sudo apt install virtualbox-guest-utils

# Ou pelo menu do VirtualBox
# Dispositivos → Inserir Imagem Guest Additions
sudo mount /media/cdrom
sudo /media/cdrom/VBoxLinuxAdditions.run
```

#### Abilitar Recursos Avançados
```bash
# Compartilhamento de pasta
sudo usermod -aG vboxsf $USER

# Clipboard compartilhado
# VirtualBox → Máquina → Configurações → Geral → Avançado
```

---

## Configuração de Rede

### NAT (Network Address Translation)
- Máquina acessa internet via host
- Host não acessa máquina (padrão)
- Isolamento melhor para testes

### Bridge Mode
- VM na mesma rede que host
- Recebe IP da rede local
- Útil para lab com múltiplas VMs

### Host-Only
- VM conectada apenas ao host
- Isolada da rede externa
- Segura para malware testing

**Configurar rede estática**:
```bash
# Ubuntu/Debian (netplan)
sudo nano /etc/netplan/00-installer-config.yaml
```

```yaml
network:
  ethernets:
    eth0:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
  version: 2
```

```bash
sudo netplan apply
```

---

## Snapshots e Clonagem

### Snapshots (Ponto de Restauração)
```
VirtualBox → Máquina → Snapshots
Criar snapshot antes de testes arriscados
Restaurar se algo der errado
```

**Benefícios**:
- Backup rápido
- Voltar a estado anterior
- Testar sem risco

### Clonagem de VMs
```
Máquina → Clonar
Full Clone: Cópia completa (mais espaço)
Linked Clone: Referencia original (economiza espaço)
```

---

## Ambiente Seguro para Pentest

### Isolamento de Rede
```
┌─────────────────┐
│   Host (PC)     │
└────────┬────────┘
         │
         ├─→ VM Kali (Atacante) → NAT
         │
         └─→ VM Ubuntu (Alvo) → Host-Only
```

### Instalação de Ferramentas

#### Kali Linux (pré-instalado)
```bash
kali-linux-full
```

#### Ubuntu com Pentest Tools
```bash
# Nmap
sudo apt install nmap

# Metasploit
sudo apt install metasploit-framework

# Wireshark
sudo apt install wireshark
sudo usermod -aG wireshark $USER

# Burp Suite
# Baixar de portswigger.net

# Nikto
sudo apt install nikto

# John the Ripper
sudo apt install john
```

---

## Performance e Otimização

### Aumentar Desempenho
1. **Alocação de CPU**: Aumentar cores (máx = host cores)
2. **RAM**: 4GB+ para Kali + Ubuntu
3. **Aceleração**: Ativar VT-x/AMD-V na BIOS
4. **Disco**: SSD melhor que HDD

### Monitorar Recursos
```bash
top
free -h
df -h
vmstat 1 5
```

---

## Dicas Importantes

✅ **Faça**: 
- Usar VMs para testes de segurança
- Criar snapshots regularmente
- Isolar rede para malware testing
- Atualizar sistemas regularmente

❌ **Não faça**:
- Testar em produção
- Conectar VM infectada à internet sem isolamento
- Ignorar backups
- Usar senhas fracas

---

## Verificação de Funcionamento

```bash
# Verificar conexão
ping 8.8.8.8

# Informações do sistema
uname -a

# Listar adaptadores
ip link show

# Testar DNS
nslookup google.com

# Instalar teste tool
sudo apt install curl
curl https://www.google.com
```

---

Próximo: Configure múltiplas VMs (atacante/alvo) para praticar pentest!
