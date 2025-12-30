# ❓ FAQ - Perguntas Frequentes

## Índice

- [Geral](#geral)
- [Instalação e Configuração](#instalação-e-configuração)
- [Uso das Ferramentas](#uso-das-ferramentas)
- [Lab e VMs](#lab-e-vms)
- [Análise de Phishing](#análise-de-phishing)
- [Wazuh e SIEM](#wazuh-e-siem)
- [Contribuindo](#contribuindo)
- [Carreira](#carreira)

---

## 🌐 Geral

### P: Para quem é este projeto?

**R:** Este projeto é ideal para:
- Estudantes de Cybersecurity
- Iniciantes em SOC/Blue Team
- Profissionais de TI transicionando para segurança
- Analistas júnior querendo praticar
- Entusiastas de segurança da informação

**Não** requer experiência prévia em segurança, mas conhecimento básico de redes e Linux ajuda.

---

### P: Preciso pagar por algo?

**R:** **NÃO!** Tudo é gratuito:
- ✅ Todas as ferramentas são open source
- ✅ Wazuh é gratuito (versão completa)
- ✅ Datasets públicos (PhishTank, OpenPhish)
- ✅ VMs usam versões trial/evaluation (Windows Server)
- ✅ Documentação e scripts são MIT license

**Custos zero** para aprender e praticar.

---

### P: Quanto tempo leva para completar?

**R:** Depende do seu ritmo:

| Componente | Tempo Mínimo | Tempo Recomendado |
|------------|--------------|-------------------|
| Setup básico (ferramentas) | 30 min | 1-2 horas |
| Lab completo (5 VMs) | 2 horas | 4-6 horas |
| Semana 1 completa | 20 horas | 35 horas |
| Documentação adicional | 5 horas | 10 horas |

**Total recomendado:** 45-50 horas para experiência completa.

---

### P: Posso usar este projeto no meu portfólio?

**R:** **SIM! Por favor, use!** 

Este projeto foi feito para isso. Adicione ao seu:
- GitHub (link direto)
- LinkedIn (seção Projetos)
- Currículo (projetos relevantes)
- Entrevistas (exemplo de trabalho prático)

É um diferencial forte para vagas entry-level em SOC.

---

### P: Preciso saber programar?

**R:** Não obrigatório, mas ajuda:

**Sem programação:** Você pode:
- ✅ Usar as ferramentas prontas
- ✅ Seguir os playbooks
- ✅ Analisar casos de phishing
- ✅ Montar o lab

**Com Python básico:** Você pode:
- ✅ Tudo acima +
- ✅ Modificar os scripts
- ✅ Criar novas funcionalidades
- ✅ Automatizar tarefas

**Recomendação:** Aprenda Python básico em paralelo (Codecademy, freeCodeCamp).

---

## 💻 Instalação e Configuração

### P: Qual sistema operacional devo usar?

**R:** Recomendações:

| SO | Viabilidade | Observações |
|----|-------------|-------------|
| **Linux (Ubuntu)** | ⭐⭐⭐⭐⭐ | Ideal, tudo funciona nativamente |
| **macOS** | ⭐⭐⭐⭐ | Funciona bem, alguns ajustes |
| **Windows** | ⭐⭐⭐ | Usar WSL2 ou Git Bash |

**Melhor opção:** Ubuntu 22.04 LTS ou Kali Linux

---

### P: Meu computador é fraco, consigo rodar o lab?

**R:** Depende:

**Requisitos Mínimos (Lab Básico - 1 VM):**
- CPU: 4 cores
- RAM: 8 GB
- Disco: 100 GB

**Recomendado (Lab Completo - 5 VMs):**
- CPU: 8+ cores
- RAM: 16-32 GB
- Disco: 200-500 GB SSD

**Alternativas se PC é fraco:**
- ✅ Use apenas ferramentas (sem VMs)
- ✅ Use cloud (AWS Free Tier, Azure Students)
- ✅ Monte apenas Wazuh All-in-One (1 VM)

---

### P: Erro "Permission denied" ao executar scripts

**R:** Dê permissão de execução:

```bash
# Linux/Mac
chmod +x tools/url-validator.sh
chmod +x tools/*.sh

# Verificar
ls -la tools/

# Ou execute com bash
bash tools/url-validator.sh urls.txt
```

**No Windows:** Use Git Bash ou WSL2.

---

### P: "Python module not found" mesmo após pip install

**R:** Soluções:

```bash
# 1. Verificar versão Python
python3 --version  # Deve ser 3.8+

# 2. Usar venv (ambiente virtual)
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# 3. Reinstalar dependências
pip3 install -r requirements.txt --upgrade

# 4. Verificar instalação
pip3 list | grep <nome-do-modulo>

# 5. Se ainda falhar, instalar individual
pip3 install beautifulsoup4
```

---

## 🛠️ Uso das Ferramentas

### P: Como analiso um email suspeito que recebi?

**R:** Passo a passo:

```bash
# 1. Salvar o email
# Gmail: Três pontos → Baixar mensagem original
# Outlook: Arquivo → Salvar Como → .eml

# 2. Analisar com nossa ferramenta
cd tools
python3 header-analyzer.py email-suspeito.eml

# 3. Se contiver URLs, extrair
grep -oP 'https?://[^\s"<>]+' email-suspeito.eml > urls.txt

# 4. Validar URLs
bash url-validator.sh urls.txt

# 5. Documentar descobertas
# Adicionar à planilha phishing-samples.xlsx
```

---

### P: O header-analyzer diz que é phishing, mas tenho certeza que é legítimo

**R:** A ferramenta não é 100% precisa. Pode haver falsos positivos:

**Causas comuns:**
- Email de newsletter (pode falhar SPF)
- Servidor mal configurado
- Email forwarded
- Domínio novo mas legítimo

**O que fazer:**
1. ✅ Verificar remetente por outro canal
2. ✅ Ligar para empresa usando número oficial
3. ✅ Acessar site digitando URL manualmente
4. ✅ **NUNCA** clicar em links se houver dúvida

**Lembre-se:** Score alto ≠ 100% de certeza. Use julgamento humano.

---

### P: Posso usar as ferramentas em produção (empresa)?

**R:** Com cuidado:

**✅ Pode usar:**
- Scripts de análise (são seguros)
- Playbooks de resposta
- Checklists
- Templates de documentação

**⚠️ Teste antes:**
- Ferramentas de validação de URL (pode gerar tráfego)
- Automações (validar lógica)
- Integrações com SIEM

**❌ Não use sem aprovação:**
- VMs em rede corporativa
- Simulações de phishing (RH/Legal devem saber)
- Coleta de logs de produção

**Recomendação:** Implemente em ambiente de teste primeiro.

---

## 🖥️ Lab e VMs

### P: Minha VM não consegue acessar a internet

**R:** Checklist de diagnóstico:

```bash
# 1. Verificar gateway
ip route show
# Deve mostrar: default via 192.168.1.1

# 2. Testar conectividade interna
ping 192.168.1.1  # pfSense

# 3. Testar DNS
ping 8.8.8.8      # IP direto
ping google.com   # Resolução DNS

# 4. Verificar configuração de rede da VM
# VirtualBox: Settings → Network → Attached to: NAT ou Bridged
# VMware: Edit → Virtual Network Editor

# 5. Reiniciar serviço de rede
sudo systemctl restart networking  # ou
sudo netplan apply
```

**Causa comum:** VM configurada em "Internal Network" ao invés de "NAT" ou "Bridged".

---

### P: Wazuh agent não conecta no manager

**R:** Troubleshooting passo a passo:

```bash
# NO MANAGER (192.168.1.102)
# 1. Verificar se manager está rodando
sudo systemctl status wazuh-manager

# 2. Verificar firewall
sudo ufw status
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
sudo ufw allow 55000/tcp

# 3. Listar agentes
sudo /var/ossec/bin/agent_control -l

# NO AGENT
# 4. Verificar configuração
# Linux: cat /var/ossec/etc/ossec.conf | grep -A5 "<client>"
# Windows: C:\Program Files (x86)\ossec-agent\ossec.conf

# 5. Verificar conectividade
telnet 192.168.1.102 1514
# Ou
nc -zv 192.168.1.102 1514

# 6. Ver logs do agent
# Linux: tail -f /var/ossec/logs/ossec.log
# Windows: C:\Program Files (x86)\ossec-agent\ossec.log

# 7. Reiniciar agent
# Linux: sudo systemctl restart wazuh-agent
# Windows: Restart-Service WazuhSvc
```

---

### P: Posso usar Docker ao invés de VMs?

**R:** SIM! Especialmente para Wazuh:

```bash
# Wazuh All-in-One com Docker
docker-compose up -d

# docker-compose.yml disponível em:
# https://documentation.wazuh.com/current/deployment-options/docker/docker-installation.html
```

**Vantagens:**
- ✅ Mais leve que VMs
- ✅ Setup mais rápido
- ✅ Fácil limpar/recriar

**Desvantagens:**
- ❌ Menos "realista" que VMs completas
- ❌ Algumas funcionalidades limitadas

**Recomendação:** Docker para aprender rápido, VMs para experiência completa.

---

## 🎣 Análise de Phishing

### P: Onde encontro mais casos de phishing para analisar?

**R:** Fontes públicas e seguras:

**Sites Oficiais:**
- [PhishTank](https://phishtank.org) - Verificado pela comunidade
- [OpenPhish](https://openphish.com) - Feed automatizado
- [URLhaus](https://urlhaus.abuse.ch) - URLs maliciosas
- [CERT.br](https://cert.br) - Incidentes no Brasil

**Comunidades:**
- Reddit: r/Scams, r/phishing
- Twitter: #phishing, #infosec
- Discord/Slack de segurança

**⚠️ IMPORTANTE:**
- **NUNCA** clique em links
- Use ambiente isolado/sandbox
- Links devem estar "defanged" (hxxp://)

---

### P: Como sei se um domínio é typosquatting de um legítimo?

**R:** Técnicas comuns:

**Substituições:**
- `paypal.com` → `paypa1.com` (l → 1)
- `google.com` → `g00gle.com` (o → 0)
- `microsoft.com` → `micros0ft.com`

**Adições:**
- `facebook.com` → `facebook-login.com`
- `amazon.com` → `amazon-br.com`

**Homógrafos (caracteres visuais similares):**
- `apple.com` → `аpple.com` (а cirílico)
- `paypal.com` → `раypal.com` (р cirílico)

**Ferramentas de detecção:**
```bash
# dnstwist - gera variações possíveis
dnstwist --registered paypal.com

# urlcrazy
urlcrazy paypal.com
```

---

### P: Recebi um phishing real, o que faço?

**R:** Protocolo de resposta:

**1. NÃO INTERAJA:**
- ❌ Não clique em links
- ❌ Não baixe anexos
- ❌ Não responda

**2. PRESERVE:**
- ✅ Salve o email como .eml
- ✅ Tire screenshot
- ✅ Não delete ainda (evidência)

**3. REPORTE:**
- ✅ Ao TI/Segurança da empresa
- ✅ Botão "Reportar Phishing" do Gmail/Outlook
- ✅ PhishTank: https://phishtank.org/add_web_phish.php
- ✅ Google: https://safebrowsing.google.com/safebrowsing/report_phish/

**4. BLOQUEIE:**
- ✅ Marque como spam
- ✅ Bloqueie remetente
- ✅ Delete após reportar

**5. EDUQUE:**
- ✅ Compartilhe com colegas (como exemplo)
- ✅ Adicione ao dataset do projeto
- ✅ Use em treinamentos

---

## 📊 Wazuh e SIEM

### P: Wazuh vs Splunk - qual usar?

**R:** Comparação:

| Aspecto | Wazuh | Splunk |
|---------|-------|--------|
| **Custo** | 🟢 Grátis | 🟡 Pago (trial 60 dias) |
| **Complexidade** | 🟡 Média | 🔴 Alta |
| **Docs** | 🟢 Excelente | 🟢 Excelente |
| **Comunidade** | 🟢 Ativa | 🟢 Muito ativa |
| **Carreira** | 🟡 Crescendo | 🟢 Muito usado |

**Recomendação:**
- **Aprendendo:** Wazuh (grátis, open source)
- **Mercado:** Aprenda ambos se possível
- **Projeto:** Use Wazuh como principal, Splunk opcional

---

### P: Como adiciono uma regra customizada no Wazuh?

**R:** Passo a passo:

```bash
# 1. Editar arquivo de regras locais
sudo nano /var/ossec/etc/rules/local_rules.xml

# 2. Adicionar regra (exemplo)
<group name="local,">
  <rule id="100200" level="10">
    <if_group>web</if_group>
    <url>malicious-domain.com</url>
    <description>Access to known malicious domain</description>
  </rule>
</group>

# 3. Testar regra
sudo /var/ossec/bin/wazuh-logtest
# Cole um log de teste e veja se a regra triggera

# 4. Reiniciar Wazuh
sudo systemctl restart wazuh-manager

# 5. Verificar logs
tail -f /var/ossec/logs/ossec.log
```

**Regras prontas:** Ver `lab-config/wazuh-config/local_rules.xml`

---

### P: Quantos logs o Wazuh consegue processar?

**R:** Depende do hardware:

| Hardware | EPS* | Agentes |
|----------|------|---------|
| 2 CPU, 4GB RAM | ~1.000 | 50-100 |
| 4 CPU, 8GB RAM | ~5.000 | 200-500 |
| 8 CPU, 16GB RAM | ~15.000 | 500-1.000 |

*EPS = Events Per Second

**Para este lab (aprendizado):**
- Setup mínimo processa ~100-500 EPS
- Suficiente para 5-10 VMs
- Não é produção, é prática!

---

## 🤝 Contribuindo

### P: Como posso contribuir sem saber programar?

**R:** Várias formas!

**Documentação:**
- ✅ Corrigir erros de português
- ✅ Clarificar seções confusas
- ✅ Adicionar exemplos
- ✅ Criar tutoriais

**Dados:**
- ✅ Adicionar novos casos de phishing
- ✅ Reportar domínios maliciosos
- ✅ Compartilhar IOCs

**Testes:**
- ✅ Testar scripts e reportar bugs
- ✅ Validar playbooks
- ✅ Verificar links quebrados

**Divulgação:**
- ✅ Compartilhar no LinkedIn
- ✅ Escrever blog post
- ✅ Apresentar em meetups

---

### P: Minha Pull Request foi rejeitada, e agora?

**R:** Não desanime!

**Possíveis motivos:**
- Código não segue padrões do projeto
- Falta documentação
- Testes não passam
- Conflito com main branch

**O que fazer:**
1. Ler feedback do reviewer
2. Fazer ajustes solicitados
3. Atualizar PR
4. Se não concordar, discutir educadamente

**Lembre-se:** Rejeição não é pessoal. É sobre manter qualidade do projeto.

---

## 💼 Carreira

### P: Este projeto me qualifica para qual nível de vaga?

**R:** Principalmente **entry-level** e **júnior**:

**Vagas onde ajuda:**
- SOC Analyst (Tier 1)
- Security Analyst Junior
- Cyber Defense Analyst
- Incident Response Analyst (Junior)
- Security Operations Junior
- Blue Team Analyst

**Combinado com:**
- Certificação (Security+, CySA+)
- Outros projetos
- Networking (LinkedIn, eventos)
= **Alta chance de entrevista**

---

### P: Que certificação devo fazer depois deste projeto?

**R:** Recomendações por ordem:

**Iniciante (escolha 1):**
1. **CompTIA Security+** (mais reconhecida)
2. **Certified Ethical Hacker (CEH)** (mais cara)
3. **BTL1 (Blue Team Level 1)** (focada em SOC)

**Depois (em 1-2 anos):**
4. **CompTIA CySA+** (Cybersecurity Analyst)
5. **GIAC GSEC** (Security Essentials)

**Longo prazo (3-5 anos):**
6. **CISSP** (Certified Information Systems Security Professional)

**Dica:** Security+ é o melhor custo-benefício para começar.

---

### P: Como uso este projeto em entrevistas?

**R:** Prepare-se para falar sobre:

**Técnico:**
- "Explique como configurou o lab"
- "Como você detectaria um brute force?"
- "Qual a diferença entre SPF, DKIM e DMARC?"
- "Conte sobre um caso de phishing que analisou"

**Prepare:**
```
Situação: Recebi email suspeito de phishing
Tarefa: Analisar e determinar se era legítimo
Ação: Usei header analyzer, verifiquei SPF/DKIM/DMARC, 
       validei URLs, consultei VirusTotal
Resultado: Confirmei phishing (score 85/100), reportei 
           para PhishTank e bloqueei domínio no lab
```

**Demonstração:**
- Leve laptop com lab rodando
- Mostre dashboard do Wazuh
- Execute script de análise ao vivo
- Apresente relatório de incidente

**Impacto:** "Desenvolvi X ferramentas, analisei Y casos, documentei Z playbooks"

---

### P: Vale a pena fazer este projeto se quero Red Team?

**R:** **SIM!** Blue Team é base essencial:

**Por quê?**
- Red Team precisa entender defesa
- Melhores Red Teamers foram Blue Team
- Muitas empresas contratam Blue primeiro
- Path típico: Blue → Purple → Red

**Red Team usa este projeto para:**
- Entender como SOC detecta ataques
- Saber quais logs são gerados
- Aprender a evitar detecção
- Praticar OPSEC

**Dica:** Faça este projeto + adicione módulo de "como atacante evitaria detecção".

---

## 🔧 Troubleshooting Geral

### P: Onde encontro ajuda adicional?

**R:** Recursos em ordem de prioridade:

1. **Este FAQ** (você está aqui!)
2. **Documentação do projeto** (`docs/`)
3. **Issues no GitHub** (procure problemas similares)
4. **Discussions no GitHub** (faça perguntas)
5. **Discord/Slack da comunidade** (se houver)
6. **Stack Overflow** (para erros de código)
7. **Documentação oficial** (Wazuh, Python, etc.)

**Ao pedir ajuda, inclua:**
- Sistema operacional
- Versão do Python
- Comando exato executado
- Erro completo (copiar/colar)
- O que já tentou

---

### P: Projeto não está funcionando, posso desistir?

**R:** **NÃO DESISTA!** 

Erros são parte do aprendizado em cybersecurity.

**Dicas:**
1. 🔍 Leia mensagem de erro com atenção
2. 🔎 Google o erro exato (entre aspas)
3. 📚 Consulte documentação
4. ⏸️ Faça pausa (às vezes ajuda!)
5. 💬 Peça ajuda na comunidade
6. 🎯 Tente abordagem diferente

**Lembre-se:** 
- Todo profissional de segurança já travou em algum erro
- Resolver problemas é a essência do trabalho
- Cada erro resolvido = habilidade adquirida

**"Não funciona" é início da jornada, não o fim!**

---

## 📞 Ainda tem dúvidas?

**Não encontrou sua pergunta aqui?**

1. 🔍 Pesquise nas [Issues do GitHub](https://github.com/seu-usuario/phishing-analysis-lab/issues)
2. 💬 Abra uma [Discussion](https://github.com/seu-usuario/phishing-analysis-lab/discussions)
3. 📧 Email: seu@email.com

**Contribua:** Se sua dúvida não está aqui, abra PR adicionando ao FAQ!

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0

*"A única pergunta boba é a que não foi feita."*
