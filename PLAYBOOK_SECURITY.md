# 🔐 Security Best Practices

## Visão Geral

Este documento descreve práticas de segurança recomendadas ao trabalhar com este projeto, especialmente ao lidar com phishing samples, malware e vulnerabilidades.

---

## ⚠️ AVISOS CRÍTICOS

### 🚨 NUNCA FAÇA ISSO

| ❌ Ação Proibida | Por Quê | Consequência |
|------------------|---------|--------------|
| Clicar em links de phishing | Pode comprometer seu sistema | Malware, roubo de dados |
| Executar anexos maliciosos | Instala malware real | Infecção, ransomware |
| Usar credenciais reais | Pode vazar informações | Roubo de identidade |
| Testar em rede de produção | Afeta sistemas críticos | Downtime, perda de dados |
| Compartilhar samples sem defang | Propaga malware | Responsabilidade legal |
| Desabilitar antivírus para testar | Remove proteção | Infecção do sistema |

---

## 🛡️ Princípios Fundamentais

### 1. Isolamento

**Sempre use ambiente isolado:**

```
✅ Ambiente Seguro:
- VM isolada
- Rede separada
- Snapshot antes de testes
- Sem acesso a dados reais

❌ Ambiente Inseguro:
- Host principal
- Rede corporativa
- Sem backup
- Dados de produção
```

### 2. Defanging

**Sempre "desarme" URLs e domínios antes de compartilhar:**

```
Original:  http://malicious.com
Defanged:  hxxp://malicious[.]com

Original:  malware@evil.org
Defanged:  malware[@]evil[.]org

Original:  192.168.1.100
Defanged:  192[.]168[.]1[.]100
```

**Por quê?**
- Previne cliques acidentais
- Previne execução automática
- Protege quem lê a documentação

### 3. Least Privilege

**Use sempre o mínimo de privilégios necessário:**

```bash
# ❌ ERRADO - Tudo como root
sudo su
# executar tudo aqui

# ✅ CORRETO - Sudo apenas quando necessário
python3 script.py                    # sem sudo
sudo systemctl restart wazuh-manager # com sudo apenas aqui
```

---

## 🔒 Segurança do Lab

### Isolamento de Rede

**Configuração Recomendada:**

```
┌─────────────────────────────────────┐
│         Internet                     │
└──────────────┬──────────────────────┘
               │
        ┌──────▼──────┐
        │   pfSense   │ ← Firewall rules
        │   (NAT)     │
        └──────┬──────┘
               │
    ┌──────────┴──────────┐
    │   Lab Network       │
    │   192.168.1.0/24    │
    │   (ISOLADA)         │
    └─────────────────────┘
```

**Regras de Firewall:**

```
# pfSense Rules

# 1. DENY acesso do lab à rede corporativa
Block: Source LAB_NET → Dest CORP_NET

# 2. ALLOW saída controlada para internet
Allow: Source LAB_NET → Dest ANY (ports 80,443,53)

# 3. DENY todo resto
Block: Source LAB_NET → Dest ANY
```

### Snapshots Obrigatórios

**SEMPRE tire snapshot antes de:**
- Executar qualquer malware
- Fazer mudanças importantes
- Testar ferramentas desconhecidas

```bash
# VirtualBox
VBoxManage snapshot "VM-Name" take "SNAPSHOT-NAME"

# VMware
# Via GUI: VM → Snapshot → Take Snapshot
```

**Convenção de nomes:**
```
CLEAN-STATE-2025-12-29
BEFORE-MALWARE-TEST-2025-12-29
WORKING-CONFIG-2025-12-29
```

---

## 🎣 Manuseio de Phishing Samples

### Coleta Segura

**Ao coletar samples:**

```markdown
✅ FAÇA:
- Use VM isolada
- Navegador em modo privado
- VPN ou Tor (opcional)
- Defang imediatamente após coletar
- Salve em formato .eml ou .txt

❌ NÃO FAÇA:
- Usar seu email pessoal/trabalho
- Clicar em links
- Baixar anexos
- Responder a emails
- Compartilhar sem defang
```

### Armazenamento Seguro

```bash
# Estrutura de diretórios
phishing-samples/
├── emails/                    # .eml files
│   ├── YYYYMMDD-sample-001.eml
│   └── README.md             # Avisos de segurança
├── urls/                      # URLs defanged
│   └── malicious-urls.txt
└── analysis/                  # Análises
    └── reports/

# Permissões restritas
chmod 700 phishing-samples/
chmod 600 phishing-samples/emails/*
```

**Arquivo README.md em /emails/:**
```markdown
⚠️ WARNING: This directory contains REAL phishing samples
- DO NOT click on any links
- DO NOT open attachments
- Use isolated environment only
- All URLs are defanged for safety
```

### Análise Segura

**Ferramentas Online (Usar com Cuidado):**

```
✅ Seguro:
- VirusTotal (submissão de hash apenas)
- URLScan.io (modo público)
- ANY.RUN (sandboxed)

⚠️ Cuidado:
- VirusTotal (submissão de arquivo completo - pode vazar)
- Hybrid Analysis (mesma coisa)

❌ Nunca:
- Executar localmente fora de sandbox
- Upload de emails com informações internas
```

---

## 🔑 Gestão de Credenciais

### Senhas de Lab

**Use senhas fortes mas não reutilize:**

```bash
# ✅ CORRETO
Wazuh Lab: 6vQ#mP9$kL2@wZ8n (única, forte, só para lab)
Windows Lab: tY4&nB7^jK1!qW5x (única, forte, só para lab)

# ❌ ERRADO
Qualquer sistema: SenhaFacil123 (fraca)
Múltiplos sistemas: MinhaSenh@Real (reutilizada)
```

**Geradores recomendados:**
- `pwgen -s 16` (Linux)
- `openssl rand -base64 16`
- [1Password Generator](https://1password.com/password-generator/)

### Credenciais em Código

**NUNCA commite credenciais:**

```python
# ❌ ERRADO
api_key = "sk-1234567890abcdef"
password = "minha_senha_secreta"

# ✅ CORRETO
import os
api_key = os.environ.get('API_KEY')
password = os.environ.get('PASSWORD')

# Ou use python-dotenv
from dotenv import load_dotenv
load_dotenv()
api_key = os.getenv('API_KEY')
```

**Arquivo .env (gitignored):**
```bash
# .env
API_KEY=sk-1234567890abcdef
PASSWORD=minha_senha_secreta
```

---

## 🗑️ Descarte Seguro

### Ao Deletar VMs

```bash
# 1. Deletar snapshots primeiro
VBoxManage snapshot "VM-Name" delete "SNAPSHOT-NAME"

# 2. Deletar VM e arquivos
VBoxManage unregistervm "VM-Name" --delete

# 3. Verificar se arquivos foram removidos
ls -la ~/VirtualBox\ VMs/

# 4. Secure delete (opcional, para dados muito sensíveis)
shred -vfz -n 3 ~/VirtualBox\ VMs/old-vm/*
```

### Ao Deletar Samples

```bash
# Não use apenas 'rm'
# Use shred (Linux) ou equivalente

# Linux
shred -vfz -n 3 phishing-sample.eml

# macOS
srm -m phishing-sample.eml

# Ou simplesmente garantir que não vai para lixeira recuperável
rm -P phishing-sample.eml  # macOS
```

---

## 📋 Checklist de Segurança

### Antes de Começar Qualquer Exercício

```
[ ] VM está isolada da rede corporativa?
[ ] Snapshot "clean state" foi criado?
[ ] Antivírus está ativo (mas não vai interferir no teste)?
[ ] Dados reais NÃO estão na VM?
[ ] Tenho backup de tudo importante?
[ ] Sei como reverter se algo der errado?
[ ] Notifiquei outras pessoas (se rede compartilhada)?
```

### Ao Trabalhar com Malware/Phishing

```
[ ] Estou em VM isolada?
[ ] URLs estão defanged na documentação?
[ ] NÃO vou clicar em links reais?
[ ] NÃO vou executar anexos?
[ ] Tenho ferramenta de análise (ANY.RUN, VirusTotal)?
[ ] Documentando tudo que faço?
```

### Antes de Commitar Código

```
[ ] Sem credenciais hardcoded?
[ ] Sem IPs/domínios reais de produção?
[ ] Sem dados pessoais/sensíveis?
[ ] .gitignore atualizado?
[ ] Revisei o diff?
```

---

## 🚨 Resposta a Incidentes

### Se Você Clicou em Link de Phishing Real

**AÇÃO IMEDIATA (0-5 min):**

```
1. DESCONECTE da rede (WiFi OFF, cabo desplugado)
2. NÃO desligue o computador ainda
3. Tire screenshot do que aconteceu
4. Anote horário exato
```

**CURTO PRAZO (5-30 min):**

```
5. De OUTRO dispositivo:
   - Troque TODAS senhas
   - Habilite MFA onde possível
   - Verifique logins recentes

6. Reporte:
   - Ao time de segurança (se trabalho)
   - Ao banco (se forneceu dados financeiros)
```

**MÉDIO PRAZO (30min-24h):**

```
7. Scan completo de antivírus/antimalware
8. Verificar processos suspeitos
9. Checar tarefas agendadas
10. Monitorar contas por 30 dias
```

### Se Executou Malware Acidentalmente

```
1. DESLIGAR IMEDIATAMENTE
   - Manter botão power por 5s

2. ISOLAR
   - Não ligar conectado à rede
   - Se precisar ligar, sem rede

3. RESTAURAR
   - Reverter para snapshot
   - Ou reinstalar SO

4. DOCUMENTAR
   - O que executou
   - Quando
   - Sintomas observados

5. APRENDER
   - Como aconteceu
   - Como prevenir
```

---

## 📖 Práticas Recomendadas por Área

### Para Análise de Logs

```python
# ✅ CORRETO - Não loga dados sensíveis
logger.info(f"Processing event type: {event_type}")
logger.info(f"Event count: {count}")

# ❌ ERRADO - Loga dados que podem ser sensíveis
logger.info(f"User password: {password}")
logger.info(f"Credit card: {cc_number}")
logger.info(f"Full event: {event}")  # pode conter PII
```

### Para Desenvolvimento de Scripts

```python
# ✅ CORRETO - Valida input
import re

def process_url(url):
    # Valida formato
    if not re.match(r'^https?://', url):
        raise ValueError("Invalid URL format")
    
    # Limita tamanho
    if len(url) > 2000:
        raise ValueError("URL too long")
    
    # Processa com segurança
    # ...

# ❌ ERRADO - Aceita qualquer input
def process_url(url):
    os.system(f"curl {url}")  # Command injection!
```

### Para Documentação

```markdown
✅ CORRETO:
"Detectamos ataque vindo do IP 10[.]0[.]0[.]100"
"Domínio malicioso: hxxp://phishing[.]site"

❌ ERRADO:
"Detectamos ataque vindo do IP 10.0.0.100" (clickable)
"Domínio malicioso: http://phishing.site" (clickable)
```

---

## 🎓 Treinamento de Segurança

### Para Iniciantes

**Antes de usar este lab, certifique-se que entende:**
- [ ] O que é malware e como se propaga
- [ ] O que é phishing e como funciona
- [ ] Conceitos básicos de rede (IP, porta, DNS)
- [ ] Como usar máquinas virtuais
- [ ] Conceitos de isolamento

**Recursos recomendados:**
- [Cybrary - Introduction to IT & Cybersecurity](https://www.cybrary.it)
- [TryHackMe - Pre Security Path](https://tryhackme.com)
- [Professor Messer - Security+ (grátis)](https://professormesser.com)

### Exercícios de Segurança

**Exercício 1: Identificar Vulnerabilidades**
```
Tempo: 30 min
Objetivo: Revisar código e encontrar problemas de segurança

Exemplo:
def execute_command(user_input):
    os.system(user_input)  # Qual o problema?
```

**Exercício 2: Secure Configuration**
```
Tempo: 45 min
Objetivo: Configurar VM com melhores práticas de segurança

Checklist:
- Firewall configurado
- Apenas portas necessárias abertas
- Senhas fortes
- SSH com key apenas
- Updates aplicados
```

---

## 📞 Reporte de Vulnerabilidades

### Encontrou Problema de Segurança?

**NÃO faça:**
- ❌ Abrir issue pública imediatamente
- ❌ Postar detalhes em redes sociais
- ❌ Explorar a vulnerabilidade

**FAÇA:**
1. ✅ Email privado para: security@projeto.com
2. ✅ Descreva o problema detalhadamente
3. ✅ Dê tempo para correção (90 dias padrão)
4. ✅ Aguarde resposta antes de disclosure

**Template de Reporte:**
```markdown
Subject: [SECURITY] Vulnerability in <component>

Severity: Critical/High/Medium/Low
Component: <nome do componente>
Version: <versão afetada>

Description:
[Descrição clara do problema]

Steps to Reproduce:
1. ...
2. ...
3. ...

Impact:
[O que um atacante pode fazer]

Proposed Fix:
[Se tiver sugestão]

Contact:
[Seu email para follow-up]
```

---

## 📚 Recursos Adicionais

### Documentação de Referência
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [SANS Security Awareness](https://www.sans.org/security-awareness-training/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

### Ferramentas de Segurança
- [ClamAV](https://www.clamav.net/) - Antivírus open source
- [Lynis](https://cisofy.com/lynis/) - Security auditing
- [OSSEC](https://www.ossec.net/) - HIDS (Wazuh é fork dele)

### Comunidades
- [r/cybersecurity](https://reddit.com/r/cybersecurity)
- [Information Security Stack Exchange](https://security.stackexchange.com/)
- [SANS Reading Room](https://www.sans.org/reading-room/)

---

## ⚖️ Responsabilidade Legal

### Uso Ético Obrigatório

**Você concorda que irá:**
- ✅ Usar apenas para fins educacionais e de pesquisa
- ✅ Seguir todas as leis aplicáveis
- ✅ Obter autorização antes de testar em sistemas de terceiros
- ✅ Reportar vulnerabilidades de forma responsável

**Você concorda que NÃO irá:**
- ❌ Usar para ataques reais
- ❌ Distribuir malware
- ❌ Acessar sistemas sem autorização
- ❌ Violar privacidade de outros

### Disclaimer

```
ESTE PROJETO É FORNECIDO "COMO ESTÁ", SEM GARANTIAS.
OS AUTORES NÃO SÃO RESPONSÁVEIS POR:
- Uso indevido das ferramentas
- Danos causados por malware
- Violações de leis
- Perda de dados

VOCÊ É TOTALMENTE RESPONSÁVEL PELO SEU USO.
```

---

## ✅ Checklist Final de Segurança

Antes de considerar seu ambiente "seguro":

```
Isolamento:
[ ] VMs em rede separada
[ ] Firewall configurado
[ ] Snapshots criados

Dados:
[ ] Sem credenciais reais
[ ] Sem dados de produção
[ ] Sem informações sensíveis

Código:
[ ] Sem senhas hardcoded
[ ] Input validado
[ ] .gitignore atualizado

Documentação:
[ ] URLs defanged
[ ] Avisos de segurança presentes
[ ] Práticas seguras documentadas

Backup:
[ ] Snapshots regulares
[ ] Documentos versionados
[ ] Plano de recuperação

Educação:
[ ] Li e entendi este documento
[ ] Conheço os riscos
[ ] Sei como agir em incidente
```

---

**🔐 Segurança é responsabilidade de todos. Pratique com segurança!**

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0  
**Revisor:** Security Team
