# Máquinas Virtuais — Conceitos

## O que é Virtualização?

Virtualização é a criação de ambientes computacionais simulados (virtuais) sobre um hardware real.

### Benefícios
- ✅ Múltiplos SOs no mesmo hardware
- ✅ Isolamento e segurança
- ✅ Eficiência de recursos
- ✅ Mobilidade (Live migration)
- ✅ Snapshots e backups
- ✅ Recuperação de desastres

### Desvantagens
- ❌ Overhead de performance
- ❌ Complexidade
- ❌ Vulnerabilidades do hypervisor

---

## Hypervisor: Tipo 1 vs Tipo 2

### Hypervisor Tipo 1 (Bare Metal)
Roda diretamente no hardware, sem SO intermediário.

```
┌─────────────────┐
│   Aplicações    │
├─────────────────┤
│   SO Convidado  │
├─────────────────┤
│   Hipervisor    │
├─────────────────┤
│    Hardware     │
└─────────────────┘
```

**Exemplos**: Proxmox, ESXi, Hyper-V Server
**Uso**: Servidores, datacenters
**Performance**: Melhor

### Hypervisor Tipo 2 (Hosted)
Roda sobre um SO hospedeiro como aplicação.

```
┌─────────────────┐
│   Aplicações    │
├─────────────────┤
│   SO Convidado  │
├─────────────────┤
│   Hipervisor    │
├─────────────────┤
│   SO Hospedeiro │
├─────────────────┤
│    Hardware     │
└─────────────────┘
```

**Exemplos**: VirtualBox, VMware Workstation, Hyper-V
**Uso**: Desktop, laboratório
**Performance**: Menor que Tipo 1

---

## Isolamento de VM

### O que é Isolado?

1. **CPU**: Cada VM pode usar cores específicas
2. **Memória**: Cada VM tem espaço isolado
3. **Disco**: Arquivos separados (ou LVM)
4. **Rede**: Adaptadores virtuais separados
5. **Periféricos**: USB, impressora, som

### Segurança do Isolamento

A VM não pode acessar dados de outra VM diretamente, exceto:
- Via rede (se conectadas)
- Via compartilhamento de pasta configurado
- Se conseguir escapar do hipervisor (raro)

---

## Snapshots

### O que é um Snapshot?
Captura do estado completo da VM em um momento específico.

### Conteúdo
- Estado da memória RAM
- Conteúdo do disco
- Configurações
- Estado de periféricos

### Uso em Cybersecurity

```
Teste Normal → Snapshot A
             ↓
Teste Arriscado (malware)
             ↓
Revert para Snapshot A ✓
```

### Exemplo Prático

```bash
# VirtualBox CLI
VBoxManage snapshot "VM Name" take "Backup Antes de Teste"
VBoxManage snapshot "VM Name" restore "Backup Antes de Teste"
VBoxManage snapshot "VM Name" list
```

### Cuidados
- ⚠️ Snapshots usam espaço em disco
- ⚠️ Não substituem backups completos
- ⚠️ Performance diminui com muitos snapshots

---

## Clonagem de VMs

### Tipos de Clone

#### 1. Full Clone
Cópia completa e independente.

```
VM Original (30GB) → Full Clone (30GB)
```

**Vantagens**:
- Totalmente independente
- Sem dependências

**Desvantagens**:
- Usa mais espaço
- Mais lento

#### 2. Linked Clone
Referencia o estado original, apenas mudanças são copiadas.

```
VM Original (30GB) → Linked Clone (5GB)
```

**Vantagens**:
- Economiza espaço
- Mais rápido

**Desvantagens**:
- Depende do original
- Mais complexo

### Comando Prático

```bash
# VirtualBox Full Clone
VBoxManage clonevm "VM Original" --name "VM Cópia" --register

# Linked Clone
VBoxManage clonevm "VM Original" --name "VM Link" --register --mode machine
```

---

## Ambientes Seguros para Pentest

### Topologia Recomendada

```
Internet
   │
   └─→ Host (PC)
        │
        ├─→ [VM 1: Kali Linux]
        │    ├─ Nat Mode
        │    └─ Atacante
        │
        └─→ [VM 2: Ubuntu Server]
             ├─ Host-Only Network
             └─ Alvo
```

### Modos de Rede

| Modo | Internet | Host | VMs |
|------|----------|------|-----|
| NAT | ✓ | ✗ | ✗ |
| Bridge | ✓ | ✓ | ✓ |
| Host-Only | ✗ | ✓ | ✓ |
| Internal | ✗ | ✗ | ✓ |

### Configuração Segura para Malware Testing

```
[VM Kali: NAT] →  Internet (Isolado via NAT)
   ↓
[VM Alvo: Host-Only] → Comunicação entre VMs
   ↓
[Host: Protegido]
```

---

## VM Populares em Pentest

### Kali Linux
- **Uso**: Sistema operacional do atacante
- **Ferramentas**: Nmap, Burp, Metasploit, Wireshark, etc.
- **RAM Mín**: 4GB

### Ubuntu Server
- **Uso**: Alvo para praticar
- **Servidor web, SSH, FTP, etc.
- **RAM Mín**: 2GB

### Parrot Security OS
- **Uso**: Alternativa ao Kali
- **Mais leve, interface melhor
- **RAM Mín**: 2GB

### Windows 10/Server
- **Uso**: Alvo ou teste de malware
- **Mais pesada
- **RAM Mín**: 4-8GB

---

## Recursos de VM para Pentest

### Configuração Mínima
```
CPU: 2 cores
RAM: 2GB (Kali: 4GB mínimo)
Disco: 20GB
Rede: NAT ou Host-Only
```

### Configuração Recomendada
```
CPU: 4 cores
RAM: 8GB
Disco: 50GB SSD
Rede: NAT + Host-Only (2 adaptadores)
```

---

## Performance e Otimização

### Aumentar Velocidade

1. **Habilitar Aceleração de Hardware**
   - BIOS: Habilitar VT-x (Intel) ou AMD-V
   - VirtualBox: Máquina → Configurações → Sistema → Aceleração

2. **Aumentar Recursos**
   - CPU cores até o máximo disponível
   - RAM até 75% do host

3. **Disco SSD**
   - Muito mais rápido que HDD
   - Essencial para produtividade

4. **Snapshot Limpo**
   - Excluir snapshots desnecessários
   - Consolidar snapshots antigos

---

## Uso de Snapshot em Pentest

### Workflow Exemplo

```
1. VM Fresca (Snapshot "Clean")
   ↓
2. Executar Nmap (Snapshot "Post-Nmap")
   ↓
3. Executar Exploit (Snapshot "Post-Exploit")
   ↓
4. Algo deu errado? Volta para Snapshot anterior
   ↓
5. Tenta outro exploit
```

### Boas Práticas

✅ Criar snapshot antes de testes arriscados
✅ Nomear snapshots descritivamente
✅ Documentar passos no snapshot
✅ Revisar snapshots antigos

❌ Não usar snapshots como backup único
❌ Não deixar cadeia de snapshots infinita
❌ Não confundir snapshot com clone

---

## Migrando VMs

### Portabilidade
- VMs podem ser movidas entre hosts
- Usar OVA/OVF para transportar
- Live migration em ambientes enterprise

### Comando

```bash
# Exportar VM
VBoxManage export "VM Name" --output "VM.ova"

# Importar VM
VBoxManage import "VM.ova" --vsys 0 --vmname "Nova VM"
```

---

## Troubleshooting

### Problemas Comuns

| Problema | Solução |
|----------|---------|
| VM lenta | Aumentar CPU/RAM |
| Sem internet | Verificar modo de rede |
| Sem acesso ao host | Usar Bridge mode |
| Disco cheio | Aumentar tamanho de disco |
| Erro de aceleração | Verificar BIOS/UEFI |

---

**Próximo**: Voltar para `pentest/` e praticar!
