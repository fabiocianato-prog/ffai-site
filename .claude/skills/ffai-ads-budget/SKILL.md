---
name: ffai-ads-budget
description: Decisões e ações de orçamento e estratégia de lances das campanhas FFAI. Use quando o Dr. Fábio pedir "subir o lance", "aumentar verba", "realocar entre Search e YT", "trocar pra maximizar conversões", "mudar CPC máx", "diminuir gasto". Verifica gatilhos do plano consolidado antes de recomendar mudança. Verba total fixa em R$ 2.000/mês — qualquer remanejamento é jogo de soma zero.
---

# FF.AI — Gestão de Orçamento e Lances

Sub-skill de mudanças estratégicas em verba, CPC máx, estratégia de lances e alocação entre campanhas.

## Restrições duras (não reabrir sem motivo novo)

- **Verba total Google Ads = R$ 2.000/mês.** Search R$ 1.520 + YT R$ 480. Meta cortada do plano.
- **Search é canal de LEAD. YouTube é AUTORIDADE+RETARGETING**, não de conversão direta.
- **NÃO inverter alocação YT > Search por ansiedade.** B2B saúde tem volume baixo. Reavaliar só com 7-10 dias de dado.
- **Maximizar cliques até atingir 15-30 conversões**, daí migrar pra Maximizar conversões.
- **PMax volta só em Fase 2-3, após 30+ conversões.**

## Quando usar

- "Subir CPC máx pra R$ X"
- "Aumentar verba diária pra Y"
- "Realocar saldo do Search pro YT" (ou inverso)
- "Mudar pra maximizar conversões"
- "Tá gastando muito pouco, o que fazer?"
- "Tá gastando demais e sem retorno"

## Pré-requisitos

- 7+ dias de dado por campanha (mudanças sem isso são chute)
- Em campanha ativa → confirmação dupla (verba é estrutural)

## Gatilhos e ações correspondentes

| Gatilho | Ação |
|---|---|
| Search <50 imp/dia há 3+ dias | Subir CPC máx pra R$ 7-8 (de R$ 5) |
| Search gasta <R$ 30/dia há 7+ dias | Realocar R$ 10-20/dia pro YT |
| 15-30 conversões atingidas | Migrar Search pra "Maximizar conversões" + CPA-alvo |
| 30+ conversões | Considerar reativar PMax (fora desta skill, decisão consolidada) |
| CP-diag <R$ 300 (super 🟢) e volume sob controle | Considerar +R$ 10/dia na Search |
| CP-diag >R$ 900 (🔴) | NÃO subir verba. Revisar copy/keywords antes |
| Agenda enche antes do mês | Pausar mídia (decidir com Dr. Fábio quanto pausar) |

## Procedimento — Mudar CPC máx

1. Validar campanha (Search é só onde se mexe CPC máx; YouTube tem CPV desejado)
2. Confirmar valor novo. Saltos típicos: R$ 5 → R$ 7 → R$ 10
3. Pedir confirmação ao Dr. Fábio
4. Via Playwright: Campanha → Configurações → Estratégia de lances → CPC máx → editar
5. Salvar, screenshot
6. Reportar e marcar pra revisar em 3 dias

## Procedimento — Mudar verba diária

1. **Validar gatilho:** existe motivo no plano pra esse ajuste?
2. **Validar soma zero:** se subir Search, descer YT (ou vice-versa). Total mensal = R$ 2.000
3. Confirmar com Dr. Fábio (verba é estrutural, sempre confirma)
4. Via Playwright: Campanha → Orçamento → editar
5. Salvar, screenshot
6. Reportar AMBAS as mudanças se foi realocação
7. **Atualizar mentalmente** o orçamento de referência das outras skills

## Procedimento — Realocar Search ↔ YT

Quando os gatilhos sinalizarem realocação:

1. **Calcular delta** (ex: tirar R$ 15/dia da Search, somar R$ 15/dia no YT)
2. **Validar com regra:** Search nunca abaixo de R$ 30/dia (mínimo pra ter dado). YT nunca abaixo de R$ 10/dia (mínimo de exposição)
3. **Confirmar com Dr. Fábio explicitamente** — não é reversível sem novo passo
4. **Executar SEQUENCIALMENTE**:
   - Subir o que cresce primeiro
   - Depois descer o que cai
   - (evita gap de cobertura de mercado)
5. Reportar com tabela antes/depois

## Procedimento — Mudar estratégia de lances

| De | Para | Quando |
|---|---|---|
| Max Clicks | Max Conv | 15-30 conv acumuladas |
| Max Conv | tCPA | 30+ conv + CPA estável por 2 sem |
| tCPA | Max ROAS | quando tiver valor de conversão consistente (Fase 3) |

Procedimento:
1. Confirmar threshold atingido via Windsor (conversions)
2. Confirmar com Dr. Fábio
3. Via Playwright: Campanha → Configurações → Estratégia de lances → Trocar
4. Se for tCPA: definir alvo = média histórica * 1.0 (não tentar otimizar de cara)
5. Salvar, screenshot
6. **Aviso obrigatório:** "Vai entrar em learning de 7-14 dias. Performance pode oscilar. NÃO mexer nesse período."

## Actions via Windsor MCP

Windsor expõe `enable_campaign` e `pause_campaign`. Outras ações de budget/lance NÃO estão na API — usar Playwright.

```
mcp__615a3892-...__execute_action(
  connector="google_ads",
  action_id="pause_campaign",
  params={"campaign_id": "23863387080"}
)
```

Útil quando: pausar emergencial (agenda lotada, CP-diag 🔴, problema técnico) — mais rápido que UI.

## Anti-padrões

- ❌ Subir verba sem 7 dias de dado
- ❌ Subir verba com CP-diag 🔴 (queima dinheiro pior)
- ❌ Trocar estratégia de lances com <15 conv (algoritmo morre de fome)
- ❌ Mexer em CPC máx + verba + estratégia ao mesmo tempo (impossível atribuir o efeito depois)
- ❌ Esquecer de informar o período de learning depois de mudança grande
- ❌ Realocar pro YT sem ele estar ativo (verba some)
- ❌ Inverter aloc YT > Search "porque YT tá performando" sem dado robusto
