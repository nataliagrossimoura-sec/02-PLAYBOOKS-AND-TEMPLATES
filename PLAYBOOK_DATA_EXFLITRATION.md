# 💾 PLAYBOOK: Resposta a Exfiltração de Dados

## 📋 Informações do Documento

| Campo | Valor |
|-------|-------|
| **Versão** | 1.0 |
| **Tipo de Incidente** | Data Exfiltration / Data Breach |
| **Severidade** | Alta a Crítica |
| **MITRE ATT&CK** | T1048 - Exfiltration Over C2 Channel |

---

## 🎯 Objetivo

Detectar, conter e responder a exfiltração de dados corporativos ou sensíveis, incluindo cumprimento de obrigações legais (LGPD, GDPR).

---

## 🚨 Severidade e Impacto

| Tipo de Dado | Severidade | Obrigação Legal | Tempo Notificação |
|--------------|------------|-----------------|-------------------|
| **PII (CPF, RG, dados pessoais)** | 🔴 Crítica | LGPD Art. 48 | 72 horas |
| **Dados financeiros (cartões)** | 🔴 Crítica | PCI-DSS | Imediato |
| **Dados de saúde (HIPAA)** | 🔴 Crítica | HIPAA | 60 dias |
| **Propriedade intelectual** | 🟠 Alta | Depende | N/A |
| **Dados corporativos gerais** | 🟡 Média | Não | N/A |

---

## 📍 DETECÇÃO (0-30 min)

### Indicadores de Exfiltração

**Anomalias de Rede:**
```
🚩 Upload grande e incomum
🚩 Transferência para IP externo desconhecido
🚩 Conexões para serviços de file-sharing (Dropbox, Mega, WeTransfer)
🚩 Tráfego DNS anômalo (tunelling)
🚩 Uso de protocolos incomuns (FTP, SFTP fora do normal)
🚩 Compressão de múltiplos arquivos
🚩 Exfiltração noturna/fim de semana
```

**Anomalias de Acesso a Dados:**
```
🚩 Acesso a volumes incomuns de arquivos
🚩 Download de bases de dados completas
🚩 Queries SQL massivas
🚩 Acesso a arquivos nunca acessados antes
🚩 Acesso fora do perfil normal do usuário
🚩 Cópia de arquivos para mídia removível (USB)
```

### Queries SIEM

**Wazuh - Detecção de Upload Grande:**
```
rule.groups: "web" AND data.size: > 50000000
```

**Wazuh - Acesso a File Sharing:**
```
data.url: (*dropbox.com* OR *mega.nz* OR *wetransfer.com*) AND method: "POST"
```

**Splunk - Transferência de Dados:**
```
index=proxy action=POST bytes_out>50000000
| stats sum(bytes_out) as total_uploaded by src_ip, dest
| where total_uploaded > 100000000
```

### DLP Alerts (se disponível)

```
[ ] Documento marcado como confidencial saindo da rede
[ ] PII detectado em email externo
[ ] Dados sensíveis em dispositivo não gerenciado
[ ] Criptografia não autorizada de arquivos
```

---

## 📍 RESPOSTA IMEDIATA (30-60 min)

### Fase 1: Contenção de Rede (15 min)

**Opção A: Bloquear IP/Domínio de Destino**
```bash
# Firewall
iptables -A OUTPUT -d <IP_DESTINO> -j DROP

# pfSense
# Firewall → Rules → Block outbound to <IP>

# Bloquear domínio via DNS
echo "0.0.0.0 malicious-c2-domain.com" >> /etc/hosts
```

**Opção B: Isolar Sistema Fonte**
```bash
# Desconectar da rede
# Físico: desplugue cabo
# Virtual: desabilitar interface

# Ou limitar apenas a tráfego essencial
iptables -P OUTPUT DROP
iptables -A OUTPUT -d 192.168.1.0/24 -j ACCEPT  # LAN apenas
```

**Opção C: QoS / Throttling**
```bash
# Reduzir banda drasticamente para conter exfiltração
tc qdisc add dev eth0 root tbf rate 1kbit burst 1kb latency 70ms
```

### Fase 2: Preservação de Evidências

**Capturar Tráfego de Rede:**
```bash
# Iniciar captura PCAP
tcpdump -i eth0 -w /evidence/exfil-$(date +%Y%m%d-%H%M).pcap host <IP_SUSPEITO>

# Capturar por 30 minutos
timeout 1800 tcpdump -i any -w /evidence/network-traffic.pcap
```

**Logs Críticos:**
```bash
# Proxy/Firewall logs
grep "<IP_SUSPEITO>" /var/log/squid/access.log > /evidence/proxy.log

# Web server logs (se aplicável)
grep "POST\|PUT" /var/log/apache2/access.log | grep "<DATE>" > /evidence/uploads.log

# File access logs
ausearch -f /path/to/sensitive/data > /evidence/file-access.log

# Windows - File access
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4663} | 
  Where-Object {$_.Properties[6].Value -like "*sensitive-folder*"}
```

---

## 📍 ANÁLISE (1-4 horas)

### Determinar Escopo

**Quais dados foram exfiltrados?**
```bash
# Identificar arquivos acessados
# Linux
ausearch -k sensitive_data_access | aureport -f

# Windows
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4663} | 
  Select-Object TimeCreated, @{Name='File';Expression={$_.Properties[6].Value}}

# Queries de banco de dados executadas
grep "SELECT" /var/log/mysql/mysql.log | grep "<timestamp>"
```

**Volume de Dados:**
```bash
# Análise de logs de proxy
cat proxy.log | awk '{sum+=$10} END {print sum/1024/1024 " MB"}'

# Transferências via SCP/SFTP
grep "sent\|received" /var/log/auth.log | grep scp

# Cloud storage (se logging habilitado)
# AWS S3
aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=GetObject
```

**Para Onde?**
```bash
# Analisar destinos
cat netstat.log | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr

# Geolocalização de IPs
for ip in $(cat suspicious-ips.txt); do
  echo "=== $ip ==="
  curl -s "https://ipapi.co/$ip/json/" | jq '.city, .country_name'
done

# Whois dos domínios
whois suspicious-domain.com | grep -i "registrant\|creation"
```

### Identificar Método de Exfiltração

**Vetores Comuns:**
```
[ ] Email (anexos grandes)
[ ] Cloud storage (Dropbox, Google Drive, etc)
[ ] FTP/SFTP
[ ] HTTP/HTTPS POST
[ ] DNS Tunneling
[ ] Steganografia (dados em imagens)
[ ] Impressão/escaneamento
[ ] Mídia removível (USB)
```

**Análise de PCAP:**
```bash
# Identificar uploads HTTP
tshark -r capture.pcap -Y "http.request.method == POST" -T fields -e ip.dst -e http.host -e http.content_length

# Extrair arquivos transferidos
tcpflow -r capture.pcap -o /evidence/extracted-files/

# Verificar DNS queries anômalas (tunneling)
tshark -r capture.pcap -Y "dns" -T fields -e dns.qry.name | sort | uniq -c | sort -nr
```

### Classificar Dados

**Categorização:**
```
Tipo: [ ] PII  [ ] Financeiro  [ ] Saúde  [ ] IP  [ ] Outros

Sensibilidade:
[ ] Público
[ ] Interno
[ ] Confidencial
[ ] Restrito/Crítico

Quantidade:
Número de registros: _______
Volume (GB): _______
Número de arquivos: _______

Impacto:
[ ] Clientes afetados
[ ] Funcionários afetados  
[ ] Parceiros afetados
[ ] Sem PII identificável
```

---

## 📍 CONTENÇÃO COMPLETA (4-8 horas)

### Remover Acesso do Atacante

**Se conta comprometida:**
```powershell
# Desabilitar conta
Disable-ADAccount -Identity <compromised_user>

# Revogar sessões
Revoke-AzureADUserAllRefreshToken -ObjectId user@domain.com

# Resetar senha
Set-ADAccountPassword -Identity <user> -Reset

# Revisar permissões
Get-ADPrincipalGroupMembership <user>
```

**Se malware/backdoor:**
```
Ver playbook: malware-containment.md
```

### Bloquear Exfiltração Futura

**Implementar DLP (Data Loss Prevention):**
```
[ ] Bloquear uploads para file-sharing não autorizado
[ ] Monitorar emails com anexos grandes
[ ] Alertar em acessos massivos a dados
[ ] Criptografia obrigatória para dados em repouso
[ ] Watermarking de documentos sensíveis
```

**Segmentação de Rede:**
```
[ ] Isolar banco de dados em VLAN separada
[ ] Implementar Zero Trust Network
[ ] Restringir acesso a dados por necessidade (least privilege)
[ ] Micro-segmentação para sistemas críticos
```

---

## 📍 OBRIGAÇÕES LEGAIS (Paralelo à Resposta)

### LGPD (Brasil)

**Se PII de brasileiros foi exposto:**
```
Prazo: 72 horas para notificar ANPD

Notificação deve incluir:
[ ] Natureza dos dados pessoais afetados
[ ] Informações sobre os titulares envolvidos
[ ] Medidas técnicas e de segurança utilizadas
[ ] Riscos relacionados ao incidente
[ ] Motivos da demora (se houver)
[ ] Medidas adotadas para reverter ou mitigar efeitos

Contato ANPD: 
comunicacao@anpd.gov.br
```

**Notificar Titulares (Usuários Afetados):**
```
Quando notificar:
✓ Dados sensíveis (saúde, orientação sexual, etc)
✓ Alto risco aos direitos dos titulares
✓ Volume significativo de dados

Template de comunicação:
Ver: communication-templates.md
```

### GDPR (União Europeia)

**Se dados de cidadãos EU:**
```
Prazo: 72 horas para notificar autoridade supervisora

Autoridades por país:
- Portugal: CNPD
- Alemanha: BfDI
- França: CNIL
etc.

Requisitos similares ao LGPD +
[ ] Nome e contato do DPO (Data Protection Officer)
[ ] Possíveis consequências do breach
```

### PCI-DSS (Dados de Cartão)

**Se dados de pagamento expostos:**
```
Notificação IMEDIATA:
[ ] Bandeiras (Visa, Mastercard, etc)
[ ] Banco adquirente
[ ] Processadora de pagamento

Forensics obrigatório:
[ ] PCI Forensic Investigator (PFI) deve ser contratado
[ ] Relatório forense completo
[ ] Remediação de vulnerabilidades
```

---

## 📍 RECUPERAÇÃO (1-7 dias)

### Melhorias de Segurança

**Curto Prazo (1-2 semanas):**
```
[ ] Implementar monitoramento de exfiltração
[ ] DLP básico configurado
[ ] Logs de acesso a dados habilitados
[ ] Alertas para uploads grandes
[ ] Revisão de permissões de acesso
```

**Médio Prazo (1-3 meses):**
```
[ ] DLP completo implementado
[ ] Classificação de dados
[ ] Criptografia em repouso
[ ] Data masking em não-produção
[ ] CASB (Cloud Access Security Broker)
```

**Longo Prazo (3-6 meses):**
```
[ ] Zero Trust Architecture
[ ] Data governance programa
[ ] Regular security awareness
[ ] Red team exercise focado em exfiltração
[ ] Continuous monitoring
```

### Remediação para Afetados

**Se PII foi exposto:**
```
[ ] Oferecer monitoramento de crédito (1-2 anos)
[ ] Fornecer orientações sobre proteção
[ ] Canal de comunicação dedicado
[ ] Compensação (se aplicável)
```

---

## 📍 POST-MORTEM

### Análise de Causa Raiz

**Como exfiltração foi possível?**
```
[ ] Credenciais fracas/comprometidas
[ ] Phishing bem-sucedido
[ ] Insider threat (funcionário malicioso)
[ ] Malware com capacidade de exfiltração
[ ] Vulnerabilidade em aplicação
[ ] Permissões excessivas
[ ] Falta de monitoramento
[ ] DLP inexistente/inadequado
```

### Métricas

```
Tempo de exfiltração: _______ (quando começou → quando parou)
Volume total exfiltrado: _______ GB
Número de arquivos: _______
Registros afetados: _______
MTTD (Mean Time To Detect): _______ horas
MTTR (Mean Time To Respond): _______ horas
```

### Custos

```
Investigação forense: R$ _______
Notificações legais: R$ _______
Multas/penalidades: R$ _______
Monitoramento de crédito: R$ _______
Remediação técnica: R$ _______
Perda de negócio: R$ _______
Dano reputacional: (não quantificável)

TOTAL ESTIMADO: R$ _______
```

---

## 🛠️ Ferramentas

### Detecção
- **Splunk / Wazuh** - SIEM
- **Varonis** - Data security platform
- **McAfee DLP** - Data Loss Prevention
- **Wireshark** - Network analysis

### Forense
- **FTK** - Forensic Toolkit
- **EnCase** - Digital forensics
- **Volatility** - Memory forensics
- **Autopsy** - Disk analysis

### Monitoramento Proativo
- **OSSEC** - File integrity monitoring
- **Tripwire** - File integrity
- **Tenable** - Vulnerability management
- **Qualys** - Continuous monitoring

---

## 📊 IOCs para Documentar

```
IPs de destino:
- <IP1> | <País> | <Serviço>
- <IP2> | <País> | <Serviço>

Domínios:
- <domain1.com> | <Registrar> | <Data Registro>

Contas envolvidas:
- <user1> | <Departamento> | <Permissões>

Arquivos exfiltrados:
- /path/to/sensitive-file1.pdf | <Tamanho> | <Timestamp>
- /path/to/database-dump.sql | <Tamanho> | <Timestamp>

Métodos:
- [ ] Email
- [ ] Cloud Storage
- [ ] FTP
- [ ] HTTP POST
```

---

## 📧 Templates de Comunicação

### Email para Usuários Afetados

```
Assunto: Notificação Importante sobre Segurança de Dados

Prezado(a) [Nome],

Estamos entrando em contato para informá-lo(a) sobre um incidente de 
segurança que pode ter afetado seus dados pessoais.

O QUE ACONTECEU:
Em [data], identificamos que [descrição breve do incidente].

QUAIS DADOS FORAM AFETADOS:
[Lista de tipos de dados: nome, email, CPF, etc]

O QUE ESTAMOS FAZENDO:
- Investigação completa do incidente
- Implementação de medidas de segurança adicionais
- Notificação às autoridades competentes
- [outras medidas]

O QUE VOCÊ DEVE FAZER:
1. Monitore suas contas bancárias e de crédito
2. Altere senhas de contas relacionadas
3. Ative autenticação de dois fatores onde possível
4. Fique atento a emails/ligações suspeitas (phishing)

SUPORTE:
Oferecemos [X meses] de monitoramento de crédito gratuito.
Para dúvidas: [email] ou [telefone]

Lamentamos profundamente este incidente e estamos comprometidos em 
proteger seus dados.

Atenciosamente,
[Nome]
[Cargo]
[Empresa]
```

---

## ✅ Checklist de Resposta

```
DETECÇÃO (0-30 min):
[ ] Anomalia de rede detectada
[ ] Volume de dados determinado
[ ] Destino identificado
[ ] Tipo de dados classificado

CONTENÇÃO (30-60 min):
[ ] Exfiltração interrompida
[ ] Sistema fonte isolado
[ ] Evidências preservadas
[ ] Logs coletados

ANÁLISE (1-4 horas):
[ ] Escopo completo determinado
[ ] Dados afetados classificados
[ ] Método de exfiltração identificado
[ ] Root cause determinado

OBRIGAÇÕES LEGAIS (paralelo):
[ ] LGPD/GDPR avaliado
[ ] Notificações necessárias identificadas
[ ] Autoridades notificadas (se aplicável)
[ ] Usuários notificados (se aplicável)

RECUPERAÇÃO (dias):
[ ] Vulnerabilidades corrigidas
[ ] Monitoramento reforçado
[ ] DLP implementado/melhorado
[ ] Políticas atualizadas

POST-MORTEM:
[ ] Relatório completo
[ ] Lições aprendidas
[ ] Melhorias implementadas
[ ] Treinamento de equipe
```

---

**💾 Lembre-se:** Exfiltração de dados pode ter consequências legais sérias. Sempre envolva jurídico cedo no processo.

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0  
**Aprovado por:** CISO + Legal
