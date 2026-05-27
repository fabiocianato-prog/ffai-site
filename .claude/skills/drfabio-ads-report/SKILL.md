---
name: drfabio-ads-report
description: Gera relatório executivo (semanal ou mensal) das 3 campanhas Dr. Fábio Faleiro (CID 115-976-6090). Salva em Cerebro/01-Projetos/DrFabio-Trafego-Pago/Relatorios/. Foco em CP-WhatsApp, utilização da capacidade cirúrgica (20/mês alvo, 6-8 atual) e ROI projetado vs ticket. Use quando ele pedir "relatório da semana da cirurgia", "resume o mês Dr Fábio", "report executivo".
---

# Dr. Fábio Faleiro — Relatório Executivo

Síntese e arquivamento. Não toma ação.

## Quando usar

- "Relatório da semana da cirurgia"
- "Resume o mês [mês]"
- "Manda pro vault"
- Rotina semanal/mensal

## Pré-requisitos

- Conta `115-976-6090` no Windsor
- Período mínimo 7 dias
- Idealmente: números do Kommo passados manualmente pra fechar funil completo

## Procedimento

### 1. Confirmar período

Semanal (7d), Mensal (mês corrente), ou personalizado.

### 2. Puxar consolidado

```
get_data(
  accounts=["115-976-6090"],
  fields=["campaign","impressions","clicks","spend","ctr","cpc",
          "conversions","cost_per_conversion","conversion_action_name"],
  date_from=..., date_to=...
)
```

Período anterior do mesmo tamanho pra delta %.

### 3. Por campanha — detalhar AGs + top keywords/criativos

### 4. Cruzar com gatilhos do master `drfabio-ads`

### 5. Template

Nome:
- Semanal: `Relatorio-DrFabio-Semana-YYYY-MM-DD.md`
- Mensal: `Relatorio-DrFabio-Mes-YYYY-MM.md`

Caminho: `Cerebro/01-Projetos/DrFabio-Trafego-Pago/Relatorios/<arquivo>.md`

```markdown
---
tipo: relatorio-trafego
conta: 115-976-6090 (Dr Fábio Faleiro)
periodo: YYYY-MM-DD a YYYY-MM-DD
gerado_em: YYYY-MM-DD HH:MM
gerado_por: Claude (drfabio-ads-report)
---

# Relatório Tráfego Pago Dr. Fábio Faleiro — [período]

## TL;DR (3 linhas)
- Investido R$ X · WhatsApps Y · CP-WhatsApp R$ Z · vs alvo R$ 3.000/mês
- Δ vs período anterior: imp ±A% · cliques ±B% · conv ±C%
- Status: 🟢/🟡/🔴 — em uma frase

## Verba: gasto vs plano
| Item | Plano | Real | Δ |
|---|---|---|---|
| Total | R$ 3.000/mês | R$ X | ... |
| BOFU Portfolio (R$60/dia) | R$ 1.800 | R$ X | ... |
| TOFU YT (R$30/dia) | R$ 900 | R$ X | ... |
| BOFU Brand (R$10/dia) | R$ 300 | R$ X | ... |

## Por campanha
[tabela performance]

## Top keywords / search terms
[se Search]

## Top criativos
[se YT]

## Funil (se Dr. Fábio passou números Kommo)
| Etapa | Qtd | Taxa | Custo |
|---|---|---|---|
| Gasto | R$ X | | |
| Cliques | Y | | |
| WhatsApps (Google tag) | Z | Z/Y | R$/wpp |
| Consultas agendadas (Kommo) | A | A/Z | R$/consulta |
| Consultas realizadas | B | B/A | |
| Cirurgias fechadas | C | C/B | CAC = R$/cir |

## Capacidade cirúrgica
- Realizadas no período: X de 20/mês operacional (4-5/semana de capacidade)
- Espaço: Y cirurgias/mês ainda livre
- Recomendação: [escalar / manter / pausar parcial]

## Gatilhos disparados no período
[lista]

## Decisões tomadas
[lista cronológica]

## Recomendações próximo período
1. ...
2. ...
3. ...

## Pontos de revisão
- [ ] ...
```

### 6. Salvar + devolver TL;DR no chat + 1-3 recomendações

### 7. Mensal extra

- Comparar com mês anterior completo
- Tendência (subindo/plateau/caindo)
- Avaliar utilização da capacidade cirúrgica
- Recomendar ajustes de verba estrutural

## Anti-padrões

- ❌ Salvar em pasta da FFAI (caminhos diferentes — `DrFabio-Trafego-Pago/Relatorios/`)
- ❌ Misturar dados das duas contas no mesmo report
- ❌ Reportar CP-WhatsApp como se fosse CP-cirurgia (etapas diferentes do funil)
- ❌ "Manter como está" em todo relatório
- ❌ Esquecer da utilização da capacidade (importante porque tem espaço pra crescer)
