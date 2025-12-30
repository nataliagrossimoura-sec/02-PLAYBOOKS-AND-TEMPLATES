# 🚨 RELATÓRIO DE INCIDENTE DE SEGURANÇA

## 📋 Informações Básicas

| Campo | Valor |
|-------|-------|
| **Número do Incidente** | INC-2025-XXXX |
| **Data de Detecção** | DD/MM/YYYY HH:MM |
| **Data de Resolução** | DD/MM/YYYY HH:MM |
| **Analista Responsável** | Nome do Analista |
| **Severidade** | 🔴 Crítico / 🟠 Alto / 🟡 Médio / 🟢 Baixo |
| **Status** | 🔴 Ativo / 🟡 Em Investigação / 🟢 Resolvido |
| **Tipo de Incidente** | Phishing / Malware / Brute Force / DDoS / Outro |

---

## 📝 SUMÁRIO EXECUTIVO

> Resumo breve (3-5 linhas) do incidente para liderança executiva

**O que aconteceu:**
[Descrição concisa do incidente]

**Impacto:**
[Qual foi o impacto nos negócios/operações]

**Ação tomada:**
[Resumo das principais ações de contenção]

**Status atual:**
[Situação atual e próximos passos]

---

## 🔍 DETECÇÃO

### Como o incidente foi detectado?
- [ ] Alerta automático do SIEM
- [ ] Reporte de usuário
- [ ] Email gateway
- [ ] Monitoramento proativo
- [ ] Descoberta acidental
- [ ] Outro: ___________

### Quem detectou?
**Nome:** [Nome da pessoa/sistema]  
**Cargo/Função:** [Cargo ou função do sistema]  
**Data/Hora:** [DD/MM/YYYY HH:MM:SS]

### Primeira Evidência
[Descrever a primeira evidência do incidente - log, alerta, screenshot, etc.]

```
[Copiar log ou evidência inicial aqui]
```

---

## 📊 ANÁLISE TÉCNICA

### Tipo de Ataque
**Classificação:** [Phishing / Spear Phishing / BEC / Malware / etc.]  
**MITRE ATT&CK ID:** [Ex: T1566.001 - Phishing: Spearphishing Attachment]

### Vetor de Ataque
[Como o atacante ganhou acesso inicial?]
- [ ] Email de phishing
- [ ] Link malicioso
- [ ] Anexo malicioso
- [ ] Credenciais comprometidas
- [ ] Vulnerabilidade explorada
- [ ] Engenharia social
- [ ] Outro: ___________

### Indicadores de Comprometimento (IOCs)

#### Domínios Maliciosos
```
domain1.malicious.com
domain2.phishing.net
```

#### IPs Suspeitos
```
1.2.3.4 (País: XX, ASN: ASXXXXX)
5.6.7.8 (País: YY, ASN: ASYYYYY)
```

#### Hashes de Arquivos
```
SHA256: a1b2c3d4e5f6g7h8i9j0...
MD5:    1a2b3c4d5e6f7g8h9i0j...
```

#### URLs Maliciosas
```
hxxp://phishing[.]site/login
hxxps://fake-bank[.]com/verify
```

#### Endereços de Email
```
attacker@malicious.com
spoofed@legitimate-looking.com
```

### Timeline do Ataque

| Data/Hora | Evento | Evidência |
|-----------|--------|-----------|
| DD/MM HH:MM | Primeiro email de phishing enviado | Log do email gateway |
| DD/MM HH:MM | Usuário clicou no link | Proxy logs |
| DD/MM HH:MM | Credenciais inseridas | Web server logs |
| DD/MM HH:MM | Login anômalo detectado | SIEM alert |
| DD/MM HH:MM | Contenção iniciada | Ticket SOC |

**Diagrama de Timeline:**
```
T0 ───────► T1 ───────► T2 ───────► T3 ───────► T4
Envio      Clique     Compromisso  Detecção   Contenção
```

---

## 🎯 ESCOPO DO INCIDENTE

### Sistemas Afetados

| Sistema | IP/Hostname | Nível de Comprometimento | Status |
|---------|-------------|-------------------------|--------|
| Server01 | 192.168.1.50 | Totalmente comprometido | Isolado |
| Workstation-Finance | PC-FIN-01 | Credenciais expostas | Em análise |
| Email Account | user@company.com | Acesso não autorizado | Senha resetada |

### Dados Potencialmente Comprometidos

- [ ] Credenciais de usuário
- [ ] Informações financeiras
- [ ] Dados pessoais (PII)
- [ ] Propriedade intelectual
- [ ] Informações de clientes
- [ ] Outros: ___________

**Quantidade estimada:** [Ex: 500 registros de clientes]  
**Sensibilidade:** 🔴 Crítica / 🟠 Alta / 🟡 Média / 🟢 Baixa

### Usuários/Contas Afetadas

| Nome | Departamento | Email | Nível de Privilégio | Ação Tomada |
|------|--------------|-------|---------------------|-------------|
| João Silva | Financeiro | joao@empresa.com | Usuário padrão | Senha resetada |
| Maria Santos | TI | maria@empresa.com | Admin | MFA forçado |

---

## 🛡️ RESPOSTA E CONTENÇÃO

### Ações Imediatas (0-1 hora)

**Timestamp de início:** [DD/MM/YYYY HH:MM]

- [x] Isolar sistemas afetados da rede
- [x] Revogar sessões ativas do usuário
- [x] Desabilitar contas comprometidas
- [x] Bloquear IPs/domínios maliciosos no firewall
- [x] Preservar evidências (logs, memória, disco)
- [x] Notificar stakeholders internos

**Evidências preservadas:**
- [ ] Imagem de disco
- [ ] Dump de memória
- [ ] Logs do sistema
- [ ] Tráfego de rede (PCAP)
- [ ] Screenshots

### Ações de Contenção (1-4 horas)

- [x] Análise forense inicial
- [x] Identificação de escopo completo
- [x] Quarentena de emails similares
- [x] Scan de antivírus em todos endpoints
- [x] Verificação de logins anômalos
- [x] Atualização de regras de detecção

### Ações de Erradicação (4-24 horas)

- [x] Remover malware/backdoors
- [x] Fechar vulnerabilidades exploradas
- [x] Resetar senhas de todas contas afetadas
- [x] Remover persistência do atacante
- [x] Verificar criação de contas suspeitas
- [x] Restaurar sistemas a partir de backup limpo

---

## 🔧 RECUPERAÇÃO

### Ações de Recuperação

- [ ] Restaurar sistemas do backup
- [ ] Reabilitar contas de usuários
- [ ] Remover bloqueios de rede
- [ ] Verificar integridade dos dados
- [ ] Testar funcionalidade dos sistemas
- [ ] Monitoramento intensivo (48h)

### Validação

- [ ] Sistemas operando normalmente
- [ ] Sem sinais de reinfecção
- [ ] Usuários conseguem acessar recursos
- [ ] Sem atividade anômala detectada
- [ ] Backups testados e funcionais

**Data de retorno à operação normal:** [DD/MM/YYYY HH:MM]

---

## 📈 MÉTRICAS

### Tempos de Resposta

| Métrica | Tempo | Meta | Status |
|---------|-------|------|--------|
| **MTTD** (Mean Time To Detect) | 45 minutos | < 1 hora | ✅ |
| **MTTR** (Mean Time To Respond) | 2 horas | < 4 horas | ✅ |
| **MTTE** (Mean Time To Eradicate) | 6 horas | < 24 horas | ✅ |
| **Tempo Total de Incidente** | 8 horas | - | - |

### Impacto

**Impacto Financeiro Estimado:**
- Custo de resposta: R$ _______
- Perda de produtividade: R$ _______
- Possível multa/compliance: R$ _______
- **Total:** R$ _______

**Impacto Operacional:**
- Sistemas offline: _____ horas
- Usuários afetados: _____ pessoas
- Transações interrompidas: _____ operações

---

## 🔍 ANÁLISE DE CAUSA RAIZ

### Como o ataque foi possível?

**Vulnerabilidade Explorada:**
[Descrever a falha que permitiu o ataque]

**Controles que Falharam:**
- [ ] Filtro de email não detectou
- [ ] Usuário não identificou phishing
- [ ] MFA não estava habilitado
- [ ] Antivírus não detectou malware
- [ ] Falta de monitoramento
- [ ] Outro: ___________

### Por que os controles falharam?

**Falhas Técnicas:**
[Descrever problemas técnicos]

**Falhas de Processo:**
[Descrever problemas de processo]

**Falhas Humanas:**
[Descrever fatores humanos, SEM culpar indivíduos]

---

## 💡 LIÇÕES APRENDIDAS

### O que funcionou bem?
1. [Aspecto positivo da resposta]
2. [Ferramenta/processo que ajudou]
3. [Colaboração efetiva]

### O que pode melhorar?
1. [Área que precisa atenção]
2. [Processo que atrasou resposta]
3. [Ferramenta/visibilidade que faltou]

### Surpresas/Descobertas
[Algo inesperado que foi descoberto durante o incidente]

---

## 📝 RECOMENDAÇÕES

### Curto Prazo (1-2 semanas)

| # | Recomendação | Responsável | Prazo | Status |
|---|--------------|-------------|-------|--------|
| 1 | Atualizar regras do email gateway | TI | DD/MM | 🟡 |
| 2 | Treinamento adicional para dept. afetado | Seg | DD/MM | 🔴 |
| 3 | Implementar MFA para todos usuários | TI | DD/MM | 🟡 |

### Médio Prazo (1-3 meses)

| # | Recomendação | Responsável | Prazo | Status |
|---|--------------|-------------|-------|--------|
| 4 | Avaliar solução de sandboxing | Seg | DD/MM | 🔴 |
| 5 | Simulação de phishing mensal | Seg | DD/MM | 🔴 |
| 6 | Atualizar playbooks de resposta | SOC | DD/MM | 🟡 |

### Longo Prazo (3-6 meses)

| # | Recomendação | Responsável | Prazo | Status |
|---|--------------|-------------|-------|--------|
| 7 | Implementar SOAR para automação | Seg | DD/MM | 🔴 |
| 8 | Programa de Security Champions | RH+Seg | DD/MM | 🔴 |
| 9 | Red team exercise completo | Seg | DD/MM | 🔴 |

---

## 📎 ANEXOS

### Evidências Técnicas

**Anexo A:** Logs completos do SIEM  
**Anexo B:** Headers dos emails maliciosos  
**Anexo C:** Screenshots da página de phishing  
**Anexo D:** Análise de malware (se aplicável)  
**Anexo E:** Resultados de scans de rede

### Comunicações

**Anexo F:** Email de notificação interna  
**Anexo G:** Comunicação com usuários afetados  
**Anexo H:** Reporte a autoridades (se aplicável)

### Referências

- NIST Cybersecurity Framework
- MITRE ATT&CK: [Link para técnica]
- Playbook utilizado: [Nome/versão]
- Ferramentas utilizadas: [Lista]

---

## ✅ FECHAMENTO

### Declaração de Resolução

Este incidente é considerado **RESOLVIDO** com base nos seguintes critérios:

- [x] Ameaça foi completamente contida e erradicada
- [x] Sistemas foram restaurados e validados
- [x] Não há evidência de persistência
- [x] Monitoramento intensivo não detectou atividade anômala
- [x] Usuários foram notificados e treinados
- [x] Recomendações foram documentadas

**Data de Fechamento:** [DD/MM/YYYY]  
**Aprovado por:** [Nome do CISO/Manager]

---

## 📊 DISTRIBUIÇÃO

**Este relatório deve ser distribuído para:**

- [x] CISO / Gerente de Segurança
- [x] CIO / Gerente de TI
- [x] Time SOC
- [x] Compliance / Jurídico
- [ ] CEO / Board (apenas se severidade Crítica)
- [ ] Auditoria Interna
- [ ] Seguradoras (se aplicável)

**Classificação:** 🔴 Confidencial - Distribuição Limitada

---

## 📝 HISTÓRICO DE REVISÕES

| Versão | Data | Autor | Mudanças |
|--------|------|-------|----------|
| 1.0 | DD/MM/YYYY | [Nome] | Versão inicial |
| 1.1 | DD/MM/YYYY | [Nome] | Adicionado análise de causa raiz |
| 2.0 | DD/MM/YYYY | [Nome] | Versão final aprovada |

---

**Documento Preparado Por:**  
Nome: _________________  
Cargo: Analista SOC / Incident Responder  
Data: _________________  
Assinatura: _________________

**Revisado Por:**  
Nome: _________________  
Cargo: SOC Manager / CISO  
Data: _________________  
Assinatura: _________________

**Aprovado Por:**  
Nome: _________________  
Cargo: CISO / CIO  
Data: _________________  
Assinatura: _________________

---

<p align="center">
  <strong>FIM DO RELATÓRIO</strong><br>
  Documento Confidencial - Não Distribuir sem Autorização
</p>
