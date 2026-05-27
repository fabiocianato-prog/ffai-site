---
name: drfabio-ads-youtube
description: Performance da campanha TOFU YT-DemandGen-CRD do Dr. Fábio Faleiro (CID 115-976-6090). Demand Gen é formato moderno (substituto do Discovery) — combina YouTube Shorts/Feed/In-Stream + Gmail + Discover. Use quando ele pedir "como tá o YT da cirurgia", "trocar criativo YT", "DemandGen", "shorts performance". Distinto do canal orgânico (→ drfabio-youtube-canal).
---

# Dr. Fábio Faleiro — YouTube Ads (Demand Gen)

## Estado da campanha

- **Nome:** `TOFU | YT-DemandGen-CRD | SP+GO | 2026-05`
- **ID:** 23856432623
- **Tipo:** **Demand Gen** (formato novo do Google, substitui Discovery + Video Action)
- **Status:** ATIVA
- **Verba atual:** R$ 280/dia · **Verba alvo:** R$ 30/dia
- **Geo:** SP + GO
- **Função na arquitetura:** TOFU (topo) — gera demanda, alimenta o BOFU Search
- **Conversão otimizada:** Botão do WhatsApp
- **Performance até agora (~3-5 dias):** 55.468 imp · 2.654 cliques · CTR 4,8% · 18 conv · CP-conv R$ 20

## Quando usar

- "Como tá o YT da cirurgia"
- "Performance Demand Gen"
- "Trocar criativo"
- "Mudar audiência"
- "Pausar/ligar YT"

⚠️ NÃO confundir com canal orgânico — pra isso, `drfabio-youtube-canal`.

## Conceito Demand Gen

Diferente do antigo "Video Visualizações" (YT in-stream pulável):
- **Inventário:** YouTube Home/Watch Next/Shorts + Gmail Promotions + Discover
- **Otimização:** baseada em ações (clicks/conv), não em views
- **Criativos:** vídeos curtos OU imagens estáticas (idealmente AMBOS)
- **Audiências:** Lookalike, listas customizadas, e otimização do Google
- **Métrica-chave:** **CPA / CP-conversão** (não VTR)

## Procedimento — Auditar

```
get_data(
  accounts=["115-976-6090"],
  fields=["asset_id","asset_type","video_id","video_title","impressions",
          "clicks","ctr","spend","conversions","cost_per_conversion"],
  date_preset="last_30d",
  filters=[["campaign","contains","TOFU | YT-DemandGen"]]
)
```

Identificar:
- Top criativos (vídeo + imagem) por conversão
- Bottom criativos com CTR <2% e 1000+ imp → candidatos a substituir
- Inventário: onde tá rodando mais (YT vs Gmail vs Discover)
- Audiências: quais segmentos performam

## Classificar criativos

- 🟢 **CTR ≥5% E CP-conv ≤R$ 25** → manter, é cavalo
- 🟡 **CTR 3-5% OU CP-conv R$ 25-40** → observar
- 🔴 **CTR <3% E CP-conv >R$ 40** → substituir
- ⚪ <1000 imp → ainda sem dado

## Procedimento — Trocar criativo

1. Identificar criativo 🔴
2. Sugerir substituto:
   - Pegar do canal Dr. Fábio Faleiro (81k inscritos, com vídeos longos e Shorts novos sobre cirurgia robótica)
   - Confirmar que NÃO é vídeo de bariátrica (legado off-target)
   - Vídeos curtos (15-60s) performam melhor em Demand Gen
3. Confirmar com Dr. Fábio
4. Via Playwright: Campanha → AG → Anúncios → Editar criativos
5. Salvar
6. Reportar

## Procedimento — Audiências

Demand Gen aprende sozinho, mas dá pra acelerar:

- **Audiência customizada por intenção:** termos como "cirurgia robótica", "hérnia", "refluxo"
- **Audiência customizada por URL:** quem visita sites de hospitais, dores digestivas
- **Lookalike de clientes:** se Dr. Fábio fornecer lista de pacientes
- **Excluir:** quem já clicou no WhatsApp recentemente (evitar canibalização)

Audiências via Playwright: Campanha → Audiências → Editar.

## Procedimento — Trocar copy do anúncio

Title (≤40 chars Demand Gen) + descrição. Tom igual aos RSAs Search (paciente final, sub-especialidade, autoridade, NÃO promessa).

## Procedimento — Ações via Windsor

```
mcp__615a3892-...__execute_action(
  connector="google_ads",
  action_id="pause_campaign",  # ou enable
  params={"campaign_id": "23856432623"}
)
```

## Gatilhos específicos

| Gatilho | Ação |
|---|---|
| CP-conv >R$ 40 sustentado 5 dias | Pausar criativos piores, revisar audiência |
| CTR geral <2% por 1 semana | Problema de criativo/targeting |
| Conv crescendo mas BOFU não | Sinal de que TOFU funciona — bom |
| BOFU saturado e TOFU canibalizando | Reduzir verba TOFU |
| Agenda cirúrgica chegando ao limite | Pausar TOFU primeiro (BOFU mantém quente) |

## Anti-padrões

- ❌ Usar vídeos de bariátrica (legado off-posicionamento atual)
- ❌ Esperar conversão imediata (Demand Gen é TOFU)
- ❌ Comparar com Video Visualizações (formatos diferentes)
- ❌ Trocar criativo bom (CTR >5%) sem motivo
- ❌ Subir verba TOFU enquanto BOFU não escala
- ❌ Confundir métricas: views ≠ cliques ≠ conversões
- ❌ Mudar destino pra outro lugar que não seja drfabiofaleiro.com.br ou subdomínios
