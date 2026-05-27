---
name: drfabio-ads-budget
description: Verba, lances e estratégia das 3 campanhas Dr. Fábio Faleiro (CID 115-976-6090). Use quando ele pedir "subir lance cirurgia", "aumentar verba", "realocar Search↔YT", "trocar pra max conversões", "diminuir gasto" no contexto Dr. Fábio. Verba total planejada R$ 3.000/mês — qualquer realocação é soma zero (até decisão de escalar).
---

# Dr. Fábio Faleiro — Verba e Lances

## Verba: plano vs realidade (27/05/2026)

**Plano:** R$ 3.000/mês = R$ 100/dia. Divisão recomendada:

| Campanha | % alvo | Alvo/dia | Atual REAL |
|---|---|---|---|
| BOFU Portfolio-CRD | 60% | R$ 60 | R$ 30 |
| TOFU YT-DemandGen | 30% | R$ 30 | R$ 35 |
| BOFU Brand-DrFabio | 10% | R$ 10 | R$ 5 |
| **Total** | 100% | **R$ 100** | **R$ 70** |

**Status:** modo conservador — R$ 30/dia abaixo do plano. **Não é problema** — espaço pra escalar quando dado validar.

⚠️ **NÃO confiar em `campaign_budget` do Windsor.** Validar verba real pelo app Google Ads (UI) sempre que decidir mexer.

## Quando escalar R$ 70 → R$ 100/dia

Após **14+ dias** com:
- CP-WhatsApp 🟢 (<R$ 30) sustentado
- Cruzamento Kommo: WhatsApp→consulta ≥10%
- Pelo menos 1 cirurgia fechada vinda do tráfego (sinal real de ROI)
- Capacidade cirúrgica ainda folgada (<15/mês)

Distribuição ideal pra atingir R$ 100/dia:
- Portfolio R$ 30 → R$ 50 (Search BOFU primeiro, captura intenção quente)
- YT R$ 35 → R$ 35 (mantém — já no alvo, "limitada por orçamento" do Google é interesse dele)
- Brand R$ 5 → R$ 15 (eficientíssimo CTR 13%, pouco espaço perdido)

## Decisão de escalar (revisar quando)

Após 14+ dias de dado robusto, considerar escalar SE:
- CP-WhatsApp <R$ 30 (semáforo 🟢)
- Cruzamento Kommo: WhatsApp→consulta ≥10%, consulta→cirurgia ≥30%
- Utilização cirúrgica <15/mês (ainda tem espaço operacional)
- ROI projetado >5× (CAC < ~R$ 6k vs honorários R$ 30-40k)

Escalada sugerida: R$ 3k → R$ 5k → R$ 8k → R$ 12k em saltos com 14d entre cada.

## Quando usar

- "Subir/baixar lance"
- "Aumentar verba"
- "Realocar"
- "Mudar estratégia (max clicks→max conv)"
- "Diminuir gasto"

## Pré-requisitos

- 7+ dias de dado por campanha
- Em campanha ativa → confirmação dupla

## Gatilhos e ações

| Gatilho | Ação |
|---|---|
| Verba >R$ 100/dia total | **Reduzir imediatamente** pra alinhar plano |
| CP-WhatsApp >R$ 60 (🔴) | NÃO subir verba. Revisar copy/keywords antes |
| CTR Search <3% por 5+ dias | Subir CPC máx ou revisar RSA — não verba |
| Sem volume (imp <50/dia) na Brand | Normal — Brand depende de busca pelo nome |
| 30+ conv na BOFU Portfolio | Migrar pra Maximizar conversões |
| 50+ conv com CPA estável | Migrar pra tCPA |
| WhatsApp→consulta <5% (Kommo) | Problema NÃO é verba — é SDR/site |
| Agenda cirúrgica chegando a 18+/mês | Cortar TOFU YT, manter só BOFU |

## Procedimento — Reduzir verba (urgente, alinhar plano)

Verba atual está em R$ 620/dia. Pra trazer pra R$ 100/dia:

1. Confirmar mudança com Dr. Fábio (verba é estrutural)
2. Via Playwright OU app Google Ads UI:
   - BOFU Portfolio-CRD: R$ 300 → R$ 60
   - TOFU YT-DemandGen: R$ 280 → R$ 30
   - BOFU Brand-DrFabio: R$ 40 → R$ 10
3. Reportar antes/depois
4. Próximo `drfabio-ads-daily` valida que verba foi aplicada

## Procedimento — Mudar verba diária

1. Validar gatilho (existe motivo no plano?)
2. Validar soma zero ou aumento de teto autorizado
3. Confirmar
4. Via Playwright: Campanha → Orçamento → editar
5. Salvar, screenshot
6. **Atualizar master** se o teto mensal mudar (ex: R$ 3k → R$ 5k)

## Procedimento — Mudar CPC máx

Search só. YT usa CPV/CPA.

1. Confirmar valor (saltos: R$ X → R$ X+2 → R$ X+5)
2. Confirmar com Dr. Fábio
3. Via Playwright: Campanha → Lances → CPC máx
4. Salvar, screenshot
5. Revisar em 3 dias

## Procedimento — Mudar estratégia de lances

| De | Para | Quando |
|---|---|---|
| Max Clicks | Max Conv | 15-30 conv acumuladas |
| Max Conv | tCPA | 30+ conv + CPA estável 2 sem |
| tCPA | tROAS | Fase 3, quando tiver valor de conversão consistente |

Procedimento:
1. Confirmar threshold via Windsor
2. Confirmar com Dr. Fábio
3. Via Playwright: Estratégia de lances → Trocar
4. tCPA: alvo = média histórica × 1.0
5. **AVISO obrigatório:** "Vai entrar em learning 7-14 dias. NÃO mexer no período."

## Procedimento — Realocar Search ↔ YT

1. Calcular delta
2. Validar pisos: BOFU Search nunca <R$ 30/dia · YT nunca <R$ 10/dia
3. Confirmar
4. Executar sequencial (sobe primeiro, desce depois)
5. Reportar tabela antes/depois

## Actions via Windsor MCP

```
mcp__615a3892-...__execute_action(
  connector="google_ads",
  action_id="pause_campaign",   # ou enable_campaign
  params={"campaign_id": "23849168373"}
)
```

Útil pra pausa emergencial. Verba/lances precisam de Playwright.

## Anti-padrões

- ❌ Subir verba sem 7 dias de dado
- ❌ Subir verba com CP-WhatsApp 🔴
- ❌ Trocar estratégia com <15 conv
- ❌ Mexer verba + CPC + estratégia juntos
- ❌ Esquecer aviso do period de learning
- ❌ Realocar pra TOFU YT quando agenda cirúrgica já chegando ao limite
- ❌ Reduzir BOFU Brand abaixo de R$ 10/dia (mata o branded, tráfego mais barato)
