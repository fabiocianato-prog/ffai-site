---
name: drfabio-ads-search-terms
description: Análise de termos de busca da campanha BOFU Search Portfolio-CRD do Dr. Fábio Faleiro (CID 115-976-6090). Sugere quais termos promover para exact match, quais negativar, quais já estão como keyword. Use quando ele pedir "termos de busca da cirurgia", "o que negativar na conta cirurgia", "expansão de keywords" no contexto Dr. Fábio (não FFAI).
---

# Dr. Fábio Faleiro — Análise de Termos de Busca

Sub-skill de refino da Search BOFU Portfolio-CRD. Lê relatório de termos via Windsor, cruza com keywords/negativas existentes, classifica em buckets acionáveis.

## Quando usar

- "Termos de busca da cirurgia"
- "O que negativar na conta cirurgia"
- "Expansão de keywords"
- Rotina semanal pós-50+ cliques na BOFU Portfolio

## Pré-requisitos

- Conta `115-976-6090` no Windsor
- BOFU Portfolio-CRD com 50+ cliques acumulados (abaixo disso, refino é prematuro)

## Procedimento

### 1. Puxar relatório de termos

```
mcp__615a3892-...__get_data(
  connector="google_ads",
  accounts=["115-976-6090"],
  fields=["search_term","keyword","match_type","ad_group","impressions",
          "clicks","ctr","spend","conversions","cost_per_conversion"],
  date_preset="last_30d",
  filters=[["campaign","contains","BOFU | Portfolio-CRD"]]
)
```

### 2. Mapear intenção (público é PACIENTE, não médico)

Categorias de intenção pra esse negócio:

| Tipo de termo | Exemplo | Ação default |
|---|---|---|
| Dor + região + cirurgia | "cirurgia de vesícula", "cirurgia de hérnia robótica", "cirurgia de refluxo" | 🟢 Promover pra exact se performance ok |
| Sintoma + sub-especialidade | "dor abdominal hérnia", "refluxo gastroesofágico cirurgia" | 🟡 Manter phrase |
| Robótica/tecnologia | "cirurgia robótica", "cirurgião robótico", "Da Vinci" | 🟢 Promover pra exact |
| Cidade + cirurgia | "cirurgião robótico São Paulo", "cirurgia robótica Goiânia" | 🟢 Promover pra exact |
| Cirurgião + nome | "Dr Fábio Faleiro" | Já é da campanha Brand, **negativar na Portfolio-CRD** (evitar canibalização) |
| Bariátrica | "cirurgia bariátrica", "redução de estômago" | 🔴 **NEGATIVAR** — pivô fora desse escopo na conta atual |
| Hospital/SUS/convênio | "cirurgia pelo SUS", "convênio Unimed cirurgia" | 🔴 Negativar — pacientes buscam preço, não premium |
| "Grátis", "barato", "valor baixo" | "cirurgia barata", "cirurgia grátis" | 🔴 Negativar — público errado |
| Profissão médica | "curso cirurgia robótica", "residência cirurgia geral" | 🔴 Negativar — médicos, não pacientes |
| Concorrentes/outros médicos | "Dr [outro cirurgião]" | 🔴 Negativar |

### 3. Classificar cada termo

| Bucket | Critério | Ação |
|---|---|---|
| ✅ Já é exact | search == keyword exact | Nenhuma, validar perf |
| 🟢 Promover pra exact | CTR ≥3% E (conv ≥1 OU clicks ≥10) E não é exact ainda | Add `[termo]` |
| 🟡 Manter phrase | CTR 1-3%, sem conv, intenção válida | Aguardar |
| 🔴 Negativar campanha | CTR <0.5% após 30+ imp · OU intenção off-topic Dr. Fábio | Add `-termo` em campanha |
| ⚪ Negativar conta (todas campanhas) | Bariátrica · SUS · barato · curso · residência · concorrente | Add em lista compartilhada (criar uma se não houver) |
| ❓ Inconclusivo | <30 imp | Aguardar |

### 4. Decidir destino de promoção

Os AGs da BOFU Portfolio-CRD precisam ser auditados primeiro — se ainda não houver estrutura, pode ser que tudo esteja num AG só ("Portfolio CRD"). Recomendar splitting se houver clusters claros:

- AG **Vesícula**
- AG **Hérnia abdominal**
- AG **Refluxo / Gastro**
- AG **Robótica genérica** (catch-all)
- AG **Cidade** (geo-específicas se houver volume)

⚠️ Antes de propor split, validar com Dr. Fábio.

### 5. Formato de saída

```
🔍 Termos analisados: N · Período: últimos 30d · BOFU Portfolio-CRD

🟢 PROMOVER PRA EXACT (X termos)
1. "cirurgia robótica de vesícula" → AG Vesícula · 47 imp · CTR 9% · 4 cliques · 1 conv WhatsApp
2. ...

🔴 NEGATIVAR EM CAMPANHA (Y termos)
1. "cirurgia bariátrica SUS" → 89 imp · CTR 0.2% · público errado
2. ...

⚪ NEGATIVAR EM LISTA COMPARTILHADA (Z termos)
[criar se não existir: "Negativas DrFabio Geral"]
1. "curso cirurgia robótica"
2. "SUS bariátrica"
3. ...

🟡 OBSERVAÇÃO (W termos)

❓ INCONCLUSIVO (V termos)

📋 Próxima ação sugerida:
[ ] Promover X exact via Playwright
[ ] Negativar Y em campanha
[ ] Criar lista compartilhada + adicionar Z termos
Aguardo OK.
```

### 6. Executar (com confirmação)

Em campanha ATIVA, pedir OK explícito. Playwright:
1. Promover: AG alvo → Palavras-chave → + → `[termo]`
2. Negativar campanha: Negativas da campanha → +
3. Lista compartilhada: Ferramentas → Biblioteca compartilhada → criar lista ou usar existente

## Anti-padrões

- ❌ Promover termo com 1 clique
- ❌ Negativar "robótica" inteiro porque um termo veio com "industrial" (use "industrial" específico)
- ❌ Esquecer de negativar bariátrica (legado conflitante com posicionamento atual)
- ❌ Criar AG novo com 1-2 termos solitários
- ❌ Não validar se o termo já é keyword (canibalização)
- ❌ Usar lista compartilhada da FFAI (são contas/listas separadas — Dr. Fábio precisa da própria)
