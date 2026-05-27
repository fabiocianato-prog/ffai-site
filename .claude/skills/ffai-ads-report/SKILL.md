---
name: ffai-ads-report
description: Gera relatório executivo (semanal ou mensal) da performance de tráfego pago FFAI em markdown, salvo no Vault Obsidian em Cerebro/01-Projetos/FFAI-Trafego-Pago/. Use quando o Dr. Fábio pedir "faz o relatório da semana", "resume o mês", "manda pro vault", "report executivo", ou em rotina semanal (toda sexta, por exemplo).
---

# FF.AI — Relatório Executivo de Tráfego Pago

Sub-skill de síntese e arquivamento. Não toma ação. Produz documento markdown formatado, em PT-BR, com tom executivo e direto.

## Quando usar

- "Faz o relatório da semana"
- "Resume o mês de [mês]"
- "Manda pro vault"
- "Report executivo"
- Rotina semanal (ex: toda sexta de manhã)

## Pré-requisitos

- Connector `google_ads` no Windsor.ai conectado
- Período mínimo: 7 dias de dado (relatório com <7 dias é pouco útil; avisar)

## Procedimento

### 1. Confirmar período com o Dr. Fábio

Se ele não especificar, perguntar:
- "Semanal (últimos 7 dias)"
- "Mensal (mês corrente)"
- "Personalizado (de X a Y)"

### 2. Puxar dados consolidados via Windsor.ai

Para CADA campanha (Search + YT), puxar:

```
mcp__615a3892-...__get_data(
  connector="google_ads",
  fields=["campaign","impressions","clicks","spend","ctr","cpc","conversions","cost_per_conversion","conversion_value"],
  date_from=...,
  date_to=...
)
```

E para comparação (período imediatamente anterior do mesmo tamanho):

```
date_from=... (período anterior)
date_to=...
```

Calcular delta % WoW (week-over-week) ou MoM (month-over-month).

### 3. Para Search: detalhar por ad group e top keywords

```
fields=["ad_group","impressions","clicks","spend","conversions","cost_per_conversion"]
fields=["keyword","match_type","impressions","clicks","ctr","conversions"]  # top 10
```

### 4. Para YouTube: detalhar por vídeo

```
fields=["video_id","video_title","impressions","video_views","video_view_rate","cost_per_video_view","spend"]
```

### 5. Cruzar com gatilhos (do master)

Para cada gatilho ativo ou disparado no período, sinalizar no relatório.

### 6. Formato do relatório (template)

Nome do arquivo:
- Semanal: `Relatorio-Trafego-FFAI-Semana-YYYY-MM-DD.md` (DD = primeiro dia da semana)
- Mensal: `Relatorio-Trafego-FFAI-Mes-YYYY-MM.md`

Caminho: `Cerebro/01-Projetos/FFAI-Trafego-Pago/Relatorios/<arquivo>.md`

Template:

```markdown
---
tipo: relatorio-trafego
periodo: YYYY-MM-DD a YYYY-MM-DD
gerado_em: YYYY-MM-DD HH:MM
gerado_por: Claude (ffai-ads-report)
---

# Relatório de Tráfego Pago FFAI — [Semana de X a Y | Mês de Z]

## TL;DR (3 linhas, executivo)

- Investido: R$ X · Leads: Y · CP-lead: R$ Z · CP-diag-qualificado: R$ W
- Comparado ao período anterior: imp ±A% · cliques ±B% · conv ±C%
- Status: 🟢 saudável | 🟡 atenção | 🔴 crítico — em UMA frase, o porquê.

## Search FFAI

### Consolidado
| Métrica | Período | Anterior | Δ |
|---|---|---|---|
| Impressões | ... | ... | ... |
| Cliques | ... | ... | ... |
| CTR | ...% | ...% | ... |
| Gasto | R$... | R$... | ... |
| CPC médio | R$... | R$... | ... |
| Conversões | ... | ... | ... |
| CP-conversão | R$... | R$... | ... |

### Por Ad Group
| AG | Imp | Cliques | CTR | Gasto | Conv | CP-Conv |
|---|---|---|---|---|---|---|
| Secretária virtual | ... | ... | ...% | R$... | ... | R$... |
| Automação e Gestão | ... | ... | ...% | R$... | ... | R$... |
| Chatbot e WhatsApp | ... | ... | ...% | R$... | ... | R$... |
| No-show e Confirmação | ... | ... | ...% | R$... | ... | R$... |

### Top 5 keywords (por conversão, depois por CTR)
...

### Termos novos relevantes (do search terms report)
- Promovidos pra exact: ...
- Negativados: ...

## YouTube FFAI

[Mesma estrutura — se pausada no período, escrever "PAUSADA, sem dados"]

### Performance por vídeo
| Vídeo | Imp | Views | VTR | CPV |
|---|---|---|---|---|

## Gatilhos no período

- [✅ atingido / 🚨 disparado / — não atingido] Search >50 imp/dia: ...
- [...] CTR ≥1% com 50+ cliques: ...
- [...] 15-30 conversões: ...

## Decisões tomadas no período

1. ... (data: ...)
2. ...

## Recomendações pra próxima semana/mês

1. [Ação concreta] — porque [dado]
2. ...
3. ...

## Próximos pontos de revisão

- [ ] ...
- [ ] ...

---
*Relatório gerado automaticamente. Dados via Windsor.ai · Google Ads CID 585-625-0870.*
```

### 7. Salvar e confirmar

- Escrever o arquivo no caminho do Vault (criar pasta `Relatorios/` se não existir)
- No fim, devolver pro Dr. Fábio:
  - Path completo do arquivo gerado
  - TL;DR colado no chat (3 linhas)
  - 1-3 recomendações concretas pra próxima semana

### 8. Se for relatório mensal: extra

- Comparar mês inteiro com mês anterior
- Calcular tendência (subindo, plateau, caindo)
- Avaliar capacidade: quantos diags qualificados foram fechados vs 5-8 ideal
- Recomendar ajustes de verba ou pausa de mídia

## Anti-padrões

- ❌ Inventar número se Windsor falhou — escreva "dado indisponível"
- ❌ Esconder mau resultado em prosa fofa — TL;DR honesto, 🔴 quando é 🔴
- ❌ Recomendar "manter como está" em todo relatório (significa que a skill não pensou)
- ❌ Comparar com período de tamanho diferente (semana vs mês)
- ❌ Esquecer de salvar o arquivo (só colar no chat não vale)
