---
name: drfabio-ads-negatives
description: Gestão de negativas da conta Dr. Fábio Faleiro (CID 115-976-6090) — lista compartilhada própria (a criar) + negativas de campanha. Use quando ele pedir "negativa esse termo da cirurgia", "audita as negativas Dr Fábio". Decide se vai pra lista compartilhada (off-topic universal) ou campanha específica. Distinta das negativas FFAI.
---

# Dr. Fábio Faleiro — Negativas

Sub-skill especializada. Lista compartilhada própria (separada da FFAI).

## Quando usar

- "Negativa o termo [X]"
- "Adiciona na lista geral Dr Fábio"
- "Tira essa negativa"
- "Audita negativas cirurgia"

## Estado

- **Lista compartilhada:** **a criar** — "Negativas DrFabio Geral"
- **Negativas de campanha:** verificar via Playwright (Windsor não expõe)

## Regra de roteamento

| Termo | Onde negativar | Por quê |
|---|---|---|
| "bariátrica", "redução de estômago", "sleeve" | **Lista compartilhada** | Posicionamento atual é cirurgia robótica digestiva/parede — bariátrica é legado fora do escopo |
| "SUS", "convênio popular", "barato", "grátis", "preço baixo" | **Lista compartilhada** | Público errado pra cirurgia premium |
| "curso", "residência", "concurso", "estágio" | **Lista compartilhada** | Médico/estudante, não paciente |
| Nome de outro cirurgião | **Lista compartilhada** | Decisão de não pagar por concorrente |
| "advogado", "restaurante", outros off-topic | **Lista compartilhada** | Universal |
| "cirurgia plástica", "lipoaspiração" | **Lista compartilhada** | Especialidade diferente |
| "cirurgia para cachorro", "veterinária" | **Lista compartilhada** | Veterinária |
| "cirurgia industrial", "automação" | **Campanha** | Termo ambíguo de "robótica" |
| "Dr Fábio Faleiro", "Faleiro cirurgião" | **NÃO negativar** | Branded (Brand-DrFabio captura) |

## Primeira ação: criar lista compartilhada

Se ainda não existir, criar via Playwright:

1. Google Ads → Ferramentas → Biblioteca compartilhada → Listas de palavras-chave negativas
2. Criar: "Negativas DrFabio Geral"
3. Aplicar à BOFU Portfolio-CRD e BOFU Brand-DrFabio
4. **NÃO aplicar ao TOFU YT-DemandGen** — Demand Gen não usa search-terms keywords, não faz sentido

## Match types

- **Phrase** `"termo"` → default
- **Exact** `[termo]` → muito específico
- **Broad** → só pra palavras claramente ruins universais (ex: `-bariátrica`, `-curso`, `-SUS`)

## Procedimento — Adicionar

1. Classificar com tabela acima
2. Validar match
3. Confirmar com Dr. Fábio
4. Via Playwright:
   - Lista compartilhada: Ferramentas → Biblioteca → Negativas DrFabio Geral → Adicionar
   - Campanha: Campanha → Palavras-chave negativas → Adicionar
5. Reportar

## Procedimento — Tirar negativa

Quando:
- Lista estrangulando volume
- Negativa errada bloqueando intenção legítima

1. Validar motivo
2. Confirmar
3. Via Playwright: remover
4. Reportar

## Procedimento — Auditoria

1. Puxar lista compartilhada (Playwright)
2. Puxar negativas de campanha
3. Cruzar com termos de busca de 30 dias (via Windsor)
4. Reportar:
   - Termos bem negativados
   - Suspeitos derrubando volume
   - Negativas broad demais

## Sugestões iniciais pra lista compartilhada (sem auditoria, recomendado)

Começar com (verificar antes de aplicar):

```
"bariátrica"          (phrase) — pivô fora desse escopo
"redução de estômago" (phrase) — bariátrica disfarçada
"sleeve gástrico"     (phrase) — bariátrica
"banda gástrica"      (phrase) — bariátrica
"balão gástrico"      (phrase) — bariátrica
"obesidade"           (phrase) — proxy de bariátrica
-SUS                  (broad) — público errado
-grátis               (broad) — público errado
-barato               (broad) — público errado
-curso                (broad) — médico/estudante
-residência           (broad) — médico
-estágio              (broad) — médico
"cirurgia plástica"   (phrase) — outra especialidade
"lipoaspiração"       (phrase) — outra especialidade
"abdominoplastia"     (phrase) — outra especialidade
"cirurgia para cachorro" (phrase) — veterinária
"veterinária"         (phrase) — veterinária
"cirurgia industrial" (phrase) — robótica off-topic
```

Validar com Dr. Fábio antes de aplicar tudo de uma vez.

## Anti-padrões

- ❌ Adicionar broad "cirurgia" (mata todo o tráfego)
- ❌ Negativar "robótica" universal
- ❌ Negativar marca própria "Dr Fábio"
- ❌ Usar a lista compartilhada da FFAI (negócios totalmente diferentes)
- ❌ Aplicar a lista ao Demand Gen (não funciona)
- ❌ Duplicar negativa (campanha + lista)
- ❌ Negativar antes de 30 imp do termo
