---
name: ffai-ads-daily
description: Check rápido diário das campanhas FFAI (Google Ads Search + YouTube Ads). Use quando o Dr. Fábio perguntar "como tá hoje?", "confere o que rodou", "tá vivo?", "performance", ou quiser saber se algum gatilho disparou. Puxa últimas 24-48h via Windsor.ai, calcula métricas e checa gatilhos automaticamente.
---

# FF.AI — Check Diário de Tráfego Pago

Sub-skill de leitura rápida. **NÃO toma ações** — só reporta e sinaliza. Para ações, devolve a decisão pro Dr. Fábio.

## Quando usar

Disparar quando o Dr. Fábio disser:
- "Como tá hoje?", "confere", "tá vivo?"
- "Como tão as campanhas?"
- "Performance", "números", "status"
- "Já gastou hoje?", "alguma conversão?"

## Pré-requisito

Connector `google_ads` conectado no Windsor.ai (`https://app.windsor.ai`).
Se não estiver: pare, peça pro Dr. Fábio conectar, explique o porquê.

## Procedimento

### 1. Descobrir CID e campanhas via Windsor.ai

Chame `mcp__615a3892-...__get_connectors` e confirme que `google_ads` aparece com a conta `585-625-0870`.

Se não aparecer: avise o Dr. Fábio que precisa conectar a conta no Windsor.ai e PARE.

### 2. Descobrir os campos disponíveis

```
mcp__615a3892-...__get_fields(connector="google_ads")
```

Procure campos canônicos: `date`, `campaign`, `campaign_id`, `ad_group`, `keyword`, `impressions`, `clicks`, `spend` (ou `cost`), `ctr`, `cpc`, `conversions`, `conversion_value`, `cost_per_conversion`, `search_term`, `match_type`, `video_views`, `video_view_rate` (VTR), `cost_per_video_view` (CPV).

### 3. Puxar dados das últimas 48h

```
mcp__615a3892-...__get_data(
  connector="google_ads",
  fields=["date","campaign","campaign_id","impressions","clicks","spend","ctr","conversions","cost_per_conversion"],
  date_preset="last_2_days",   # ou date_from/date_to com hoje e ontem
  ...
)
```

Agrupe por `campaign` × `date`. Filtre só as duas campanhas FFAI:
- `FFAI | Search | Intenção` (id 23863387080)
- `FFAI | YouTube | Visualizações`

### 4. Para Search: puxe também por ad group e por keyword

```
fields=["ad_group","keyword","impressions","clicks","spend","ctr","cpc","conversions"]
```

Identifique:
- Ad groups com 0 impressões nas 48h (problema)
- Top 3 keywords por impressão
- Top 3 keywords por CTR (mínimo 10 impressões)

### 5. Para YouTube (se estiver ativa): puxe por vídeo

```
fields=["video_id","video_title","impressions","video_views","video_view_rate","cost_per_video_view","spend"]
```

### 6. Calcule e reporte

Formato de saída esperado (markdown enxuto, sem floreio):

```
📊 FFAI Search · últimas 48h
- Imp: X · Cliques: Y · CTR: Z% · Gasto: R$ W · CPC médio: R$ V
- Conversões: N · CP-conv: R$ M
- Ad groups silenciosos (0 imp): [...]
- Top 3 keywords (imp): [...]
- Top 3 CTR: [...]

🎬 FFAI YouTube · últimas 48h    [ou "PAUSADA, sem dados"]
- Imp: X · Views: Y · VTR: Z% · Gasto: R$ W · CPV: R$ V
- Top vídeo: [...]
```

### 7. Cheque gatilhos automaticamente

Compare com os gatilhos do master (skill `ffai-ads`):

- 🚨 **Search <50 imp/dia há 3+ dias** → sugerir subir CPC máx pra R$ 7-8
- 🚨 **CTR <1% com 50+ cliques acumulados** → sugerir revisar RSAs
- 🚨 **Search gasta <R$ 30/dia há 7+ dias** → sugerir realocar pro YouTube
- 🚨 **15-30 conversões atingidas** → sugerir migrar pra Maximizar conversões
- 🚨 **30+ conversões** → considerar PMax
- 🚨 **CP-diag >R$ 900 (🔴)** ou capacidade de 5-8 diags do mês atingida → considerar pausar
- 🟢 **CP-diag <R$ 500** → tá saudável, manter

Se algum gatilho disparou, **listar no fim** com a recomendação clara e perguntar se quer executar.

### 8. NÃO faça

- Não invente número se Windsor.ai falhou — diga "não consegui ler"
- Não execute ação direta. Só sugira.
- Não recalcule métricas que o Windsor já entrega (CTR, CPC, CP-conv).
- Não compare semana vs semana — isso é trabalho do `ffai-ads-report`.

## Anti-padrões

- ❌ "Tudo bem, campanha rodando!" sem números
- ❌ "Vou pausar a keyword X" (sem confirmação)
- ❌ Estimar conversões com base em cliques se Windsor não retornou conversion
- ❌ Misturar dados de Search e YouTube na mesma linha
