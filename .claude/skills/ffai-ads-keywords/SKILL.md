---
name: ffai-ads-keywords
description: Adicionar, pausar, ou mudar match type de keywords nas Ad Groups da Search FFAI. Use quando o Dr. Fábio pedir "adicionar a keyword X", "pausa essa keyword", "muda essa pra exact", "expandir keywords no AG Y". Atua via Playwright no UI do Google Ads (Windsor MCP não expõe ação granular de keyword). Sempre pede confirmação antes de executar em campanha ativa.
---

# FF.AI — Gerência de Keywords (Search)

Sub-skill de ação direta no nível keyword. Toca SÓ a campanha Search (id 23863387080). YouTube tem keywords contextuais geridas pela skill `ffai-ads-youtube`.

## Quando usar

- "Adiciona a keyword [X]"
- "Pausa essa keyword"
- "Muda [X] pra exact"
- "Expande keywords no AG [Y]"
- "Tira essa keyword"
- Após análise de termos com `ffai-ads-search-terms` (consome o output)

## Pré-requisitos

- Campanha Search é ATIVA → **pedir confirmação explícita** antes de qualquer alteração
- Dr. Fábio logado no Google Ads UI (CID 585-625-0870)
- Playwright MCP funcionando (evitar Chrome extension — trava)

## Ad Groups da Search FFAI

| Ad Group | Tema |
|---|---|
| Secretária virtual | atendimento, secretária, atendente IA |
| Automação e Gestão | automação clínica, gestão consultório, agenda |
| Chatbot e WhatsApp | chatbot médico, WhatsApp, bot consulta |
| No-show e Confirmação | lembrete, no-show, confirmação automática |

## Procedimento — Adicionar keyword

1. **Validar match type pedido**: `broad`, `phrase` (default na FFAI), ou `exact`
2. **Validar AG de destino**: se Dr. Fábio não disse, infira pelo tema. Se ambíguo, pergunte
3. **Validar duplicata**: puxe via Windsor as keywords atuais da AG. Se a nova já existe (com mesmo match), recuse e avise
4. **Confirmar com Dr. Fábio**: "Vou adicionar `[keyword]` em **phrase** no AG **Secretária virtual**. OK?"
5. **Executar via Playwright**:
   - Navegar: Campanhas → FFAI | Search | Intenção → AG alvo → Palavras-chave → +
   - Inserir keyword com sintaxe correta (`broad`, `"phrase"`, `[exact]`)
   - Salvar
6. **Confirmar visualmente**: tirar screenshot pós-save, validar que está "Qualificada"
7. **Reportar**: "Adicionada: [keyword] em [AG] como [match]. Status: Qualificada."

## Procedimento — Pausar keyword

1. **Identificar keyword exata + AG** (pelo nome + match type, evita ambiguidade)
2. **Confirmar motivo** com Dr. Fábio (baixa performance, off-topic, teste). Não pausar sem motivo registrado
3. **NÃO DELETAR** — só pausar. Histórico se preserva
4. **Executar via Playwright**:
   - Navegar até a keyword → Status → Pausada
5. **Reportar**: "Pausada: [keyword] em [AG]. Motivo: [...]"

## Procedimento — Mudar match type

Não existe "alterar match type" no Google Ads — match é parte da keyword. O que se faz é:

1. **Adicionar** a versão nova (ex: `[lembrete consulta médica]` em exact)
2. **Pausar** a versão antiga (`lembrete consulta médica` em phrase)
3. Reportar ambas as ações

**Por que pausar e não deletar a antiga:** se a exact não performar, dá pra reativar a phrase rápido.

## Procedimento — Expandir (lote)

Quando vem do `ffai-ads-search-terms` com lista de promoção:

1. **Validar lista**: AG + keyword + match pra cada uma
2. **Resumir batch**: "Vou adicionar [N] keywords:\n- ... \n- ... \nConfirma?"
3. **Executar uma por uma via Playwright** (Google Ads UI não tem bulk-add bom em mobile/web; aceitar lentidão)
4. **Reportar**: status de cada uma

## Anti-padrões

- ❌ Adicionar keyword com mesmo termo + match que já existe (canibalização)
- ❌ Deletar keyword pausada antiga
- ❌ Adicionar broad match (Search FFAI não usa — só phrase e exact, decisão consolidada)
- ❌ Adicionar keyword sem AG definido
- ❌ Executar sem confirmação em campanha ativa
- ❌ Adicionar 10+ keywords sem revisão (perigo de poluir AG)

## Convenções de sintaxe

- Broad: `keyword` → **não usar** na FFAI
- Phrase: `"keyword"` → default
- Exact: `[keyword]` → para termos validados via search terms
