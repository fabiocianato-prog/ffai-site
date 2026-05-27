---
name: ffai-ads-youtube
description: Performance e gerência da campanha YouTube Ads FFAI (FFAI | YouTube | Visualizações). Use quando o Dr. Fábio pedir "como tá o YouTube?", "performance dos vídeos no anúncio", "trocar o criativo", "ajustar CPV", "ligar/pausar YouTube", "audiências de retargeting". Analisa VTR, CPV, view-through por vídeo, e ações específicas de Vis (não confundir com canal orgânico — pra isso, ffai-youtube-canal).
---

# FF.AI — YouTube Ads (Visualizações)

Sub-skill da campanha **paga** YouTube. Foco em VTR, CPV, criativos in-stream/in-feed/Shorts, audiências contextuais.

## Quando usar

- "Como tá o YouTube?", "performance dos vídeos"
- "VTR baixo, o que mudar?"
- "Troca esse criativo", "tira o vídeo X"
- "Aumenta o CPV pra Y"
- "Liga o YouTube", "pausa o YT"
- "Tá batendo o orçamento?"
- "Audiência de retargeting"

**NÃO usar pra:** otimizar canal orgânico (banner/playlists/SEO) → use `ffai-youtube-canal`.

## Estado da campanha

- **Nome:** FFAI | YouTube | Visualizações
- **Status atual:** PAUSADA (até Dr. Fábio ligar)
- **Verba:** R$ 16/dia (~R$ 480/mês)
- **País/Idioma:** Brasil/PT
- **Rede:** só YouTube (Parceiros de Vídeo OFF)
- **Formatos:** in-stream pulável + in-feed + Shorts
- **CPV desejado:** R$ 0,08
- **Destino:** `/diagnostico` (Visualizações não aceita "canal" como CTA)
- **AG:** "Donos de clínica — intenção"
- **12 keywords contextuais:** IA p/ clínica, automação, chatbot, gestão, no-show, etc.
- **4 criativos:**
  - "Doutor, descubra como aplicar IA na sua clínica" (X9pCYFnyo1U) — long-form
  - "4 Motivos para o Médico implantar IA" (1_ZMaHe4rZI) — long-form
  - "Doutor, cuidado com armadilha de IA em 2026" (HgQ63Kintqg) — Short
  - "Doutor, veja o que minha IA fez às 5h23" (4iwkzdbkK94) — Short

## Pré-requisitos

- Connector `google_ads` no Windsor.ai
- Campanha YT precisa estar ATIVA pra ter dado (se pausada → reportar e parar)

## Procedimento — Auditar performance

1. **Puxar dados via Windsor**:
```
fields=["video_id","video_title","impressions","video_views","video_view_rate",
        "cost_per_video_view","spend","clicks","ctr"]
date_preset="last_30_days"
campaign_name_filter="FFAI | YouTube | Visualizações"
```

2. **Métricas-chave a reportar:**
   - **VTR (View-Through Rate)** = views / impressions. Bom: >25% (in-stream), >5% (in-feed)
   - **CPV** = spend / views. Alvo: ≤R$ 0,08
   - **Earned views** = visualizações além do anúncio (assistir mais vídeos do canal)
   - **Subscribers earned** = inscritos no canal pós-anúncio
   - **CTR** = pra avaliar in-feed especificamente

3. **Por vídeo, classificar:**
   - 🟢 **VTR ≥30% E CPV ≤R$ 0,08** → manter, é o cavalo
   - 🟡 **VTR 20-30% OU CPV R$ 0,08-0,12** → observar
   - 🔴 **VTR <20% OU CPV >R$ 0,15** → candidato a substituir
   - ⚪ **<500 imp** → ainda sem dado

## Procedimento — Sugerir trocas de criativo

Quando algum vídeo está 🔴:

1. **Identificar problema:**
   - Long-form com VTR baixo → primeiros 5s fracos (hook ruim)
   - Short com VTR baixo → conteúdo não retém / não fala com público
   - In-feed com CTR baixo → thumb/título ruins
2. **Sugerir substituto** — dos vídeos no canal Dr. Fábio que NÃO estão no anúncio. Se não houver, recomendar gravação de novo criativo (não é tarefa da skill — vira pendência humana)
3. **Confirmar com Dr. Fábio** antes de remover do anúncio

## Procedimento — Ações específicas

### Ligar/pausar campanha (rápido via Windsor)
```
mcp__615a3892-...__execute_action(
  connector="google_ads",
  action_id="enable_campaign",
  params={"campaign_id": "[id YT]"}
)
```
Antes: confirmar com Dr. Fábio. Logo após: rodar `ffai-ads-daily` em 24h pra ver o primeiro dado.

### Mudar CPV desejado
Via Playwright: Campanha → Estratégia de lances → CPV alvo. Saltos típicos R$ 0,08 → R$ 0,10 → R$ 0,12.

### Adicionar/remover vídeo do AG
Via Playwright: Campanha → AG → Anúncios → Editar. Adicionar URL do vídeo novo, remover o antigo.

### Trocar copy do anúncio (título + descrição)
Via Playwright: editar anúncio. Restrições:
- Título: até 30 chars (in-stream)
- Descrição: até 90 chars

### Criar audiência de retargeting
Lista típica:
- Quem assistiu ≥50% de qualquer vídeo do anúncio
- Visitantes de `ffai.ia.br/diagnostico` (via tag AW-18172890851)
- Criar em Audiências → Segmentos → Personalizado

Só vale a pena depois de 500+ views (senão lista vazia).

## Gatilhos específicos do YouTube

| Gatilho | Ação |
|---|---|
| CPV >R$ 0,15 por 5+ dias | Pausar vídeo de pior performance ou reduzir CPV alvo |
| VTR <15% em todos vídeos | Problema de targeting (keywords muito amplas) — revisar AG |
| Earned views >2x paid views | Sinal forte de conteúdo bom — manter, aumentar verba |
| Zero conversão após R$ 200 gastos | Lembrar: YouTube é AUTORIDADE, não conversão direta. Não inverter alocação |

## Anti-padrões

- ❌ Esperar conversão direta do YouTube — função é autoridade+retargeting
- ❌ Trocar criativo bom (VTR >30%) sem motivo
- ❌ Subir verba do YT antes de validar VTR dos novos vídeos
- ❌ Confundir métricas: views YouTube Ads ≠ views orgânicas do canal
- ❌ Comparar CPV com CPC do Search (escalas diferentes)
- ❌ Mudar destino pra `/checklist` ou outra LP sem essa LP existir (PENDÊNCIA do Dr. Fábio)
