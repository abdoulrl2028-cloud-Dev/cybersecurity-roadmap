# Análise de Risco de Ransomware

## Framework de Análise

### Risk = Probability × Impact

```
Risk Baixo:    Prob. Baixa × Impact Baixo
Risk Médio:    Prob. Alta × Impact Baixo  OU  Prob. Baixa × Impact Alto
Risk Alto:     Prob. Alta × Impact Alto
```

---

## Probabilidade

### Fatores de Risco Alto

```
✓ Acesso RDP exposto
✓ Phishing efetivo (sem MFA)
✓ Vulnerabilidades não patchadas
✓ Sem segmentação de rede
✓ Backup desabilitado
✓ Antivírus desatualizado
✓ EDR não implementado
```

### Fatores de Risco Baixo

```
✓ MFA ativado
✓ Sistemas patchados regularmente
✓ Segmentação de rede
✓ Backups offline regulares
✓ EDR presente
✓ Filtro de email avançado
✓ User awareness training
```

---

## Impacto

### Dimensões

#### 1. Financeiro

```
Perda Direta:
- Resgate exigido: $10,000 - $50,000,000
- Criptomoeda volatilidade
- Não garantia de descriptografia

Perda Indireta:
- Tempo de downtime: $1,000/hora
- Recuperação: $100,000+
- Consultoria de resposta: $50,000+
- Perda de clientes/negócio
```

#### 2. Operacional

```
- Downtime: horas a dias
- Interrupção de produção
- Serviços indisponíveis
- Impacto em cadeia de suprimentos
```

#### 3. Reputacional

```
- Perda de confiança de clientes
- Regulamentação (GDPR, HIPAA)
- Multas: até $4,000,000 (GDPR)
- Litígio de clientes
- Cobertura de mídia negativa
```

#### 4. Legal

```
- Notificação obrigatória
- Conformidade regulatória
- Investigação criminal
- Responsabilidade civil
```

---

## Matriz de Risco

### Exemplo Genérico

```
        | Prob. Baixa | Prob. Alta
--------|-------------|----------
Imp. Baixa|   Baixo   |  Médio
Imp. Alta |   Médio   |  ALTO
```

### Exemplo Específico (Empresa)

```
Cenário 1: PME sem defesa
- Prob: 70% (ano)
- Impact: 8/10 (negócio parado)
- Risk: ALTO

Cenário 2: Enterprise com defesa
- Prob: 5% (ano)
- Impact: 3/10 (recuperado em 2h)
- Risk: BAIXO
```

---

## Identificação de Ativos

### Críticos

```
Dados:
- Customer PII
- Financial records
- Source code
- Trade secrets

Sistemas:
- Email
- Backup
- Database
- Website

Impacto se criptografado:
- Negócio parado
- Obrigação legal
- Pesadas multas
```

### Não-Críticos

```
- Publicações
- Arquivos temporários
- Mídia

Impacto:
- Mínimo
- Recuperável
```

---

## Cenários de Ataque

### Cenário 1: Phishing → Ransomware

```
1. Email de phishing com malware
2. Usuário executa arquivo
3. Ransomware dispara
4. Criptografa compartilhamentos de rede
5. Nota de resgate

Timing: Minutos a horas
Impact: ALTO
```

### Cenário 2: Exploração RDP

```
1. RDP exposto (3389)
2. Bruteforce ou credencial roubada
3. Acesso remoto
4. Instala backdoor
5. Ransomware disparado

Timing: Horas a dias
Impact: MUITO ALTO
```

### Cenário 3: Supply Chain

```
1. Fornecedor comprometido
2. Software/patch malicioso
3. Distribuição em massa
4. Ransomware se espalha
5. Múltiplas organizações afetadas

Timing: Dias
Impact: CRÍTICO
```

---

## Avaliação Rápida

### Checklist de Risco

```
☐ RDP exposto na internet?
   SIM = Risco CRÍTICO
   NÃO = Continuar

☐ MFA ativado?
   NÃO = Risco ALTO
   SIM = Continuar

☐ Backup offline regular?
   NÃO = Risco ALTO
   SIM = Continuar

☐ EDR/antivírus atualizado?
   NÃO = Risco MÉDIO
   SIM = Continuar

☐ Plano de resposta documentado?
   NÃO = Risco MÉDIO
   SIM = Risco BAIXO
```

---

## Mitigação

### Camadas de Defesa

```
1. Prevenção
   - Patch management
   - Antivírus/EDR
   - Firewall
   - MFA

2. Detecção
   - Monitoramento de arquivo
   - Alertas C2
   - Anomalia de comportamento

3. Resposta
   - Isolamento rápido
   - Backup recovery
   - Comunicação incidente

4. Recuperação
   - RTO (Recovery Time Objective): 1 hora
   - RPO (Recovery Point Objective): 1 dia
```

### Prioridades por Nível de Risco

#### CRÍTICO
```
Ação IMEDIATA:
1. Desabilitar RDP externo
2. Ativar MFA
3. Implementar EDR
4. Backup offline offline
```

#### ALTO
```
30 dias:
1. Patch todos os sistemas
2. Segmentação de rede
3. Plano de resposta a incidente
4. Teste de backup
```

#### MÉDIO
```
90 dias:
1. User training
2. Antivírus upgrade
3. Procedure documentation
4. Simulação de ataque
```

---

## Metricas de Risco

### KRIs (Key Risk Indicators)

```
- % de sistemas sem patches
- Taxa de falha em backup
- Logins falhados por hora
- Alertas EDR não investigados
- Taxa de click em phishing
- Uptime de email filter
```

### SLAs (Service Level Agreements)

```
- MTTR (Mean Time To Respond): < 1 hora
- MTRS (Mean Time To Recover): < 4 horas
- RPO (Recovery Point): < 24 horas
- RTO (Recovery Time): < 4 horas
```

---

## Relatório de Risco

### Executivo

```
RISCO GERAL: MÉDIO → BAIXO (com implementação)

Recomendações Top 3:
1. Implementar MFA (30 dias)
2. Backup offline (7 dias)
3. EDR (60 dias)

Investimento: $150,000
ROI: Evitar $10,000,000 potencial

Próximos passos:
- Aprovação do orçamento
- Designar responsáveis
- Cronograma de implementação
```

---

**Conclusão**: Ransomware é ameaça real. Avaliação contínua de risco é essencial.
