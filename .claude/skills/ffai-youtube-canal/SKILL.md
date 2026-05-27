---
name: ffai-youtube-canal
description: Otimização do canal ORGÂNICO do YouTube FF.AI (não é tráfego pago). Use quando o Dr. Fábio pedir "otimizar canal", "playlists do YouTube", "banner do canal", "trailer", "descrição do canal", "SEO de vídeo", "tag de vídeo". Canal está logado no Google fabiocianato@gmail.com (não contatoffai), então a maioria das ações é MANUAL com o Dr. Fábio — esta skill prepara o material/sugere e ele aplica.
---

# FF.AI — Otimização do Canal YouTube Orgânico

Sub-skill do canal orgânico. **Distinta da campanha paga** (`ffai-ads-youtube`). Função: criar autoridade, organizar acervo, otimizar SEO de vídeos pra que o canal por si só vire um motor de geração de leads de longo prazo.

## Quando usar

- "Otimizar canal", "ajustar canal"
- "Banner do canal"
- "Trailer do canal"
- "Descrição do canal"
- "Playlist nova"
- "SEO desse vídeo", "tags", "thumb", "título"
- "Organizar vídeos por pilar"

**NÃO usar pra:** análise da campanha paga YouTube → `ffai-ads-youtube`.

## Restrição importante

O canal FF.AI no YouTube está logado com `fabiocianato@gmail.com` — **conta pessoal**, não `contatoffai@gmail.com`. Isso significa:

- Não tem Playwright session pra editar via Claude automatizado
- **Todas as ações são instruções para o Dr. Fábio aplicar manualmente**
- A skill PREPARA: textos prontos, links de referência, instruções passo-a-passo, mas o clique final é manual

## Os 4 pilares de conteúdo FFAI

| Pilar | Tipo de vídeo | Exemplos atuais |
|---|---|---|
| **Dor** | Médico/clínica sofre com problema operacional | "Doutor, sua secretária esquece consulta?" |
| **Funciona** | Demonstração + benefício | "Veja o que minha IA fez às 5h23" |
| **Mitos** | Quebra de objeções comuns | "Cuidado com armadilha de IA em 2026" |
| **Prova** | Casos reais, depoimentos, resultados | (a desenvolver) |

## Estado atual do canal

- **Vídeos disponíveis no anúncio pago:**
  - X9pCYFnyo1U — "Doutor, descubra como aplicar IA na sua clínica" (Dor + Funciona)
  - 1_ZMaHe4rZI — "4 Motivos para o Médico implantar IA" (Funciona + Mitos)
  - HgQ63Kintqg — "Doutor, cuidado com armadilha de IA em 2026" (Short — Mitos)
  - 4iwkzdbkK94 — "Doutor, veja o que minha IA fez às 5h23" (Short — Funciona)
- **Banner:** [verificar manualmente]
- **Trailer:** [verificar]
- **Descrição "Sobre":** [verificar]
- **Playlists:** [verificar — provável que ainda não estejam estruturadas por pilar]

## Procedimento — Auditar canal

1. Pedir pro Dr. Fábio mandar 3 screenshots:
   - Home do canal (banner + trailer + featured)
   - Aba "Vídeos"
   - Aba "Playlists"
2. Analisar:
   - Banner alinha com brand FFAI?
   - Trailer fala pro público-alvo (donos de clínica)?
   - Descrição "Sobre" tem keywords + proposta de valor + CTA?
   - Vídeos têm thumbs próprias e títulos otimizados?
   - Playlists organizam por pilar?
3. Reportar diagnóstico estruturado

## Procedimento — Banner

Especificações YouTube:
- 2560×1440 px (mín)
- Área segura (texto e logo): 1546×423 px central
- Formatos: JPG/PNG/GIF não animado
- Tamanho máx: 6 MB

Conteúdo recomendado FF.AI:
- Logo FF.AI (lockup horizontal, light em fundo dark)
- Tagline: "IA aplicada para clínicas médicas"
- Sub: "CEO Dr. Fábio Faleiro · ffai.ia.br/diagnostico"
- Visual: rede de agentes ou grafo (alinha com identidade)

Quem faz: Canva/Figma. Material no Vault: `Branding/Logo/`. Dr. Fábio aplica.

## Procedimento — Trailer

Trailer é o vídeo que toca pra **não-inscritos** na home do canal.

Características de um bom trailer FF.AI:
- 30-60 segundos
- Hook nos primeiros 3s (dor de clínica)
- Quem é Dr. Fábio (autoridade)
- O que canal entrega (4 pilares)
- CTA: inscrever-se + visitar `ffai.ia.br/diagnostico`

Sugerir gravação dedicada OU usar o "Descubra como aplicar IA na sua clínica" como trailer interino. Definir com Dr. Fábio.

## Procedimento — Descrição "Sobre" do canal

Template:

```
FF.AI é a primeira consultoria brasileira focada em implantar redes de agentes de IA em clínicas médicas e odontológicas.

Aqui no canal você encontra:
🩺 [Pilar Dor] Os problemas operacionais que sangram receita na sua clínica
🤖 [Pilar Funciona] Como a IA realmente resolve (com exemplos práticos)
⚠️ [Pilar Mitos] Armadilhas e mitos sobre IA na saúde
📈 [Pilar Prova] Casos reais de clínicas que implantaram

Quem fala: Dr. Fábio Faleiro, médico, CEO da FF.AI.

📞 Solicite um diagnóstico gratuito da sua clínica:
👉 ffai.ia.br/diagnostico

#IA #Clinica #ConsultorioMedico #AutomacaoMedica
```

## Procedimento — Playlists por pilar

Criar (ou orientar criação manual de):

1. **Sangrias silenciosas da clínica** (Pilar Dor) — vídeos sobre os problemas
2. **IA aplicada — o que realmente funciona** (Pilar Funciona) — demos e casos
3. **Mitos e armadilhas de IA** (Pilar Mitos) — desmistificação
4. **Resultados reais** (Pilar Prova) — depoimentos, números

Pra cada playlist:
- Título curto e específico
- Descrição com 2-3 frases + CTA
- Thumb consistente (sugestão: usar brand pattern visual)

## Procedimento — SEO por vídeo

Pra cada vídeo (existente ou novo):

### Título
- ≤60 chars (idealmente 50)
- Keyword principal nos primeiros 30 chars
- Hook + curiosidade. Exemplo:
  - ❌ "IA para clínica médica explicado"
  - ✅ "Doutor, sua secretária esquece consulta? Veja a solução"

### Descrição
Estrutura:
```
[Hook 1-2 linhas — repete o problema do vídeo]

[Parágrafo 1-3 frases — quem é Dr. Fábio + por que esse vídeo importa]

📞 Diagnóstico gratuito da sua clínica:
👉 ffai.ia.br/diagnostico

📺 Mais vídeos sobre [tema]:
👉 [link da playlist]

⏱️ Capítulos:
00:00 - Intro
01:23 - O problema
03:45 - Como a IA resolve
07:10 - Próximos passos

#hashtag1 #hashtag2 #hashtag3
```

### Tags
- 10-15 tags
- Mix: específicas ("automação clínica médica") + genéricas ("IA", "consultório")
- Sem stuffing (>20 tags pune)

### Thumb
- 1280×720, mosto/contraste alto
- Rosto Dr. Fábio (expressão de surpresa/preocupação puxa CTR)
- Texto curto (3-5 palavras), legível em mobile
- Cor da marca FF.AI

### End screen
- Sempre incluir: subscribe + outro vídeo + link diagnóstico (se canal verificado)

## Anti-padrões

- ❌ Subir vídeo SEM título/descrição/tags otimizados ("publicarei depois")
- ❌ Banner com 5 fontes diferentes e poluído
- ❌ Trailer >90s
- ❌ Playlist genérica ("Outros") — sempre tematizar
- ❌ Tag-stuffing (tags irrelevantes pra ranquear em buscas erradas)
- ❌ Thumb com 7 palavras e fonte 8pt
- ❌ Esquecer link `ffai.ia.br/diagnostico` na descrição
- ❌ Esquecer hashtags relevantes (mín 3, máx 5 no topo da descrição)
