# Checklist de Segurança — Validação Geral

---

## REDE

### Firewall

```
☐ Firewall instalado e ativo
☐ Regras de entrada: Deny All (padrão)
☐ Regras de saída: Allow All (revisar)
☐ Logging habilitado
☐ Alertas configurados
☐ Review mensal de regras
☐ Teste de failover
```

### Segmentação

```
☐ DMZ separada
☐ Rede interna isolada
☐ VLAN segregação
☐ Controle de acesso entre VLANs
☐ Teste de lateral movement
```

### DNS/DHCP

```
☐ DNS: Usar resolver confiável
☐ DNSSEC habilitado
☐ DHCP: Dentro de rede controlada
☐ DHCP snooping ativo
```

### Wireless

```
☐ WPA3 (ou WPA2 mínimo)
☐ Senha de 16+ caracteres
☐ MAC filtering (opcional)
☐ Esconder SSID (opcional)
☐ Monitoramento de rogue AP
```

---

## SERVIDORES

### Sistema Operacional

```
☐ OS atualizado com latest patches
☐ Kernel security hardening
☐ SELinux/AppArmor ativo (Linux)
☐ ASLR habilitado
☐ Permissões padrão revisadas
☐ Usuários desnecessários removidos
```

### Acesso

```
☐ SSH: Porta não-padrão
☐ SSH: Sem root login
☐ SSH: Autenticação por chave
☐ SSH: PermitEmptyPasswords=No
☐ SSH: MaxAuthTries limitado
☐ Sudo: Auditado e restrito
☐ RDP: Disabled ou com MFA
```

### Dados

```
☐ Criptografia em repouso (AES-256)
☐ Criptografia em transito (TLS 1.2+)
☐ Backup encriptado
☐ Backup offline armazenado
☐ Teste de restore mensal
☐ Retenção de backup documentada
```

### Logs

```
☐ Logging centralizado (SIEM)
☐ Retenção: 90+ dias
☐ Imutabilidade implementada
☐ Alertas para eventos críticos
☐ Review regular de logs
```

---

## APLICAÇÕES WEB

### OWASP Top 10

```
☐ A01: Injection (SQL, OS, NoSQL)
☐ A02: Authentication (MFA, sessão, password)
☐ A03: Sensitive Data Exposure (criptografia)
☐ A04: XML External Entities (XXE)
☐ A05: Broken Access Control (autorização)
☐ A06: Security Misconfiguration (headers)
☐ A07: XSS (refletida, armazenada, DOM)
☐ A08: Insecure Deserialization
☐ A09: Using Components with Known Vulnerabilities
☐ A10: Insufficient Logging & Monitoring
```

### API

```
☐ Autenticação obrigatória
☐ Rate limiting
☐ CORS configurado corretamente
☐ Input validation
☐ Output encoding
☐ Versioning
☐ Documentação atualizada
```

### WAF (Web Application Firewall)

```
☐ WAF instalado e ativo
☐ Regras OWASP Core Rule Set
☐ Modo: Bloqueio (não apenas log)
☐ Falsos positivos investigados
☐ Logs centralizados
```

---

## BANCO DE DADOS

```
☐ Senhas default alteradas
☐ Acesso restrito (não público)
☐ Replicação de dados com autenticação
☐ Backup automático + verificação
☐ Princípio do menor privilégio
☐ Auditoria habilitada
☐ Criptografia de conexão
☐ Monitoramento de queries suspeitas
☐ Vulnerabilidades patched
```

---

## IDENTIDADE E ACESSO (IAM)

### Senhas

```
☐ Comprimento mínimo: 12 caracteres
☐ Complexidade requerida
☐ Histórico: Últimas 5 senhas não reutilizáveis
☐ Expiração: 90 dias (ou MFA)
☐ Lockout após 5 tentativas
☐ Sem valores padrão
☐ Gerenciador de senha corporativo
```

### MFA (Multi-Factor Authentication)

```
☐ Habilitado para: Admin, VPN, Email
☐ Tipos suportados: App, SMS, Hardware token
☐ Backup codes armazenados seguramente
☐ Teste de recuperação
```

### Grupos e Permissões

```
☐ Revisão trimestral de acesso
☐ Princípio do menor privilégio
☐ Separação de deveres
☐ Onboarding/Offboarding automated
☐ PAM (Privileged Access Management) para admin
```

### Single Sign-On (SSO)

```
☐ Implementado (Okta, Azure AD, etc.)
☐ Auditoria de logins
☐ Conditional access policies
☐ Risk-based authentication
```

---

## ENDPOINTS

### Workstations

```
☐ OS atualizado
☐ Antivírus/EDR ativo
☐ Firewall pessoal ativo
☐ Disk encryption (BitLocker, FileVault)
☐ Bloqueio automático 5-10 min
☐ Screen lock obrigatório
☐ WiFi: Apenas WPA3
☐ USB: Restringido
☐ Software: Whitelist aprovado
```

### Mobile

```
☐ MDM (Mobile Device Management) instalado
☐ Senhas obrigatórias
☐ Criptografia ativa
☐ Localização rastreável
☐ Antivírus instalado
☐ Aplicativos: Apenas via app store
☐ Wi-Fi: Restringido
```

---

## DESENVOLVIMENTO

### Código

```
☐ Code review obrigatório
☐ SAST (Static Application Security Testing)
☐ DAST (Dynamic Application Security Testing)
☐ Dependency scanning (npm audit, etc.)
☐ Secrets management (não em git)
☐ Logging adequado
☐ Testes automatizados
☐ Documentação de segurança
```

### CI/CD

```
☐ Acesso restrito ao pipeline
☐ Builds verificados
☐ Assinatura de artefatos
☐ Ambiente isolado (dev/staging/prod)
☐ Rollback automático para falhas
```

---

## PESSOAS

### Treinamento

```
☐ Awareness anual (todos os colaboradores)
☐ Phishing simulation (trimestral)
☐ Role-based training
☐ Documentação de políticas
☐ Código de conduta
```

### Incidente

```
☐ Plano de resposta documentado
☐ Time designado
☐ Contatos de escalação
☐ Teste tabletop (anual)
☐ MTTR definido
```

---

## CONFORMIDADE

### Regulamentações

```
☐ LGPD (Lei Geral de Proteção de Dados)
☐ GDPR (EU General Data Protection)
☐ PCI-DSS (Pagamento com cartão)
☐ HIPAA (Saúde)
☐ SOC 2 Type II (se aplicável)
```

### Auditorias

```
☐ Teste de Penetração: Anual
☐ Avaliação de Vulnerabilidade: Trimestral
☐ Audit Interno: Semestral
☐ Audit Externo: Anual
```

---

## RESPOSTA A INCIDENTE

### Preparação

```
☐ Plano documentado e testado
☐ Team designado
☐ Comunicação: Interna e externa
☐ Escalação: Claras
☐ Ferramentas: Prontas
```

### Resposta

```
☐ Detectar incidente
☐ Conter: Isolar sistemas
☐ Erradicar: Remover ameaça
☐ Recuperar: Restaurar sistemas
☐ Analisar: Causa raiz
☐ Documentar: Lições aprendidas
```

---

## SCORE DE SEGURANÇA

### Cálculo

```
Total de Itens: [X]
Itens Aprovados: [Y]

Score = (Y/X) * 100%

Resultado:
90-100%: EXCELENTE
80-89%: BOM
70-79%: ACEITÁVEL
< 70%: CRÍTICO
```

### Interpretação

```
CRÍTICO: Remediação imediata necessária
ACEITÁVEL: Plano de melhoria em 30 dias
BOM: Continuar monitoramento
EXCELENTE: Manter nível atual
```

---

**Próximas Etapas**:
1. Identificar gaps críticos
2. Priorizar remediação
3. Definir propriedade
4. Agendar revisão em 30 dias
