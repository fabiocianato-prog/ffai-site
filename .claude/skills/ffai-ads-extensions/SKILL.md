---
name: ffai-ads-extensions
description: Gerência das extensões de anúncio da Search FFAI — sitelinks (4), callouts (6), structured snippets (1). Use quando o Dr. Fábio pedir "adiciona sitelink", "muda os callouts", "audita as extensões", "criar promoção", "performance das extensões". Extensões viram parte do anúncio e aumentam CTR sem custo extra — otimização contínua importa.
---

# FF.AI — Gestão de Extensões de Anúncio

Sub-skill das extensões da Search FFAI. Sitelinks, callouts, snippets, e também imagens/preço/promoção quando aplicável.

## Quando usar

- "Adiciona um sitelink novo"
- "Muda os callouts"
- "Audita as extensões — alguma com CTR ruim?"
- "Cria uma promoção"
- "Performance das extensões"

## Estado atual

- **4 sitelinks** (URLs específicas dentro do site)
- **6 callouts** (frases curtas reforçando benefícios)
- **1 snippet** estruturado "Serviços"
- **0** imagens, preço, promoção, telefone (oportunidades)

## Conceitos rápidos

| Extensão | O que faz | Tamanho típico |
|---|---|---|
| Sitelink | Links abaixo do anúncio pra páginas específicas | Título 25 chars + 2 descrições 35 chars |
| Callout | Frase de benefício, não-clicável | 25 chars |
| Snippet estruturado | Lista de itens sob um header | Header padrão (ex: "Serviços") + 3-10 valores |
| Imagem | 1-2 imagens 1:1 | 1200×1200 |
| Preço | Itens com preço | 10 chars header + 25 desc + R$ |
| Promoção | Oferta com data | Texto + datas |
| Telefone | Click-to-call mobile | Número BR |

## Procedimento — Auditar performance

1. **Puxar via Windsor:**
```
fields=["extension_type","extension_id","extension_text","impressions","clicks","ctr"]
date_preset="last_30_days"
campaign_name_filter="FFAI | Search | Intenção"
```

2. **Classificar cada extensão:**
   - 🟢 CTR ≥3% → manter
   - 🟡 CTR 1-3% → observar, pode otimizar texto
   - 🔴 CTR <1% com 100+ imp → trocar
   - ⚪ <100 imp → ainda em learning

3. **Reportar tabela com diagnóstico por extensão**

## Procedimento — Adicionar sitelink

Sitelinks bons na FFAI:
- "Diagnóstico gratuito" → `/diagnostico`
- "Conheça a FF.AI" → `/` (home)
- "Como funciona" → `/diagnostico#como-funciona`
- "Casos reais" → futuro
- "Fale no WhatsApp" → link direto Bruna (`wa.me/...`)
- "Vídeo do CEO" → `/visita` (peça avatar Dr. Fábio)

Procedimento:
1. Validar URL existe (testar com Bash se preciso)
2. Compor título (≤25 chars) + 2 descrições (≤35 chars cada)
3. Confirmar com Dr. Fábio
4. Via Playwright: Anúncios e extensões → Extensões → + → Sitelink
5. Salvar
6. Reportar

**Limite:** até 8 sitelinks ativos por campanha. Mais que isso, Google rotaciona.

## Procedimento — Adicionar/trocar callouts

Callouts bons FFAI (≤25 chars cada):
- "Diagnóstico gratuito"
- "IA dedicada à saúde"
- "Implantação em 4 semanas"
- "Atendimento 24h por IA"
- "Mais agenda, menos no-show"
- "CEO Dr. Fábio Faleiro"
- "Equipe própria, sem revenda"
- "Brasil, suporte em português"

Procedimento similar ao sitelink. Mínimo recomendado: 4 callouts (Google mostra 2-6 por vez).

## Procedimento — Snippet estruturado

Já tem 1 com header "Serviços". Outros headers úteis FFAI:
- **Tipos de clínica:** Dermatologia, Cirurgia, Pediatria, Geral
- **Features:** Agenda IA, Chatbot WhatsApp, Lembretes, CRM
- **Cidades atendidas:** São Paulo, Rio, Belo Horizonte, etc. (se for o caso)

**Cuidado:** não adicionar header "Marcas" ou "Cursos" — confunde a IA do Google sobre o vertical do negócio.

## Procedimento — Adicionar extensões novas (não usadas hoje)

### Imagem
1. Tirar do brand kit: logo `ffai-wordmark-light.svg`, foto do Dr. Fábio, ilustração da rede de agentes
2. Converter pra PNG 1200x1200 se necessário
3. Aprovação pode demorar 1-2 dias úteis

### Telefone (mobile click-to-call)
- Pra FFAI, ideal seria o número Bruna no WhatsApp Business — mas extensão de chamada chama número, não WhatsApp
- **Decidir antes:** o lead deve ligar (call) ou ir pro form/WhatsApp? Hoje funil é form → Kommo → WhatsApp. Adicionar telefone pode confundir
- **Recomendação default:** NÃO adicionar telefone hoje. Reavaliar se Bruna passar a atender ligações

### Promoção
- Só faz sentido se houver oferta real com prazo (ex: "Diagnóstico gratuito até 30/06")
- NÃO inventar promoção pra encher extensão

## Anti-padrões

- ❌ Sitelink apontando pra URL que não existe (404 mata score)
- ❌ Callout genérico ("Qualidade", "Tradição") — não significa nada
- ❌ Header de snippet errado (não combina com vertical)
- ❌ 8+ sitelinks (Google rotaciona, dilui)
- ❌ Imagem com texto pesado (Google rejeita)
- ❌ Promoção fake (com data) — viola política
- ❌ Adicionar telefone sem alguém disponível pra atender
