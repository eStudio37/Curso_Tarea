---
name: extrair-competencias
description: Extrai competências de uma vaga (JD) e mapeia evidências contra um currículo. Produz JSON estruturado com competências, níveis, pesos, match scores e gaps. Use quando precisar avaliar fit técnico de candidato a uma vaga.
allowed-tools: Read, Write
effort: medium
---

# extrair-competencias

> 💡 **Time A (Extrator)** — esse skill é o INÍCIO da pipeline. Output dele alimenta o Time B (`gerar-perguntas-comportamentais`).

## O que esse skill faz

Recebe dois documentos não-estruturados (Job Description + Currículo do candidato) e produz um JSON estruturado contendo:

- 5 a 7 competências críticas extraídas da JD
- Para cada competência: nível requerido + peso (essencial/importante/nice)
- Mapeamento de evidências do CV para cada competência
- Match score 0-100 por competência (com heurística declarada)
- Lista de gaps: competências essenciais sem evidência

## Quando acionar

Acione quando o usuário fornecer:
- Uma JD (em qualquer formato: texto, markdown, PDF colado)
- Um currículo do candidato (idem)

Pode disparar com:
- "Analise o fit desse candidato pra essa vaga"
- "Extraia as competências dessa vaga e mapeia contra o CV"
- "Estou montando uma entrevista, quais são os pontos fortes e gaps?"

## Workflow (a preencher pelo Time A)

> Aqui o time descreve, em texto comum em português, o passo-a-passo que a skill segue.
>
> Sugestão de etapas (vocês podem ajustar):

1. Ler a JD por completo, identificar a seção de competências/responsabilidades
2. Extrair de 5 a 7 competências críticas — combinar essenciais (que aparecem como "must-have") + as 1-2 importantes mais relevantes
3. Para cada competência, classificar:
   - Nível requerido: júnior / pleno / sênior
   - Peso: essencial / importante / nice
4. Ler o currículo por completo
5. Para cada competência da JD, procurar evidência no CV — citação direta da experiência (cargo + ação/conquista)
6. Calcular `match_score` (0-100) com heurística:
   - _[a equipe define a regra. Sugestão: Sem evidência = 20. Evidência fraca/relacionada = 40-60. Evidência sólida = 70-85. Evidência abundante e em nível superior = 90-100.]_
7. Identificar gaps: competências com peso "essencial" e match_score < 50
8. Calcular match geral (média ponderada pelos pesos)
9. Emitir JSON conforme contrato abaixo

## Contrato de saída (JSON)

> ⚠ **Não alterem essa estrutura.** O Time B vai consumir esse JSON exatamente assim. Mudanças aqui quebram a pipeline.

```json
{
  "vaga": "Nome do cargo extraído da JD",
  "candidato": "Nome do candidato extraído do CV",
  "competencias": [
    {
      "nome": "Nome da competência",
      "nivel_requerido": "junior | pleno | senior",
      "peso": "essencial | importante | nice",
      "match_score": 0,
      "evidencia_cv": "Citação direta do CV ou null se não houver",
      "gap": "Descrição do gap ou null se não for gap"
    }
  ],
  "match_geral": 0,
  "gaps_criticos": ["Competência X", "Competência Y"]
}
```

## Exemplo de saída esperada (para validação)

```json
{
  "vaga": "Gerente de Operações Logísticas",
  "candidato": "Roberta Almeida Souza",
  "competencias": [
    {
      "nome": "Liderança de equipes operacionais multi-unidade",
      "nivel_requerido": "senior",
      "peso": "essencial",
      "match_score": 78,
      "evidencia_cv": "Liderou 2 filiais combinadas (95 colaboradores) na TBL desde 2022. Reestruturou Guarulhos em 8 meses.",
      "gap": null
    },
    {
      "nome": "Compliance ANTT",
      "nivel_requerido": "pleno",
      "peso": "essencial",
      "match_score": 25,
      "evidencia_cv": null,
      "gap": "Sem menção a Resoluções ANTT (5.998/22, 6.034/25), telemetria classe 3+, ou auditoria interna de jornada"
    }
  ],
  "match_geral": 64,
  "gaps_criticos": ["Compliance ANTT"]
}
```

## Princípios não-negociáveis

1. **Nada inventado.** Se uma evidência não está literalmente no CV, é null. Não interpretar, não estender, não inferir.
2. **Citação literal.** A `evidencia_cv` deve ser uma frase ou cargo+conquista que aparece no CV original. Sem paráfrase.
3. **Heurística declarada.** Se vocês adotarem uma regra para `match_score`, expliquem essa regra em comentário no início do JSON ou em uma nota separada.
4. **JSON válido.** Output deve ser parseável. Sem texto antes ou depois do bloco JSON.

## Como testar antes de entregar para o Time B

1. Rodem o skill com a JD e o CV de exemplo (`vaga-gerente-operacoes.md` + `cv-candidato-finalista.md`).
2. Verifiquem que o JSON sai com 5-7 competências.
3. Verifiquem que match_score é coerente — competências com evidência forte devem ter score alto.
4. Validem o JSON em https://jsonlint.com (ou similar).
5. Compartilhem o JSON com o Time B verbalmente: "esse é o que vocês vão receber."

## Falhas conhecidas e como recuperar

- **Competência ambígua na JD:** se a JD lista algo vago tipo "boa comunicação", não inclua. Foque em competências verificáveis com evidência observável.
- **Múltiplas evidências para uma competência:** escolha a mais forte (a mais recente OU a com maior impacto numérico). Não cite duas.
- **CV menciona algo que a JD não pede:** ignore. Vocês só extraem o que a JD pede.
