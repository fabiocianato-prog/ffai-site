---
name: ffai-ads-conversions
description: Análise do funil de conversão FFAI — do clique ao diagnóstico qualificado. Use quando o Dr. Fábio pedir "quanto tá saindo o CP-diag?", "analisa o funil", "leads viraram diag?", "qualidade dos leads", "Bruna deu retorno?". Calcula CP-lead, taxa de qualificação, CP-diag-qualificado e cruza com agenda. NÃO é a skill de "quanto gastei hoje" (essa é ffai-ads-daily) — é a skill de "o lead viralizou em business?".
---

# FF.AI — Análise de Conversões e Funil

Sub-skill da métrica-norte: **CP-diagnóstico qualificado**, não volume de lead.

## Quando usar

- "Quanto tá saindo o CP-diag?"
- "Analisa o funil"
- "Os leads viraram diagnóstico mesmo?"
- "Bruna conseguiu falar com quantos?"
- "Qualidade dos leads tá boa?"
- "Vale a pena continuar nessa keyword?"

## O funil FFAI

```
Clique no anúncio
   ↓
Form em /diagnostico (conversão Google Ads = "Enviar formulário de lead")
   ↓
Lead em Kommo (pipeline 13700424)
   ↓
Bruna contata via WhatsApp
   ↓
Lead RESPONDE → vira "qualificado" (entrou em contato)
   ↓
Call de diagnóstico agendada
   ↓
Call REALIZADA → "diagnóstico qualificado" ← MÉTRICA-NORTE
   ↓
Proposta enviada
   ↓
Fechamento
```

## Métricas a calcular

| Métrica | Fórmula | Onde fica |
|---|---|---|
| **Cliques** | direto do Windsor | Google Ads |
| **Leads (form submit)** | conversion "Enviar formulário de lead" | Google Ads |
| **CP-lead** | spend / leads | Google Ads |
| **Taxa conversão (lead/clique)** | leads / cliques | Calcular |
| **Leads contatados** | Bruna responde no WhatsApp | Kommo (manual ou via export) |
| **Leads qualificados** | Lead responde de volta | Kommo |
| **Calls agendadas** | Kommo estágio "Agendado" | Kommo |
| **Calls realizadas** | Kommo estágio "Diag feito" | Kommo |
| **CP-diag qualificado** | spend / calls realizadas | Calcular |
| **Propostas enviadas** | Kommo estágio "Proposta" | Kommo |
| **Fechamentos** | Kommo estágio "Ganho" | Kommo |
| **CAC** | spend / fechamentos | Calcular |

## Pré-requisitos

- Connector `google_ads` (clicks/leads/spend) ✅
- Dados do Kommo — **não tem MCP automatizado hoje**. Precisa do Dr. Fábio ou Bruna passar os números do pipeline manualmente, OU configurar Kommo no Zapier MCP (futuro)
- Mínimo de 7-14 dias rodando pra ter dado significativo

## Procedimento — Análise simples (Google Ads only)

1. **Puxar via Windsor:**
```
fields=["campaign","clicks","conversions","cost_per_conversion","spend","ctr"]
date_preset="last_30_days"
campaign_name_filter="FFAI | Search | Intenção"
```

2. **Reportar:**
```
🎯 Conversões FFAI · últimos 30 dias

Cliques: X
Leads (form): Y
Taxa de conversão: Z%
CP-lead: R$ W
Gasto: R$ V
```

3. **Avaliar contra semáforo:**
   - 🟢 CP-lead <R$ 100 (saudável pra B2B premium)
   - 🟡 R$ 100-200
   - 🔴 >R$ 200

## Procedimento — Análise completa (com Kommo)

Quando Dr. Fábio passar os números do Kommo (ou tiver MCP configurado):

1. **Coletar:**
   - Período X
   - Leads que entraram no Kommo
   - Quantos viraram "qualificado"
   - Quantos call agendado
   - Quantos call realizada
   - Quantos viraram proposta
   - Quantos fecharam

2. **Cruzar com Google Ads:**
   - Leads do Kommo bate com leads do Google Ads? (deve bater dentro de 90-95%)
   - Onde tá o gap? Form preenchido mas não chegou no Kommo? (problema técnico)

3. **Calcular taxas:**
   - Taxa qualificação = qualificados / leads
   - Taxa agendamento = agendados / qualificados
   - Taxa show = realizados / agendados
   - Taxa fechamento = fechados / realizados

4. **Reportar funil:**
```
🎯 Funil FFAI · 2026-05 (parcial)

| Etapa | Qtd | Taxa do anterior | Custo acumulado |
|---|---|---|---|
| Gasto | R$ X |||
| Cliques | Y || R$/clique = ... |
| Leads form | Z | Z/Y = ...% | R$/lead = ... |
| Qualificados (Bruna) | A | A/Z = ...% | R$/qual = ... |
| Calls agendadas | B | B/A = ...% | R$/agend = ... |
| Calls realizadas (DIAG) | C | C/B = ...% | R$/DIAG = ... ⭐ |
| Propostas | D | D/C = ...% | R$/prop = ... |
| Fechamentos | E | E/D = ...% | CAC = ... |

📌 Métrica-norte: CP-diag qualificado = R$/DIAG
   Semáforo: [🟢🟡🔴]
   Capacidade do mês: C de 5-8 ideais
```

## Procedimento — Diagnóstico de gargalo

Se o funil mostrar perda anômala em uma etapa, investigar:

| Etapa caindo | Possível causa | Próxima ação |
|---|---|---|
| Cliques → Leads <5% | LP ruim, copy desalinhada, form difícil | Revisar `/diagnostico` no site (fora desta skill) |
| Leads → Qualificados <40% | Leads ruins (intent off) ou Bruna devagar | Revisar keywords/copy + treinar Bruna |
| Qual → Agendados <60% | Bruna roteiro fraco ou lead não responde | Revisar mensagens, qualidade do contato |
| Agend → Realizado <70% | No-show — lembrete fraco | Implementar lembrete automatizado |
| Realizado → Proposta <80% | Dr. Fábio não consegue gerar interesse | Revisar pitch da call |
| Proposta → Fechamento <30% | Preço, objeção, follow-up fraco | Trabalho de vendas, não tráfego |

## Decisões orientadas por conversões

### CP-diag verde (<R$ 500) + capacidade não cheia
→ Considerar aumentar verba Search (consultar `ffai-ads-budget`)

### CP-diag amarelo (R$ 500-900)
→ Otimizar antes de mais verba. Foco em:
   - `ffai-ads-search-terms` (refinar termos)
   - `ffai-ads-rsa` (melhorar copy)

### CP-diag vermelho (>R$ 900) ou agenda vazia
→ NÃO subir verba. Auditoria completa:
   - Funil tá quebrado? (gargalo identificável)
   - Keywords tão atraindo público errado?
   - Site `/diagnostico` tá conversion-ready?

### Capacidade lotada (5-8 diags marcados)
→ PAUSAR mídia até abrir slots. Brand sai prejudicada se Dr. Fábio começar a recusar leads

## Anti-padrões

- ❌ Reportar CP-lead como se fosse CP-diag (métricas diferentes)
- ❌ Tomar decisão de verba só com Google Ads, ignorando Kommo
- ❌ Inflar conversão contando lead duplicado (mesmo email/tel)
- ❌ Comparar CP-diag de período curto (<7 dias)
- ❌ Concluir que "tá ruim" sem ver capacidade — talvez já esteja cheio
- ❌ Esquecer de cruzar com a métrica-norte: CP-diag QUALIFICADO, não CP-lead
