---
name: drfabio-ads-conversions
description: Análise do funil completo Dr. Fábio Faleiro — do clique no anúncio à cirurgia realizada. Use quando ele perguntar "quanto tá saindo o CP-cirurgia?", "analisa o funil cirurgia", "quantos WhatsApps viraram consulta?", "ROI". Cruza dados Google Ads (até WhatsApp) com Kommo (consulta → cirurgia). É a skill que conecta tráfego ao business real.
---

# Dr. Fábio Faleiro — Análise de Funil e ROI

Sub-skill da métrica de negócio. CP-WhatsApp é só a primeira etapa — o que importa é **CAC vs ticket cirúrgico**.

## Quando usar

- "Quanto tá saindo o CP-cirurgia?"
- "Analisa o funil"
- "Quantos WhatsApps viraram consulta?"
- "Quantas cirurgias do mês vieram de Ads?"
- "ROI"

## O funil

```
Anúncio (Search BOFU + YT TOFU + Brand)
   ↓
Site (drfabiofaleiro.com.br ou subdomínio)
   ↓
CLICK no botão WhatsApp  ← Google Ads tag "Botão do WhatsApp"
   ↓
SDR IA atende
   ↓
Pipeline Kommo
   ↓
Consulta agendada (R$ 1.200)
   ↓
Consulta realizada
   ↓
Cirurgia robótica (R$ 30-40k honorários + R$ 40k hospital)
```

## Métricas a calcular

| Métrica | Fórmula | Fonte |
|---|---|---|
| Cliques | Windsor | Google Ads |
| WhatsApp clicks | Windsor (conv) | Google Ads tag |
| CP-WhatsApp | spend/wpps | Calcular |
| Taxa WhatsApp→consulta agendada | agendadas/wpps | Kommo manual |
| CP-consulta agendada | spend/agendadas | Calcular |
| Taxa consulta agendada → realizada | realizadas/agendadas | Kommo (no-show) |
| CP-consulta realizada | spend/realizadas | Calcular |
| Taxa consulta → cirurgia | cirurgias/realizadas | Kommo |
| **CAC cirurgia** | spend/cirurgias | Calcular |
| **ROI** | (honorários × cirurgias) / spend | Calcular |

## Pré-requisitos

- Conta `115-976-6090` no Windsor (Google Ads parte) ✅
- Dados Kommo — **sem MCP automático ainda**. Dr. Fábio passa manualmente OU configura Zapier MCP no futuro
- Mín 14 dias rodando pra ter consulta→cirurgia (ciclo longo)

## Procedimento — Análise simples (só Google Ads)

```
get_data(
  accounts=["115-976-6090"],
  fields=["campaign","clicks","conversions","spend","cost_per_conversion"],
  date_preset="last_30d"
)
```

Reportar:
```
🩺 Conversões Dr. Fábio · últimos 30 dias
WhatsApp clicks: X
Gasto: R$ Y
CP-WhatsApp: R$ Z
Por campanha:
  - BOFU Portfolio-CRD: X wpps · CP R$ Y
  - TOFU YT-DemandGen: X wpps · CP R$ Y
  - BOFU Brand-DrFabio: X wpps · CP R$ Y
```

Avaliar contra semáforo:
- 🟢 CP-WhatsApp <R$ 30
- 🟡 R$ 30-60
- 🔴 >R$ 60

## Procedimento — Análise completa (com Kommo)

1. Pedir pro Dr. Fábio passar:
   - Período X
   - WhatsApps recebidos pela SDR no período
   - Consultas agendadas
   - Consultas realizadas (não no-show)
   - Cirurgias fechadas (com vinculação ao mês do Ads)

2. Cruzar:
   - WhatsApps Google Ads vs WhatsApps SDR (deve bater ~90%)
   - Gap pode ser: WhatsApp orgânico vs pago (separar se possível)

3. Reportar funil:
```
🩺 Funil Dr. Fábio Faleiro · 2026-05 (parcial)

| Etapa | Qtd | Taxa anterior | Custo acumulado |
|---|---|---|---|
| Gasto | R$ X |||
| Cliques anúncio | Y || R$/clique |
| WhatsApps (Google tag) | Z | Z/Y % | R$ Z/wpp |
| Consultas agendadas (Kommo) | A | A/Z % | R$ A/agend |
| Consultas realizadas | B | B/A % | R$ B/real |
| Cirurgias fechadas | C | C/B % | **CAC R$ C/cir** ⭐ |

Receita potencial: C × R$ 30k = R$ X
ROI: R$ X / spend = Y×

📌 Métrica-norte primária: CP-WhatsApp = R$ Z
📌 Métrica-norte final: CAC cirurgia = R$ C/cir

Capacidade cirúrgica:
  Realizadas no período: D de 20/mês alvo
  Espaço: 20 - D = E ainda livre/mês
```

## Diagnóstico de gargalo

| Etapa caindo | Provável causa | Próxima ação |
|---|---|---|
| Cliques → WhatsApp <5% | LP fraca, botão WhatsApp escondido, copy desalinhada | Auditar site `drfabiofaleiro.com.br` (fora desta skill) |
| WhatsApp → consulta agendada <30% | SDR IA pobre OU lead frio | Auditar SDR (prompts, fluxo, qualificação) |
| Agendada → realizada <70% | No-show — lembrete fraco | Implementar lembrete automatizado, ligação confirmação |
| Realizada → cirurgia <30% | Pitch da consulta OU paciente buscando 2ª opinião | Trabalho de fechamento, não tráfego |
| Cirurgia fechada → executada (gap mês) | Agenda cheia OU paciente desistiu | Agendar mais perto da consulta |

## Decisões orientadas

### CP-WhatsApp 🟢 (<R$ 30) + capacidade folgada
→ Sugerir escalar verba (consultar `drfabio-ads-budget`)
→ Manter foco em BOFU (Search), reforçar TOFU YT proporcionalmente

### CP-WhatsApp 🟡 (R$ 30-60)
→ Otimizar antes de mais verba:
  - `drfabio-ads-search-terms` (refinar termos)
  - `drfabio-ads-rsa` (melhorar copy)
  - `drfabio-ads-negatives` (limpar tráfego ruim)

### CP-WhatsApp 🔴 (>R$ 60)
→ NÃO subir verba. Auditoria:
  - Termos estão atraindo público errado? (bariátrica? SUS? curso?)
  - Copy desalinhada?
  - Site/SDR conversão baixa?

### Capacidade cirúrgica atingida (18+/mês)
→ Pausar TOFU YT (deixar BOFU Search + Brand quentes)
→ Avaliar pausar mídia se ultrapassar 20/mês

### ROI <3× sustentado
→ Reavaliar verba. Modelo de B2C premium tem CAC alto natural — mas <3× é prejuízo

## Anti-padrões

- ❌ Reportar CP-WhatsApp como se fosse CP-cirurgia
- ❌ Decidir verba só com Google Ads, ignorando Kommo
- ❌ Inflar WhatsApp contando duplicados (mesma pessoa)
- ❌ Concluir "tá ruim" sem ver capacidade cirúrgica
- ❌ Tomar decisão com <14 dias (cirurgia tem ciclo longo)
- ❌ Esquecer de subtrair WhatsApps orgânicos (não vieram do Ads)
- ❌ Comparar com FFAI (negócios e ciclos diferentes)
