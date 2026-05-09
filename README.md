# FF.AI — Site institucional

Site one-page da **FF.AI**, consultoria premium de implantação de redes de agentes de IA para clínicas médicas e odontológicas.

Domínio em produção: [ffai.ia.br](https://ffai.ia.br)

## Stack

- HTML estático puro (sem framework)
- CSS inline no `<head>` (zero build)
- SVG inline (logo + visualização da rede de agentes)
- Tipografia: Inter + JetBrains Mono via Google Fonts
- ~1 KB de JavaScript (apenas scroll-shadow no nav)

## Estrutura

```
Site-One-Page/
├── index.html         # toda a LP — single-file
├── assets/
│   ├── dr-fabio.jpg           # foto oficial CEO
│   ├── ffai-favicon.svg       # favicon
│   ├── ffai-icon.svg          # app/social icon
│   ├── ffai-wordmark-light.svg
│   └── ffai-wordmark-dark.svg
├── README.md
└── .gitignore
```

## Deploy

Hospedado em **Vercel** (free tier), com domínio próprio `ffai.ia.br` apontado via CNAME no DNS do registro.br. SSL automático via Let's Encrypt.

## Local dev

Servidor local rápido (Python):

```bash
python -m http.server 8765
# abre em http://127.0.0.1:8765/
```

## Brand

Documentação canônica da marca, paleta, tipografia e manual de uso de logo:
`../Branding/Logo/README.md` no Obsidian Vault do Dr. Fábio.
