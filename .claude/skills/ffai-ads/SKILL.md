---
name: ffai-ads
description: Skill mestre de gestão contínua do tráfego pago da FF.AI (Google Ads Search + YouTube Ads). Use quando o Dr. Fábio pedir qualquer análise, decisão ou ação sobre as campanhas — esta skill roteia para a sub-skill correta (ffai-ads-daily, ffai-ads-search-terms, ffai-ads-report) ou conduz a ação diretamente quando for fora do escopo das filhas.
---

# FF.AI — Gestão de Tráfego Pago (master)

Skill orquestradora. Carregue PRIMEIRO em qualquer sessão de gestão de tráfego pago da FF.AI. Define convenções, gatilhos de ação, identificadores de conta e quando delegar para sub-skills.

## Contexto fixo do negócio

- **Cliente:** FF.AI — consultoria de IA pra clínicas médicas/odonto, B2B premium
- **Ticket:** R$ 15-20k implantação + R$ 2-5k mensal · LTV >R$ 50k
- **Capacidade operacional:** 5-8 diagnósticos qualificados/mês (limite duro)
- **Funil:** anúncio → `ffai.ia.br/diagnostico` → form → Kommo pipeline 13700424 → Bruna (WhatsApp) → call diagnóstico → proposta → fechamento
- **Métrica-norte:** CP por diagnóstico QUALIFICADO (não custo por lead). Volume de lead é vaidade.

## Conta Google Ads

- **CID:** `585-625-0870`
- **Login:** `contatoffai@gmail.com`
- **Tag de conversão:** AW-18172890851
- **Ação principal:** "Enviar formulário de lead" (page-load em `/obrigado`)
- **GA4:** secundária (não detectada no site)
- **Meta Pixel:** 1724483235588497 (instalado, standby)

## Campanhas ativas

### FFAI | Search | Intenção (id `23863387080`)
- Status: ATIVA · qualificada · score 97,6%
- Orçamento: R$ 50/dia (~R$ 1.520/mês)
- Lance: Maximizar cliques + CPC máx R$ 5
- IA Max OFF · Expansão de URL OFF
- Destino: `/diagnostico`
- 4 RSAs aprovados
- 16 keywords ativas (phrase + exact) distribuídas em 4 ad groups: Secretária virtual, Automação e Gestão, Chatbot e WhatsApp, No-show e Confirmação
- 15 keywords pausadas por "Baixo volume" (preservar, não deletar)
- Lista compartilhada `Negativas FFAI Geral` (28 termos)
- 11 extensões: 4 sitelinks + 6 callouts + 1 snippet

### FFAI | YouTube | Visualizações
- Status: PAUSADA (Dr. Fábio liga quando confortável)
- Orçamento: R$ 16/dia (~R$ 480/mês)
- Brasil/PT · YouTube apenas (Parceiros de Vídeo OFF)
- Formatos: in-stream pulável + in-feed + Shorts
- CPV desejado: R$ 0,08
- Destino: `/diagnostico` (Visualizações não aceita canal como CTA)
- 1 ad group: "Donos de clínica — intenção"
- 12 keywords contextuais
- 4 criativos: 2 long-form + 2 Shorts

## Verba total

R$ 2.000/mês = R$ 1.520 Search + R$ 480 YouTube. **Meta Ads cortada do plano** (não reabrir sem motivo novo). **PMax volta só em Fase 2-3, após 30+ conversões.**

## Semáforo CP-diagnóstico qualificado

- 🟢 Verde: < R$ 500
- 🟡 Amarelo: R$ 500 – R$ 900
- 🔴 Vermelho: > R$ 900 OU agenda não enche no mês

## Gatilhos de ação (acione quando o gatilho disparar)

| Gatilho | Ação |
|---|---|
| Search >3 dias sem chegar a 50 imp/dia | Subir CPC máx pra R$ 7-8 |
| CTR <1% depois de 50 cliques | Revisar copy dos RSAs |
| Search gasta <R$ 30/dia por 7 dias | Realocar saldo pro YouTube |
| Atingir 15-30 conversões | Migrar pra Maximizar conversões |
| Atingir 30+ conversões | Considerar reativar PMax |
| Agenda encher antes do fim do mês | PAUSAR mídia (bom problema) |

## Quando chamar cada sub-skill

| Pedido do Dr. Fábio | Sub-skill |
|---|---|
| "Como tá hoje?", "Confere o que rodou", "Performance" | **ffai-ads-daily** |
| "Que termos entraram?", "Relatório de termos", "O que negativar?" | **ffai-ads-search-terms** |
| "Faz o relatório da semana", "Resume o mês", "Manda pro vault" | **ffai-ads-report** |
| "Adiciona/pausa/muda keyword", "expandir AG" | **ffai-ads-keywords** |
| "Revisa RSAs", "CTR baixo", "trocar headline", "nova copy" | **ffai-ads-rsa** |
| "Subir lance/verba", "realocar Search↔YT", "max conversões" | **ffai-ads-budget** |
| "YouTube performance", "VTR", "trocar criativo", "ligar YT" | **ffai-ads-youtube** |
| "Negativa esse termo", "audita negativas", "lista geral" | **ffai-ads-negatives** |
| "Sitelink", "callout", "snippet", "auditar extensões" | **ffai-ads-extensions** |
| "CP-diag", "funil", "qualidade do lead", "Bruna" | **ffai-ads-conversions** |
| "Canal YouTube" (orgânico), "banner", "playlist", "SEO vídeo" | **ffai-youtube-canal** |
| Outra coisa fora desse mapa | Fica AQUI — pede confirmação antes de qualquer ação |

## Fonte de dados

- **Leitura (números, métricas, termos de busca):** Windsor.ai MCP, connector `google_ads`. Slug: `google_ads`. Se não estiver conectado, pare e peça pro Dr. Fábio conectar via `https://app.windsor.ai`.
- **Ações (criar/pausar/mudar lance):** Playwright MCP no UI do Google Ads. Login é manual do Dr. Fábio. Evitar Chrome extension (trava).
- **NUNCA invente números.** Se Windsor.ai falhar ou retornar vazio, diga "não consegui ler dado" — não estime.

## Convenções operacionais

- **Idioma:** PT-BR (nunca PT-PT)
- **Antes de mexer em campanha ATIVA:** sempre pedir confirmação explícita
- **Em campanha PAUSADA:** autorização ampla, pode editar
- **Empurre de volta:** se Dr. Fábio sugerir algo que contradiz o plano consolidado (ex: inverter verba YT > Search), argumente — só inverta se ele trouxer info nova
- **Preservar histórico:** keywords/ads pausados por "baixo volume" ou teste — não deletar
- **Não reabrir decisões consolidadas** (verba R$ 2.000, Search é lead, YT é autoridade, CTA YT em `/diagnostico`, manter Max Clicks até 15-30 conv, etc.) sem motivo novo

## Documentos de referência no Vault

Antes de qualquer decisão estratégica grande, abrir:
- `Cerebro/01-Projetos/FFAI-Trafego-Pago/Auditoria-Google-Ads-2026-05-20.md`
- `Cerebro/01-Projetos/FFAI-Trafego-Pago/Projeto-YouTube-FFAI-2026-05-20.md`
- `Cerebro/01-Projetos/FFAI-Trafego-Pago/Plano-Estrategico-Trafego-FFAI-2026-05-20.md`
- `Cerebro/01-Projetos/FFAI-Trafego-Pago/Build-Search-Campanha-FFAI-2026-05-21.md`

## Saudação padrão de sessão

Quando esta skill carregar no início de uma sessão, ofereça as opções:
1. Conferir performance atual (→ `ffai-ads-daily`)
2. Revisar termos de busca (→ `ffai-ads-search-terms`)
3. Gerar relatório semanal/mensal (→ `ffai-ads-report`)
4. Análise de conversões e funil (→ `ffai-ads-conversions`)
5. Ações específicas:
   - keywords (→ `ffai-ads-keywords`)
   - RSAs (→ `ffai-ads-rsa`)
   - Negativas (→ `ffai-ads-negatives`)
   - Extensões (→ `ffai-ads-extensions`)
   - Verba/lances (→ `ffai-ads-budget`)
   - YouTube Ads (→ `ffai-ads-youtube`)
   - Canal YouTube orgânico (→ `ffai-youtube-canal`)
6. Outra coisa

## Mapa visual das skills

```
ffai-ads (master)
├── LEITURA / ANÁLISE
│   ├── ffai-ads-daily          ← check rápido 24-48h
│   ├── ffai-ads-search-terms   ← análise de termos
│   ├── ffai-ads-conversions    ← funil + CP-diag
│   └── ffai-ads-report         ← relatório executivo
│
├── AÇÃO SEARCH
│   ├── ffai-ads-keywords       ← keywords (add/pause/match)
│   ├── ffai-ads-rsa            ← copy de anúncios
│   ├── ffai-ads-negatives      ← negativas (campanha + lista)
│   ├── ffai-ads-extensions     ← sitelinks/callouts/snippets
│   └── ffai-ads-budget         ← verba + lances + estratégia
│
└── YOUTUBE
    ├── ffai-ads-youtube        ← campanha PAGA Visualizações
    └── ffai-youtube-canal      ← canal ORGÂNICO (manual)
```
