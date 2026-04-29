# Divisão de Times — "Assumindo o Controle"

> Conteúdo para slide + script falado. Como você divide ~25 executivos em 3 times de forma balanceada.

---

## Princípio de divisão

**3 times de 8 pessoas (~24-25 participantes).** Cada time pega UMA skill da pipeline:

| Time | Skill | Papel |
|---|---|---|
| **Time A — Extratores** | `extrair-competencias` | Lê JD + CV, produz JSON estruturado de competências |
| **Time B — Enriquecedores** | `gerar-perguntas-comportamentais` | Recebe JSON, gera perguntas STAR + rubricas |
| **Time C — Montadores** | `montar-pacote-entrevista` | Recebe JSON do B, monta HTML final imprimível |

**Ninguém conhece o output da skill anterior em detalhe — só conhece o CONTRATO.** Isso é proposital. É como funciona qualquer pipeline real: vocês concordam no formato de handoff e cada time entrega o seu pedaço.

---

## Como balancear (regra prática)

### Evitar concentração de função

> ❌ NÃO coloque 3 RHs no mesmo time.
> ❌ NÃO coloque todos os 3 advogados/compliance no mesmo time.
> ❌ NÃO coloque 4 fundadores/CEOs no mesmo time (eles vão dominar e o time vira "palco do fundador").

**Por quê:** se um time inteiro é da mesma função, eles vão escrever a skill com a lente daquela função, não com lente de pipeline. O Time B com 3 RHs vai escrever perguntas de RH em vez de perguntas pro contrato de skill.

### Distribuir senioridade

Cada time precisa ter **pelo menos 1 sênior (CEO/diretor)** e **pelo menos 1 mais hands-on (gerente/coordenador/IC)**. Mistura ajuda — sênior contextualiza, hands-on aterra na execução.

### Distribuir afinidade com tech

Identifique os "early adopters" (pessoas que já mexem com Claude/ChatGPT no dia a dia) e **espalhe nos 3 times** — eles vão acelerar a leitura do template e desbloquear quem não conhece.

---

## Sugestão de fluxo na sala (3-5 min de divisão)

1. **Anuncie o exercício** (slide com a pipeline visual)
2. **Mostre a regra de balanceamento** acima
3. **Pergunte:** "Quem aqui já trabalhou em RH ou já contratou alguém em sua empresa?"
   - Levante de mãos. Distribua essas pessoas em 1 por time.
4. **Pergunte:** "Quem aqui já mexeu em Claude ou ChatGPT além de pedir resumo?"
   - Levante de mãos. Distribua em 1 por time.
5. **Conte 1-2-3 nas mesas restantes.** 1 vai pro Time A, 2 pro Time B, 3 pro Time C.
6. **Time A senta numa ponta, Time C na outra ponta** (eles vão trabalhar em sequência, não em paralelo — mas senta separado pra evitar "espiar a próxima fase").

---

## Materiais que cada time recebe

### Time A (Extratores)
- ✅ `inputs/vaga-gerente-operacoes.md` (JD)
- ✅ `inputs/cv-candidato-finalista.md` (CV)
- ✅ `templates/extrair-competencias-template.md` (esqueleto da skill)
- ✅ `cheatsheet-anatomia-skill.md`

### Time B (Enriquecedores)
- ✅ `templates/gerar-perguntas-comportamentais-template.md`
- ✅ `cheatsheet-anatomia-skill.md`
- 🔁 (no fim, recebem o JSON do Time A — ou usam `fallbacks/handoff-A-to-B.json` se Time A travar)

### Time C (Montadores)
- ✅ `templates/montar-pacote-entrevista-template.md`
- ✅ `cheatsheet-anatomia-skill.md`
- ✅ `target/exemplo-pacote-entrevista.html` (o "good looks like")
- 🔁 (no fim, recebem o JSON do Time B — ou usam `fallbacks/handoff-B-to-C.json` se Time B travar)

---

## Tempo

- **0–3 min:** divisão de times + entrega de materiais
- **3–35 min:** cada time preenche seu template (workflow + contrato + princípios)
- **35–45 min:** cada time roda sua skill com input real (passa output adiante)
- **45–55 min:** Time C apresenta o pacote final
- **55–60 min:** debrief — o que funcionou, o que quebrou, onde a pipeline real precisa de mais cuidado

---

## Sinais de problema durante o exercício

| Sintoma | Causa provável | Intervenção |
|---|---|---|
| Time todo está em silêncio aos 5 min | Não entenderam o template | Cass passa na mesa, lê em voz alta a primeira pergunta do template |
| Time discute função em vez de skill ("isso não é como RH faz") | Misturou demais função com pipeline | Aponte: "vocês estão escrevendo a skill, não defendendo a função" |
| Time A demora demais e Time B fica parado | Pipeline travou | Entregue ao Time B o `fallbacks/handoff-A-to-B.json` e siga |
| Time C está reescrevendo o exemplar | Não entendeu que o exemplar É a referência | "Copiem o pattern. O exercício é o JSON virar HTML, não inventar layout." |
| Alguém quer "fazer todas as 3 skills" | Excited, mas vai estourar tempo | "Hoje vocês fazem uma. A pipeline funciona porque cada um é especialista." |
