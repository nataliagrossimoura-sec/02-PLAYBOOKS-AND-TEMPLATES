# 02-PLAYBOOKS-AND-TEMPLATES 🛡️

> Playbooks, checklists, templates e guias completos para resposta a incidentes de segurança e análise de phishing

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Last Updated](https://img.shields.io/badge/Last%20Updated-December%202025-blue)]()
[![Version](https://img.shields.io/badge/Version-1.0-green)]()

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Documentos Principais](#documentos-principais)
- [Playbooks de Resposta](#playbooks-de-resposta)
- [Templates](#templates)
- [Checklists](#checklists)
- [Como Usar](#como-usar)
- [Contribuindo](#contribuindo)
- [Licença](#licença)

## 🎯 Visão Geral

Este repositório contém uma coleção abrangente de **playbooks operacionais**, **templates de documentação**, **checklists de resposta** e **guias de melhores práticas** para equipes de Segurança da Informação, especificamente focado em:

- 🎣 **Análise e resposta a phishing**
- 🦠 **Contenção e remediação de malware**
- 🔐 **Resposta a brute force e ataques de credenciais**
- 💾 **Investigação de exfiltração de dados**
- 🔍 **Coleta e preservação de evidências digitais**
- 🚨 **Gestão de incidentes de segurança**

### Para Quem é Este Projeto?

- ✅ **Analistas SOC** (Níveis 1, 2 e 3)
- ✅ **Incident Responders**
- ✅ **Blue Team / Defensive Security**
- ✅ **Estudantes de Cybersecurity**
- ✅ **Profissionais em transição para Segurança**
- ✅ **Gestores de TI** que precisam estruturar processos

## 📁 Estrutura do Repositório

```
02-PLAYBOOKS-AND-TEMPLATES/
│
├── CHECKLIST_PHISHING.md              # Checklist passo a passo para identificação de phishing
├── FAQ.md                             # Perguntas frequentes sobre uso dos playbooks
├── LICENSE                            # Licença MIT
├── README.md                          # Este arquivo
│
├── PLAYBOOK_BRUTE_FORCE.md           # Resposta a ataques de força bruta
├── PLAYBOOK_DATA_EXFILTRATION.md     # Resposta a exfiltração de dados
├── PLAYBOOK_EVIDENCE_COLLECTION.md   # Coleta forense de evidências digitais
├── PLAYBOOK_INCIDENT_INVESTIGATION.md # Investigação geral de incidentes
├── PLAYBOOK_MALWARE_CONTAINMENT.md   # Contenção e resposta a malware
├── PLAYBOOK_RESPOSTA_PHISHING.md     # Resposta completa a incidentes de phishing
├── PLAYBOOK_SECURITY.md              # Melhores práticas de segurança
│
├── TEMPLATE_COMMUNICATION.md         # Templates de comunicação de incidentes
└── TEMPLATE_INCIDENT_REPORT.md       # Template de relatório de incidente
```

## 📚 Documentos Principais

### 📖 Checklists

#### [CHECKLIST_PHISHING.md](CHECKLIST_PHISHING.md)
**Checklist completo de identificação de phishing em 4 fases:**

- **Fase 1: Verificação do Remetente**
  - Análise de endereço de email
  - Verificação de headers (SPF/DKIM/DMARC)
  - Validação de autenticidade

- **Fase 2: Análise do Conteúdo**
  - Identificação de linguagem urgente/ameaçadora
  - Detecção de erros gramaticais
  - Análise de saudações genéricas
  - Verificação de pedidos de informações sensíveis

- **Fase 3: Verificação de Links**
  - Inspeção visual de URLs
  - Detecção de typosquatting
  - Identificação de encurtadores de URL
  - Análise técnica com ferramentas online

- **Fase 4: Análise de Anexos**
  - Verificação de extensões perigosas
  - Identificação de nomes suspeitos
  - Validação de expectativa de recebimento

**Inclui:**
- ✅ Sistema de pontuação de risco (0-15+ pontos)
- ✅ Matriz de interpretação de severidade
- ✅ Ações recomendadas por nível de risco
- ✅ Procedimentos se você já clicou
- ✅ Contatos úteis (CERT.br, Polícia Federal, etc.)

#### [FAQ.md](FAQ.md)
**Perguntas frequentes sobre:**

- 🌐 **Geral**: Para quem é o projeto, custos, tempo necessário
- 💻 **Instalação**: Requisitos de sistema, troubleshooting comum
- 🛠️ **Uso das Ferramentas**: Como analisar emails suspeitos
- 🖥️ **Lab e VMs**: Configuração de ambiente de testes
- 🎣 **Análise de Phishing**: Onde encontrar samples, como identificar
- 📊 **Wazuh e SIEM**: Configuração e uso
- 💼 **Carreira**: Como usar o projeto em portfólio e entrevistas

## 🚨 Playbooks de Resposta

### [PLAYBOOK_RESPOSTA_PHISHING.md](PLAYBOOK_RESPOSTA_PHISHING.md)
**Playbook completo de resposta a phishing em 7 fases:**

#### Informações do Playbook:
- **Versão:** 1.0 (Dezembro 2025)
- **Classificação:** Interno - Confidencial
- **Revisão:** Trimestral recomendada

#### Estrutura:

**1. Preparação e Contexto**
- Papéis e responsabilidades (com SLAs)
- Matriz de severidade (Crítico/Alto/Médio/Baixo)
- Tempos de resposta esperados

**2. FASE 1: Detecção (0-15 min)**
- Fontes de detecção (usuário, gateway, SIEM)
- Informações a coletar
- Preservação de evidências

**3. FASE 2: Triagem (15 min)**
- Checklist de triagem
- Árvore de decisão
- Ações imediatas se houve clique

**4. FASE 3: Análise (30min - 2h)**
- Análise de headers (SPF/DKIM/DMARC)
- Análise de corpo do email
- Extração e análise de URLs
- Análise de anexos
- Determinação de impacto
- Classificação final

**5. FASE 4: Contenção (1 hora)**
- Bloqueios em email gateway
- Bloqueios em firewall/proxy
- Configuração de EDR/Antivírus
- Gestão de usuários comprometidos
- Template de comunicação interna

**6. FASE 5: Erradicação (2-4 horas)**
- Remoção de emails da campanha
- Reset de senhas
- Verificação de regras de email suspeitas
- Scan de antivírus
- Scripts úteis (PowerShell/Bash)

**7. FASE 6: Recuperação (1-2 dias)**
- Reabilitação de contas
- Validação de funcionalidade
- Monitoramento intensivo

**8. FASE 7: Post-Mortem**
- Template de relatório
- Análise de lições aprendidas
- Implementação de melhorias

**Inclui:**
- 📊 Métricas e KPIs (MTTD, MTTR, Taxa de cliques)
- 🛠️ Ferramentas requeridas (essenciais e recomendadas)
- 📚 Referências (NIST, SANS, MITRE ATT&CK)
- ✅ Checklist de aprovações

### [PLAYBOOK_BRUTE_FORCE.md](PLAYBOOK_BRUTE_FORCE.md)
**Resposta a ataques de força bruta contra sistemas de autenticação**

#### Cobertura:
- SSH (porta 22)
- RDP (porta 3389)
- HTTP/HTTPS (formulários web)
- VPN, FTP, Email

#### Tipos de Ataque:
- Brute Force Tradicional
- Dictionary Attack
- Credential Stuffing
- Password Spraying
- Reverse Brute Force

#### Fases de Resposta:

**Detecção:**
- Indicadores de brute force
- Queries SIEM (Wazuh)
- Regras de alerta customizadas

**Resposta Rápida (0-30 min):**
- Validação do ataque
- Bloqueio de IP
- Rate limiting
- Desabilitação de contas alvo

**Análise (20 min):**
- Coleta de informações sobre tentativas
- Geolocalização de IPs
- Verificação de reputação
- Identificação de sucessos

**Contenção (30-60 min):**
- Proteção de contas comprometidas
- Implementação de controles preventivos
- SSH/RDP hardening
- Rate limiting em aplicações

**Erradicação e Prevenção:**
- Implementação de MFA
- Política de senhas forte
- Account lockout policy
- Monitoramento contínuo com Fail2ban
- Geofencing (opcional)

**Inclui:**
- 📊 Métricas de medição
- 🛠️ Scripts de auto-bloqueio
- 📧 Alertas via webhook (Slack/Teams)
- ✅ Checklist completo de resposta

### [PLAYBOOK_DATA_EXFILTRATION.md](PLAYBOOK_DATA_EXFILTRATION.md)
**Resposta a exfiltração de dados corporativos ou sensíveis**

#### Matriz de Severidade:
- 🔴 **Crítica**: PII (CPF, RG), Dados financeiros, Saúde (HIPAA)
- 🟠 **Alta**: Propriedade intelectual
- 🟡 **Média**: Dados corporativos gerais

#### Obrigações Legais:
- **LGPD** (Brasil): 72 horas para notificação
- **GDPR** (EU): 72 horas para autoridade supervisora
- **PCI-DSS**: Notificação imediata

#### Fases de Resposta:

**FASE 1: Detecção (0-30 min)**
- Anomalias de rede (uploads grandes, IPs externos)
- Anomalias de acesso a dados
- Queries SIEM e DLP alerts

**FASE 2: Resposta Imediata (30-60 min)**
- Contenção de rede (bloqueio de IP/domínio, isolamento)
- Preservação de evidências (PCAP, logs)

**FASE 3: Análise (1-4 horas)**
- Determinação de escopo (quais dados, volume, destino)
- Identificação de método de exfiltração
- Classificação de dados (sensibilidade, quantidade)

**FASE 4: Contenção Completa (4-8 horas)**
- Remoção de acesso do atacante
- Bloqueio de exfiltração futura
- Implementação de DLP
- Segmentação de rede

**FASE 5: Obrigações Legais (Paralelo)**
- Notificação LGPD/GDPR
- Notificação a titulares
- Compliance com PCI-DSS
- Templates de comunicação

**FASE 6: Recuperação (1-7 dias)**
- Melhorias de segurança (curto/médio/longo prazo)
- Remediação para afetados

**FASE 7: Post-Mortem**
- Análise de causa raiz
- Métricas (MTTD, MTTR, volumes, custos)

**Inclui:**
- 🛠️ Ferramentas (Splunk, Varonis, Wireshark, FTK)
- 📊 IOCs para documentar
- 📧 Templates de email para usuários afetados
- ✅ Checklist completo de resposta

### [PLAYBOOK_MALWARE_CONTAINMENT.md](PLAYBOOK_MALWARE_CONTAINMENT.md)
**Contenção e resposta a malware (ransomware, trojans, worms, rootkits)**

#### Tipos de Malware Cobertos:
- 🔴 **Ransomware**: Criptografa arquivos
- 🟠 **Trojan**: Backdoor, controle remoto
- 🟠 **Worm**: Auto-propagação
- 🟡 **Spyware**: Roubo de informações
- 🔴 **Rootkit**: Esconde presença
- 🟡 **Cryptominer**: Minera criptomoeda

#### Matriz de Severidade:
- **P0 - Emergência**: Ransomware ativo, propagação rápida
- **P1 - Crítico**: Servidor comprometido
- **P2 - Alto**: Workstation comprometido
- **P3 - Médio**: Malware detectado e bloqueado

#### Fases de Resposta:

**FASE 1: Detecção (0-5 min)**
- Indicadores de infecção (sintomas comuns)
- Verificação rápida (processos, conexões, arquivos)

**FASE 2: Isolamento Imediato (5-15 min)**
- ⚡ Prioridade: Parar propagação
- Isolamento de rede (físico, firewall, switch)
- Preservação de evidências (memória, processos, conexões)

**FASE 3: Análise (15-60 min)**
- Identificação do malware (hash, VirusTotal)
- Identificação de família (ransom notes, extensões)
- Análise de comportamento (comandos, tarefas, serviços)
- Determinação de vetor de entrada
- Escopo da infecção

**FASE 4: Contenção Completa (1-4 horas)**
- Decisão: Limpar vs Reimagear
- Contenção de dados (se ransomware)
- Contenção de propagação (rede, AD)

**FASE 5: Erradicação (4-24 horas)**
- Remoção completa (opção A: reimagem, opção B: limpeza manual)
- Correção de vulnerabilidades
- Hardening pós-infecção
- Validação de limpeza

**FASE 6: Recuperação (1-7 dias)**
- Restauração de dados (de backups)
- Retorno à produção (faseado)
- Monitoramento intensivo (30 dias)

**FASE 7: Post-Mortem**
- Lições aprendidas
- Melhorias (técnicas, processuais, pessoas)

**Casos Especiais:**
- **Ransomware**: Decisão de pagar (geralmente NÃO), recursos
- **Rootkit**: Reimagem obrigatória
- **Cryptominer**: Identificação e impacto

**Inclui:**
- 📊 IOCs comuns (arquivos, processos, conexões)
- 🛠️ Ferramentas (ClamAV, VirusTotal, Volatility, Malwarebytes)
- 📞 Contatos de emergência
- ✅ Checklist completo

### [PLAYBOOK_INCIDENT_INVESTIGATION.md](PLAYBOOK_INCIDENT_INVESTIGATION.md)
**Metodologia padronizada para investigação geral de incidentes**

#### Quando Usar:
- ✅ Tipo de incidente desconhecido
- ✅ Múltiplos indicadores sem padrão claro
- ✅ Incidente complexo com várias técnicas
- ✅ Necessidade de investigação forense profunda

#### Papéis e Responsabilidades:
- **Incident Commander**: Coordena investigação
- **Lead Investigator**: Análise técnica
- **Forensic Analyst**: Coleta evidências
- **System Admin**: Acesso a sistemas
- **Legal/Compliance**: Aspectos legais

#### Metodologia em 7 Fases:

**1. PREPARAÇÃO (0-15 min)**
- Criar war room
- Iniciar documentação
- Preservar estado inicial

**2. IDENTIFICAÇÃO (15-60 min)**
- Coleta de informações iniciais (5W1H)
- Coleta de evidências (logs, conexões, processos, memória)
- Análise SIEM
- Classificação inicial (MITRE ATT&CK)

**3. CONTENÇÃO INICIAL (1-2 horas)**
- Decisão: Isolar ou Monitorar?
- Contenção de rede (bloqueio, isolamento, segmentação)
- Contenção de contas (desabilitar, revogar sessões)

**4. ANÁLISE PROFUNDA (2-8 horas)**
- Análise de timeline
- Análise forense (disco, memória, rede, malware)
- Identificação de escopo completo

**5. ERRADICAÇÃO (4-24 horas)**
- Remover malware/backdoors
- Patch de vulnerabilidades
- Reconstruir vs Restaurar

**6. RECUPERAÇÃO (1-7 dias)**
- Retorno gradual (faseado)
- Checklist de validação
- Monitoramento pós-incidente (30 dias)

**7. LIÇÕES APRENDIDAS (1-2 semanas)**
- Post-mortem meeting
- Relatório final
- Melhorias a implementar

**Inclui:**
- 📊 Métricas e KPIs (MTTD, MTTR, MTTE)
- 🛠️ Ferramentas essenciais (FTK, Volatility, Wireshark)
- 📚 Referências (NIST, SANS, MITRE)
- ✅ Checklist de resposta rápida

### [PLAYBOOK_EVIDENCE_COLLECTION.md](PLAYBOOK_EVIDENCE_COLLECTION.md)
**Guia completo de coleta e preservação de evidências digitais**

#### Princípios Fundamentais:

**1. Chain of Custody (Cadeia de Custódia)**
- Documentação cronológica completa
- Rastreabilidade de acesso
- Validade legal

**2. Integridade**
- Hashes criptográficos (SHA-256, MD5)
- Write-blockers
- Documentação de alterações

**3. Preservação**
- Ordem de volatilidade (RAM → Disco → Logs)

#### Fases de Coleta:

**FASE 1: PREPARAÇÃO**
- Kit de resposta (hardware e software)
- Templates de documentação

**FASE 2: IDENTIFICAÇÃO**
- Avaliação da cena
- Decisão: Live vs Dead Forensics
- Fotografia do estado atual

**FASE 3: COLETA**

**A) Dados Voláteis (Live System):**
- Memória RAM (LiME, DumpIt)
- Processos em execução
- Conexões de rede
- Usuários logados
- Captura de tráfego (tcpdump)

**B) Dados Persistentes:**
- Disco completo (dd, dc3dd, FTK Imager)
- Arquivos específicos
- Logs do sistema
- Configurações

**C) Dispositivos Móveis:**
- Android (ADB)
- iOS (ferramentas comerciais)

**FASE 4: PRESERVAÇÃO**
- Chain of Custody Form (preenchimento)
- Armazenamento seguro (físico e digital)
- Etiquetagem padronizada

**FASE 5: ANÁLISE**
- Ambiente de análise seguro
- Ferramentas (Autopsy, Volatility, Wireshark)
- Documentação de descobertas

**FASE 6: APRESENTAÇÃO**
- Relatório forense completo
- Checklist de qualidade

**Inclui:**
- 📋 Chain of Custody Forms
- 🔐 Padrões de nomenclatura
- 🛠️ Comandos detalhados (Linux/Windows)
- 📚 Referências (NIST SP 800-86, ISO 27037)
- ❌ Erros comuns a evitar
- ✅ Checklist de qualidade

### [PLAYBOOK_SECURITY.md](PLAYBOOK_SECURITY.md)
**Melhores práticas de segurança ao trabalhar com o projeto**

#### Avisos Críticos:

**🚨 NUNCA FAÇA:**
- ❌ Clicar em links de phishing
- ❌ Executar anexos maliciosos
- ❌ Usar credenciais reais
- ❌ Testar em rede de produção
- ❌ Compartilhar samples sem defang
- ❌ Desabilitar antivírus

#### Princípios Fundamentais:

**1. Isolamento**
- Sempre usar VM isolada
- Rede separada
- Snapshots antes de testes

**2. Defanging**
- URLs: `hxxp://malicious[.]com`
- Emails: `malware[@]evil[.]org`
- IPs: `192[.]168[.]1[.]100`

**3. Least Privilege**
- Usar mínimo de privilégios necessário
- Sudo apenas quando realmente necessário

#### Segurança do Lab:

**Isolamento de Rede:**
- Configuração de pfSense
- Regras de firewall
- Bloqueio de acesso à rede corporativa

**Snapshots Obrigatórios:**
- Antes de executar malware
- Antes de mudanças importantes
- Convenção de nomes

**Manuseio de Phishing Samples:**
- Coleta segura (o que fazer e não fazer)
- Armazenamento seguro (permissões restritas)
- Análise segura (ferramentas online)

**Gestão de Credenciais:**
- Senhas fortes e únicas para lab
- Geradores recomendados
- NUNCA commitar credenciais no código
- Uso de .env e variáveis de ambiente

**Descarte Seguro:**
- Deletar VMs corretamente
- Secure delete de samples (shred, srm)

#### Checklists de Segurança:

**Antes de Começar:**
- VM isolada? Snapshot criado? Antivírus ativo?

**Ao Trabalhar com Malware:**
- Em VM isolada? URLs defanged? Não vai clicar?

**Antes de Commitar:**
- Sem credenciais? Sem IPs reais? .gitignore atualizado?

#### Resposta a Incidentes:

**Se Clicou em Phishing Real:**
- Ação imediata (0-5 min): Desconectar da rede
- Curto prazo (5-30 min): Trocar senhas, habilitar MFA
- Médio prazo (30min-24h): Scan, verificar processos

**Se Executou Malware:**
- Desligar imediatamente
- Isolar
- Restaurar de snapshot
- Documentar

#### Práticas por Área:

**Análise de Logs:**
- O que logar e não logar

**Desenvolvimento de Scripts:**
- Validação de input
- Evitar command injection

**Documentação:**
- Defanging correto

#### Treinamento e Recursos:

**Para Iniciantes:**
- Conceitos que deve entender
- Recursos recomendados (Cybrary, TryHackMe)

**Exercícios de Segurança:**
- Identificar vulnerabilidades
- Secure configuration

**Reporte de Vulnerabilidades:**
- Como reportar responsavelmente
- Template de reporte

**Recursos Adicionais:**
- OWASP Top 10
- SANS Security Awareness
- Ferramentas (ClamAV, Lynis, OSSEC)
- Comunidades (Reddit, Stack Exchange)

#### Responsabilidade Legal:

**Uso Ético Obrigatório:**
- ✅ Apenas fins educacionais
- ✅ Seguir leis aplicáveis
- ✅ Reportar vulnerabilidades responsavelmente

**Disclaimer:**
- Projeto fornecido "como está"
- Autores não responsáveis por uso indevido

**Inclui:**
- ✅ Checklist final de segurança
- 📖 Guias detalhados com comandos
- ⚖️ Aspectos legais e éticos

## 📄 Templates

### [TEMPLATE_COMMUNICATION.md](TEMPLATE_COMMUNICATION.md)
**8 templates de comunicação para diferentes situações**

#### Princípios de Comunicação:
- Transparente mas não alarmista
- Factual e preciso
- Linguagem apropriada ao público

#### Templates Incluídos:

**1. Alerta Interno Inicial**
- Quando: Primeiros 30 min de incidente crítico
- Público: Equipe TI, Segurança, Management

**2. Atualização de Status**
- Frequência: A cada 2-4 horas
- Estrutura: Progresso, situação, próximas ações

**3. Comunicação para Usuários Finais**
- Tom: Calmo, reassegurador
- Conteúdo: O que está acontecendo, o que fazer

**4. Notificação de Violação de Dados (LGPD)**
- Requisito: LGPD Art. 48
- Inclui: Informações afetadas, o que fazer, direitos

**5. Comunicado à Imprensa**
- Aprovar com: Legal, PR, CEO
- Formato: Press release profissional

**6. Comunicação Pós-Incidente**
- Quando: Incidente resolvido
- Conteúdo: Resumo, causa raiz, melhorias

**7. Email Interno - Treinamento**
- Quando: Após gap de treinamento identificado
- Objetivo: Promover awareness

**8. FAQ para Site/Intranet**
- Onde: Status page
- Conteúdo: Perguntas e respostas comuns

**Inclui:**
- ✅ Checklist pré-envio
- 📧 Templates prontos para usar
- 📋 Estruturas recomendadas

### [TEMPLATE_INCIDENT_REPORT.md](TEMPLATE_INCIDENT_REPORT.md)
**Template completo de relatório de incidente**

#### Estrutura Completa:

**1. Informações Básicas**
- Número do incidente
- Datas (detecção, resolução)
- Analista responsável
- Severidade e status
- Tipo de incidente

**2. Sumário Executivo**
- O que aconteceu (para liderança)
- Impacto
- Ação tomada
- Status atual

**3. Detecção**
- Como foi detectado
- Quem detectou
- Primeira evidência

**4. Análise Técnica**
- Tipo de ataque (MITRE ATT&CK)
- Vetor de ataque
- IOCs completos (domínios, IPs, hashes, URLs, emails)
- Timeline detalhada com diagrama

**5. Escopo do Incidente**
- Sistemas afetados (tabela)
- Dados potencialmente comprometidos
- Usuários/contas afetadas

**6. Resposta e Contenção**
- Ações imediatas (0-1h)
- Ações de contenção (1-4h)
- Ações de erradicação (4-24h)
- Evidências preservadas

**7. Recuperação**
- Ações de recuperação
- Validação
- Data de retorno à operação

**8. Métricas**
- Tempos de resposta (MTTD, MTTR, MTTE)
- Impacto financeiro e operacional

**9. Análise de Causa Raiz**
- Como o ataque foi possível
- Controles que falharam
- Por que falharam

**10. Lições Aprendidas**
- O que funcionou bem
- O que pode melhorar
- Surpresas/descobertas

**11. Recomendações**
- Curto prazo (1-2 semanas)
- Médio prazo (1-3 meses)
- Longo prazo (3-6 meses)
- Tabelas com responsáveis e prazos

**12. Anexos**
- Evidências técnicas
- Comunicações
- Referências

**13. Fechamento**
- Declaração de resolução
- Critérios de fechamento
- Aprovações

**14. Distribuição**
- Lista de destinatários
- Classificação de confidencialidade

**15. Histórico de Revisões**
- Controle de versões

**Inclui:**
- ✅ Checkboxes interativos
- 📊 Tabelas formatadas
- 🎨 Emojis para severidade
- 📋 Campos para assinaturas

## 🚀 Como Usar

### 1. Para Estudantes / Aprendizado

```bash
# Clone o repositório
git clone https://github.com/nataliagrossimoura-sec/02-PLAYBOOKS-AND-TEMPLATES.git

# Navegue pelos playbooks
cd 02-PLAYBOOKS-AND-TEMPLATES

# Comece pelo checklist de phishing
cat CHECKLIST_PHISHING.md

# Depois explore os playbooks
cat PLAYBOOK_RESPOSTA_PHISHING.md
```

**Dica:** Leia os documentos na seguinte ordem:
1. `CHECKLIST_PHISHING.md` - Fundamentos
2. `PLAYBOOK_RESPOSTA_PHISHING.md` - Procedimento completo
3. `FAQ.md` - Dúvidas comuns
4. Outros playbooks conforme interesse

### 2. Para Profissionais / Implementação em Empresa

```bash
# 1. Customize os templates para sua organização
# Edite contatos, ferramentas específicas, processos internos

# 2. Adapte os playbooks
# Inclua suas ferramentas (SIEM, EDR, Email Gateway específicos)

# 3. Treine a equipe
# Use os playbooks como base para treinamentos

# 4. Faça simulações
# Execute tabletop exercises baseados nos cenários
```

**Adaptação Recomendada:**
- Substitua contatos genéricos por seus contatos reais
- Adapte comandos para suas ferramentas específicas
- Adicione compliance específicoda sua indústria
- Traduza/adapte para o contexto da sua empresa

### 3. Para Análise de Casos Reais

```bash
# 1. Recebeu um email suspeito?
# Siga o CHECKLIST_PHISHING.md

# 2. Confirmou que é phishing?
# Use o PLAYBOOK_RESPOSTA_PHISHING.md

# 3. Encontrou malware?
# Use o PLAYBOOK_MALWARE_CONTAINMENT.md

# 4. Suspeita de brute force?
# Use o PLAYBOOK_BRUTE_FORCE.md

# 5. Documente TUDO
# Use o TEMPLATE_INCIDENT_REPORT.md
```

### 4. Para Portfólio / Entrevistas

**Destaque em seu currículo/LinkedIn:**
- "Estudei e apliquei playbooks de resposta a incidentes baseados em frameworks NIST e MITRE ATT&CK"
- "Experiência prática com procedimentos de resposta a phishing, malware e exfiltração de dados"
- "Documentação de incidentes seguindo melhores práticas de chain of custody"

**Prepare-se para demonstrar:**
- Como você seguiria o fluxo de um playbook
- Decisões que tomaria em cada fase
- Como documentaria o incidente
- Melhorias que proporia

## 🎓 Casos de Uso Práticos

### Cenário 1: Campanha de Phishing Massiva
**Situação:** 50+ usuários receberam email suspeito

**Playbooks a usar:**
1. `CHECKLIST_PHISHING.md` - Validação inicial
2. `PLAYBOOK_RESPOSTA_PHISHING.md` - Resposta coordenada
3. `TEMPLATE_COMMUNICATION.md` - Alerta interno + comunicação usuários
4. `TEMPLATE_INCIDENT_REPORT.md` - Documentação

**Tempo estimado:** 4-8 horas

### Cenário 2: Ransomware Detectado
**Situação:** Servidor comprometido, arquivos sendo criptografados

**Playbooks a usar:**
1. `PLAYBOOK_MALWARE_CONTAINMENT.md` - Contenção imediata
2. `PLAYBOOK_EVIDENCE_COLLECTION.md` - Preservação de evidências
3. `PLAYBOOK_INCIDENT_INVESTIGATION.md` - Investigação profunda
4. `TEMPLATE_INCIDENT_REPORT.md` - Relatório forense

**Tempo estimado:** 24-72 horas

### Cenário 3: Suspeita de Exfiltração
**Situação:** DLP alertou upload anômalo

**Playbooks a usar:**
1. `PLAYBOOK_DATA_EXFILTRATION.md` - Resposta específica
2. `PLAYBOOK_EVIDENCE_COLLECTION.md` - Coleta de evidências
3. `TEMPLATE_COMMUNICATION.md` - Notificação LGPD (se aplicável)
4. `TEMPLATE_INCIDENT_REPORT.md` - Relatório completo

**Tempo estimado:** 8-24 horas + obrigações legais

### Cenário 4: Brute Force em SSH
**Situação:** Múltiplas tentativas de login falhadas

**Playbooks a usar:**
1. `PLAYBOOK_BRUTE_FORCE.md` - Resposta e hardening
2. `PLAYBOOK_SECURITY.md` - Implementação de melhores práticas
3. `TEMPLATE_INCIDENT_REPORT.md` - Documentação

**Tempo estimado:** 2-4 horas

## 📊 Métricas e KPIs

Ao implementar estes playbooks, meça:

### Tempos de Resposta
- **MTTD** (Mean Time To Detect): < 1 hora
- **MTTR** (Mean Time To Respond): < 4 horas
- **MTTE** (Mean Time To Eradicate): < 24 horas

### Efetividade
- **Taxa de Detecção**: % de incidentes detectados por controles automatizados
- **Taxa de Falsos Positivos**: < 5%
- **Taxa de Contenção Bem-Sucedida**: > 95%

### Maturidade
- **Playbooks Atualizados**: Revisão trimestral
- **Equipe Treinada**: 100% da equipe SOC
- **Simulações Realizadas**: 1 por trimestre

## 🤝 Contribuindo

Contribuições são muito bem-vindas! Este é um projeto colaborativo.

### Como Contribuir:

1. **Fork** o repositório
2. **Crie** uma branch para sua feature (`git checkout -b feature/NovoPlaybook`)
3. **Commit** suas mudanças (`git commit -m 'Adiciona playbook de DDoS'`)
4. **Push** para a branch (`git push origin feature/NovoPlaybook`)
5. Abra um **Pull Request**

### Tipos de Contribuição:

- 📝 **Novos playbooks** (ex: DDoS, Insider Threat)
- 🔧 **Melhorias em playbooks existentes**
- 🐛 **Correções** de erros ou imprecisões
- 📖 **Documentação** adicional
- 🌍 **Traduções** para outros idiomas
- 💡 **Casos de uso** e exemplos práticos
- 🛠️ **Scripts** de automação

### Diretrizes:

- Siga o formato Markdown dos documentos existentes
- Use emojis para melhor visualização
- Inclua exemplos práticos quando possível
- Adicione referências (NIST, MITRE, etc.)
- Teste seus procedimentos antes de submeter
- Mantenha linguagem profissional mas acessível

## 📞 Contato e Suporte

**Dúvidas sobre os playbooks?**
- 💬 Abra uma [Issue](https://github.com/nataliagrossimoura-sec/02-PLAYBOOKS-AND-TEMPLATES/issues)
- 📧 Email: [contato]
- 💼 LinkedIn: [perfil]

**Encontrou um bug ou problema de segurança?**
- 🐛 Reporte via Issue (para bugs gerais)
- 🔒 Email direto (para vulnerabilidades de segurança)

## 📚 Recursos Complementares

### Frameworks e Padrões
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [SANS Incident Handler's Handbook](https://www.sans.org/white-papers/33901/)
- [CIS Controls](https://www.cisecurity.org/controls)

### Ferramentas Mencionadas
- [Wazuh](https://wazuh.com/) - SIEM open source
- [VirusTotal](https://www.virustotal.com/) - Análise de malware
- [URLScan.io](https://urlscan.io/) - Análise de URLs
- [PhishTank](https://phishtank.org/) - Database de phishing
- [ANY.RUN](https://any.run/) - Sandbox online

### Treinamentos Recomendados
- [Cybrary](https://www.cybrary.it/) - Cursos gratuitos
- [TryHackMe](https://tryhackme.com/) - Labs práticos
- [SANS Cyber Aces](https://www.cyberaces.org/) - Tutoriais gratuitos

## ⚖️ Licença

Este projeto está licenciado sob a **MIT License** - veja o arquivo [LICENSE](LICENSE) para detalhes.

### O que isso significa:
- ✅ **Uso comercial permitido**
- ✅ **Modificação permitida**
- ✅ **Distribuição permitida**
- ✅ **Uso privado permitido**
- ⚠️ **Sem garantia**
- ⚠️ **Sem responsabilidade do autor**

**Resumo:** Você pode usar, modificar e distribuir livremente, mas sem garantias. Ideal para uso educacional e empresarial.

## 🙏 Agradecimentos

Este projeto foi inspirado por:
- **Comunidade de Infosec brasileira** 🇧🇷
- **Frameworks NIST, MITRE e SANS**
- **Experiências reais de SOC Teams**
- **Contribuições da comunidade open source**

Agradecimento especial a todos que contribuíram com feedback, correções e melhorias.

---

## 🎯 Roadmap

### Q1 2026
- [ ] Adicionar playbook de DDoS
- [ ] Adicionar playbook de Insider Threat
- [ ] Tradução para inglês
- [ ] Vídeos explicativos

### Q2 2026
- [ ] Adicionar playbook de Supply Chain Attack
- [ ] Integração com SOAR (Security Orchestration)
- [ ] Dashboard de métricas de incidentes
- [ ] Certificação/badge de conclusão

### Futuro
- [ ] App mobile para consulta rápida
- [ ] API para integração com sistemas
- [ ] Comunidade/fórum de discussão
- [ ] Casos de estudo detalhados

---

## 📈 Estatísticas do Projeto

- **Documentos:** 13 arquivos principais
- **Playbooks:** 7 completos
- **Templates:** 2 profissionais
- **Checklists:** 1 completo (4 fases)
- **Última Atualização:** Dezembro 2025
- **Versão:** 1.0

---

## 💡 Dica Final

> "A melhor defesa é uma boa preparação. Estes playbooks são seu guia, mas a prática e a adaptação ao seu contexto são essenciais."

**Não espere um incidente real para usar estes playbooks. Pratique com simulações, faça tabletop exercises, treine sua equipe. Quando o incidente real acontecer, você estará pronto.**

---

**⭐ Se este projeto foi útil para você, considere dar uma estrela no GitHub!**

**🔄 Mantenha este repositório atualizado com `git pull` regularmente**

**📢 Compartilhe com colegas que podem se beneficiar**

---

**Desenvolvido com 💙 para a comunidade de Cybersecurity**

**#BlueTeam #SOC #IncidentResponse #Phishing #Malware #DFIR #CyberSecurity #InfoSec**
