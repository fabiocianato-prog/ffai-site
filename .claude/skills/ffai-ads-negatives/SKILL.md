---
name: ffai-ads-negatives
description: Gestão das palavras-chave negativas — tanto na lista compartilhada "Negativas FFAI Geral" (28 termos hoje) quanto nas negativas específicas de campanha. Use quando o Dr. Fábio pedir "negativa esse termo", "adiciona na lista geral", "tira essa negativa", "audita as negativas". Decide se nova negativa vai pra lista compartilhada (off-topic universal) ou pra campanha específica (intenção parcial). Atua via Playwright.
---

# FF.AI — Gestão de Negativas

Sub-skill especializada em negativas. Quando o `ffai-ads-search-terms` recomenda negativar, essa skill executa. Também tira negativas que estão estrangulando volume.

## Quando usar

- "Negativa o termo [X]"
- "Adiciona na lista geral"
- "Tira a negativa Y"
- "Audita as negativas, tem alguma boba?"
- "Esse termo entrou mesmo com a lista geral?"

## Estado das negativas FFAI

### Lista compartilhada `Negativas FFAI Geral`
28 termos universais (off-topic, vagas, freemium, concorrência, etc.). Aplicada à campanha Search. Termos que **NÃO** devem aparecer NUNCA, em qualquer AG.

Categorias típicas:
- Cursos/vagas: "curso", "vaga", "freelancer", "aula"
- Freemium: "grátis", "free", "trial"
- B2C: "pessoa física", "para mim", "pessoal"
- Concorrência direta: nomes de softwares concorrentes
- Off-topic: outras profissões (advogado, contador, etc.)

### Negativas de campanha
Específicas da Search FFAI. Termos com intenção parcial — não universalmente ruins, mas ruins pra ESSA campanha agora.

## Regra de roteamento — onde colocar

| Termo | Onde negativar | Por quê |
|---|---|---|
| "advogado", "restaurante", "barbearia" | **Lista compartilhada** | Off-topic universal |
| "curso de IA grátis", "vaga programador" | **Lista compartilhada** | Não-comercial |
| "chatbot para escola" | **Campanha** | Mercado adjacente, talvez futuro |
| "automação industrial" | **Campanha** | Só FFAI Search exclui — se houver outra campanha (PMax Fase 2+), pode ser relevante lá |
| Nome de concorrente direto | **Lista compartilhada** | Decisão tática de não pagar pra concorrente |
| Nome do próprio Dr. Fábio ou "FF.AI" | **NÃO negativar** | Branded, deixar entrar |

## Procedimento — Adicionar negativa

1. **Classificar** com a tabela acima
2. **Validar match type:**
   - Negativa broad (sem aspas) → bloqueia o termo em qualquer ordem
   - Negativa phrase ("...") → bloqueia o termo na ordem exata
   - Negativa exact ([...]) → bloqueia só essa combinação exata
3. **Padrão FFAI:** preferir **phrase negative** (mais cirúrgico). Exact só pra termos muito específicos. Broad só pra palavras claramente ruins (ex: `-grátis`, `-curso`)
4. **Confirmar com Dr. Fábio:** "Vou negativar `[termo]` como `phrase negative` na **lista compartilhada**. OK?"
5. **Executar via Playwright:**
   - **Lista compartilhada:** Ferramentas → Biblioteca compartilhada → Negativas FFAI Geral → Adicionar
   - **Campanha:** Campanha → Palavras-chave → Negativas → Adicionar
6. **Reportar:** "Negativado `[termo]` em [destino] como [match]. Total na lista: 29 termos."

## Procedimento — Tirar negativa

Raro, mas acontece quando:
- Você percebe que a lista está estrangulando demais o tráfego (gasto <R$ 30/dia há 7+ dias)
- Termo negativado bloqueando intenção legítima ("automação consultório" bloqueado por causa de "automação" broad negative — erro)

1. **Validar motivo** (não tirar negativa por capricho)
2. **Confirmar com Dr. Fábio**
3. **Executar via Playwright:** localizar, remover
4. **Reportar**

## Procedimento — Auditoria

Quando "audita as negativas":

1. **Puxar lista compartilhada** (via Playwright, Windsor não expõe negativas)
2. **Puxar negativas de campanha**
3. **Cruzar com termos de busca** dos últimos 30 dias (via Windsor) — algum termo BOM foi bloqueado?
4. **Reportar:**
```
🛡️ Negativas FFAI — Auditoria

Lista compartilhada (N termos):
- Categoria off-topic: X termos
- Categoria freemium: Y termos
- Categoria B2C: Z termos
- Categoria concorrência: W termos

Negativas de campanha (M termos): [lista]

🔎 Análise:
- ✅ Termos bem negativados (visivelmente ruins)
- ⚠️ Suspeitos (talvez derrubando volume): [...]
- 🔴 Erros (negativa broad demais): [...]

Sugestões:
1. Remover [termo] — pode estar bloqueando intenção legítima
2. Trocar [termo] de broad pra phrase
```

## Anti-padrões

- ❌ Adicionar palavra muito genérica como broad negative ("clínica" — bloqueia metade dos termos bons)
- ❌ Negativar marca própria FFAI
- ❌ Encher lista compartilhada com termos de uma campanha específica (poluindo o filtro universal)
- ❌ Negativar antes de avaliar (esperar mínimo 30 imp do termo)
- ❌ Duplicar negativa (já está na lista compartilhada E na campanha — desnecessário)
- ❌ Tirar negativa sem auditar impacto
