---
name: drfabio-ads-keywords
description: Adicionar/pausar/mudar match type de keywords na BOFU Portfolio-CRD e BOFU Brand-DrFabio (CID 115-976-6090). Use quando ele pedir "adiciona keyword cirurgia X", "pausa essa keyword", "muda pra exact" no contexto Dr. Fábio. Atua via Playwright. Sempre confirma antes em campanha ativa.
---

# Dr. Fábio Faleiro — Gerência de Keywords

Sub-skill de ação direta no nível keyword. Toca as 2 campanhas Search ativas:
- `BOFU | Portfolio-CRD | SP+GO | 2026-05`
- `BOFU | Brand-DrFabio | GO e SP | 2026-05`

YT DemandGen tem keywords contextuais geridas pela skill `drfabio-ads-youtube`.

## Quando usar

- "Adiciona keyword [X]"
- "Pausa essa keyword"
- "Muda pra exact"
- "Expande keywords"
- Consumir output de `drfabio-ads-search-terms`

## Pré-requisitos

- Campanhas Search ativas → confirmação explícita
- Dr. Fábio logado no Google Ads UI (CID 115-976-6090, login fabiocianato@gmail.com)
- Playwright funcionando

## Estrutura de AGs (mapear via Windsor antes da primeira ação)

A estrutura de AGs da BOFU Portfolio-CRD não foi documentada no briefing. **Primeira ação:** puxar AGs existentes via Windsor:

```
get_data(
  accounts=["115-976-6090"],
  fields=["ad_group","keyword","match_type","impressions","clicks","conversions"],
  filters=[["campaign","contains","BOFU | Portfolio-CRD"]]
)
```

Clusters esperados (deduzir + confirmar):
- **Vesícula** — cirurgia de vesícula, colecistectomia, pedra na vesícula
- **Hérnia** — hérnia inguinal, hérnia umbilical, hérnia abdominal, hérnia incisional
- **Refluxo** — refluxo gastroesofágico, fundoplicatura, hérnia de hiato
- **Robótica genérica** — cirurgia robótica, Da Vinci, cirurgião robótico
- **Geo** (se houver volume): SP, GO

Cada keyword nova deve mapear pra um AG existente — ou propor criar AG.

## Procedimento — Adicionar

1. Validar match (default **phrase**, exact pra termos validados)
2. Validar AG (clusters acima); se ambíguo, perguntar
3. Validar duplicata (via Windsor) e canibalização com keyword da BOFU Brand
4. Confirmar com Dr. Fábio
5. Executar via Playwright: Campanhas → BOFU Portfolio-CRD → AG → Palavras-chave → +
6. Validar status Qualificada pós-save (screenshot)
7. Reportar

## Procedimento — Pausar

1. Identificar keyword + AG + match (evita ambiguidade)
2. Confirmar motivo (perf, off-topic, teste). Não pausar sem motivo
3. **NÃO deletar** — só pausar
4. Via Playwright: Pause
5. Reportar

## Procedimento — Mudar match

1. Adicionar versão nova
2. Pausar versão antiga
3. Reportar ambas

## Procedimento — Expandir em lote

Quando vem de `drfabio-ads-search-terms`:

1. Validar lista (AG + keyword + match)
2. Resumir batch + confirmar
3. Executar uma a uma via Playwright
4. Reportar por keyword

## Sintaxe e padrões

- **Phrase** `"keyword"` — default
- **Exact** `[keyword]` — pra termos validados
- **Broad** — **não usar** (Dr. Fábio é nicho premium; broad atrai pesca off-topic)

## Anti-padrões

- ❌ Adicionar broad ("cirurgia") — atrai mundo errado
- ❌ Canibalização Brand vs Portfolio (ex: "Dr Fábio Faleiro cirurgia robótica" — deixar só na Brand)
- ❌ Adicionar termo de bariátrica (posicionamento atual é cirurgia robótica digestiva)
- ❌ Deletar pausada
- ❌ Adicionar sem AG definido
- ❌ Confundir AGs com FFAI (ex: AG "Secretária virtual" não existe aqui)
- ❌ Executar sem confirmação em campanha ativa
