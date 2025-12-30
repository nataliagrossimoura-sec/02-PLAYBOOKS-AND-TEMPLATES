# 📧 TEMPLATES: Comunicação de Incidentes

## 📋 Visão Geral

Templates de comunicação para diferentes stakeholders durante e após incidentes de segurança.

---

## 🎯 Princípios de Comunicação

### Geral
- ✅ Transparente mas não alarmista
- ✅ Factual e preciso
- ✅ Linguagem apropriada ao público
- ✅ Atualizar regularmente
- ✅ Ponto de contato único

### O Que Comunicar
- ✅ O que aconteceu (fatos conhecidos)
- ✅ O que está sendo feito
- ✅ O que esperar a seguir
- ✅ Como obter mais informações

### O Que NÃO Comunicar
- ❌ Especulações
- ❌ Detalhes técnicos vulnerabilidades não corrigidas
- ❌ Culpabilização de indivíduos
- ❌ Informações confidenciais da investigação

---

## 📧 TEMPLATE 1: Alerta Interno Inicial

**Quando usar:** Primeiros 30 minutos de um incidente crítico  
**Público:** Equipe de TI, Segurança, Management  
**Canal:** Email, Slack/Teams urgente

```
Assunto: [URGENTE] Incidente de Segurança em Andamento - INC-2025-001

Equipe,

ALERTA DE INCIDENTE DE SEGURANÇA

Data/Hora: 29/12/2025 14:30 BRT
Severidade: 🔴 CRÍTICA / 🟠 ALTA / 🟡 MÉDIA / 🟢 BAIXA
Status: EM INVESTIGAÇÃO

O QUE SABEMOS:
[Descrição breve e factual do incidente - 2-3 frases]

SISTEMAS AFETADOS:
- [Sistema 1]
- [Sistema 2]
- [Sistema 3]

IMPACTO ATUAL:
[Breve descrição do impacto]

AÇÕES TOMADAS:
- [Ação 1]
- [Ação 2]

PRÓXIMOS PASSOS:
1. [Passo 1]
2. [Passo 2]

CANAL DE COORDENAÇÃO:
Slack: #incident-2025-001
War Room: [local/link]

PONTO DE CONTATO:
Incident Commander: [Nome] - [Telefone] - [Email]

PRÓXIMA ATUALIZAÇÃO:
[Data/Hora - geralmente 1-2 horas]

---
NÃO REPASSAR EXTERNAMENTE SEM AUTORIZAÇÃO
```

---

## 📧 TEMPLATE 2: Atualização de Status

**Quando usar:** Updates regulares durante incidente  
**Frequência:** A cada 2-4 horas (crítico), diariamente (baixa severidade)  
**Público:** Stakeholders internos

```
Assunto: [UPDATE #2] Incidente de Segurança INC-2025-001

Equipe,

ATUALIZAÇÃO DE INCIDENTE - #2

Incidente: INC-2025-001
Data/Hora Update: 29/12/2025 16:30 BRT
Status: CONTIDO / EM INVESTIGAÇÃO / RESOLVIDO

PROGRESSO DESDE ÚLTIMA ATUALIZAÇÃO:
✅ [Conquista 1]
✅ [Conquista 2]
🔄 [Em andamento 1]
⏳ [Pendente 1]

SITUAÇÃO ATUAL:
[Resumo da situação - 2-3 frases]

IMPACTO:
- Usuários afetados: [número]
- Sistemas offline: [lista]
- ETA para recuperação: [estimativa]

PRÓXIMAS AÇÕES:
1. [Ação 1] - Responsável: [Nome] - ETA: [tempo]
2. [Ação 2] - Responsável: [Nome] - ETA: [tempo]

SUPORTE NECESSÁRIO:
[Se algum recurso adicional é necessário]

PRÓXIMA ATUALIZAÇÃO:
[Data/Hora]

Dúvidas: [Contato do Incident Commander]
```

---

## 📧 TEMPLATE 3: Comunicação para Usuários Finais (Incidente Ativo)

**Quando usar:** Quando incidente afeta usuários  
**Público:** Todos colaboradores / Clientes  
**Tom:** Calmo, reassegurador, informativo

```
Assunto: Aviso: Manutenção de Emergência - Serviços Temporariamente Indisponíveis

Prezados Colaboradores / Clientes,

Estamos cientes de que alguns de nossos serviços estão temporariamente indisponíveis.

O QUE ESTÁ ACONTECENDO:
Identificamos um problema técnico que está afetando [sistema/serviço]. 
Nossa equipe está trabalhando ativamente para restaurar o serviço normal.

SERVIÇOS AFETADOS:
- [Serviço 1]: Indisponível
- [Serviço 2]: Funcionando com intermitência
- [Serviço 3]: Operando normalmente

O QUE ESTAMOS FAZENDO:
Nossa equipe de TI está investigando e corrigindo o problema. 
Esperamos restaurar todos os serviços dentro de [X horas].

O QUE VOCÊ DEVE FAZER:
- [Ação recomendada 1]
- [Ação recomendada 2]
- [Workaround temporário, se houver]

INFORMAÇÕES ADICIONAIS:
Atualizações serão postadas em [local - intranet, status page, etc]

Para urgências, contate: [email/telefone]

Agradecemos sua compreensão e paciência enquanto resolvemos esta situação.

Atenciosamente,
[Nome]
[Cargo]
[Empresa]
```

---

## 📧 TEMPLATE 4: Notificação de Violação de Dados (LGPD)

**Quando usar:** Data breach confirmado  
**Público:** Usuários afetados  
**Requisito Legal:** LGPD Art. 48

```
Assunto: Notificação Importante sobre Segurança de Dados

Prezado(a) [Nome],

NOTIFICAÇÃO DE INCIDENTE DE SEGURANÇA

Estamos entrando em contato para informá-lo(a) sobre um incidente de 
segurança de dados que pode ter afetado suas informações pessoais.

DATA DO INCIDENTE:
[Data de quando ocorreu]

DATA DE DESCOBERTA:
[Data de quando foi detectado]

O QUE ACONTECEU:
[Descrição factual, sem jargões técnicos, do que ocorreu]

INFORMAÇÕES AFETADAS:
Os seguintes tipos de dados pessoais podem ter sido expostos:
[ ] Nome completo
[ ] CPF
[ ] Endereço
[ ] Email
[ ] Telefone
[ ] [Outros dados específicos]

VOLUME:
Aproximadamente [número] de registros foram potencialmente afetados.

DADOS NÃO AFETADOS:
As seguintes informações NÃO foram comprometidas:
- Senhas (criptografadas e não expostas)
- Dados de cartão de crédito
- [Outros dados não afetados]

O QUE ESTAMOS FAZENDO:
1. Investigação forense completa foi iniciada
2. Vulnerabilidade foi corrigida
3. Sistemas de segurança foram reforçados
4. Autoridade Nacional de Proteção de Dados (ANPD) foi notificada
5. [Outras medidas específicas]

O QUE VOCÊ DEVE FAZER:
Recomendamos as seguintes ações para proteger suas informações:

IMEDIATAMENTE:
1. Altere sua senha na nossa plataforma
2. Ative autenticação de dois fatores (se disponível)
3. Monitore suas contas bancárias e extratos de cartão

NOS PRÓXIMOS 30 DIAS:
4. Fique atento a emails/ligações suspeitas
5. Não clique em links de remetentes desconhecidos
6. Verifique sua pontuação de crédito

OFERECEMOS:
Como medida de segurança adicional, estamos oferecendo:
- [X meses] de monitoramento de crédito gratuito
- Linha direta de suporte dedicada
- [Outros benefícios]

PARA ATIVAR O MONITORAMENTO GRATUITO:
Acesse: [URL seguro]
Código: [código individual]
Válido até: [data]

MAIS INFORMAÇÕES:
Para mais detalhes sobre este incidente, visite:
[URL da página de FAQ]

CONTATO:
Para dúvidas ou preocupações:
Email: seguranca-dados@empresa.com
Telefone: 0800 XXX XXXX (seg-sex, 9h-18h)

SEUS DIREITOS (LGPD):
Você tem o direito de:
- Confirmar a existência de tratamento de seus dados
- Acessar seus dados
- Corrigir dados incompletos ou inexatos
- Solicitar a eliminação de dados
- Revogar consentimento

Para exercer seus direitos, contate nosso DPO (Encarregado de Proteção 
de Dados): dpo@empresa.com

Lamentamos profundamente este incidente e estamos comprometidos com a 
proteção de suas informações pessoais.

Atenciosamente,

[Nome Completo]
[Cargo - ex: Diretor de Segurança da Informação]
[Empresa]
[Contato]

---
Enviado em cumprimento à Lei Geral de Proteção de Dados (LGPD)
Lei nº 13.709/2018
```

---

## 📧 TEMPLATE 5: Comunicado à Imprensa

**Quando usar:** Incidente público de grande escala  
**Público:** Mídia, público geral  
**Aprovar com:** Legal, PR, CEO

```
COMUNICADO À IMPRENSA

PARA DIVULGAÇÃO IMEDIATA

[NOME DA EMPRESA] INFORMA SOBRE INCIDENTE DE SEGURANÇA

[Cidade, Estado] - [Data] - A [Nome da Empresa] está informando sobre 
um incidente de segurança que ocorreu em [data].

RESUMO:
[Breve descrição do incidente em linguagem não-técnica]

AÇÕES TOMADAS:
A empresa tomou as seguintes medidas imediatas:
- [Ação 1]
- [Ação 2]
- [Ação 3]

IMPACTO:
[Descrição factual do impacto, sem minimizar mas sem alarmar]

CLIENTES AFETADOS:
Aproximadamente [número] de clientes foram potencialmente afetados. 
Notificações individuais estão sendo enviadas conforme exigido por lei.

PROTEÇÃO DE DADOS:
A [Nome da Empresa] leva a segurança de dados muito a sério. 
Implementamos [medidas de segurança que estavam em vigor].

PRÓXIMOS PASSOS:
- Investigação forense completa em andamento
- Cooperação total com autoridades competentes
- Reforço de medidas de segurança
- [Outros passos específicos]

SUPORTE AO CLIENTE:
Clientes podem obter mais informações em:
Website: [URL]
Email: [email dedicado]
Telefone: [número toll-free]

DECLARAÇÃO DO EXECUTIVO:
"[Citação do CEO/CISO expressando preocupação, comprometimento 
com segurança, e apoio aos clientes]"
- [Nome], [Cargo]

SOBRE A [NOME DA EMPRESA]:
[Boilerplate padrão da empresa]

CONTATO PARA MÍDIA:
[Nome do porta-voz]
[Cargo]
[Email]
[Telefone]

###
```

---

## 📧 TEMPLATE 6: Comunicação Pós-Incidente (Fechamento)

**Quando usar:** Incidente resolvido  
**Público:** Todos stakeholders

```
Assunto: [RESOLVIDO] Incidente de Segurança INC-2025-001 - Encerramento

Equipe,

INCIDENTE RESOLVIDO

Incidente: INC-2025-001
Data de Detecção: 29/12/2025 14:30 BRT
Data de Resolução: 30/12/2025 08:00 BRT
Duração Total: 17 horas 30 minutos

RESUMO:
[Breve descrição do que aconteceu]

CAUSA RAIZ:
[Explicação da causa identificada]

IMPACTO FINAL:
- Sistemas afetados: [lista]
- Usuários impactados: [número]
- Tempo de downtime: [duração]
- Dados comprometidos: Nenhum / [descrição]

AÇÕES DE REMEDIAÇÃO IMPLEMENTADAS:
✅ [Correção 1]
✅ [Correção 2]
✅ [Correção 3]

MELHORIAS DE SEGURANÇA:
Como resultado deste incidente, implementaremos:
- [Melhoria 1] - ETA: [data]
- [Melhoria 2] - ETA: [data]
- [Melhoria 3] - ETA: [data]

LIÇÕES APRENDIDAS:
O que funcionou bem:
- [Ponto positivo 1]
- [Ponto positivo 2]

O que pode ser melhorado:
- [Área de melhoria 1]
- [Área de melhoria 2]

RECONHECIMENTOS:
Agradecemos especialmente a:
- [Nome/Equipe] por [contribuição]
- [Nome/Equipe] por [contribuição]

PRÓXIMOS PASSOS:
- Reunião de post-mortem agendada para: [data/hora]
- Relatório completo disponível em: [local]

RETORNO À OPERAÇÃO NORMAL:
Todos os sistemas estão operando normalmente e o incidente está 
oficialmente encerrado.

Para dúvidas sobre este incidente: [contato]

Atenciosamente,
[Nome do Incident Commander]
[Data]
```

---

## 📧 TEMPLATE 7: Email Interno - Treinamento Pós-Incidente

**Quando usar:** Após incidente que expõe gap de treinamento  
**Público:** Equipe relevante ou toda empresa

```
Assunto: Importante: Treinamento de Segurança Obrigatório

Prezados Colaboradores,

TREINAMENTO DE SEGURANÇA - AÇÃO NECESSÁRIA

Recentemente, identificamos um incidente de segurança que destacou 
a importância de [tema específico - ex: identificação de phishing].

POR QUE ESTE TREINAMENTO:
[Breve contexto sobre o incidente - sem detalhes sensíveis]

O QUE VOCÊ APRENDERÁ:
- [Tópico 1]
- [Tópico 2]
- [Tópico 3]

INFORMAÇÕES DO TREINAMENTO:
Formato: [Online / Presencial]
Duração: [X minutos]
Prazo para conclusão: [data]
Link: [URL]

OBRIGATÓRIO:
Este treinamento é obrigatório para todos os colaboradores e deve ser 
concluído até [data]. O não cumprimento será reportado ao RH.

SEU PAPEL:
Cada um de nós é uma linha de defesa contra ameaças cibernéticas. 
Sua vigilância e conhecimento protegem não apenas a empresa, mas 
também nossos clientes e colegas.

PERGUNTAS:
Para dúvidas sobre o treinamento: [contato]

Agradecemos sua colaboração na manutenção de um ambiente seguro.

Atenciosamente,
[Nome]
Equipe de Segurança da Informação
```

---

## 📧 TEMPLATE 8: FAQ para Site/Intranet

**Quando usar:** Incidentes públicos  
**Onde:** Status page, FAQ dedicada

```markdown
# Perguntas Frequentes - Incidente de Segurança

Atualizado em: [Data/Hora]

## O que aconteceu?
[Descrição em linguagem simples]

## Quando isso aconteceu?
O incidente foi detectado em [data] às [hora].

## Meus dados foram comprometidos?
[Resposta específica - sim/não/possivelmente]

## Que tipo de informação foi afetada?
[Lista específica de tipos de dados]

## O que vocês estão fazendo sobre isso?
[Lista de ações tomadas]

## Preciso fazer alguma coisa?
[Instruções claras passo a passo]

## Minha senha está segura?
[Resposta específica e recomendações]

## Devo me preocupar com fraude?
[Orientação sobre monitoramento e sinais de alerta]

## Vocês vão me compensar?
[Explicar o que está sendo oferecido]

## Como posso obter mais informações?
Contato: [email/telefone]
Esta página será atualizada conforme novas informações estiverem disponíveis.

## Quando tudo voltará ao normal?
[Estimativa realista ou explicação de por que não pode estimar]

## Como vocês vão evitar que isso aconteça novamente?
[Medidas de segurança sendo implementadas]
```

---

## ✅ Checklist de Comunicação

```
ANTES DE ENVIAR:
[ ] Factualmente correto
[ ] Aprovado por Legal (se necessário)
[ ] Tom apropriado ao público
[ ] Sem jargões técnicos desnecessários
[ ] Inclui ações claras
[ ] Contato para mais informações
[ ] Revisado por duas pessoas mínimo

APÓS ENVIAR:
[ ] Documentar quem foi notificado e quando
[ ] Preparar para perguntas/respostas
[ ] Monitorar reações e feedback
[ ] Planejar próxima atualização
```

---

**📧 Lembre-se:** Comunicação em crise pode ser tão importante quanto a resposta técnica. Sempre priorize transparência, empatia e clareza.

---

**Última atualização:** Dezembro 2025  
**Versão:** 1.0  
**Aprovado por:** CISO + Legal + PR
