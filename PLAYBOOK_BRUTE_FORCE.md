# 🔐 PLAYBOOK: Resposta a Ataques de Brute Force

## 📋 Informações do Documento

| Campo | Valor |
|-------|-------|
| **Versão** | 1.0 |
| **Tipo de Incidente** | Brute Force / Credential Stuffing |
| **Severidade** | Média a Alta |
| **MITRE ATT&CK** | T1110 - Brute Force |

---

## 🎯 Objetivo

Detectar, conter e responder a tentativas de brute force contra sistemas de autenticação (SSH, RDP, HTTP, VPN, etc).

---

## 📊 Escopo

### Tipos de Ataque Cobertos

| Tipo | Descrição | Alvo Comum |
|------|-----------|------------|
| **Brute Force Tradicional** | Tenta todas combinações possíveis | Senhas fracas |
| **Dictionary Attack** | Tenta palavras de dicionário | Senhas comuns |
| **Credential Stuffing** | Usa credenciais vazadas | Senhas reutilizadas |
| **Password Spraying** | Tenta poucas senhas em muitas contas | Evitar lockout |
| **Reverse Brute Force** | Muitas contas, mesma senha | Senhas padrão |

### Protocolos/Serviços

- SSH (porta 22)
- RDP (porta 3389)
- HTTP/HTTPS (formulários web)
- VPN
- FTP/SFTP
- Email (IMAP, POP3, SMTP)
- API endpoints

---

## 🚨 Detecção

### Indicadores de Brute Force

**Padrões de Log:**
```
✗ Múltiplas falhas de login (5+ em 5 min)
✗ Mesmo usuário, múltiplos IPs diferentes
✗ Mesmo IP, múltiplos usuários
✗ Tentativas fora do horário comercial
✗ Logins de países incomuns
✗ User-agents suspeitos (bots)
✗ Tentativas em ordem alfabética/numérica
```

### Queries SIEM (Wazuh)

```
# SSH brute force
rule.id: 5710 AND rule.level: >= 5

# RDP brute force
rule.id: 60122 AND EventID: 4625

# Múltiplas falhas seguidas de sucesso
rule.id: (5710 OR 60122) AND rule.id: (5501 OR 4624)

# Password spraying (muitos usuários, mesmo IP)
data.srcip: <IP> AND rule.description: *failed* | stats count by data.dstuser
```

### Alertas a Configurar

**Wazuh Rule:**
```xml
<rule id="100300" level="10" frequency="5" timeframe="300">
  <if_matched_sid>5710</if_matched_sid>
  <same_source_ip />
  <description>Brute force attack detected - 5+ failures in 5 min</description>
  <mitre>
    <id>T1110</id>
  </mitre>
</rule>

<rule id="100301" level="12">
  <if_sid>5501</if_sid>
  <if_matched_sid>100300</if_matched_sid>
  <same_source_ip />
  <description>Successful login after brute force - Possible compromise</description>
  <mitre>
    <id>T1078</id>
  </mitre>
</rule>
```

---

## 📍 RESPOSTA RÁPIDA (0-30 min)

### Fase 1: Validação (5 min)

**Verificar se é ataque real:**
```
[ ] Múltiplas falhas confirmadas?
[ ] Padrão anômalo (não é usuário legítimo esquecendo senha)?
[ ] IP externo ou interno?
[ ] Usuário válido ou tentativa de múltiplos usuários?
```

**Falso Positivo Comum:**
- Usuário esqueceu senha (2-3 tentativas)
- Script mal configurado
- Aplicação com credencial errada

### Fase 2: Bloqueio Imediato (10 min)

**Opção A: Bloqueio de IP**
```bash
# Firewall - Bloquear IP atacante
iptables -A INPUT -s <IP_ATACANTE> -j DROP
iptables-save

# pfSense
# Via GUI: Firewall → Rules → Add block rule

# Fail2ban (automatizado)
sudo fail2ban-client status
sudo fail2ban-client set sshd banip <IP>

# Windows Firewall
New-NetFirewallRule -DisplayName "Block Attacker" -Direction Inbound -RemoteAddress <IP> -Action Block
```

**Opção B: Rate Limiting**
```bash
# iptables rate limit SSH
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP

# Nginx rate limit
limit_req_zone $binary_remote_addr zone=login:10m rate=1r/s;
location /login {
    limit_req zone=login burst=5;
}
```

**Opção C: Desabilitar Conta Alvo (se persistente)**
```bash
# Linux
usermod -L <username>
passwd -l <username>

# Windows
Disable-ADAccount -Identity <username>

# Temporário - desabilitar por 1 hora
at now + 1 hour -f enable-user.sh
```

### Fase 3: Análise (20 min)

**Coletar Informações:**
```bash
# Quantas tentativas?
grep "Failed password" /var/log/auth.log | wc -l

# Quais usuários foram alvos?
grep "Failed password" /var/log/auth.log | awk '{print $(NF-5)}' | sort | uniq -c | sort -nr

# De quais IPs?
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

# Alguma tentativa bem-sucedida?
grep "Accepted" /var/log/auth.log | grep <IP_ATACANTE>

# Windows - Event ID 4625 (failed) e 4624 (success)
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4625} | 
  Where-Object {$_.Properties[19].Value -eq '<IP>'} | 
  Measure-Object
```

**Geolocalização:**
```bash
# Verificar localização do IP
curl "https://ipapi.co/<IP>/json/"
whois <IP> | grep -i country
```

**Reputação do IP:**
```bash
# AbuseIPDB
curl "https://api.abuseipdb.com/api/v2/check?ipAddress=<IP>" \
  -H "Key: YOUR_API_KEY"

# Verificar em blacklists
host <IP> | grep "NXDOMAIN"  # Não está em blacklist se retornar NXDOMAIN
```

---

## 📍 CONTENÇÃO (30-60 min)

### Proteger Contas Comprometidas

**Se login bem-sucedido após brute force:**
```powershell
# 1. Desabilitar conta IMEDIATAMENTE
Disable-ADAccount -Identity <compromised_user>

# 2. Revogar todas sessões ativas
# Azure AD
Revoke-AzureADUserAllRefreshToken -ObjectId user@domain.com

# Linux
pkill -KILL -u <username>

# 3. Resetar senha
Set-ADAccountPassword -Identity <username> -Reset

# 4. Forçar MFA no próximo login
Set-ADUser -Identity <username> -Replace @{msodsPhoneAuthenticationMFAOnNextLogon="1"}

# 5. Revisar atividade recente
Get-ADUser -Identity <username> -Properties LastLogonDate, LastBadPasswordAttempt
```

### Implementar Controles Preventivos

**SSH Hardening:**
```bash
# /etc/ssh/sshd_config

# Desabilitar password auth (usar keys apenas)
PasswordAuthentication no
PubkeyAuthentication yes

# Desabilitar root login
PermitRootLogin no

# Limitar tentativas
MaxAuthTries 3

# Limitar usuários permitidos
AllowUsers admin user1 user2

# Reiniciar SSH
systemctl restart sshd
```

**RDP Hardening:**
```powershell
# Account Lockout Policy
net accounts /lockoutthreshold:5
net accounts /lockoutduration:30
net accounts /lockoutwindow:30

# Network Level Authentication
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name 'UserAuthentication' -Value 1

# Restringir IPs permitidos
New-NetFirewallRule -DisplayName "RDP Restrict" -Direction Inbound -LocalPort 3389 -Protocol TCP -RemoteAddress <IP_PERMITIDO> -Action Allow
```

**Web Application:**
```python
# Implementar rate limiting na aplicação
from flask_limiter import Limiter

limiter = Limiter(app, key_func=lambda: request.remote_addr)

@app.route('/login', methods=['POST'])
@limiter.limit("3 per minute")
def login():
    # login logic
```

---

## 📍 ERRADICAÇÃO E PREVENÇÃO

### Medidas de Longo Prazo

**1. Implementar MFA (Multi-Factor Authentication)**
```
✅ TOTP (Google Authenticator, Authy)
✅ SMS (menos seguro, mas melhor que nada)
✅ Hardware tokens (YubiKey)
✅ Biometria (quando disponível)
```

**2. Política de Senhas Forte**
```
✅ Mínimo 12 caracteres
✅ Complexidade (maiúsculas, minúsculas, números, símbolos)
✅ Expiração (90-180 dias)
✅ Histórico (não permitir últimas 5 senhas)
✅ Bloqueio de senhas comuns (usar zxcvbn ou similar)
```

**3. Account Lockout Policy**
```
✅ Bloqueio após 5 tentativas falhadas
✅ Duração: 30 minutos
✅ Janela: 10 minutos
✅ CAPTCHA após 3 falhas
```

**4. Monitoramento Contínuo**
```bash
# Fail2ban (Linux)
apt install fail2ban -y

# /etc/fail2ban/jail.local
[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600

# Iniciar
systemctl enable fail2ban
systemctl start fail2ban

# Verificar bans ativos
fail2ban-client status sshd
```

**5. Geofencing (Opcional)**
```bash
# Bloquear países inteiros (se não tiver usuários lá)
# GeoIP com iptables
iptables -A INPUT -p tcp --dport 22 -m geoip --src-cc RU,CN,KP -j DROP
```

---

## 📍 INVESTIGAÇÃO APROFUNDADA

### Se Brute Force Foi Bem-Sucedido

**Timeline de Atividade:**
```bash
# Linux - últimos logins
last -f /var/log/wtmp | grep <username>
lastlog | grep <username>

# Comandos executados
cat ~/.bash_history
journalctl -u ssh --since "2025-12-29 00:00:00"

# Arquivos acessados/modificados
find /home/<user> -type f -mtime -1

# Windows - Event Logs
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4624} | 
  Where-Object {$_.Properties[5].Value -eq '<username>'}
```

**Verificar Ações Maliciosas:**
```
[ ] Arquivos baixados?
[ ] Arquivos exfiltrados?
[ ] Novos usuários criados?
[ ] Privilégios elevados?
[ ] Malware instalado?
[ ] Backdoor criado?
[ ] Movimento lateral?
```

---

## 📊 Métricas e Relatório

### Dados a Coletar

```
Total de tentativas: _______
Período do ataque: _______ a _______
IPs únicos envolvidos: _______
Usuários alvo: _______
Tentativas bem-sucedidas: _______
Tempo de detecção (MTTD): _______ min
Tempo de bloqueio (MTTR): _______ min
```

### IOCs para Documentar

```
IPs atacantes:
- <IP1> | <País> | <ASN>
- <IP2> | <País> | <ASN>

Contas alvejadas:
- <user1> | Comprometida: S/N
- <user2> | Comprometida: S/N

User-agents (se web):
- Mozilla/5.0 (compatible; bot/1.0)
```

---

## 🛠️ Automações Recomendadas

### Script de Auto-Bloqueio

```bash
#!/bin/bash
# auto-block-bruteforce.sh

LOG_FILE="/var/log/auth.log"
THRESHOLD=5
TIMEFRAME=300  # 5 minutos

# Encontrar IPs com 5+ falhas nos últimos 5 min
awk -v date="$(date --date='5 minutes ago' '+%b %d %H:%M')" '
  $0 ~ date {print $(NF-3)}
' "$LOG_FILE" | grep "Failed password" | sort | uniq -c | 
while read count ip; do
  if [ "$count" -ge "$THRESHOLD" ]; then
    # Verificar se já está bloqueado
    if ! iptables -L INPUT -n | grep -q "$ip"; then
      echo "Blocking $ip - $count attempts"
      iptables -A INPUT -s "$ip" -j DROP
      # Log
      logger "SECURITY: Blocked $ip after $count failed login attempts"
    fi
  fi
done
```

### Cron Job para Monitoramento

```bash
# /etc/cron.d/monitor-bruteforce
*/5 * * * * root /usr/local/bin/auto-block-bruteforce.sh
```

### Alert via Webhook (Slack/Teams)

```python
#!/usr/bin/env python3
import requests
import json

def send_alert(ip, attempts, user):
    webhook_url = "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
    
    message = {
        "text": f"🚨 Brute Force Alert",
        "attachments": [{
            "color": "danger",
            "fields": [
                {"title": "IP Atacante", "value": ip, "short": True},
                {"title": "Tentativas", "value": str(attempts), "short": True},
                {"title": "Usuário Alvo", "value": user, "short": True},
                {"title": "Ação", "value": "IP foi bloqueado automaticamente", "short": False}
            ]
        }]
    }
    
    requests.post(webhook_url, data=json.dumps(message))

# Uso
send_alert("1.2.3.4", 15, "admin")
```

---

## 📚 Referências

- [NIST SP 800-63B - Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [CIS Controls v8 - Access Control](https://www.cisecurity.org/controls/v8)

---

## ✅ Checklist de Resposta

```
DETECÇÃO:
[ ] Padrão de brute force confirmado
[ ] Severidade determinada
[ ] IP(s) atacante(s) identificado(s)
[ ] Conta(s) alvo identificada(s)

CONTENÇÃO:
[ ] IP bloqueado no firewall
[ ] Rate limiting implementado
[ ] Conta comprometida desabilitada (se aplicável)
[ ] Sessões ativas revogadas

ERRADICAÇÃO:
[ ] Senhas resetadas
[ ] MFA forçado
[ ] Account lockout configurado
[ ] Monitoramento reforçado

PREVENÇÃO:
[ ] Fail2ban/equivalente instalado
[ ] SSH hardening aplicado
[ ] Política de senhas fortalecida
[ ] Treinamento de usuários

DOCUMENTAÇÃO:
[ ] IOCs documentados
[ ] Timeline criada
[ ] Relatório completo
[ ] Lições aprendidas
```

---

**🔐 Lembre-se:** Brute force é jogo de números. Quanto mais difícil você tornar, mais improvável o sucesso.

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0  
**Aprovado por:** CISO
