# Hands-On 2 — Trabalhando com segurança

> **20 minutos · pesquisa multi-agente com verificação por intersecção.** Cada attendee roda DUAS versões do mesmo problema — uma single-agent, outra multi-agent — e compara.

---

## Tópico da pesquisa

**_Quais são os riscos jurídicos, operacionais e de mercado de usar IA agêntica para triagem de candidatos no processo de recrutamento, no Brasil em 2026?_**

> Tópico escolhido porque tem **respostas legitimamente diferentes** nos 3 ângulos (jurídico ≠ mercado ≠ academia). Sem isso, o exercício de intersecção fica artificial.

---

## Round 1 — Single-agent baseline (5 min)

Cole o prompt abaixo no Claude. Execute. Leia o output. Anote: **quantas afirmações você duvidaria?**

```
Quero entender os riscos de usar IA agêntica para triagem de candidatos
no processo de recrutamento no Brasil em 2026.
Produza um relatório de uma página. Cite fontes onde possível.
```

**Anote:** o que ele afirma com confiança? Onde você não saberia validar?

---

## Round 2 — Multi-agente com intersecção (10 min)

Cole o prompt abaixo. Execute. Compare com o output do Round 1.

```
Quero uma pesquisa estruturada sobre os riscos de usar IA agêntica para
triagem de candidatos no Brasil em 2026.

Dispare TRÊS sub-agentes em paralelo, cada um com uma lente diferente:

AGENTE 1 — JURÍDICO
Investigue: LGPD aplicada à decisão automatizada (Art. 20), Marco
Regulatório de IA brasileiro (status 2026), decisões TST sobre
discriminação algorítmica, jurisprudência sobre direito à explicação.

AGENTE 2 — MERCADO
Investigue: empresas brasileiras que adotaram IA em recrutamento (cases
Magazine Luiza, Itaú, Ambev, etc.), métricas de adoção, falhas públicas
(processos, recall de produto), o que vendors (Gupy, Kenoby) estão
oferecendo e como se protegem.

AGENTE 3 — ACADEMIA
Investigue: estudos publicados (USP, FGV, Harvard, MIT) sobre viés
algorítmico em seleção, métricas de fairness (demographic parity,
equalized odds), validação preditiva de scoring de candidatos, debate
sobre "AI explainability" em HR.

Depois que os três terminarem:
- Compile uma tabela de RISCOS, marcando quais foram apontados por:
  apenas 1 agente / 2 agentes / 3 agentes (intersecção plena).
- Para cada risco no quadrante "3 agentes": tratar como ALTA CONFIANÇA.
- Para riscos com apenas 1 agente: marcar como "VERIFICAR — fonte única".
- Indicar onde os três discordam (divergência ativa) — esse é o sinal
  mais valioso.
```

---

## Debrief — 5 min

Bruno pergunta à sala:

1. **Quais riscos apareceram nos 3 agentes?** _(intersecção = confiança alta)_
2. **Quais riscos só 1 agente apontou?** _(unique signal = verificar manualmente)_
3. **Onde os 3 agentes discordaram ativamente?** _(divergência = onde está a complexidade real)_
4. **A versão multi-agente mudou alguma decisão que você teria tomado com a versão single?**

---

### Por que esse tópico funciona

- **3 ângulos legitimamente diferentes em 2026:** jurídico está cauteloso, mercado está otimista, academia está cético. A intersecção é genuinamente reveladora.
- **Universalmente acionável:** todo C-suite na sala vai contratar nos próximos 12 meses.
- **Não compete com o capstone:** Hands-On 3 e capstone usam Atlântica (logística). Esse aqui usa RH e fecha o thread iniciado pelo `/comp-analysis` da manhã.
