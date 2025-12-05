# Conceitos de Sistemas Operacionais

## O que é o Kernel?

O kernel é o núcleo do sistema operacional. Funciona como intermediário entre aplicações e hardware.

### Responsabilidades do Kernel
- **Gerenciamento de processos**: Criação, escalonamento, término
- **Gerenciamento de memória**: Alocação, paginação, swap
- **Gerenciamento de I/O**: Controle de dispositivos
- **Sistema de arquivos**: Leitura/escrita em disco
- **Segurança**: Permissões e proteção

### Tipos de Kernel
- **Monolítico**: Linux, Unix (tudo em um)
- **Microkernel**: Minix (modular)
- **Híbrido**: Windows (misturado)

---

## Processos e Threads

### Processos
Um processo é uma instância de um programa em execução.

```
Características:
- PID (Process ID) único
- Espaço de memória isolado
- Pode ter múltiplas threads
- Isolamento (um não interfere com outro)
```

**Estados de processo**:
- **Running**: Executando
- **Ready**: Aguardando CPU
- **Blocked**: Aguardando I/O ou evento
- **Stopped**: Pausado
- **Zombie**: Finalizado, mas não limpado

### Threads
Threads são "mini processos" dentro de um processo.

```
Vantagens:
- Compartilham memória do processo pai
- Mais leves que processos
- Execução paralela
- Mais eficientes em multitarefa
```

**Diferença processo x thread**:
| Aspecto | Processo | Thread |
|--------|----------|--------|
| Memória | Isolada | Compartilhada |
| Criação | Lenta | Rápida |
| Comunicação | Complexa | Simples |
| Segurança | Alta | Menor |

---

## Gerenciamento de Memória

### Espaço de Endereçamento

```
┌─────────────────────┐ Endereço Alto
│   Stack (pilha)     │ Cresce para baixo
├─────────────────────┤
│   (espaço livre)    │
├─────────────────────┤
│   Heap (pilha)      │ Cresce para cima
├─────────────────────┤
│   BSS (não init.)   │
├─────────────────────┤
│   Data (init.)      │
├─────────────────────┤
│   Text (código)     │ Endereço Baixo
└─────────────────────┘
```

### Paginação
- Divide memória em páginas (4KB típico)
- Permite execução de programas maiores que RAM
- Permite isolamento

### Swap
- Usa disco como extensão de RAM
- Significativamente mais lento
- Essencial quando RAM insuficiente

### Proteção de Memória
```bash
# Ver permissões de memória
cat /proc/self/maps

# Ver uso de memória
ps aux
free -h
```

---

## Sistema de Arquivos

### Estrutura de Diretórios (Linux)

```
/ (raiz)
├── /bin → Programas essenciais
├── /sbin → Programas do root
├── /lib → Bibliotecas do sistema
├── /etc → Arquivos de configuração
├── /home → Diretórios dos usuários
├── /root → Diretório do root
├── /tmp → Arquivos temporários
├── /var → Variáveis e logs
├── /opt → Programas opcionais
├── /proc → Processos em tempo real
└── /sys → Informações do sistema
```

### Tipos de Arquivo

```
- Regular file: -
- Diretório: d
- Link simbólico: l
- Arquivo de dispositivo: c (caractere) ou b (bloco)
- Socket: s
- Pipe: p
```

### Permissões (OCtal)

```
7 (rwx) = 4 (read) + 2 (write) + 1 (execute)
6 (rw-) = 4 (read) + 2 (write)
5 (r-x) = 4 (read) + 1 (execute)
4 (r--) = 4 (read)
```

**Exemplo**: `chmod 755 script.sh`
- Dono: 7 (rwx)
- Grupo: 5 (r-x)
- Outros: 5 (r-x)

---

## Drivers

### O que são Drivers?
Programas que permitem ao SO comunicar com hardware específico.

### Tipos
- **Kernel drivers**: Integrados no kernel
- **Kernel modules**: Carregáveis dinamicamente

### Gerenciamento no Linux
```bash
# Listar módulos carregados
lsmod

# Carregar módulo
sudo insmod arquivo.ko

# Descarregar
sudo rmmod nome_modulo

# Gerenciar com modprobe
sudo modprobe driver_name
sudo modprobe -r driver_name
```

---

## Gerenciamento de Usuários

### Tipos de Usuário
- **Root (uid 0)**: Acesso total
- **System users**: Serviços (uid 1-999)
- **Regular users**: Usuários comuns (uid 1000+)

### Controle de Acesso Básico (DAC)
```bash
# Criar usuário
sudo useradd -m username
sudo passwd username

# Grupos
sudo groupadd grupo
sudo usermod -aG grupo username

# Arquivo de senhas
cat /etc/passwd
cat /etc/shadow  # Só root acessa
```

### Sudo
Permite executar comandos como outro usuário.

```bash
# Usar sudo
sudo comando

# Configurar sudoers
sudo visudo

# Exemplo sudoers
user ALL=(ALL) ALL
%group ALL=(ALL) NOPASSWD: /usr/bin/systemctl
```

---

## Políticas de Segurança do SO

### SELinux (Security-Enhanced Linux)
Controle de acesso obrigatório (MAC).

```bash
# Ver status
getenforce

# Modo permissivo
setenforce 0

# Modo enforcing
setenforce 1
```

### AppArmor
Confinamento de aplicações (alternativa ao SELinux).

```bash
sudo aa-enable /etc/apparmor.d/profile
```

### Firewall
```bash
# UFW (Ubuntu)
sudo ufw enable
sudo ufw allow 22
sudo ufw deny 80
```

---

## Hardening (Endurecimento de Segurança)

### Práticas Essenciais

1. **Manter SO atualizado**
```bash
sudo apt update && sudo apt upgrade
```

2. **Desabilitar serviços desnecessários**
```bash
sudo systemctl disable serviço
sudo systemctl stop serviço
```

3. **Configurar firewall**
```bash
sudo ufw enable
sudo ufw default deny incoming
sudo ufw allow ssh
```

4. **Proteção de SSH**
```bash
# Arquivo: /etc/ssh/sshd_config
Port 2222          # Mudar porta padrão
PermitRootLogin no # Desabilitar root login
PasswordAuthentication no # Usar chaves
```

5. **Falhas de login**
```bash
# Arquivo: /etc/pam.d/common-auth
auth required pam_tally2.so onerr=fail audit silent deny=5 unlock_time=600
```

6. **Auditoria**
```bash
# Ver logs de acesso
tail -f /var/log/auth.log
```

---

## Monitoramento do SO

### Comandos Úteis

```bash
# Processos
ps aux
top
htop

# Memória
free -h
vmstat

# Disco
df -h
du -sh /path

# Conexões de rede
netstat -tulpn
ss -tulpn

# Logs do sistema
journalctl -xe
tail -f /var/log/syslog
```

---

## Conceitos Avançados

### Namespaces (Isolamento)
Permitem que processos tenham visão isolada do sistema.

**Tipos**:
- PID: Isolamento de processos
- Network: Isolamento de rede
- Mount: Isolamento de filesystem
- User: Isolamento de usuários

### Cgroups (Limites de Recursos)
Limitam CPU, memória e I/O por processo.

```bash
# Ver cgroups
cat /proc/cgroups

# Listar tarefas
cat /sys/fs/cgroup/cpu/tasks
```

---

**Próximas leituras**: Máquinas Virtuais e Virtualização
