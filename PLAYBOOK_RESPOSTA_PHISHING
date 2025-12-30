# 📖 PLAYBOOK: Resposta a Incidente de Phishing

## 📋 Informações do Documento

| Campo | Valor |
|-------|-------|
| **Versão** | 1.0 |
| **Data de Criação** | Dezembro 2025 |
| **Última Revisão** | Dezembro 2025 |
| **Proprietário** | Equipe SOC |
| **Classificação** | Interno - Confidencial |
| **Revisão Recomendada** | Trimestral |

---

## 🎯 Objetivo

Este playbook define procedimentos padronizados para detecção, análise, contenção e resposta a incidentes de phishing na organização.

---

## 📊 Escopo

### Tipos de Phishing Cobertos
- ✅ Email Phishing (massa)
- ✅ Spear Phishing (direcionado)
- ✅ Whaling (executivos)
- ✅ Smishing (SMS)
- ✅ Vishing (telefone)
- ✅ Clone Phishing

### Fora do Escopo
- ❌ Malware avançado com persistência
- ❌ Ransomware (playbook separado)
- ❌ APT com C2 estabelecido (escalar para IR Team)

---

## 👥 Papéis e Responsabilidades

| Papel | Responsabilidade | SLA |
|-------|------------------|-----|
| **Usuário Final** | Reportar phishing suspeito | Imediato |
| **Service Desk** | Triagem inicial, coleta de informações | 15 min |
| **Analista SOC L1** | Análise técnica básica, classificação | 30 min |
| **Analista SOC L2** | Investigação profunda, correlação | 1-2 horas |
| **Security Engineer** | Implementação de bloqueios/regras | 1 hora |
| **Incident Response** | Coordenação de resposta complexa | 2 horas |
| **CISO** | Comunicação executiva (casos graves) | 4 horas |

---

## 🚨 Severidade e Priorização

### Matriz de Severidade

| Nível | Critérios | Tempo de Resposta |
|-------|-----------|-------------------|
| **🔴 CRÍTICO** | Executivo C-level comprometido OU dados sensíveis exfiltrados OU campanha ativa afetando 50+ usuários | **15 minutos** |
| **🟠 ALTO** | Usuário privilegiado comprometido OU credenciais roubadas OU campanha afetando 10-49 usuários | **30 minutos** |
| **🟡 MÉDIO** | Credenciais comuns expostas OU campanha afetando 2-9 usuários OU tentativa bloqueada mas precisa análise | **2 horas** |
| **🟢 BAIXO** | Tentativa bloqueada tecnicamente OU nenhum clique/interação OU falso positivo | **4 horas** |

---

## 🔄 FLUXO GERAL DO PROCESSO

```
┌─────────────────┐
│  1. DETECÇÃO    │ → Usuário reporta / Ferramenta detecta
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  2. TRIAGEM     │ → Service Desk: É realmente phishing?
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 3. ANÁLISE      │ → SOC L1/L2: Headers, URLs, impacto
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 4. CONTENÇÃO    │ → Bloquear domínios, quarentena emails
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 5. ERRADICAÇÃO  │ → Remover emails, resetar senhas
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 6. RECUPERAÇÃO  │ → Restaurar acesso, verificar sistemas
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 7. POST-MORTEM  │ → Documentar, melhorar, treinar
└─────────────────┘
```

---

## 📍 FASE 1: DETECÇÃO

### Fontes de Detecção

1. **Reporte de Usuário** (70% dos casos)
   - Botão "Reportar Phishing" no Outlook/Gmail
   - Email para `phishing@empresa.com`
   - Chamado no Service Desk

2. **Email Gateway** (25% dos casos)
   - Proofpoint/Mimecast sandbox hits
   - Anti-spam score alto
   - URL reputação negativa

3. **SIEM Alerts** (5% dos casos)
   - Login anômalo após clique
   - Múltiplos logins falhados
   - Exfiltração de dados detectada

### Informações a Coletar

- [ ] Email completo com headers (formato .EML ou .MSG)
- [ ] Horário do recebimento
- [ ] Número de destinatários internos
- [ ] Se o usuário interagiu (clicou, baixou anexo, forneceu dados)
- [ ] Sistemas/aplicações que o usuário tem acesso
- [ ] Screenshot do email (se possível)

---

## 📍 FASE 2: TRIAGEM (Service Desk - 15 min)

### Checklist de Triagem

```
□ Email recebido foi preservado?
□ Usuário clicou em algum link?
□ Usuário baixou algum anexo?
□ Usuário forneceu credenciais?
□ Usuário forneceu dados financeiros?
□ Outros usuários reportaram email similar?
□ Email foi enviado para lista de distribuição?
```

### Árvore de Decisão

```
Usuário clicou OU baixou anexo?
│
├─ SIM → Severidade ALTA/CRÍTICA
│   └─ Escalar imediatamente para SOC
│
└─ NÃO → Usuário apenas reportou?
    │
    ├─ SIM → Severidade BAIXA
    │   └─ Criar ticket, passar para SOC
    │
    └─ Email bloqueado automaticamente?
        └─ SIM → Registrar, enviar para análise em batch
```

### Ações Imediatas (se clicou)

1. **Isolar usuário:**
   ```
   - Desabilitar conta temporariamente (AD/Azure)
   - Revogar tokens de sessão (Office 365)
   - Desconectar dispositivo da rede (se presencial)
   ```

2. **Notificar SOC:** Via Slack/Teams urgente

3. **Criar ticket P1:** Com todas as informações coletadas

---

## 📍 FASE 3: ANÁLISE (SOC L1/L2 - 30min a 2h)

### 3.1 Análise de Email

#### A) Headers

```bash
# Extrair headers importantes
grep -E "From:|Return-Path:|Received:|Authentication-Results:" email.eml

# Verificar autenticação
SPF: pass/fail?
DKIM: pass/fail?
DMARC: pass/fail?

# Rastrear origem
Received: [último servidor] → Qual país? AS Number?
```

**Red Flags:**
- ✘ SPF/DKIM/DMARC todos falharam
- ✘ Return-Path ≠ From
- ✘ Origem: Rússia, China, Nigéria (para empresa brasileira)
- ✘ Servidor com má reputação (Spamhaus, etc)

#### B) Corpo do Email

```
□ Linguagem urgente/ameaçadora?
□ Erros gramaticais óbvios?
□ Pedido de credenciais/pagamento?
□ Saudação genérica?
□ Assinatura inconsistente com empresa real?
```

#### C) URLs

```bash
# Extrair todas URLs
grep -Eo 'https?://[^ ]+' email.eml > urls.txt

# Para cada URL:
# 1. Verificar em VirusTotal
curl "https://www.virustotal.com/api/v3/urls/{url_id}" \
     -H "x-apikey: YOUR_API_KEY"

# 2. Expandir links encurtados (sem clicar!)
curl -sI bit.ly/xxxxx | grep -i location

# 3. Whois do domínio
whois malicious-domain.com | grep -E "Creation Date|Registrar"

# 4. URLScan.io
# Manual: colar em urlscan.io/submit
```

**Red Flags:**
- ✘ Typosquatting: paypa1.com, g00gle.com
- ✘ Domínio < 30 dias de idade
- ✘ TLD suspeito: .tk, .ml, .ga
- ✘ Múltiplos redirecionamentos
- ✘ VirusTotal: 3+ engines detectam como malicioso

#### D) Anexos

```bash
# NUNCA EXECUTAR! Apenas analisar metadata

# Ver tipo real do arquivo
file documento.pdf

# Hash do arquivo
sha256sum arquivo.zip

# Verificar no VirusTotal
curl --request POST \
     --url https://www.virustotal.com/api/v3/files \
     --header 'x-apikey: YOUR_API_KEY' \
     --form file=@arquivo.zip
```

**Red Flags:**
- ✘ Extensão dupla: documento.pdf.exe
- ✘ Arquivo Office com macros (.docm, .xlsm)
- ✘ Executável: .exe, .scr, .bat
- ✘ ZIP com senha
- ✘ VirusTotal: detectado como malware

### 3.2 Análise de Impacto

```
□ Quantos usuários receberam? (verificar no email gateway)
□ Quantos clicaram? (verificar logs de proxy)
□ Quem clicou tem acesso a dados sensíveis?
□ Houve login após o clique? (verificar SIEM)
  □ De onde? IP/localização/país?
  □ Horário anômalo?
  □ Dispositivo novo?
□ Houve exfiltração de dados? (DLP alerts)
```

### 3.3 Classificação Final

Com base na análise, classificar:

| Categoria | Descrição |
|-----------|-----------|
| **Phishing Confirmado** | Todos indicadores apontam para ataque |
| **Altamente Suspeito** | 3+ red flags, origem duvidosa |
| **Suspeito** | 1-2 red flags, precisa mais análise |
| **Falso Positivo** | Email legítimo mal classificado |
| **Spam** | Não é phishing, apenas propaganda |

---

## 📍 FASE 4: CONTENÇÃO (1 hora)

### Ações Técnicas

#### A) Email Gateway

```bash
# 1. Bloquear domínio remetente
# Exemplo Proofpoint:
add-blocked-sender malicious@domain.com

# 2. Bloquear URLs maliciosas
add-blocked-url phishing-site.com

# 3. Quarentena de emails similares
search-and-quarantine \
  --from "malicious@domain.com" \
  --received-after "2025-12-29 00:00" \
  --action quarantine
```

#### B) Firewall/Proxy

```bash
# Bloquear acesso ao domínio malicioso
# Exemplo pfSense:
echo "table <phishing_domains> {malicious-domain.com}" >> /etc/pf.conf
pfctl -f /etc/pf.conf

# Verificar quem já acessou
grep "malicious-domain.com" /var/log/squid/access.log
```

#### C) EDR/Antivírus

```bash
# Adicionar IOC para detecção
# Exemplo Wazuh:
# Adicionar em /var/ossec/etc/rules/local_rules.xml

<rule id="100100" level="12">
  <if_group>web</if_group>
  <url>malicious-domain.com</url>
  <description>Acesso a domínio de phishing conhecido</description>
</rule>

# Reiniciar Wazuh
systemctl restart wazuh-manager
```

#### D) Usuários Comprometidos

```powershell
# Revogar sessões ativas (Office 365)
Revoke-AzureADUserAllRefreshToken -ObjectId user@empresa.com

# Desabilitar conta temporariamente
Set-ADUser -Identity username -Enabled $false

# Forçar reset de senha no próximo login
Set-ADUser -Identity username -ChangePasswordAtLogon $true
```

### Comunicação

**Template de Email Interno:**

```
Assunto: [ALERTA DE SEGURANÇA] Campanha de Phishing Ativa

Equipe,

Identificamos uma campanha de phishing ativa visando nossa organização.

CARACTERÍSTICAS DO EMAIL MALICIOSO:
- Remetente: [exemplo@domain.com]
- Assunto: "[Assunto do phishing]"
- Conteúdo: [breve descrição]

AÇÕES TOMADAS:
- Domínio bloqueado
- Emails quarentenados
- Usuários afetados notificados

O QUE FAZER:
- NÃO clique em links deste email
- DELETE se receber
- REPORTE clicando em "Reportar Phishing"

Dúvidas: ti@empresa.com

Equipe de Segurança
```

---

## 📍 FASE 5: ERRADICAÇÃO (2-4 horas)

### Checklist de Erradicação

```
□ Remover todos emails da campanha (inbox de todos usuários)
□ Deletar emails quarentenados após análise
□ Resetar senhas de usuários comprometidos
  □ Forçar senhas fortes (12+ chars)
  □ Ativar MFA se ainda não ativado
□ Verificar criação de regras de email suspeitas
  □ Forwarding automático?
  □ Filtros que ocultam emails?
□ Scan completo de antivírus em máquinas que clicaram
□ Verificar logins recentes de contas comprometidas
  □ Reverter mudanças suspeitas (permissões, configs)
□ Verificar instalação de apps/extensões não autorizados
```

### Scripts Úteis

#### Verificar Regras de Email Suspeitas (Office 365)

```powershell
# Verificar regras de forwarding
Get-Mailbox | Get-InboxRule | Where-Object {
    $_.ForwardTo -ne $null -or 
    $_.ForwardAsAttachmentTo -ne $null -or
    $_.RedirectTo -ne $null
} | Select Name, MailboxOwnerId, ForwardTo, Enabled
```

#### Logins Anômalos (Azure AD)

```powershell
# Logins das últimas 24h
Get-AzureADAuditSignInLogs -Filter "createdDateTime gt $(Get-Date).AddDays(-1)" |
    Where-Object {$_.Status.ErrorCode -eq 0} |
    Select UserPrincipalName, CreatedDateTime, IpAddress, Location
```

---

## 📍 FASE 6: RECUPERAÇÃO (1-2 dias)

### Ações de Recuperação

```
□ Reabilitar contas de usuários após reset de senha
□ Remover bloqueios temporários de rede
□ Restaurar acesso a sistemas
□ Verificar funcionalidade normal
□ Monitoramento intensivo por 48h:
  □ Logins incomuns
  □ Acessos a dados sensíveis
  □ Transferências de arquivos grandes
  □ Mudanças de configuração
```

### Validação

Antes de considerar o incidente resolvido:

- [ ] Usuário consegue logar normalmente?
- [ ] MFA está funcionando?
- [ ] Não há sinais de persistência do atacante?
- [ ] Não há novos emails da mesma campanha?
- [ ] Usuário foi re-treinado sobre o ataque?

---

## 📍 FASE 7: POST-MORTEM (1 semana após)

### Template de Relatório

```markdown
# Relatório de Incidente de Phishing

## Sumário Executivo
- **Data do Incidente:** 
- **Severidade:** 
- **Usuários Afetados:** 
- **Tempo Total de Resposta:** 
- **Status:** Resolvido/Em Andamento

## Timeline
| Horário | Evento |
|---------|--------|
| 09:15 | Primeiro email de phishing recebido |
| 09:47 | Usuário reportou ao Service Desk |
| 10:02 | SOC iniciou análise |
| 10:30 | Domínio bloqueado |
| ... | ... |

## Análise Técnica
- **Vetor de Ataque:** Email
- **TTPs do Atacante:** [Mapear para MITRE ATT&CK]
- **IOCs:**
  - Domínio: malicious.com
  - IP: 1.2.3.4
  - Hash: sha256...

## Impacto
- **Usuários que clicaram:** X
- **Credenciais comprometidas:** Y
- **Dados exfiltrados:** Nenhum/Quantidade
- **Perda financeira:** R$ 0

## Ações Tomadas
1. ...
2. ...

## Lições Aprendidas
### O que funcionou bem:
- Detecção rápida pelo usuário
- Resposta coordenada da equipe

### O que pode melhorar:
- Email gateway não bloqueou inicial
- Demora no bloqueio do domínio

## Recomendações
1. **Curto Prazo (1 semana):**
   - Atualizar regras de email gateway
   - Treinamento adicional para departamento X
   
2. **Médio Prazo (1 mês):**
   - Implementar DMARC em modo enforce
   - Simulação de phishing para todos usuários
   
3. **Longo Prazo (3 meses):**
   - Avaliar solução de sandboxing avançado
   - Programa de security champions
```

---

## 📊 Métricas e KPIs

### Acompanhar Mensalmente

| Métrica | Meta | Medição |
|---------|------|---------|
| **MTTD** (Mean Time To Detect) | < 1 hora | Tempo entre recebimento e primeiro reporte |
| **MTTR** (Mean Time To Respond) | < 4 horas | Tempo entre detecção e contenção completa |
| **Taxa de Cliques** | < 10% | % usuários que clicaram vs que receberam |
| **Taxa de Reporte** | > 50% | % usuários que reportaram vs que receberam |
| **Falsos Positivos** | < 5% | Emails legítimos reportados como phishing |

---

## 🛠️ Ferramentas Requeridas

### Essenciais
- ✅ Email Gateway (Proofpoint, Mimecast, etc)
- ✅ SIEM (Wazuh, Splunk, etc)
- ✅ EDR (CrowdStrike, SentinelOne, etc)
- ✅ Ticketing System (Jira, ServiceNow, etc)

### Recomendadas
- 📧 PhishTool / PhishER
- 🔍 VirusTotal API
- 🌐 URLScan.io
- 📊 MISP (Threat Intelligence Platform)
- 💬 Slack/Teams (comunicação rápida)

---

## 📚 Referências

- NIST Cybersecurity Framework
- SANS Incident Handler's Handbook
- MITRE ATT&CK Framework (T1566 - Phishing)
- CIS Controls v8

---

## 📝 Histórico de Revisões

| Versão | Data | Autor | Mudanças |
|--------|------|-------|----------|
| 1.0 | 2025-12 | SOC Team | Versão inicial |

---

## ✅ Aprovações

| Papel | Nome | Data | Assinatura |
|-------|------|------|------------|
| CISO | | | |
| SOC Manager | | | |
| IT Manager | | | |

---

**🚨 ESTE DOCUMENTO É CONFIDENCIAL**
Não compartilhe fora da organização sem autorização expressa.
