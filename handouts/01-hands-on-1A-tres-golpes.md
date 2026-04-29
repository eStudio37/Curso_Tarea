# Hands-On 1A — Os três golpes

> **30 minutos · 3 exercícios de 5 minutos cada + setup.** O que CoWork faz que ChatGPT não faz.

---

## Setup (4 min) — todos juntos

1. No Claude app: **Settings → Connectors**.
2. Autorizar **Google Workspace** (Gmail + Calendar + Drive).
3. Confirmar com Bruno que conectou. _Sem isso, exercício 1 trava._

> ⚠ Se a auth do Google falhar: pule o exercício 1 e vá direto pro 2.

---

## Exercício 1 — CONECTOR (5 min)

### Prompt principal
```
Olhe meus 20 últimos e-mails. Liste os 3 que mais provavelmente exigem
minha resposta nas próximas 24h, e por quê — em uma linha cada.
```

**Wow do exercício:** ele leu **MEUS dados reais**. ChatGPT nunca poderia.

### Variantes (escolha uma se preferir outro ângulo)
- _"Olhe minha agenda dos próximos 3 dias e me diga em uma frase qual reunião eu mais preciso preparar."_
- _"Liste os arquivos modificados no meu Drive nas últimas 48h e agrupe por projeto."_

---

## Exercício 2 — SKILL (5 min)

### Prompt default (todos)
```
/comp-analysis Quanto devemos pagar um engenheiro de software júnior em
São Paulo, primeira contratação tech da empresa?
```

**Wow do exercício:** skill **OFICIAL Anthropic + Partners** (bundle "Human resources" no Marketplace in-app, 4.0★). Comando salvo, procedimento estruturado, output em <2min. ChatGPT não tem skill — exige prompt do zero toda vez.

### Variantes — Bruno abre 1 minuto pra escolha por cadeira

#### 📊 RH (bundle "Human resources")
| Comando | O que faz |
|---|---|
| `/comp-analysis` | banda salarial + equity + benchmark de mercado |
| `/interview-prep` | plano estruturado de entrevistas com scorecards |
| `/draft-offer` | carta de oferta com base + bônus + signing |
| `/onboarding-plan` | plano de primeira semana |
| `/people-report` | snapshot de attrition / diversidade / saúde do time |

#### 💰 FINANCE (bundle "Finance")
| Comando | O que faz |
|---|---|
| `/financial-statements` | P&L Q1 2026 com variance analysis vs Q1/25 |
| `/variance-analysis` | drivers do que está fora do orçamento, ranqueados |
| `/journal-entry` | lançamentos de fim de mês (accruals, prepaid) |
| `/reconciliation` | GL vs sub-ledger / banco / terceiros |
| `/close-management` | calendário de fechamento mensal com dependências |

> ⚠ **Evitar `/audit-support`** — é SOX-específico (US), não ressoa com Brasil.

**Pedagogia:** o exercício é o MESMO independente de qual sub-skill — o ponto é que CoWork tem **comandos pré-engenheirados para domínios profissionais**.

---

## Exercício 3 — COMPUTER USE (4 min)

### Prompt principal
```
Tire um screenshot da minha tela e me diga qual janela aberta tem maior
chance de me distrair na próxima hora.
```

**Wow do exercício:** ele **VÊ o que estou vendo**. Reasoning sobre realidade visual.

### Backup (se OS permission falhou)
```
Liste os 5 PDFs mais recentes na minha pasta Downloads e me diga em uma
linha do que cada um trata.
```
_(filesystem read, não exige Accessibility permission)_

### Variante mais ousada
```
Abra meu Calendar no navegador, encontre minha próxima reunião com [nome], e drafte (sem enviar) a pauta dela em um e-mail novo.
```

---

## Fechamento (2 min)

Bruno pergunta à sala: _"Qual dos três te chocou mais?"_ — colhe 3-4 reações antes do almoço.

---

### Folha de troubleshooting (lateral pro Bruno circular)

| Sintoma | Provável causa | Atalho |
|---|---|---|
| "Connector não autoriza" | bloqueio de pop-up | Liberar pop-up no browser, retentar |
| "Não encontro Settings → Connectors" | versão app desatualizada | Atualizar app, reabrir |
| "Skill `/comp-analysis` não aparece" | bundle não instalado | Marketplace → Human resources → Install |
| "Computer use diz permission denied" | macOS Accessibility / Windows UAC | Pular pro Backup #3 (filesystem) |
| "Skill pede contexto adicional" | empresa/setor faltando | Attendee fornece "Brasil, 2026" e segue |
