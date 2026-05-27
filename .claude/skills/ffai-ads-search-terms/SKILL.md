---
name: ffai-ads-search-terms
description: Análise de termos de busca (search terms report) da Search FFAI. Sugere quais termos promover para exact match, quais negativar, e quais já estão como keyword. Use quando o Dr. Fábio pedir "relatório de termos", "que termos entraram?", "o que negativar?", "expansão de keywords", ou semanalmente como rotina de refino.
---

# FF.AI — Análise de Termos de Busca

Sub-skill de refino da Search. Lê o relatório de termos de busca via Windsor.ai, cruza com keywords existentes e negativas, e devolve uma classificação acionável.

## Quando usar

- "Que termos entraram?"
- "Relatório de termos", "search terms"
- "O que negativar?", "o que promover?"
- "Tem termo bom escondido?"
- Rotina semanal de refino (toda segunda, por ex.)

## Pré-requisito

Connector `google_ads` conectado no Windsor.ai. Search com pelo menos 50 cliques acumulados — abaixo disso, refino é prematuro (avisar o Dr. Fábio).

## Procedimento

### 1. Puxar relatório de termos via Windsor.ai

```
mcp__615a3892-...__get_data(
  connector="google_ads",
  fields=["search_term","keyword","match_type","ad_group","impressions","clicks","ctr","spend","conversions","cost_per_conversion"],
  date_preset="last_30_days",
  campaign_name_filter="FFAI | Search | Intenção"
)
```

### 2. Carregar negativas atuais (memória do master)

Lista compartilhada **Negativas FFAI Geral** tem 28 termos (ver `ffai-ads` master). Se o Dr. Fábio quiser a lista atualizada do Windsor, puxar via campo `negative_keyword` ou via Playwright (UI). Manter um cache mental do que já está negativado pra não duplicar.

### 3. Classificar cada termo

Para cada `search_term` recebido, classifique em UM bucket:

| Bucket | Critério | Ação sugerida |
|---|---|---|
| ✅ **Já é keyword (exact)** | search_term == keyword com match_type=EXACT | Nenhuma, só validar performance |
| 🟢 **Promover pra exact** | CTR ≥3% E (conv ≥1 OU clicks ≥10) E ainda não é exact | Adicionar como `[search_term]` na AG correspondente |
| 🟡 **Manter em phrase** | CTR 1-3%, sem conv ainda, intenção clara | Nenhuma, deixar marinhar |
| 🔴 **Negativar** | CTR <0.5% após 30+ imp · OU intenção off (curso, vaga, freemium, pessoa física, "como fazer", concorrente, etc.) | Adicionar como `-termo` ou `[-termo]` em campanha (não na lista compartilhada se for específico) |
| ⚪ **Off-topic / não-clínica** | Termos de outras áreas (advogado, contador, restaurante, etc.) | Negativar na **lista compartilhada** "Negativas FFAI Geral" |
| ❓ **Inconclusivo** | <30 imp, sem dado suficiente | Aguardar |

### 4. Decidir AD GROUP de destino para promoções

Mapear termo → AG pelo tema:
- "secretária", "atendente virtual", "atendimento clínica" → **Secretária virtual**
- "automação clínica", "gestão consultório", "agenda" → **Automação e Gestão**
- "chatbot médico", "WhatsApp clínica", "bot consulta" → **Chatbot e WhatsApp**
- "lembrete consulta", "no-show", "confirmação automática" → **No-show e Confirmação**

Se não encaixar em nenhum, propor criar nova AG (mas só se houver 3+ termos do mesmo cluster).

### 5. Formato de saída

```
🔍 Termos analisados: N · Período: últimos 30 dias · Filtros: FFAI Search

🟢 PROMOVER PARA EXACT (X termos)
1. "lembrete de consulta médica" → AG No-show · 47 imp · CTR 6.4% · 3 cliques · 0 conv
2. ...

🔴 NEGATIVAR EM CAMPANHA (Y termos)
1. "chatbot grátis" → 89 imp · CTR 0.3% · intenção off (busca freemium)
2. ...

⚪ NEGATIVAR EM LISTA COMPARTILHADA (Z termos)
1. "advogado tributário" → totalmente off-topic
2. ...

🟡 MANTER EM OBSERVAÇÃO (W termos)
- "automação para clínica veterinária" → intenção parcial, deixar marinhar

❓ INCONCLUSIVO — pouco dado (V termos)
- ...

✅ JÁ É KEYWORD EXACT (U termos performando)

📋 Próxima ação sugerida:
[ ] Promover os X exact via Playwright
[ ] Negativar os Y termos em campanha
[ ] Adicionar Z termos na lista compartilhada
Aguardo OK pra executar.
```

### 6. Executar (com confirmação)

Se Dr. Fábio aprovar, abrir Playwright MCP no Google Ads UI e:
1. Promover: nas AGs respectivas, adicionar keywords `[termo]` (exact)
2. Negativar campanha: campanha → palavras-chave negativas → adicionar
3. Negativar compartilhada: ferramentas → biblioteca compartilhada → Negativas FFAI Geral → adicionar

**Sempre confirmar campanha-ativa antes** (master skill exige). YouTube não tem search terms, então essa skill só toca Search.

## Anti-padrões

- ❌ Promover termo com 1 clique e 1 conv (estatística zero)
- ❌ Negativar termo "automação" porque um único search "automação industrial" entrou (use negativa específica "industrial")
- ❌ Encher a lista compartilhada com termos de campanha específica (poluindo o filtro global)
- ❌ Sugerir nova AG por causa de 1 termo solitário
- ❌ Esquecer de checar se o termo já é keyword (gera duplicata e canibalização)
