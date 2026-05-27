---
name: drfabio-ads-daily
description: Check rápido diário das 3 campanhas ativas do Dr. Fábio Faleiro (CID 115-976-6090). Use quando o Dr. Fábio perguntar "como tá hoje?", "performance", "tá vivo?" no contexto da conta cirurgia (não FFAI). Puxa últimas 24-48h via Windsor.ai e checa gatilhos de ação. Foco em CP-WhatsApp e queima de verba vs plano de R$ 100/dia.
---

# Dr. Fábio Faleiro — Check Diário

Sub-skill de leitura rápida. NÃO toma ações — reporta + sinaliza.

## Quando usar

- "Como tá hoje?", "tá vivo?", "performance"
- "Quanto gastei?"
- "Tem conversão hoje?"
- "Tá batendo a verba?"

⚠️ Se o Dr. Fábio estiver claramente falando da FFAI (palavras "FF.AI", "Bruna", "diagnóstico", "Search Intenção"), use `ffai-ads-daily` em vez desta.

## Pré-requisito

Connector `google_ads` no Windsor.ai com a conta `115-976-6090` na lista. Se sumiu (workspace pode ter sobrescrito), avisar Dr. Fábio pra reconectar.

## Procedimento

### 1. Confirmar conta

```
mcp__615a3892-...__get_connectors()
```

Procurar `google_ads` com `accounts: [{id: "115-976-6090", ...}]`. Se ausente, parar.

### 2. Puxar dados consolidados (últimas 48h)

```
mcp__615a3892-...__get_data(
  connector="google_ads",
  accounts=["115-976-6090"],
  fields=["date","campaign","campaign_id","campaign_status","spend","clicks",
          "impressions","ctr","conversions","cost_per_conversion","campaign_budget"],
  date_preset="last_2dT",
  filters=[["campaign_status","eq","ENABLED"]]
)
```

### 3. Verificar verba vs plano

Verba alvo por campanha (R$ 100/dia total = R$ 3.000/mês):

| Campanha | Verba REAL (27/05) | Verba alvo/dia |
|---|---|---|
| BOFU Portfolio-CRD (23849168373) | R$ 30 | R$ 60 |
| TOFU YT-DemandGen (23856432623) | R$ 35 | R$ 30 |
| BOFU Brand-DrFabio (23858813905) | R$ 5 | R$ 10 |
| **Total** | **R$ 70** | **R$ 100** |

⚠️ **NÃO usar `campaign_budget` do Windsor pra verificar verba** — o valor não é confiável (pode estar cacheado, ser lifetime, etc.). Pra checar verba diária real, pedir pro Dr. Fábio confirmar no app Google Ads OU usar Playwright pra abrir a campanha no UI.

A verba do Windsor pode ser usada pra **estimar projeção** se cruzar com `spend` real, mas pra **decidir** sempre confirmar UI.

### 4. Calcular projeção mensal

```
projecao_mensal = soma(spend dos últimos 2 dias) / 2 * 30
```

Se >R$ 3.500 → 🟡, se >R$ 5.000 → 🔴.

### 5. Formato de saída

```
🩺 Dr. Fábio Faleiro · últimas 48h

⚠️ ATENÇÃO VERBA (se aplicável):
  Verba diária total configurada: R$ X (alvo: R$ 100)
  Projeção mensal: R$ Y (alvo: R$ 3.000)
  Ação: ajustar via Playwright ou app Google Ads

📊 Por campanha:

BOFU | Portfolio-CRD | SP+GO
  R$ X gastos · Y cliques · CTR Z%
  W conversões WhatsApp · CP R$ V
  Verba/dia: R$ A (alvo R$ 60) — [✅ ok / ⚠️ alto / 🔴 muito alto]

TOFU | YT-DemandGen-CRD | SP+GO
  R$ X · Y cliques · Z views · CTR W%
  V conversões · CP R$ U
  Verba/dia: R$ A (alvo R$ 30)

BOFU | Brand-DrFabio
  R$ X · Y cliques · CTR Z%
  W conversões · CP R$ V
  Verba/dia: R$ A (alvo R$ 10)

🎯 Métrica-norte: CP-WhatsApp médio = R$ X
   Semáforo: [🟢 <R$30 / 🟡 R$30-60 / 🔴 >R$60]

🚨 Gatilhos disparados:
   [listar gatilhos do master que se aplicam HOJE]
```

### 6. Checar gatilhos automaticamente

Do master `drfabio-ads`:
- 🚨 CP-WhatsApp >R$ 60 sustentado 5+ dias → revisar keywords + copy
- 🚨 CTR Search <3% ou YT <2% → revisar criativo
- 🚨 Verba >R$ 100/dia total → reduzir IMEDIATAMENTE
- 🚨 30+ conv na BOFU Portfolio → migrar pra Max Conversions
- 🚨 Sem dado de Kommo (taxa WhatsApp→consulta) → recomendar pedir dado pro Dr. Fábio

Listar gatilhos no fim com recomendação clara + pergunta "quer executar?".

## Anti-padrões

- ❌ Não confundir com FFAI — sempre filtrar pela conta `115-976-6090`
- ❌ Não estimar conversão se Windsor retornou 0
- ❌ Não silenciar alerta de verba mesmo que o Dr. Fábio "já saiba"
- ❌ Não comparar CP-WhatsApp com CP-diag (FFAI) — métricas e contextos diferentes
- ❌ Não calcular projeção mensal sem ter ≥2 dias de dado
