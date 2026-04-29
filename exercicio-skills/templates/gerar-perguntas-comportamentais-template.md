---
name: gerar-perguntas-comportamentais
description: Gera perguntas de entrevista comportamental (formato STAR) e de probing direto a partir de um JSON de competências mapeadas. Cria rubricas de pontuação 1-5 com âncoras descritas. Use quando precisar montar roteiro de entrevista a partir de scorecard.
allowed-tools: Read, Write
effort: medium
---

# gerar-perguntas-comportamentais

> 💡 **Time B (Enriquecedor)** — esse skill recebe o JSON do Time A (`extrair-competencias`) e produz um JSON enriquecido para o Time C (`montar-pacote-entrevista`).

## O que esse skill faz

Recebe o JSON estruturado do Time A (competências mapeadas + gaps) e produz um JSON enriquecido contendo:

- Para cada competência: 1 ou 2 perguntas comportamentais no formato **STAR** (Situação–Tarefa–Ação–Resultado)
- Para cada gap explícito: 1 pergunta de **probing direto** ("Como você lidaria com…")
- Para cada pergunta: rubrica de pontuação 1-5 com **3 âncoras descritas** (não números soltos)
- 1 pergunta de **case scenario** específica do papel
- Tempo estimado por pergunta e tempo total da entrevista

## Quando acionar

Acione quando o usuário fornecer um JSON conforme o contrato do `extrair-competencias`. Pode disparar com:

- "Gere as perguntas de entrevista para esse candidato"
- "Monte o roteiro comportamental"
- "Quais perguntas eu faço pra avaliar esse fit?"

## Workflow (a preencher pelo Time B)

> Aqui o time descreve, em texto comum em português, o passo-a-passo que a skill segue.
>
> Sugestão de etapas (vocês podem ajustar):

1. Ler o JSON de entrada e identificar:
   - Lista de competências (com peso e match_score)
   - Lista de gaps_criticos
2. Para cada competência:
   - Se peso = essencial OU match_score < 60: gerar **2 perguntas STAR** (uma sobre situação positiva, uma sobre desafio)
   - Caso contrário: gerar **1 pergunta STAR**
3. Para cada gap_critico (do JSON de entrada): gerar **1 pergunta de probing direto** que teste como o candidato lidaria com a situação onde a evidência é fraca
4. Para cada pergunta gerada, criar **rubrica 1-5 com 3 âncoras textuais**:
   - Âncora 1: o que caracteriza resposta vaga / inadequada
   - Âncora 3: o que caracteriza resposta sólida / aceitável
   - Âncora 5: o que caracteriza resposta excepcional / referência
5. Gerar **1 case scenario** baseado no contexto da vaga (ex: para Gerente de Operações Logísticas, um case sobre KPI fora da banda)
6. Estimar tempo por pergunta (STAR: 6-8 min, probing: 4-5 min, case: 8-10 min)
7. Calcular tempo total
8. Emitir JSON conforme contrato abaixo

## Contrato de saída (JSON)

> ⚠ **Não alterem essa estrutura.** O Time C vai consumir exatamente assim. Mudanças aqui quebram a pipeline.

```json
{
  "vaga": "Nome da vaga (passa adiante)",
  "candidato": "Nome do candidato (passa adiante)",
  "perguntas": [
    {
      "competencia_alvo": "Nome da competência (referência ao Time A)",
      "tipo": "STAR | probing",
      "pergunta": "Texto da pergunta em português",
      "rubrica": {
        "1": "Âncora descritiva — o que é uma resposta nível 1",
        "3": "Âncora descritiva — o que é uma resposta nível 3",
        "5": "Âncora descritiva — o que é uma resposta nível 5"
      },
      "tempo_estimado_min": 0
    }
  ],
  "case_scenario": {
    "pergunta": "Texto do case",
    "rubrica": {
      "1": "...",
      "3": "...",
      "5": "..."
    },
    "tempo_estimado_min": 0
  },
  "tempo_total_estimado_min": 0
}
```

## Exemplo de saída esperada (para validação)

```json
{
  "vaga": "Gerente de Operações Logísticas",
  "candidato": "Roberta Almeida Souza",
  "perguntas": [
    {
      "competencia_alvo": "Liderança de equipes operacionais multi-unidade",
      "tipo": "STAR",
      "pergunta": "Conte-me sobre uma vez em que você teve que liderar uma equipe operacional em um momento de crise (queda de KPI, conflito com cliente, ou falha sistêmica). Qual foi a situação, o que você decidiu fazer, e qual foi o resultado mensurável?",
      "rubrica": {
        "1": "Resposta vaga, sem situação concreta. Foca em personalidade ('sou líder por exemplo') sem citar fato verificável. Sem métrica de resultado.",
        "3": "Situação clara com contexto. Ação tomada é defensável. Resultado mensurável citado (ex: 'OTD subiu de X para Y').",
        "5": "Crise de alto impacto (financeiro ou reputacional). Decisão sob pressão real. Resultado com métrica + reflexão sobre o que faria diferente. Demonstra aprendizado pós-evento."
      },
      "tempo_estimado_min": 8
    },
    {
      "competencia_alvo": "Compliance ANTT",
      "tipo": "probing",
      "pergunta": "Você nunca trabalhou diretamente em conformidade com a ANTT no nível que essa posição exige (Resoluções 5.998/22 e 6.034/25, telemetria classe 3+, auditoria interna semestral). Caso assuma a posição, descreva como você se prepararia nos primeiros 30 dias para evitar não-conformidade na próxima fiscalização.",
      "rubrica": {
        "1": "Resposta genérica ('vou estudar', 'vou contratar consultoria') sem plano específico. Não demonstra conhecimento de qual seria a primeira ação.",
        "3": "Plano estruturado em fases. Identifica recursos certos (consultoria especializada, ANTT direto, peer network). Reconhece que vai precisar de apoio interno (Compliance, Jurídico).",
        "5": "Plano com timeline e entregáveis. Cita ações específicas (ex: 'vou pedir auditoria preliminar nos 30 primeiros dias para identificar gaps antes da fiscalização'). Demonstra humildade sobre o gap mas confiança no método para fechar."
      },
      "tempo_estimado_min": 5
    }
  ],
  "case_scenario": {
    "pergunta": "Você acabou de assumir a operação Sudeste da Atlântica. OTD está em 84% (meta 94%). Tem 3 meses até a próxima reunião do Conselho. Quais são suas três primeiras ações nos próximos 30 dias, e qual sinal você espera ver para saber que está no caminho certo?",
    "rubrica": {
      "1": "Lista de ações genéricas sem priorização. Sem critério para julgar progresso.",
      "3": "3 ações concretas com lógica clara. Identifica pelo menos 1 sinal antecedente (não só o KPI final) para validar trajetória.",
      "5": "Ações priorizadas por hipótese de causa raiz. Sinais antecedentes específicos por ação (ex: 'na semana 2 espero ver tempo médio de imobilização cair'). Distingue entre o que é controlável e o que é estrutural."
    },
    "tempo_estimado_min": 10
  },
  "tempo_total_estimado_min": 55
}
```

## Princípios não-negociáveis

1. **Nada inventado sobre o candidato.** As perguntas testam comportamento futuro ou passado. Não citem fatos do CV — isso é tarefa do Time C.
2. **STAR de verdade.** A pergunta deve forçar o candidato a contar uma situação concreta. "Fale sobre você" e "Quais suas qualidades" não são STAR.
3. **Rubrica útil.** Cada âncora deve ser uma frase descritiva, não um número genérico. Um entrevistador deve saber se a resposta é 3 ou 5 lendo a rubrica.
4. **Probing para gaps.** Quando o gap é claro, a pergunta deve nomear o gap e testar como o candidato lidaria. Não é "pegadinha", é honestidade.
5. **JSON válido.** Output parseável, sem texto extra.

## Como testar antes de entregar para o Time C

1. Rodem o skill com o JSON de saída do Time A.
2. Verifiquem que cada competência essencial gerou pelo menos 1 pergunta.
3. Verifiquem que cada gap_critico gerou exatamente 1 pergunta de probing.
4. Leiam cada rubrica em voz alta — entrevistador conseguiria distinguir 1, 3 e 5?
5. Validem o JSON.
6. Soma de tempo: deve dar entre 50 e 60 minutos (com buffer para abertura/fechamento).

## Falhas conhecidas e como recuperar

- **Pergunta muito longa:** se a pergunta passa de 60 palavras, está pedindo demais. Quebrem em duas.
- **Rubrica indistinta:** se 3 e 5 são "parecidas", aprofundem o nível 5 — adicionem uma reflexão pós-evento ou uma comparação.
- **Tempo total > 60 min:** priorizem competências essenciais. Importantes podem ficar com 1 pergunta só.
