# 🎯 CHECKLIST: Identificação de Phishing

## 📋 Guia Rápido de Verificação

Use este checklist ANTES de interagir com qualquer email suspeito. Cada item marcado aumenta a probabilidade de phishing.

---

## 🔍 FASE 1: VERIFICAÇÃO DO REMETENTE

### ✅ Análise do Endereço de Email

- [ ] O domínio após @ é EXATAMENTE o oficial da empresa?
  - ⚠️ Cuidado: `paypa1.com` (1 no lugar de l)
  - ⚠️ Cuidado: `amazon-security.com` (hífen suspeito)
  - ✅ Correto: `@paypal.com`, `@amazon.com`

- [ ] O nome de exibição confere com o domínio?
  - ❌ "Banco Brasil `<contato@fraudador.com>`"
  - ✅ "Banco Brasil `<contato@bb.com.br>`"

- [ ] Você ESPERAVA receber este email desta pessoa/empresa?

- [ ] A pessoa/empresa costuma se comunicar por este canal?

### ✅ Verificação de Headers (avançado)

- [ ] Return-Path é igual ao From? (Se não, ALERTA!)
- [ ] SPF/DKIM/DMARC passaram na autenticação?
- [ ] O servidor remetente é de país suspeito? (Rússia, China para email de banco brasileiro)

**Pontuação até aqui:**
- 0-1 itens marcados: Provavelmente legítimo
- 2-3 itens marcados: SUSPEITO, continue investigando
- 4+ itens marcados: PHISHING, não interaja!

---

## 📧 FASE 2: ANÁLISE DO CONTEÚDO

### ✅ Linguagem e Tom

- [ ] Há senso de URGÊNCIA excessiva?
  - "Sua conta será bloqueada em 24h!"
  - "Ação imediata necessária!"
  - "Último aviso!"

- [ ] Há AMEAÇAS diretas ou indiretas?
  - "Processo judicial será aberto"
  - "Conta será encerrada"
  - "Acesso será negado"

- [ ] Há promessas BOM DEMAIS para ser verdade?
  - "Você ganhou R$ 10.000!"
  - "Prêmio acumulado para você"
  - "Oferta exclusiva que expira hoje"

- [ ] Há ERROS gramaticais ou de tradução?
  - Português estranho ou robótico
  - Mistura de idiomas
  - Erros de concordância óbvios

- [ ] A saudação é GENÉRICA?
  - ❌ "Prezado cliente"
  - ❌ "Olá usuário"
  - ✅ "Olá, [Seu Nome Completo]"

- [ ] Pede informações SENSÍVEIS por email?
  - Senha, PIN, CVV de cartão
  - Número completo de conta
  - CPF ou documentos

**Pontuação até aqui:**
- 0-1 itens: Baixo risco
- 2-3 itens: MÉDIO risco, verifique links
- 4+ itens: ALTO risco de phishing

---

## 🔗 FASE 3: VERIFICAÇÃO DE LINKS

### ✅ Inspeção Visual (SEM CLICAR!)

- [ ] Passe o mouse sobre o link - o destino real é diferente do texto?
  - Texto: "www.bancobrasil.com.br"
  - Real: "www.bancobrasii.com" (dois ii's)

- [ ] O link usa ENCURTADOR de URL?
  - bit.ly, tinyurl, ow.ly = Impossível verificar destino

- [ ] O domínio tem caracteres ESTRANHOS?
  - Letras trocadas: g00gle.com (zeros)
  - Caracteres cirílicos: раypal.com (р russo)
  - Hífens extras: face-book.com

- [ ] O link tem MÚLTIPLOS subdomínios suspeitos?
  - ❌ `paypal.login.verify.phishing.com` (domínio real é .com)
  - ✅ `login.paypal.com` (subdomínio de paypal.com)

### ✅ Análise Técnica (se possível)

- [ ] Copie o link e verifique em URLScan.io ou VirusTotal
- [ ] O domínio foi registrado há menos de 30 dias?
- [ ] O servidor está em país de alto risco?
- [ ] Há redirecionamentos múltiplos (3+)?

**Pontuação até aqui:**
- 0 itens: Link provavelmente seguro
- 1-2 itens: Não clique, verifique por outro canal
- 3+ itens: Definitivamente malicioso

---

## 📎 FASE 4: ANÁLISE DE ANEXOS

### ✅ Verificação Básica

- [ ] Você ESPERAVA receber este anexo?
- [ ] O remetente normalmente envia arquivos?
- [ ] O nome do arquivo é genérico ou suspeito?
  - "Fatura_12345.exe" (executável!)
  - "Documento.scr" (screensaver = vírus)
  - "Comprovante.zip" (com senha!)

### ✅ Extensões Perigosas

**NUNCA abra anexos com estas extensões:**
- [ ] .exe, .scr, .bat, .cmd (executáveis Windows)
- [ ] .js, .vbs, .ps1 (scripts)
- [ ] .com, .pif (programas antigos)

**Cuidado com estas extensões:**
- [ ] .zip, .rar com senha (esconde conteúdo)
- [ ] .docm, .xlsm, .pptm (Office com macros)
- [ ] .html, .htm (pode ter JavaScript malicioso)

**Geralmente seguras (mas ainda verifique!):**
- [ ] .pdf (sem solicitação de senha)
- [ ] .jpg, .png (imagens puras)
- [ ] .txt (texto puro)

**Pontuação:**
- Anexo com extensão perigosa = PHISHING
- Nome genérico + não esperado = SUSPEITO
- PDF/imagem esperado = Provavelmente OK

---

## 🚨 PONTUAÇÃO FINAL

Some todos os itens marcados nas 4 fases:

### 📊 Interpretação

| Pontos | Classificação | Ação |
|--------|---------------|------|
| 0-3 | **Baixo Risco** | Provavelmente legítimo, mas mantenha cautela |
| 4-7 | **Médio Risco** | Verificar por outro canal antes de interagir |
| 8-12 | **Alto Risco** | Muito provável phishing, NÃO interaja |
| 13+ | **PHISHING CONFIRMADO** | Reporte imediatamente, delete |

---

## ⚡ O QUE FAZER SE FOR PHISHING

### 🔴 NÃO FAÇA:
- ❌ Não clique em nenhum link
- ❌ Não baixe anexos
- ❌ Não responda ao email
- ❌ Não forneça informações
- ❌ Não delete imediatamente (preserve evidência)

### 🟢 FAÇA:
1. **Marque como spam/phishing** no seu cliente de email
2. **Reporte ao setor de TI/Segurança** se for email corporativo
3. **Reporte à empresa verdadeira** (se for tentativa de spoofing)
4. **Delete após reportar**

---

## 🆘 SE VOCÊ JÁ CLICOU

### ⏱️ AÇÃO IMEDIATA (0-5 minutos)

1. **DESCONECTE da rede/WiFi** imediatamente
2. **NÃO faça login** em nenhum site
3. **Tire foto/screenshot** do email (evidência)

### 🔐 AÇÃO URGENTE (5-30 minutos)

4. **Troque TODAS as senhas** de outro dispositivo:
   - Email
   - Banco
   - Redes sociais
   - Trabalho
   - Qualquer site que use a mesma senha

5. **Ative autenticação de dois fatores (2FA)** onde possível

6. **Verifique logins recentes** em:
   - Gmail: https://myaccount.google.com/device-activity
   - Facebook: Configurações → Segurança → Sessões ativas
   - Banco: histórico de acessos

### 📞 REPORTE (dentro de 1 hora)

7. **Contate seu banco** se forneceu dados financeiros
8. **Reporte ao TI/Segurança** da empresa
9. **Faça boletim de ocorrência** se houver perda financeira

### 🔍 MONITORAMENTO (dias seguintes)

10. **Scan completo** de antivírus/antimalware
11. **Monitore extratos bancários** por 30 dias
12. **Fique alerta** para novas tentativas

---

## 📞 CONTATOS ÚTEIS

### Brasil
- **CERT.br:** https://www.cert.br/
- **Polícia Federal:** https://www.gov.br/pf/
- **Procon:** 151

### Reportar Phishing
- **Google Safe Browsing:** https://safebrowsing.google.com/safebrowsing/report_phish/
- **PhishTank:** https://www.phishtank.com/
- **APWG:** reportphishing@apwg.org

---

## 📚 Recursos Adicionais

- **[PhishTank.org](https://phishtank.org)** - Verificar URLs suspeitas
- **[URLScan.io](https://urlscan.io)** - Analisar links
- **[VirusTotal](https://virustotal.com)** - Verificar arquivos/URLs
- **[Have I Been Pwned](https://haveibeenpwned.com)** - Ver se seu email vazou

---

## ✍️ Notas Pessoais

Use este espaço para anotar phishings específicos que você encontrou:

```
Data: ___/___/___
Tipo: _______________
Remetente: _______________
Tema: _______________
O que te alertou: _______________
```

---

**🔄 Versão:** 1.0  
**📅 Última atualização:** Dezembro 2025  
**✅ Validado por:** SOC Team

---

⚡ **LEMBRE-SE:** Na dúvida, SEMPRE verifique por outro canal (telefone, presencialmente, site oficial digitado manualmente no navegador). Phishing explora a pressa e o medo - pause, respire, e verifique!
