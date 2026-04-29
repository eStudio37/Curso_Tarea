# Anatomia de uma Skill — Cheat-sheet

> Folha única. Imprima ou abra ao lado enquanto preenche o template do seu time.

---

## O que é uma skill

Uma skill é **um arquivo Markdown** que ensina o Claude a fazer uma tarefa específica de um jeito específico.

É **só um documento**. Não tem código. Não tem build. Não tem deploy. Você escreve, o Claude lê, e a partir daí ele executa aquela tarefa do jeito que você descreveu.

Pense numa skill como **o procedimento operacional padrão (POP) de um analista júnior** — você está documentando como uma tarefa deve ser feita pra que qualquer um (incluindo o Claude) faça igual.

---

## Estrutura mínima

```markdown
---
name: nome-em-kebab-case
description: Uma frase que diz O QUE a skill faz e QUANDO acionar. Específico.
allowed-tools: Read, Write
effort: medium
---

# Nome da skill

## O que esse skill faz
[explicação curta — 2 a 4 parágrafos]

## Quando acionar
[gatilhos de uso — frases que o usuário diria]

## Workflow
[passo-a-passo numerado]

## Contrato de saída
[formato exato do output — JSON, HTML, lista, etc]

## Princípios não-negociáveis
[regras que não podem ser quebradas]
```

---

## Os 5 elementos críticos

### 1. Frontmatter (cabeçalho YAML)

```yaml
---
name: extrair-competencias
description: Extrai competências de uma vaga e mapeia evidências contra um currículo. Use quando precisar avaliar fit técnico de candidato a uma vaga.
allowed-tools: Read, Write
effort: medium
---
```

- `name`: identificador único, kebab-case, sem acento
- `description`: **o Claude usa isso pra decidir se aciona a skill** — seja específico sobre QUANDO usar
- `allowed-tools`: o que a skill pode usar (Read, Write, Bash...)
- `effort`: low / medium / high — calibração de quanto trabalho dá

### 2. "O que esse skill faz"

Em **uma única frase no topo**, quem lê tem que saber se é a skill certa. Depois, 2-4 parágrafos com o detalhe.

❌ Ruim: "Faz análise de coisas relacionadas a dados."  
✅ Bom: "Recebe JD + CV não-estruturados e produz JSON com 5-7 competências mapeadas, match score 0-100 por competência, e lista de gaps críticos."

### 3. Workflow

**Passos numerados, em ordem, em português comum.** Não invente notação. Não use pseudo-código. Escreva como se estivesse explicando pra um analista júnior no primeiro dia.

✅ Exemplo:
```
1. Ler a JD por completo, identificar a seção de competências/responsabilidades
2. Extrair de 5 a 7 competências críticas
3. Para cada competência, classificar:
   - Nível requerido: júnior / pleno / sênior
   - Peso: essencial / importante / nice
4. Ler o currículo
5. Para cada competência, procurar evidência no CV
```

### 4. Contrato de saída

**O formato exato do output.** Se a próxima skill na pipeline vai consumir esse output, esse contrato é o ponto de acoplamento. Mude aqui = quebra a próxima.

Para JSON, **mostre um exemplo completo**:

```json
{
  "vaga": "Gerente de Operações Logísticas",
  "competencias": [
    {
      "nome": "Liderança multi-unidade",
      "match_score": 78
    }
  ]
}
```

### 5. Princípios não-negociáveis

Coisas que NÃO podem acontecer. Curto, direto, em frases imperativas.

✅ Exemplos:
- "Nada inventado. Se evidência não está literalmente no CV, é null."
- "JSON válido. Sem texto antes ou depois."
- "Citação literal. Sem paráfrase."

---

## O que NÃO incluir

- ❌ Código rodável (skill não é programa)
- ❌ Justificativa filosófica de por que a tarefa importa (vai pro briefing, não pra skill)
- ❌ Decoração ou marketing ("nossa skill incrível...")
- ❌ Variáveis ou parâmetros estilo função — descreva em texto comum

---

## Como saber se a skill está boa

Teste em 4 minutos:

1. **Outra pessoa pegou a skill e fez o mesmo trabalho?** → claro
2. **A próxima skill na pipeline conseguiu consumir o output?** → bem definido
3. **Os princípios não-negociáveis bloqueiam falhas comuns?** → robusto
4. **Você conseguiria explicar a skill em 30 segundos lendo só o frontmatter?** → bem nomeado

Se sim pra todas: pronto pra usar.

---

## Pipeline de skills (o que vocês estão fazendo hoje)

Skills compõem **pipelines**. O output de uma alimenta a próxima.

```
Time A          Time B                       Time C
[JD + CV] →  [extrair-competencias] →  [gerar-perguntas] →  [montar-pacote] →  [HTML final]
              JSON                       JSON                 HTML
```

**O contrato JSON é o ponto de acoplamento.** Se o Time A muda a estrutura, o Time B quebra. Por isso o template diz: **"Não alterem essa estrutura"**.

A pipeline funciona porque cada skill é especialista em **uma coisa**. Não tente fazer uma mega-skill que faz tudo. Faça três pequenas que se conectam.

---

## Onde aprofundar depois do workshop

- **Especificação oficial Anthropic Skills:** ver documentação no app Claude.
- **Skills do Marketplace:** "Human resources" e "Finance" (no app) trazem exemplos completos.
- **Pasta `~/.claude/skills/`:** skills do PAI estão lá — abra qualquer SKILL.md pra ver mais um padrão.
