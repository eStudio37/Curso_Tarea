---
name: montar-pacote-entrevista
description: Monta o pacote de entrevista final em HTML imprimível (A4) a partir do JSON de perguntas. Inclui capa, perfil sintético do candidato, plano da conversa, cards de pergunta com rubrica, área de anotação livre e bloco de decisão. Use quando precisar gerar o material final que o entrevistador leva impresso para a sala.
allowed-tools: Read, Write
effort: high
---

# montar-pacote-entrevista

> 💡 **Time C (Montador)** — esse skill é o FIM da pipeline. Recebe o JSON do Time B (`gerar-perguntas-comportamentais`) e produz o HTML final que o usuário imprime ou abre no navegador.

## O que esse skill faz

Recebe o JSON enriquecido do Time B (perguntas + rubricas + case + tempos) e produz **um arquivo HTML único, autocontido, imprimível em A4**, contendo:

- **Capa**: vaga, candidato, entrevistador, data, marca SmartOps
- **Perfil sintético do candidato**: 3 frases factuais (vai precisar receber o CV também, ou um resumo)
- **Plano da conversa**: timeline visual dos 60 minutos com blocos
- **Cards de pergunta**: para cada pergunta do JSON do Time B — texto, tipo (STAR/probing), rubrica 1-3-5 visível, espaço pautado para anotação manual
- **Case scenario**: card destacado com tratamento visual distinto
- **Bloco de decisão final**: 3 radios (Avançar / Reservas / Não avançar) com espaço para justificativa

O HTML deve ser **imprimível direto sem ajustes**: 4 páginas A4, margens corretas, tipografia legível em papel.

## Quando acionar

Acione quando o usuário fornecer o JSON conforme o contrato de saída do `gerar-perguntas-comportamentais`. Pode disparar com:

- "Monte o pacote final pra impressão"
- "Gera o HTML da entrevista"
- "Imprime esse roteiro pra eu levar amanhã"

## Workflow (a preencher pelo Time C)

> Aqui o time descreve, em texto comum em português, o passo-a-passo que a skill segue.
>
> Sugestão de etapas (vocês podem ajustar):

1. Ler o JSON de entrada do Time B e validar campos mínimos:
   - `vaga`, `candidato`, `perguntas[]`, `case_scenario`, `tempo_total_estimado_min`
2. Solicitar (ou ler) os dados de contexto que o JSON não traz:
   - Nome do entrevistador
   - Data e horário da entrevista
   - Resumo do CV (3 frases) para o perfil sintético — _se o CV original estiver acessível, leiam direto. Caso contrário, peçam ao usuário._
3. Construir o esqueleto HTML com:
   - `<style>` inline com tokens da marca SmartOps (`--so-orange #FEBA12`, Ubuntu, gray scale)
   - Tema claro (fundo branco, A4 imprimível) — referência: `relatorio-360/templates/conselho-skeleton.html`
   - `@page { size: A4; margin: 18mm; }` para impressão correta
   - `@media print` com `page-break-inside: avoid` nos cards de pergunta
4. Renderizar a **capa** (página 1):
   - Marca SmartOps no topo
   - Título grande: "Pacote de Entrevista — [Vaga]"
   - Bloco de metadados: candidato, entrevistador, data, duração
5. Renderizar **perfil sintético** + **plano da conversa** (mesma página da capa, abaixo):
   - Perfil: 3 frases neutras
   - Timeline: barras horizontais com tempos (5 min abertura, X min STAR, Y min probing, Z min case, 5 min fechamento)
6. Para cada pergunta no array `perguntas[]`, renderizar um **card** (cada pergunta começa em página nova ou bloco com `page-break-inside: avoid`):
   - Header: número da pergunta + tipo (STAR | probing) + tempo estimado
   - Texto da pergunta em destaque
   - Caixa de rubrica com as 3 âncoras (1, 3, 5) lado a lado ou empilhadas
   - **8 a 12 linhas pautadas** para anotação manual (use `border-bottom: 1px solid` em divs vazios)
   - Marcador "competência alvo" no rodapé do card
7. Renderizar o **case scenario** com tratamento visual distinto (borda mais grossa, ou cor de destaque) — mesmo formato de card, mas marcado claramente como CASE
8. Renderizar o **bloco de decisão final** (última página):
   - 3 `<input type="radio" name="decisao">` com labels: "Avançar para próxima fase", "Avançar com reservas (justificar)", "Não avançar"
   - Espaço pautado (8 linhas) para justificativa
   - CSS `:has(input:checked)` para destacar a opção selecionada (vai funcionar no browser; no papel é só um círculo pra marcar com caneta)
9. Salvar HTML no caminho que o usuário pediu (default: `pacote-entrevista-[candidato-slug].html`)

## Contrato de entrada (consumindo do Time B)

> Time C deve aceitar exatamente o JSON que o Time B emite. Não inventem campos novos.

```json
{
  "vaga": "string",
  "candidato": "string",
  "perguntas": [
    {
      "competencia_alvo": "string",
      "tipo": "STAR | probing",
      "pergunta": "string",
      "rubrica": { "1": "string", "3": "string", "5": "string" },
      "tempo_estimado_min": 0
    }
  ],
  "case_scenario": {
    "pergunta": "string",
    "rubrica": { "1": "string", "3": "string", "5": "string" },
    "tempo_estimado_min": 0
  },
  "tempo_total_estimado_min": 0
}
```

## Inputs adicionais (não vêm do JSON do Time B)

A skill **deve perguntar ao usuário** ou ler de arquivo:

- `entrevistador`: nome de quem vai conduzir (ex: "Carlos Mendes")
- `data_entrevista`: data e hora (ex: "29/04/2026 14h00")
- `perfil_sintetico`: 3 frases sobre o candidato — se o CV original estiver no contexto, podem gerar; se não, pedir.

## Contrato de saída (HTML)

Não há JSON de saída. A saída é o **arquivo HTML salvo** + uma confirmação no chat informando o caminho.

Estrutura esperada do HTML:

```
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8">
    <title>Pacote de Entrevista — [Vaga] — [Candidato]</title>
    <style>/* tokens SmartOps + light theme + A4 print */</style>
  </head>
  <body>
    <main class="pacote">
      <section class="capa">...</section>
      <section class="perfil">...</section>
      <section class="plano">...</section>
      <section class="perguntas">
        <article class="card-pergunta">...</article>  <!-- repetido N vezes -->
      </section>
      <section class="case">...</section>
      <section class="decisao">...</section>
    </main>
  </body>
</html>
```

## Referências visuais (obrigatórias)

1. **Brand tokens**: `relatorio-360/references/brand-smartops.md` — Ubuntu, --so-orange #FEBA12, gray scale.
2. **Light theme + A4**: `relatorio-360/templates/conselho-skeleton.html` — fundo branco, tema claro, padrão de header.
3. **Exemplar de saída**: `target/exemplo-pacote-entrevista.html` — esse é o "good looks like". Copiem o pattern, não inventem layout.

## Princípios não-negociáveis

1. **Imprimível sem ajuste.** Se o entrevistador imprimir o HTML e tiver que cortar conteúdo na régua, vocês falharam. Testem com Ctrl+P → Salvar como PDF antes de entregar.
2. **Rubrica visível ao lado da pergunta.** O entrevistador precisa ler a pergunta e a rubrica no mesmo plano de visão. Não escondam rubrica em rodapé.
3. **Espaço real pra escrever.** Linhas pautadas devem caber caneta humana — mínimo 8mm entre linhas. Não economizem espaço.
4. **Page breaks limpos.** Card de pergunta não pode ser cortado pela metade pela impressora. Usem `page-break-inside: avoid`.
5. **HTML válido e autocontido.** Sem CDN externo, sem fonte do Google quebrando offline. Tudo inline.
6. **Tema claro só.** Não mandem tema escuro pro Time C. A entrega final é papel.

## Como testar antes de entregar

1. Rodem o skill com o JSON de saída do Time B (real ou fallback `handoff-B-to-C.json`).
2. Abram o HTML gerado no Chrome/Firefox.
3. Ctrl+P → veja a pré-visualização de impressão. **Tem que dar 4 páginas A4 limpas, sem cortes feios.**
4. Salvem como PDF. Imprima 1 cópia se possível.
5. Tentem preencher uma resposta a caneta — couberam 8 linhas mesmo? A rubrica está legível?
6. Validem que o nome do candidato, vaga, data e entrevistador estão corretos em **TODAS** as páginas (header repete?).

## Falhas conhecidas e como recuperar

- **Texto da pergunta cortado pela margem direita:** verifiquem `overflow-wrap: break-word` no card.
- **Rubrica gigante estourando o card:** se as âncoras passam de 30 palavras cada, peçam ao Time B pra encurtar (não compensem com fonte 8pt).
- **Mais de 4 páginas:** se o JSON tem 8+ perguntas, o pacote naturalmente cresce. Acima de 5 páginas, vocês podem comprimir o plano da conversa pra meia-página, mas não comprometam o card de pergunta.
- **JSON inválido entrando:** se o Time B mandar JSON quebrado, NÃO inventem campos. Devolvam erro pro usuário com qual campo está faltando. Há um fallback em `fallbacks/handoff-B-to-C.json` pra usar como base se a pipeline travar.
