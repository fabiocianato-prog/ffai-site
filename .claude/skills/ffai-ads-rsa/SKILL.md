---
name: ffai-ads-rsa
description: Otimização de RSAs (Responsive Search Ads) da Search FFAI. Use quando o Dr. Fábio pedir "revisa os RSAs", "CTR tá baixo", "trocar headline", "novo anúncio", "testar nova copy", ou quando o gatilho de CTR <1% (após 50 cliques) disparar. Analisa performance por headline/description (via Windsor), sugere variações e executa edição via Playwright.
---

# FF.AI — Otimização de RSAs

Sub-skill focada nos 4 RSAs ativos da Search FFAI. Mexe em copy, ordem, fixação e teste A/B intra-ad-group.

## Quando usar

- "Revisa os RSAs", "como tão os anúncios"
- "CTR tá baixo, o que mudar?"
- "Adiciona uma headline tipo [X]"
- "Cria um RSA novo"
- "Testa uma copy mais agressiva"
- Gatilho automático: CTR <1% após 50 cliques no AG → revisão obrigatória

## Pré-requisitos

- 4 RSAs ativos (1 por AG na FFAI Search)
- Dr. Fábio logado no Google Ads UI
- Mínimo de 200 imp por RSA pra ter sinal estatístico (abaixo disso, recomendar esperar)

## Estrutura de um RSA bom (referência FFAI)

- **Headlines (até 15):** mix de dor + solução + autoridade + CTA + nome marca
  - 5-7 headlines focadas em DOR ("Sua secretária esquece consulta?")
  - 3-4 em SOLUÇÃO ("IA atende WhatsApp 24h")
  - 2-3 em AUTORIDADE ("FF.AI – consultoria IA pra clínicas")
  - 1-2 em CTA ("Diagnóstico gratuito", "Fale com especialista")
- **Descriptions (até 4):** explicar o quê + como + prova + urgência
- **Fixação:** só fixar 1-2 headlines críticas (marca/CTA). Deixe o Google testar o resto
- **Pinagem agressiva mata o RSA** — evitar

## Procedimento — Auditar RSAs

1. **Puxar via Windsor performance por anúncio**:
```
fields=["ad_id","ad_group","headlines","descriptions","impressions","clicks","ctr","conversions"]
date_preset="last_30_days"
campaign_name_filter="FFAI | Search | Intenção"
```

2. **Para cada RSA, reportar**:
   - AG, imp, cliques, CTR
   - Headlines/descriptions atuais (lista)
   - Asset Performance (FORTE / BOM / BAIXO / PENDENTE) por headline/description — esse é o sinal do Google
   - Quaisquer pin/fixação ativa

3. **Identificar problemas**:
   - 🔴 CTR <1% com 50+ cliques → revisão obrigatória
   - 🟡 CTR 1-2% → otimização incremental
   - 🟢 CTR >3% → mexer com cautela (pode quebrar)
   - Assets em "BAIXO" → candidatos a substituir
   - Assets em "PENDENTE" → ainda em learning, esperar

## Procedimento — Sugerir nova copy

Para cada problema identificado:

1. **Gerar 3-5 alternativas** seguindo o framework:
   - Dor explícita (perda de paciente, no-show, secretária sobrecarregada)
   - Solução específica (não genérica "IA pra clínica")
   - Prova/autoridade (CEO, anos, clínicas atendidas)
   - CTA direto ("diagnóstico gratuito")

2. **Restrições do Google:**
   - Headline: até 30 caracteres
   - Description: até 90 caracteres
   - Sem repetir palavra-chave excessivamente
   - Sem !!! ou MAIÚSCULAS INTEIRAS
   - Sem promessa absurda ("100% garantido")

3. **Apresentar tabela**:
```
AG: Secretária virtual · RSA atual CTR 0.7%

❌ Headline a remover: "IA pra clínica" (BAIXO, 0.4% CTR)
✅ Sugestões (escolher 2-3):
  - "Sua secretária esquece consultas?" (29 chars)
  - "Atendimento 24h sem secretária" (30 chars)
  - "Doutor, automatize seu WhatsApp" (30 chars)
```

4. **Confirmar com Dr. Fábio** quais aceitar/rejeitar/editar

## Procedimento — Executar via Playwright

Em campanha ATIVA — pedir confirmação final antes:

1. Navegar: Campanhas → FFAI Search → AG → Anúncios → Editar RSA
2. **Adicionar nova headline/description** ANTES de remover antigas (evita gap de assets)
3. Salvar
4. Esperar aprovação (geralmente instantânea, raramente "Em revisão")
5. Tirar screenshot do novo estado
6. Reportar: "RSA do AG [X] atualizado. Removidos: [...]. Adicionados: [...]"

## Procedimento — Criar novo RSA

Quando o Dr. Fábio quer um RSA 100% novo (teste A/B):

1. **Limitar a 1 RSA novo por AG** (Google recomenda 1-2 RSAs ativos por AG, mais que isso dilui)
2. Gerar 10-15 headlines + 3-4 descriptions seguindo framework acima
3. Fixar apenas 1 headline (geralmente CTA ou marca)
4. Criar como **PAUSADO** primeiro
5. Mostrar pro Dr. Fábio, confirmar
6. Ativar — vai entrar em learning ~7-14 dias

## Anti-padrões

- ❌ Trocar headlines de RSA bom (CTR >3%) sem razão
- ❌ Fixar 3+ headlines (mata o algoritmo)
- ❌ Criar 4+ RSAs num AG só (dilui dado)
- ❌ Sugerir copy genérica ("Atendimento profissional 24h")
- ❌ Sugerir copy sem fit com o AG (CTA de "no-show" no AG "Secretária virtual")
- ❌ Modificar RSA sem screenshot antes/depois (perde histórico)
- ❌ Tirar conclusão com <200 imp no anúncio (estatística zero)
