# Hands-On 3 — Trabalhando com qualidade

> **30 minutos · briefing executivo da Atlântica, agora com ISC + multi-agente aplicados.** Cada attendee gera o briefing usando os critérios coletivos definidos no bloco anterior. Segunda passada com ângulo diferente. Compara intersecção (confiança) vs divergência (verificar).

---

## Setup (1 min)

Cada attendee precisa ter no seu CoWork:
- Pasta `synthetic-docs/` da Atlântica (8 documentos) — Manos compartilha link/AirDrop antes do bloco.
- Lista dos critérios do **ISC coletivo** que a sala produziu há 10 minutos.

---

## Round 1 — Briefing com ISC explícito (10 min)

Cole o prompt abaixo. **Substitua `[CRITÉRIOS DO ISC]`** pelos 5-7 critérios que a sala definiu.

```
Você é assistente executivo do CEO da Atlântica Logística. Hoje à tarde
ele tem reunião com o Conselho. Preciso de um BRIEFING DE UMA PÁGINA
para ele levar.

INSUMOS — leia os 8 documentos na pasta da Atlântica que vou anexar:
- 01-ata-conselho-q1-2026.md
- 02-sumario-financeiro-q1.md
- 03-comunicado-rh-politicas.md
- 04-pesquisa-mercado-logistica.md
- 05-memorando-manutencao-frota.md
- 06-estrategia-2030.md
- 07-fornecedores-mestres.csv
- 08-politica-compliance-fornecedores.md

CRITÉRIOS DE SUCESSO (ISC) — definidos pela sala:
[CRITÉRIOS DO ISC]

(exemplo de critérios que a sala provavelmente definiu:
 - Cabe em uma página A4
 - Linguagem direta, sem jargão
 - Cita números específicos com fonte (qual documento, qual página)
 - Identifica os 3 riscos mais altos do trimestre, ranqueados
 - Indica explicitamente onde existe divergência entre fontes
 - Termina com 2-3 ações recomendadas, com responsável sugerido
 - Tom executivo: factual, não defensivo)

Produza o briefing satisfazendo TODOS os critérios acima. Se algum
critério não puder ser atendido com os dados disponíveis, sinalize
explicitamente em vez de inventar.
```

**Anote enquanto lê:** ele atendeu todos os critérios? Onde fugiu?

---

## Round 2 — Verificação por ângulo diferente (10 min)

Cole o prompt abaixo. Mesmo problema, ângulo diferente.

```
Mesmo conjunto de 8 documentos da Atlântica.

Agora dispare TRÊS sub-agentes em paralelo:

AGENTE 1 — VISÃO FINANCEIRA
Identifique os 3 maiores riscos FINANCEIROS do trimestre, com cifras.
Foque em: P&L, capital de giro, exposição a fornecedores.

AGENTE 2 — VISÃO OPERACIONAL
Identifique os 3 maiores riscos OPERACIONAIS do trimestre.
Foque em: OTD, sinistralidade, manutenção de frota, turnover.

AGENTE 3 — VISÃO REGULATÓRIA / COMPLIANCE
Identifique os 3 maiores riscos REGULATÓRIOS do trimestre.
Foque em: ANTT, LGPD, compliance de fornecedores, ESG.

Depois que os três terminarem:
- Compile a tabela de riscos.
- Marque os riscos que aparecem em 2+ agentes (intersecção = alta
  confiança — esses VÃO no briefing).
- Marque os riscos com apenas 1 agente (sinal único = mencionar como
  "atenção" no briefing).
- Onde os 3 discordam ativamente: esse é o ponto que precisa de mais
  investigação humana.

Produza um briefing executivo de uma página seguindo os MESMOS critérios
do round anterior.
```

---

## Debrief (9 min)

Manos pergunta à sala:

1. **Quais riscos apareceram em ambos os rounds?** _(estabilidade = alta confiança)_
2. **Onde Round 1 e Round 2 discordaram?** _(divergência entre métodos = sinal de cuidado)_
3. **O ISC explícito mudou a qualidade do output?** _(spoiler: muito)_
4. **Qual versão você levaria pro CEO?** _(provavelmente a multi-agente — porque mostra de onde a confiança vem)_

---

## O que esse hands-on ensina (Manos sintetiza no fim)

| Habilidade | Como o exercício treinou |
|---|---|
| **Aplicação de ISC** | Critérios explícitos no prompt mudaram o output radicalmente. |
| **Pesquisa multi-agente** | 3 ângulos sobre o mesmo material produziram coberturas diferentes. |
| **Verificação por intersecção** | Riscos em 2+ ângulos = alta confiança; em 1 ângulo = verificar. |
| **Sinal vs ruído** | Divergência ativa entre agentes não é problema — é informação. |

---

### Bônus — quem terminar antes

> _"Repita o Round 2, mas troque o Agente 3 por: VISÃO ESTRATÉGICA — riscos para o plano 2030. Compare com o Round 2 anterior. O que mudou na intersecção?"_
