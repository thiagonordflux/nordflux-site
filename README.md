# NordFlux Site

Site institucional da **NordFlux Consultoria** — `nordflux.co`

Estático, sem build, pronto pra Netlify.

---

## Estrutura

```
nordflux-site/
├── index.html              ← Home
├── materiais.html          ← Repositório de PDFs (com gate de senha + captura de e-mail)
├── netlify.toml            ← Headers, cache, redirects
├── README.md               ← Este arquivo
├── css/
│   ├── tokens.css          ← Design system (cores, tipo, espaçamento)
│   └── site.css            ← Estilos do site
├── fonts/                  ← Cormorant Garamond · Montserrat Alternates · Source Sans 3
├── assets/                 ← Logos, fundos, fotos dos fundadores
└── pdfs/                   ← PDFs disponíveis para download
    ├── anatomia-boa-ideia.pdf
    ├── hackeando-tempo-organizacional.pdf
    ├── inovacao-com-proposito.pdf
    └── inovacao-com-proposito-coop.pdf
```

---

## Formulários (Netlify Forms)

O Netlify captura automaticamente as submissões dos formulários abaixo. Ver tudo em **Netlify Dashboard → Forms** (exportável como CSV).

| Formulário | Onde aparece | Campos |
|---|---|---|
| `contato` | `index.html` → seção Contato | nome, e-mail, empresa, assunto, mensagem |
| `mailing-list` | `materiais.html` → modal de download | nome, e-mail, empresa, cargo, material |

**Senha de acesso aos PDFs:** `nordflux2026` (validada no JS do cliente — gate leve, não segurança real).

---

## Adicionar conteúdo no futuro

### Novo PDF
1. Coloque o arquivo em `pdfs/`
2. Em `materiais.html`, copie um `<article class="material-card">…</article>` existente e edite título / subtítulo / descrição / tags / `data-file`

### Novo artigo do LinkedIn / Instagram
1. Em `index.html`, seção `#artigos`, copie um `<a class="artigo-card">…</a>` e cole como **primeiro** dentro de `.artigos-grid`

### Novo vídeo do Instagram
1. Em `index.html`, seção `#videos`, copie um `<a class="video-card">…</a>` e cole como **primeiro** dentro de `.videos-grid`
2. Remova `is-hidden` se houver, e adicione no card que era o 8º (agora 9º) pra manter 8 visíveis por padrão

---

## Deploy

Veja o arquivo **`DEPLOY.md`** com o passo a passo completo.

---

© 2026 NordFlux Consultoria Ltda. · CNPJ 63.130.623/0001-07
