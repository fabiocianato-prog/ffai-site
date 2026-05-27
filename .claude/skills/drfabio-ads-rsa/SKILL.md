---
name: drfabio-ads-rsa
description: Otimização dos RSAs da Search Dr. Fábio Faleiro (BOFU Portfolio-CRD + BOFU Brand). Use quando ele pedir "revisa RSAs cirurgia", "CTR baixo", "trocar headline", "novo anúncio" no contexto Dr. Fábio. Tom de copy é PACIENTE-FINAL (não B2B FFAI). Foco em segurança, tecnologia, autoridade médica, sub-especialidade.
---

# Dr. Fábio Faleiro — Otimização de RSAs

## Quando usar

- "Revisa os RSAs da cirurgia"
- "CTR baixo"
- "Trocar headline / copy"
- "Cria RSA novo"
- Gatilho: CTR <3% após 50 cliques (Search BOFU é nicho premium — CTR alto é esperado)

## Pré-requisitos

- 4 RSAs ativos (a confirmar via Windsor) — provavelmente 1 por sub-especialidade ou 1 catch-all
- Mín 200 imp por RSA pra sinal estatístico

## Framework de copy — público PACIENTE final

Pilares de mensagem (≠ FFAI, que é B2B):

| Pilar | Exemplo headline (≤30 chars) |
|---|---|
| **Sintoma / dor** | "Sofre com refluxo todo dia?" / "Hérnia incomodando você?" |
| **Solução robótica** | "Cirurgia robótica de vesícula" / "Hérnia com tecnologia Da Vinci" |
| **Autoridade médica** | "Dr. Fábio Faleiro · CRM SP" / "Cirurgião robótico especialista" |
| **Diferencial premium** | "Atendimento de alto padrão" / "Tecnologia + segurança" |
| **Sub-especialidade** | "Especialista em refluxo" / "Cirurgia robótica de hérnia" |
| **CTA** | "Tire suas dúvidas no WhatsApp" / "Agende sua consulta hoje" |
| **Localização** | "Em São Paulo e Goiânia" / "Atendimento em SP+GO" |

⚠️ **Política médica do Google Ads:** copy não pode:
- Prometer cura
- Garantir resultado
- Usar before/after explícito
- Linguagem alarmista ("URGENTE!", "SEM RISCO!")
- Diagnóstico ("você tem refluxo?")

Foco em **convite à consulta**, não em prescrição.

## Procedimento — Auditar

```
get_data(
  accounts=["115-976-6090"],
  fields=["ad_id","ad_group","headlines","descriptions","impressions",
          "clicks","ctr","conversions"],
  date_preset="last_30d",
  filters=[["campaign","in","BOFU | Portfolio-CRD,BOFU | Brand-DrFabio"]]
)
```

Reportar por RSA:
- AG, imp, cliques, CTR, conv
- Headlines/descriptions
- Asset Performance (FORTE/BOM/BAIXO/PENDENTE) — sinal do Google

Classificação:
- 🔴 CTR <3% c/ 50+ cliques → revisão obrigatória (CTR baixo pra esse nicho)
- 🟡 CTR 3-5% → otimização incremental
- 🟢 CTR >5% → mexer com cautela
- Assets em BAIXO → candidatos a substituir

## Procedimento — Sugerir novos assets

Pra cada headline/description fraca, gerar 3-5 alternativas:

**Restrições Google:**
- Headline ≤ 30 chars
- Description ≤ 90 chars
- Sem !!! ou MAIÚSCULAS INTEIRAS
- Sem promessa absoluta

Apresentar tabela:
```
AG: Vesícula · RSA atual CTR 2.8%

❌ Headline a remover: "Cirurgia top" (BAIXO, 1.2% CTR)
✅ Sugestões (escolher 2-3):
  - "Cirurgia robótica de vesícula" (30 chars)
  - "Vesícula sem dor com robótica" (29 chars)
  - "Especialista em vesícula em SP" (30 chars)
```

Confirmar com Dr. Fábio.

## Procedimento — Executar (Playwright)

Em campanha ativa, confirmar:

1. Navegar: Campanhas → BOFU Portfolio-CRD → AG → Anúncios → Editar
2. **Adicionar** novos antes de **remover** antigos (evita gap de assets)
3. Salvar
4. Esperar aprovação
5. Screenshot
6. Reportar

## Procedimento — Novo RSA do zero

1. Limitar a 1 RSA novo/AG (Google recomenda 1-2 por AG)
2. Gerar 10-15 headlines + 3-4 descriptions
3. Fixar máx 1 headline (CTA ou autoridade médica)
4. Criar PAUSADO
5. Confirmar com Dr. Fábio
6. Ativar → 7-14 dias de learning

## Headlines de exemplo (banco)

Pra você ter inspiração de partida, validadas no framework:

**Vesícula:**
- "Cirurgia robótica de vesícula"
- "Vesícula com pedra? Veja a solução"
- "Especialista em vesícula em SP"
- "Tecnologia Da Vinci, segurança total"

**Hérnia:**
- "Hérnia abdominal com robótica"
- "Cirurgia de hérnia sem dor"
- "Hérnia inguinal · técnica Da Vinci"
- "Especialista em hérnia em GO"

**Refluxo:**
- "Sofre com refluxo crônico?"
- "Cirurgia robótica de refluxo"
- "Fundoplicatura robótica em SP"
- "Refluxo: cirurgia minimamente invasiva"

**Brand:**
- "Dr. Fábio Faleiro · Cirurgião"
- "Cirurgia robótica · Dr. Fábio"
- "Atendimento em SP e Goiânia"

## Anti-padrões

- ❌ Copy alarmista ("EMERGÊNCIA", "URGENTE")
- ❌ Promessa de cura
- ❌ Headline com >30 chars (Google trunca)
- ❌ Trocar RSA bom (CTR >5%) sem razão
- ❌ Fixar 3+ headlines (mata algoritmo)
- ❌ Copy genérica ("Tradição em saúde")
- ❌ Repetir headline em vários AGs (deixa o Google decidir)
- ❌ Confundir com tom FFAI (B2B premium, fala com médico — aqui é paciente)
- ❌ Modificar sem screenshot antes/depois
