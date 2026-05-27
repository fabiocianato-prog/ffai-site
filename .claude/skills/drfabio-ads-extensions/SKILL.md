---
name: drfabio-ads-extensions
description: Extensões de anúncio das Search Dr. Fábio Faleiro (BOFU Portfolio-CRD + BOFU Brand). Sitelinks, callouts, structured snippets, telefone, imagem. Use quando ele pedir "adiciona sitelink cirurgia", "audita extensões Dr Fábio", "criar callout". Tom paciente-final premium. Extensões aumentam CTR sem custo.
---

# Dr. Fábio Faleiro — Extensões

## Quando usar

- "Adiciona sitelink"
- "Muda os callouts"
- "Audita extensões"
- "Criar promoção" (com cuidado — política médica)

## Estado atual

A confirmar via Playwright (Windsor não retorna extensão de campanha facilmente). Skills assume baseline mínimo, audita primeiro.

## Sitelinks recomendados (Portfolio-CRD)

Aproveitando os subdomínios do site:

| Título | URL | Descrições (2× ≤35 chars) |
|---|---|---|
| "Cirurgia de vesícula" | vesicula.drfabiofaleiro.com.br | "Tecnologia robótica Da Vinci" + "Saiba como funciona" |
| "Cirurgia de hérnia" | hernia.drfabiofaleiro.com.br | "Hérnia abdominal e inguinal" + "Mais segurança e precisão" |
| "Cirurgia de refluxo" | refluxo.drfabiofaleiro.com.br | "Fundoplicatura robótica" + "Para refluxo crônico" |
| "Sobre Dr. Fábio" | drfabiofaleiro.com.br/sobre | "Cirurgião robótico" + "Atendimento em SP e GO" |
| "Fale no WhatsApp" | wa.me/... (número direto) | "Atendimento rápido" + "SDR responde 24h" |

Limite: 8 sitelinks ativos. Mais é dilui.

## Callouts recomendados (≤25 chars)

- "Cirurgia robótica Da Vinci"
- "Atendimento premium"
- "Especialista em SP e GO"
- "Segurança em primeiro lugar"
- "Recuperação mais rápida"
- "Equipe multidisciplinar"
- "Mínimo invasivo"
- "Dr. Fábio Faleiro · CRM"

Mín 4 ativos (Google mostra 2-6).

## Snippets estruturados

Headers úteis:

- **Especialidades:** Vesícula, Hérnia, Refluxo, Robótica
- **Tecnologias:** Da Vinci, Laparoscopia 3D
- **Cidades atendidas:** São Paulo, Goiânia

**NÃO usar:** "Marcas", "Cursos", "Estilos" — confunde vertical médico.

## Extensão de telefone

⚠️ **Decisão estratégica:** o funil atual é site → WhatsApp → SDR IA. **Não tem ninguém disponível pra ligações**.

- Se Dr. Fábio decidir habilitar ligação → contratar atendente OU configurar Click-to-WhatsApp como CTA principal
- Default: **NÃO adicionar** extensão de telefone

## Extensão de imagem

- 1200×1200, máx 6MB
- Sugestões: foto do Dr. Fábio (com jaleco), foto do robô Da Vinci, foto do hospital
- ⚠️ Política médica: **sem before/after**, sem foto cirúrgica explícita
- Aprovação demora 1-2 dias

## Extensão de promoção

Cirurgia médica **não pode** usar promoção com prazo ou desconto (política Google + ética médica). NÃO usar.

## Procedimento — Auditar

1. Via Playwright: Anúncios e extensões → Extensões → lista
2. Reportar por tipo:
   - Sitelinks: quantos ativos, URLs, performance
   - Callouts: lista atual, sugestões adicionais
   - Snippets: ativos
   - Outros (imagem, telefone): presente/ausente

Via Windsor (limitado):
```
get_data(
  accounts=["115-976-6090"],
  fields=["asset_type","impressions","clicks","ctr","spend"],
  date_preset="last_30d"
)
```

## Procedimento — Adicionar/trocar

1. Validar URL (sitelink → testar acessibilidade)
2. Validar política médica (sem promessa de cura)
3. Confirmar com Dr. Fábio
4. Via Playwright: Extensões → + → tipo
5. Salvar, aguardar aprovação
6. Reportar

## Aplicar onde?

- **Conta toda:** extensões aplicadas no nível conta cobrem tudo
- **Campanha específica:** se um sitelink só faz sentido em uma campanha (ex: "Fale no WhatsApp" só na BOFU Portfolio, não na Brand)

Default: aplicar no nível conta a menos que tenha motivo pra campanha-específico.

## Anti-padrões

- ❌ Sitelink pra URL 404
- ❌ Callout genérico ("Tradição em saúde")
- ❌ Telefone sem atendimento configurado
- ❌ Imagem com texto pesado
- ❌ Promoção com desconto (proibido em med)
- ❌ 8+ sitelinks (dilui)
- ❌ Promessa de cura em qualquer extensão
- ❌ Header de snippet errado pra vertical médico
- ❌ Confundir com extensões FFAI (negócios diferentes)
