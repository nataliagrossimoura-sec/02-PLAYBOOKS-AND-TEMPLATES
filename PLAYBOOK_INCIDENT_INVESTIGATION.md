# 🔍 PLAYBOOK: Investigação de Incidentes de Segurança

## 📋 Informações do Documento

| Campo | Valor |
|-------|-------|
| **Versão** | 1.0 |
| **Data de Criação** | Dezembro 2025 |
| **Última Revisão** | Dezembro 2025 |
| **Proprietário** | Equipe SOC |
| **Classificação** | Interno - Confidencial |

---

## 🎯 Objetivo

Este playbook fornece metodologia padronizada para investigação de incidentes de segurança quando o tipo ou escopo não está imediatamente claro.

---

## 📊 Escopo

### Quando Usar Este Playbook

- ✅ Tipo de incidente desconhecido ou misto
- ✅ Múltiplos indicadores sem padrão claro
- ✅ Incidente complexo envolvendo várias técnicas
- ✅ Necessidade de investigação forense profunda
- ✅ Incidente com impacto não determinado

### Quando NÃO Usar

- ❌ Phishing claramente identificado → usar `phishing-response.md`
- ❌ Malware confirmado → usar `malware-containment.md`
- ❌ Brute force óbvio → usar `brute-force-response.md`

---

## 👥 Papéis e Responsabilidades

| Papel | Responsabilidade | Quando Envolver |
|-------|------------------|-----------------|
| **Incident Commander** | Coordena investigação, toma decisões | Imediatamente |
| **Lead Investigator** | Conduz análise técnica | Imediatamente |
| **Forensic Analyst** | Coleta evidências, análise profunda | Se escalação necessária |
| **System Admin** | Acesso a sistemas, logs | Quando solicitado |
| **Legal/Compliance** | Aspectos legais, privacidade | Se dados sensíveis envolvidos |
| **Communications** | Stakeholder updates | Incidentes de alta visibilidade |

---

## 🚨 Matriz de Severidade

| Nível | Critérios | Tempo Resposta | Escalação |
|-------|-----------|----------------|-----------|
| **P1 - Crítico** | Dados sensíveis comprometidos, Sistema crítico offline, Breach confirmado | 15 min | CISO + CEO |
| **P2 - Alto** | Sistema importante afetado, Tentativa de breach, Múltiplos sistemas | 30 min | CISO |
| **P3 - Médio** | Sistema não-crítico afetado, Tentativa bloqueada, Usuário comum | 2 horas | Manager |
| **P4 - Baixo** | Alerta isolado, Falso positivo provável, Sem impacto | 4 horas | Lead |

---

## 🔄 METODOLOGIA DE INVESTIGAÇÃO

```
┌─────────────────────────────────────────────────────┐
│              CICLO DE INVESTIGAÇÃO                  │
├─────────────────────────────────────────────────────┤
│                                                     │
│  1. PREPARAÇÃO                                      │
│     ↓                                               │
│  2. IDENTIFICAÇÃO                                   │
│     ↓                                               │
│  3. CONTENÇÃO INICIAL                               │
│     ↓                                               │
│  4. ANÁLISE PROFUNDA                                │
│     ↓                                               │
│  5. ERRADICAÇÃO                                     │
│     ↓                                               │
│  6. RECUPERAÇÃO                                     │
│     ↓                                               │
│  7. LIÇÕES APRENDIDAS                               │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 📍 FASE 1: PREPARAÇÃO (0-15 min)

### Objetivos
- Reunir equipe de resposta
- Estabelecer canal de comunicação
- Preservar estado atual
- Iniciar documentação

### Ações Imediatas

**1.1 Criar War Room**
```
[ ] Criar canal Slack/Teams: #incident-YYYY-MM-DD
[ ] Convidar: Incident Commander, Lead Investigator, SysAdmin
[ ] Definir ponto de contato único
[ ] Estabelecer frequência de updates
```

**1.2 Iniciar Documentação**
```bash
# Criar pasta do incidente
mkdir -p incidents/INC-$(date +%Y%m%d-%H%M)
cd incidents/INC-$(date +%Y%m%d-%H%M)

# Copiar template
cp ../../playbooks/templates/incident-report-template.md investigation.md

# Iniciar log de atividades
echo "$(date) - Investigação iniciada" > timeline.log
```

**1.3 Preservar Estado Inicial**
```
[ ] NÃO desligar sistemas ainda
[ ] Tirar snapshots de VMs (se aplicável)
[ ] Exportar logs atuais
[ ] Fotografar telas se necessário
[ ] Anotar: data/hora/timezone
```

---

## 📍 FASE 2: IDENTIFICAÇÃO (15-60 min)

### Objetivos
- Determinar tipo de incidente
- Identificar sistemas afetados
- Avaliar escopo inicial
- Classificar severidade

### 2.1 Coleta de Informações Iniciais

**Perguntas Essenciais:**
```
❓ Como o incidente foi detectado?
   - Alerta automático
   - Reporte de usuário
   - Descoberta em auditoria
   - Notificação externa

❓ Quando começou?
   - Data/hora do primeiro indicador
   - Data/hora da detecção
   - Gap entre início e detecção

❓ Quais sistemas estão envolvidos?
   - Servidores
   - Workstations
   - Dispositivos de rede
   - Aplicações

❓ Quais usuários/contas?
   - Contas comprometidas
   - Contas acessando sistemas afetados
   - Privilégios dessas contas

❓ Que dados estão em risco?
   - Tipo de dados (PII, financeiro, IP)
   - Volume estimado
   - Sensibilidade
```

### 2.2 Coleta de Evidências Iniciais

**A) Logs de Sistema**
```bash
# Linux - últimas 1000 linhas de logs críticos
tail -n 1000 /var/log/auth.log > evidence/auth.log
tail -n 1000 /var/log/syslog > evidence/syslog
last -f /var/log/wtmp > evidence/logins.txt

# Windows - exportar Event Logs
wevtutil epl Security evidence/Security.evtx
wevtutil epl System evidence/System.evtx
wevtutil epl Application evidence/Application.evtx
```

**B) Conexões de Rede Ativas**
```bash
# Linux
netstat -antp > evidence/netstat.txt
ss -tunap > evidence/ss.txt

# Windows
netstat -ano > evidence\netstat.txt
Get-NetTCPConnection | Export-Csv evidence\connections.csv
```

**C) Processos em Execução**
```bash
# Linux
ps auxf > evidence/processes.txt
top -b -n 1 > evidence/top.txt

# Windows
Get-Process | Export-Csv evidence\processes.csv
tasklist /v > evidence\tasklist.txt
```

**D) Snapshots de Memória (se suspeita de malware)**
```bash
# Linux - LiME (Linux Memory Extractor)
insmod lime.ko "path=evidence/memory.lime format=lime"

# Windows - DumpIt
DumpIt.exe /OUTPUT evidence\memory.dmp
```

### 2.3 Análise SIEM

**Queries Essenciais no Wazuh:**
```
# Eventos de alta severidade nas últimas 24h
rule.level: >= 10 AND timestamp: [now-24h TO now]

# Logins falhados seguidos de sucesso
rule.id: (5710 OR 5551) AND timestamp: [now-24h TO now]

# Criação de contas
rule.id: 4720 AND timestamp: [now-7d TO now]

# Mudanças de privilégio
rule.id: 4672 AND timestamp: [now-7d TO now]

# Conexões externas suspeitas
data.srcip: NOT (10.0.0.0/8 OR 172.16.0.0/12 OR 192.168.0.0/16)
```

### 2.4 Classificação Inicial

**Determinar Tipo de Ataque (MITRE ATT&CK):**
```
Reconhecimento → T1595 Active Scanning
Acesso Inicial → T1566 Phishing, T1190 Exploit
Execução → T1059 Command Interpreter
Persistência → T1543 Create Service
Escalação → T1548 Abuse Elevation
Evasão → T1070 Indicator Removal
Credential Access → T1110 Brute Force
Descoberta → T1083 File Discovery
Movimento Lateral → T1021 Remote Services
Coleta → T1005 Data from Local System
Exfiltração → T1048 Exfiltration Over C2
Impacto → T1486 Data Encrypted (Ransomware)
```

---

## 📍 FASE 3: CONTENÇÃO INICIAL (1-2 horas)

### Objetivos
- Parar propagação do incidente
- Minimizar dano adicional
- Manter evidências intactas

### 3.1 Decisão: Isolar ou Monitorar?

**Isolar Imediatamente Se:**
- ✅ Ransomware ativo
- ✅ Exfiltração de dados em andamento
- ✅ Propagação lateral detectada
- ✅ Sistema crítico comprometido

**Monitorar (não isolar ainda) Se:**
- ⚠️ Atacante ainda ativo e você quer observar TTPs
- ⚠️ Possibilidade de pegar atacante ao vivo
- ⚠️ Necessidade de identificar todos sistemas comprometidos
- ⚠️ Coordenação com law enforcement

⚡ **REGRA:** Na dúvida, ISOLE. Evidências > Observação.

### 3.2 Contenção de Rede

**Nível 1 - Bloquear IPs Maliciosos**
```bash
# Firewall - bloquear IP atacante
iptables -A INPUT -s <IP_ATACANTE> -j DROP
iptables -A OUTPUT -d <IP_ATACANTE> -j DROP

# pfSense
# Via GUI: Firewall → Rules → Add rule to block
```

**Nível 2 - Isolar Sistema Comprometido**
```bash
# Opção A: Desconectar da rede (preserva estado)
# Desplugue cabo de rede / Disable WiFi

# Opção B: Quarentena (mantém monitoramento)
# Mover para VLAN isolada

# Opção C: Desligar (se dados em RAM não são críticos)
shutdown -h now
```

**Nível 3 - Segmentar Rede**
```
[ ] Isolar segmento afetado
[ ] Permitir apenas tráfego essencial
[ ] Monitorar tráfego entre segmentos
[ ] Bloquear movimento lateral
```

### 3.3 Contenção de Contas

**Contas Comprometidas:**
```powershell
# Desabilitar conta
Disable-ADAccount -Identity "usuario_comprometido"

# Revogar sessões ativas (Azure AD)
Revoke-AzureADUserAllRefreshToken -ObjectId user@empresa.com

# Forçar logout (Linux)
pkill -KILL -u usuario_comprometido

# Resetar senha
Set-ADAccountPassword -Identity "usuario" -Reset
```

**Contas Suspeitas (ainda investigando):**
```
[ ] Forçar MFA
[ ] Monitorar atividade
[ ] Limitar permissões temporariamente
[ ] Alertar em cada login
```

---

## 📍 FASE 4: ANÁLISE PROFUNDA (2-8 horas)

### Objetivos
- Entender completamente o ataque
- Identificar todos sistemas afetados
- Determinar dados comprometidos
- Mapear TTPs do atacante

### 4.1 Análise de Timeline

**Construir Cronologia Completa:**
```
T-7 dias: Scan da rede detectado
T-3 dias: Phishing email enviado
T-2 dias: Usuário clicou, malware baixado
T-1 dia:  Malware executado, beacon C2
T-12h:    Escalação de privilégios
T-6h:     Movimento lateral → servidor
T-2h:     Exfiltração iniciada
T-0:      Detecção e resposta
```

**Ferramentas:**
```bash
# Plaso/Log2timeline - análise forense de timeline
log2timeline.py timeline.plaso /path/to/image

# Visualizar
psort.py -o l2tcsv timeline.plaso > timeline.csv
```

### 4.2 Análise Forense

**A) Análise de Disco**
```bash
# Criar imagem forense
dd if=/dev/sda of=evidence/disk.img bs=4M status=progress

# Calcular hash (integridade)
sha256sum evidence/disk.img > evidence/disk.img.sha256

# Montar read-only
mount -o ro,loop evidence/disk.img /mnt/forensics

# Buscar IOCs
grep -r "malicious-domain.com" /mnt/forensics/
find /mnt/forensics/ -name "*.exe" -mtime -7
```

**B) Análise de Memória**
```bash
# Volatility - análise de RAM dump
volatility -f memory.dmp imageinfo
volatility -f memory.dmp --profile=Win10x64 pslist
volatility -f memory.dmp --profile=Win10x64 netscan
volatility -f memory.dmp --profile=Win10x64 malfind
```

**C) Análise de Rede (PCAP)**
```bash
# Capturar tráfego
tcpdump -i eth0 -w evidence/traffic.pcap

# Analisar com Wireshark
wireshark evidence/traffic.pcap

# Extrair objetos HTTP
tshark -r traffic.pcap --export-objects http,evidence/http-objects/

# Buscar IOCs
strings traffic.pcap | grep -i "malicious"
```

### 4.3 Análise de Malware (se encontrado)

**⚠️ APENAS EM SANDBOX ISOLADO**

```bash
# Informações básicas
file malware.exe
strings malware.exe | less
md5sum malware.exe
sha256sum malware.exe

# VirusTotal (submir hash, não arquivo completo)
curl -X POST "https://www.virustotal.com/api/v3/files/$(sha256sum malware.exe | cut -d' ' -f1)" \
  -H "x-apikey: YOUR_API_KEY"

# Análise estática (não execute!)
objdump -d malware.exe
readelf -h malware.exe

# Análise dinâmica (em sandbox)
# Use ANY.RUN, Joe Sandbox, Cuckoo, etc.
```

### 4.4 Identificação de Escopo Completo

**Responder:**
```
✓ Todos sistemas comprometidos identificados?
✓ Todas contas comprometidas identificadas?
✓ Persistência do atacante removida?
✓ Dados exfiltrados determinados?
✓ Método de entrada inicial conhecido?
✓ TTPs do atacante mapeados?
```

---

## 📍 FASE 5: ERRADICAÇÃO (4-24 horas)

### Objetivos
- Remover presença do atacante
- Fechar vulnerabilidades exploradas
- Eliminar persistência

### 5.1 Remover Malware/Backdoors

**Verificação Completa:**
```bash
# Scan de antivírus completo
clamscan -r --infected --remove /

# Verificar tarefas agendadas
crontab -l
ls -la /etc/cron.*

# Windows
schtasks /query /fo LIST /v

# Verificar serviços suspeitos
systemctl list-units --type=service --state=running

# Windows
Get-Service | Where-Object {$_.Status -eq "Running"}
```

**Remover Persistência:**
```bash
# Checar startup items
ls -la ~/.config/autostart/
ls -la /etc/xdg/autostart/

# Windows
Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Run
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Run

# Verificar contas criadas recentemente
awk -F: '$3 >= 1000' /etc/passwd

# Windows - contas criadas últimos 7 dias
Get-ADUser -Filter * -Properties Created | Where-Object {$_.Created -gt (Get-Date).AddDays(-7)}
```

### 5.2 Patch de Vulnerabilidades

**Identificar e Corrigir:**
```
[ ] Vulnerability scan completo
[ ] Aplicar patches críticos
[ ] Atualizar antivírus/EDR
[ ] Configurar firewall mais restritivo
[ ] Desabilitar serviços não necessários
[ ] Mudar senhas padrão
[ ] Implementar princípio de mínimo privilégio
```

### 5.3 Reconstruir vs Restaurar

**Reconstruir (Preferível):**
```
✅ Reinstalar SO do zero
✅ Aplicar todas atualizações
✅ Reinstalar aplicações
✅ Restaurar dados de backup limpo
✅ Testar completamente antes de produção
```

**Restaurar (Mais Rápido):**
```
⚠️ Restaurar de backup verificadamente limpo
⚠️ Aplicar todas correções
⚠️ Scan completo de malware
⚠️ Monitoramento intensivo pós-restauração
```

---

## 📍 FASE 6: RECUPERAÇÃO (1-7 dias)

### Objetivos
- Retornar à operação normal
- Validar correções
- Monitorar reinfecção

### 6.1 Retorno Gradual

**Abordagem Faseada:**
```
Fase 1 (Dia 1): Validação em ambiente de teste
Fase 2 (Dia 2): Retorno de sistemas não-críticos
Fase 3 (Dia 3-4): Monitoramento intensivo
Fase 4 (Dia 5): Retorno de sistemas críticos
Fase 5 (Dia 6-7): Normalização completa
```

### 6.2 Checklist de Validação

```
[ ] Sistemas operando normalmente
[ ] Performance dentro do esperado
[ ] Usuários conseguem acessar recursos
[ ] Sem alertas de segurança anômalos
[ ] Backups testados e funcionais
[ ] Documentação atualizada
[ ] Equipe treinada em mudanças
```

### 6.3 Monitoramento Pós-Incidente

**Intensificar por 30 dias:**
```
[ ] Logs de autenticação (diariamente)
[ ] Tráfego de rede (alertas em tempo real)
[ ] Criação de arquivos/processos
[ ] Mudanças de configuração
[ ] Tentativas de acesso a dados sensíveis
[ ] Comunicações para IPs/domínios do incidente
```

---

## 📍 FASE 7: LIÇÕES APRENDIDAS (1-2 semanas após)

### Objetivos
- Documentar completamente
- Identificar melhorias
- Atualizar controles

### 7.1 Post-Mortem Meeting

**Agenda:**
```
1. Cronologia do Incidente (15 min)
2. O que funcionou bem (15 min)
3. O que pode melhorar (30 min)
4. Próximos passos (15 min)
5. Q&A (15 min)
```

**Participantes:**
- Toda equipe de resposta
- Management relevante
- Stakeholders afetados

### 7.2 Relatório Final

Usar template: `incident-report-template.md`

**Seções Críticas:**
- Timeline completo
- Root cause analysis
- IOCs completos
- Impacto quantificado
- Recomendações priorizadas

### 7.3 Melhorias a Implementar

**Categorizar:**
```
Curto Prazo (1-2 semanas):
[ ] Patches críticos
[ ] Regras de detecção
[ ] Bloqueios de IOCs

Médio Prazo (1-3 meses):
[ ] Implementar controles novos
[ ] Treinamentos
[ ] Revisão de processos

Longo Prazo (3-6 meses):
[ ] Arquitetura
[ ] Ferramentas
[ ] Programas
```

---

## 📊 Métricas e KPIs

### Medir Efetividade

| Métrica | Fórmula | Meta |
|---------|---------|------|
| **MTTD** | Tempo início → detecção | < 1 hora |
| **MTTR** | Tempo detecção → contenção | < 4 horas |
| **MTTE** | Tempo detecção → erradicação | < 24 horas |
| **Completude** | % escopo identificado corretamente | > 95% |

### Documentar para Cada Incidente

```
Início do ataque: ___________
Detecção: ___________
Primeira contenção: ___________
Erradicação completa: ___________
Retorno à operação: ___________

MTTD: _____ horas
MTTR: _____ horas
MTTE: _____ horas

Sistemas afetados: _____
Usuários afetados: _____
Dados comprometidos: _____
Custo estimado: R$ _____
```

---

## 🛠️ Ferramentas Essenciais

### Por Fase

**Coleta de Evidências:**
- FTK Imager (Windows disk imaging)
- dd / dc3dd (Linux disk imaging)
- DumpIt (memory capture)
- tcpdump / Wireshark (network)

**Análise:**
- Volatility (memory forensics)
- Autopsy (disk forensics)
- Plaso/Log2timeline (timeline)
- Splunk / ELK (log analysis)

**Investigação:**
- VirusTotal
- URLScan.io
- AbuseIPDB
- MISP (threat intelligence)

**Documentação:**
- Jira / ServiceNow (ticketing)
- Draw.io (diagramas)
- Markdown / Pandoc (relatórios)

---

## 📚 Referências

- [NIST SP 800-61 Rev. 2 - Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
- [SANS Incident Handler's Handbook](https://www.sans.org/white-papers/33901/)
- [MITRE ATT&CK Framework](https://attack.mitre.org)
- [FIRST CSIRT Framework](https://www.first.org/standards/frameworks/csirts/csirt_services_framework_v2.1)

---

## ✅ Checklist de Resposta Rápida

```
IMEDIATO (0-15 min):
[ ] Criar war room
[ ] Iniciar documentação
[ ] Preservar evidências
[ ] Classificar severidade
[ ] Notificar stakeholders

CURTO PRAZO (15min-2h):
[ ] Contenção inicial
[ ] Coletar logs/evidências
[ ] Identificar escopo
[ ] Bloquear IOCs conhecidos

MÉDIO PRAZO (2-8h):
[ ] Análise profunda
[ ] Determinar root cause
[ ] Identificar todos afetados
[ ] Planejar erradicação

LONGO PRAZO (8-24h):
[ ] Erradicar ameaça
[ ] Começar recuperação
[ ] Validar correções
[ ] Monitoramento intensivo

PÓS-INCIDENTE (dias/semanas):
[ ] Completar documentação
[ ] Post-mortem meeting
[ ] Implementar melhorias
[ ] Atualizar playbooks
```

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0  
**Aprovado por:** CISO
