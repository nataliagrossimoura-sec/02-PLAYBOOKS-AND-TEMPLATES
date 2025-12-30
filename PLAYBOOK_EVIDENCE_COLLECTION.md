# 🔬 GUIA: Coleta de Evidências Digitais

## 📋 Visão Geral

Este guia estabelece procedimentos para coleta, preservação e documentação de evidências digitais para investigações de incidentes de segurança.

---

## ⚖️ Princípios Fundamentais

### 1. Chain of Custody (Cadeia de Custódia)

**Definição:** Documentação cronológica de quem teve acesso à evidência, quando e por quê.

**Por que é crítico:**
- Validade legal da evidência
- Prevenção de contaminação
- Rastreabilidade completa
- Admissibilidade em tribunal

### 2. Integridade

**Garantir que evidência não foi alterada:**
- Hashes criptográficos (SHA-256, MD5)
- Write-blockers para discos
- Snapshots/backups antes de análise
- Documentação de todas alterações

### 3. Preservação

**Ordem de Volatilidade (coletar nessa ordem):**
```
1. Registros de CPU, cache           (mais volátil)
2. Memória RAM
3. Estado de rede (conexões ativas)
4. Processos em execução
5. Disco (arquivos)
6. Logs remotos
7. Arquivos de configuração
8. Topologia de rede
9. Documentação física                (menos volátil)
```

---

## 🎯 FASE 1: PREPARAÇÃO

### Kit de Resposta a Incidentes

**Hardware:**
```
[ ] Laptop forense (dedicado)
[ ] Discos externos (mínimo 2TB)
[ ] Write-blocker hardware
[ ] Cabos (USB, SATA, IDE)
[ ] Adaptadores (USB-to-SATA, etc)
[ ] Câmera fotográfica
[ ] Mídia virgem (USBs, DVDs)
[ ] Etiquetas e sacos antiestáticos
[ ] Caderno de anotações
```

**Software:**
```
[ ] FTK Imager / dd / dc3dd
[ ] Volatility (memory forensics)
[ ] Wireshark / tcpdump
[ ] Autopsy / Sleuth Kit
[ ] Hashing tools (sha256sum, md5sum)
[ ] Live forensic distro (DEFT, CAINE)
```

### Documentação

**Templates Necessários:**
```
[ ] Chain of Custody Form
[ ] Evidence Log
[ ] Scene Documentation Form
[ ] Photography Log
[ ] Interview Notes Template
```

---

## 🔍 FASE 2: IDENTIFICAÇÃO

### Avaliação Inicial da Cena

**Antes de Tocar em Qualquer Sistema:**
```
[ ] Fotografar estado atual (tela, conexões, LEDs)
[ ] Anotar data/hora atual do sistema
[ ] Identificar sistemas envolvidos
[ ] Listar pessoas presentes
[ ] Determinar criticidade (pode desligar?)
[ ] Verificar se há destruição ativa de evidências
```

### Decisão Crítica: Live vs Dead Forensics

**Live Forensics (Sistema Ligado):**
```
✅ Quando usar:
- Sistema crítico que não pode ser desligado
- Dados voláteis importantes (RAM, conexões ativas)
- Malware ativo que pode detectar shutdown e auto-destruir

❌ Riscos:
- Pode alterar evidências
- Malware pode detectar análise
- Mais difícil manter integridade
```

**Dead Forensics (Sistema Desligado):**
```
✅ Quando usar:
- Sistema não-crítico
- Dados persistentes são suficientes
- Menor risco de alteração

❌ Riscos:
- Perda de dados voláteis
- Malware pode ter anti-forensics no shutdown
```

---

## 📍 FASE 3: COLETA

### A) Dados Voláteis (Live System)

**1. Memória RAM**

**Linux:**
```bash
# Usando LiME (Linux Memory Extractor)
sudo insmod lime.ko "path=/evidence/memory-$(hostname)-$(date +%Y%m%d-%H%M).lime format=lime"

# Ou dd (menos confiável)
sudo dd if=/dev/mem of=/evidence/memory.img bs=1M

# Hash imediatamente
sha256sum /evidence/memory.img > /evidence/memory.img.sha256
```

**Windows:**
```powershell
# DumpIt (ferramenta gratuita)
DumpIt.exe /OUTPUT E:\evidence\memory-$(hostname).dmp /TYPE full

# Ou Magnet RAM Capture
RAMCapture.exe E:\evidence\memory.raw

# Hash
certutil -hashfile E:\evidence\memory.raw SHA256 > E:\evidence\memory.raw.sha256
```

**2. Processos em Execução**

```bash
# Linux
ps auxf > /evidence/processes-$(date +%Y%m%d-%H%M).txt
top -b -n 1 > /evidence/top.txt
lsof > /evidence/open-files.txt

# Windows
tasklist /v > E:\evidence\processes.txt
wmic process list full > E:\evidence\processes-full.txt
Get-Process | Export-Csv E:\evidence\processes.csv
```

**3. Conexões de Rede**

```bash
# Linux
netstat -antp > /evidence/netstat.txt
ss -tunap > /evidence/ss.txt
iptables -L -n -v > /evidence/iptables.txt

# Windows
netstat -ano > E:\evidence\netstat.txt
Get-NetTCPConnection | Export-Csv E:\evidence\connections.csv
ipconfig /all > E:\evidence\ipconfig.txt
```

**4. Usuários Logados**

```bash
# Linux
w > /evidence/users-logged.txt
last -f /var/log/wtmp > /evidence/last-logins.txt
lastlog > /evidence/lastlog.txt

# Windows
query user > E:\evidence\logged-users.txt
Get-WmiObject Win32_LoggedOnUser | Export-Csv E:\evidence\loggedon.csv
```

**5. Captura de Tráfego**

```bash
# Iniciar captura (deixar rodando durante investigação)
tcpdump -i any -w /evidence/traffic-$(date +%Y%m%d-%H%M).pcap &

# Anotar PID para matar depois
TCPDUMP_PID=$!

# Quando terminar (após 30-60 min)
kill $TCPDUMP_PID
```

### B) Dados Persistentes

**1. Disco Completo (Imagem Forense)**

**Linux:**
```bash
# Usando dd (clássico)
dd if=/dev/sda of=/mnt/evidence/disk-image.dd bs=4M status=progress

# Usando dc3dd (melhor, com hash integrado)
dc3dd if=/dev/sda of=/mnt/evidence/disk-image.dd hash=md5 hash=sha256 log=/mnt/evidence/imaging.log

# Usando ddrescue (se disco com bad sectors)
ddrescue /dev/sda /mnt/evidence/disk-image.dd /mnt/evidence/rescue.log

# Comprimir (opcional, economiza espaço)
gzip /mnt/evidence/disk-image.dd

# Hash final
sha256sum /mnt/evidence/disk-image.dd.gz > /mnt/evidence/disk-image.dd.gz.sha256
```

**Windows:**
```powershell
# FTK Imager (GUI)
# 1. File → Create Disk Image
# 2. Select source (Physical Drive)
# 3. Add destination (E01 ou DD format)
# 4. Verify image after creation

# Ou linha de comando (dd for Windows)
dd.exe if=\\.\PhysicalDrive0 of=E:\evidence\disk-image.dd bs=4M --progress

# Hash
certutil -hashfile E:\evidence\disk-image.dd SHA256
```

**2. Arquivos Específicos**

```bash
# Copiar arquivos suspeitos preservando metadata
rsync -avz --progress /path/to/suspicious-file /evidence/files/

# Ou com timestamps preservados
cp -p /path/to/file /evidence/

# Hash de cada arquivo
find /evidence/files/ -type f -exec sha256sum {} \; > /evidence/file-hashes.txt
```

**3. Logs do Sistema**

```bash
# Linux - coletar todos logs relevantes
mkdir -p /evidence/logs
cp -r /var/log/* /evidence/logs/

# Logs específicos importantes
cp /var/log/auth.log* /evidence/logs/        # Autenticação
cp /var/log/syslog* /evidence/logs/          # Sistema geral
cp /var/log/apache2/* /evidence/logs/        # Web server
cp /var/log/mysql/* /evidence/logs/          # Database

# Windows - exportar Event Logs
wevtutil epl Security E:\evidence\Security.evtx
wevtutil epl System E:\evidence\System.evtx
wevtutil epl Application E:\evidence\Application.evtx
wevtutil epl "Windows PowerShell" E:\evidence\PowerShell.evtx
```

**4. Configurações**

```bash
# Linux
cp -r /etc /evidence/configs/
crontab -l > /evidence/crontabs.txt
systemctl list-units > /evidence/services.txt

# Windows
reg export HKLM E:\evidence\HKLM.reg
reg export HKCU E:\evidence\HKCU.reg
schtasks /query /fo LIST /v > E:\evidence\scheduled-tasks.txt
```

### C) Dispositivos Móveis

**Android (com ADB):**
```bash
# Backup completo
adb backup -all -apk -shared -system -f /evidence/android-backup.ab

# Logs
adb logcat -d > /evidence/logcat.txt

# Arquivos específicos
adb pull /sdcard/Download /evidence/downloads/
```

**iOS:**
```
# Requer ferramentas comerciais:
- Cellebrite UFED
- Oxygen Forensics
- Magnet AXIOM

# Ou iTunes backup (menos completo)
```

---

## 📍 FASE 4: PRESERVAÇÃO

### Chain of Custody Form

**Preencher IMEDIATAMENTE após coleta:**

```
EVIDENCE ID: EV-2025-001-DISK
DATE/TIME COLLECTED: 2025-12-29 14:30 BRT
COLLECTED BY: João Silva
LOCATION: Server Room, Rack 3, Position 5
DESCRIPTION: 1TB HDD from web server WEB-PROD-01
SERIAL NUMBER: WD-ABCD1234567890
CONDITION: Powered on, normal operation
HASH (SHA-256): a1b2c3d4e5f6...

CHAIN OF CUSTODY:
Date/Time     | Person        | Purpose           | Signature
2025-12-29 15:00 | João Silva  | Collection        | [assinatura]
2025-12-29 16:30 | Maria Souza | Transport to lab  | [assinatura]
2025-12-30 09:00 | Pedro Lima  | Forensic analysis | [assinatura]
```

### Armazenamento Seguro

**Físico:**
```
[ ] Sala trancada com acesso restrito
[ ] Armário/cofre dedicado para evidências
[ ] Controle de acesso (log de quem entra/sai)
[ ] Ambiente climatizado (temperatura/umidade controlada)
[ ] Proteção contra campos magnéticos
```

**Digital:**
```
[ ] Armazenamento em disco criptografado
[ ] Backup em 2+ locais separados
[ ] Verificação periódica de hashes
[ ] Acesso apenas por pessoal autorizado
[ ] Log de todos acessos aos arquivos
```

### Etiquetagem

**Padrão de Nomenclatura:**
```
EV-[ANO]-[NUM]-[TIPO]-[HOST]-[DATA]

Exemplos:
EV-2025-001-DISK-WEB01-20251229
EV-2025-002-MEM-DB01-20251229
EV-2025-003-PCAP-FW01-20251229
```

---

## 📍 FASE 5: ANÁLISE

### Ambiente de Análise

**Setup Seguro:**
```
[ ] Rede isolada (sem acesso à internet)
[ ] VMs dedicadas para análise
[ ] Snapshots antes de qualquer mudança
[ ] Logging de todas ações realizadas
[ ] Write-blocker para acessar evidências originais
```

### Ferramentas de Análise

**Disco:**
```bash
# Montar read-only
mount -o ro,loop /evidence/disk-image.dd /mnt/forensics

# Autopsy (GUI)
autopsy

# Sleuth Kit (CLI)
fls -r /evidence/disk-image.dd > /analysis/file-list.txt
icat /evidence/disk-image.dd <inode> > /analysis/extracted-file

# Buscar por strings
strings /evidence/disk-image.dd | grep -i "password\|secret" > /analysis/strings.txt
```

**Memória:**
```bash
# Volatility
volatility -f /evidence/memory.img imageinfo
volatility -f /evidence/memory.img --profile=Win10x64_19041 pslist
volatility -f /evidence/memory.img --profile=Win10x64_19041 netscan
volatility -f /evidence/memory.img --profile=Win10x64_19041 cmdline
```

**Rede:**
```bash
# Wireshark analysis
wireshark /evidence/traffic.pcap

# tshark (CLI)
tshark -r /evidence/traffic.pcap -Y "http.request.method == POST"
tshark -r /evidence/traffic.pcap -z io,phs  # Protocol hierarchy
```

### Documentar Descobertas

```
FINDING ID: F-001
EVIDENCE ID: EV-2025-001-DISK
DATE: 2025-12-29
ANALYST: João Silva

DESCRIPTION:
Malicious executable found in /tmp/backdoor.exe

LOCATION:
Inode: 12345
Path: /tmp/backdoor.exe
Hash: a1b2c3d4e5f6...

ANALYSIS:
VirusTotal: 45/70 detections
Family: Mirai botnet variant
Capabilities: DDoS, credential theft

TIMELINE:
Created: 2025-12-28 03:15:42
Modified: 2025-12-28 03:15:42
Accessed: 2025-12-29 10:30:15
```

---

## 📍 FASE 6: APRESENTAÇÃO

### Relatório Forense

**Estrutura:**
```markdown
1. SUMÁRIO EXECUTIVO
   - O que aconteceu (1 parágrafo)
   - Quando (timeline)
   - Impacto

2. ESCOPO DA INVESTIGAÇÃO
   - Sistemas analisados
   - Período coberto
   - Limitações

3. METODOLOGIA
   - Ferramentas usadas
   - Procedimentos seguidos
   - Chain of custody

4. DESCOBERTAS
   - Evidências encontradas
   - Análise técnica
   - IOCs extraídos

5. TIMELINE DETALHADA
   - Cronologia de eventos
   - Diagrama visual

6. CONCLUSÕES
   - Root cause
   - Como atacante ganhou acesso
   - O que foi comprometido

7. RECOMENDAÇÕES
   - Curto prazo
   - Médio prazo
   - Longo prazo

8. ANEXOS
   - Logs relevantes
   - Screenshots
   - Hashes de arquivos
   - Chain of custody forms
```

---

## ✅ Checklist de Qualidade

### Antes de Finalizar

```
COMPLETUDE:
[ ] Todas evidências coletadas estão documentadas
[ ] Chain of custody completa para cada item
[ ] Hashes calculados e verificados
[ ] Análise de todas evidências relevantes
[ ] Timeline completa construída

INTEGRIDADE:
[ ] Evidências originais não foram alteradas
[ ] Hashes conferem com originais
[ ] Write-blockers usados quando apropriado
[ ] Duplicatas criadas para análise

DOCUMENTAÇÃO:
[ ] Relatório forense completo
[ ] Fotos/screenshots anexados
[ ] Notas de campo organizadas
[ ] Ferramentas e versões documentadas
[ ] Assinaturas em chain of custody

LEGAL:
[ ] Procedimentos seguiram padrões forenses
[ ] Evidências admissíveis em tribunal
[ ] Privacy/LGPD considerados
[ ] Aprovação de legal (se aplicável)
```

---

## 📚 Referências e Padrões

- **NIST SP 800-86** - Guide to Integrating Forensic Techniques into Incident Response
- **RFC 3227** - Guidelines for Evidence Collection and Archiving
- **ISO/IEC 27037** - Guidelines for identification, collection, acquisition and preservation of digital evidence
- **ACPO Principles** (UK) - Good Practice Guide for Digital Evidence

---

## 🚫 Erros Comuns a Evitar

```
❌ NÃO analisar evidências no sistema original
❌ NÃO usar ferramentas do próprio sistema comprometido
❌ NÃO confiar em data/hora do sistema (pode estar alterado)
❌ NÃO ignorar dados voláteis
❌ NÃO pular documentação (sempre documente TUDO)
❌ NÃO quebrar chain of custody
❌ NÃO coletar sem autorização apropriada
❌ NÃO esquecer de considerar aspectos legais/privacidade
```

---

## 📞 Contatos

**Em Caso de Dúvida:**
- Lead Forensics: [contato]
- Legal: [contato]
- HR (se funcionário envolvido): [contato]

**Serviços Especializados:**
- Forensics externo: [empresa]
- Law enforcement: 197 (Brasil)

---

**🔬 Lembre-se:** Evidências mal coletadas/preservadas podem ser inadmissíveis. Quando em dúvida, consulte especialista forense.

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0  
**Aprovado por:** CISO + Legal
