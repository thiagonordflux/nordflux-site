# CLAUDE.md — Regras de colaboração no nordflux-site

Este arquivo orienta o **Claude Code** (e qualquer pessoa) que for trabalhar neste
repositório. Leia tudo antes de fazer qualquer alteração.

---

## 1. O que é este projeto

Site institucional da **NordFlux** — **HTML estático puro**, sem framework e sem
build step. Os arquivos do repositório **são** o site que vai pro ar.

- **Hospedagem:** Netlify, conectado a este repositório GitHub.
- **Deploy:** automático. Todo commit na branch principal republica o site em ~1 min.
- **Domínio:** `nordflux.co` (DNS no Cloudflare apontando pro Netlify).

### Estrutura
```
index.html         → página principal (home, fundadores, metodologia, artigos, vídeos, contato)
materiais.html     → biblioteca de PDFs (com gate de senha)
css/tokens.css     → design tokens (cores, tipografia, espaçamento) — variáveis CSS
css/site.css       → todo o estilo do site
assets/            → logos, fotos, capas de vídeo
fonts/             → fontes locais (@font-face)
pdfs/              → materiais para download
netlify.toml       → config de deploy, headers, forms
DEPLOY.md          → guia de publicação (GitHub → Netlify → Cloudflare)
```

---

## 2. Regras de ouro (FIDELIDADE DO LAYOUT)

> O layout atual foi desenhado com cuidado. **Não redesenhe.** Sua função é manter,
> estender e corrigir seguindo os padrões que já existem.

1. **Reutilize os componentes existentes.** Para adicionar conteúdo (vídeo, artigo,
   PDF), **duplique um bloco que já existe** e troque só o conteúdo. Nunca crie uma
   estrutura nova do zero.
2. **Não invente estilos.** Não adicione cores, fontes, tamanhos ou espaçamentos
   novos. Use sempre as variáveis CSS de `css/tokens.css` (`var(--...)`). Se algo
   parece faltar um token, **pergunte antes** de criar.
3. **Não mexa no `css/site.css` nem no `css/tokens.css`** para tarefas de conteúdo.
   Mudança de CSS só com pedido explícito e aprovação.
4. **Mantenha o HTML canônico:** feche todas as tags, use aspas duplas em atributos,
   não use self-closing em elementos não-void.
5. **Sem emoji** em contexto do produto/site. Tom profissional (ver design system).
6. **Idioma:** interface e conteúdo em **português (BR)**.
7. Em caso de dúvida sobre visual, **pare e pergunte** — não improvise.

---

## 3. Gestão do GitHub (responsabilidade do Claude Code)

- O Claude Code é o **dono do versionamento**: commits, branches, pull requests.
- Sugestão de fluxo: criar uma branch por mudança (`feat/novo-video-xyz`), commitar,
  e abrir PR para revisão antes do merge na principal. Para mudanças pequenas e
  seguras (ex.: novo vídeo), commit direto na principal é aceitável.
- **Mensagens de commit** claras e em português, ex.: `Adiciona vídeo "Título do Reel"`.
- Após o merge/commit na principal, o **Netlify republica sozinho** — não há passo
  manual de deploy.

---

## 4. NÃO mexer (infraestrutura)

- **DNS / Cloudflare:** registros de `api`, `clerk`, `accounts`, `app`,
  `_domainkey`, MX (Google) e TXT são login, e-mail, backend e o app Flux. Fora do
  escopo deste repositório — nunca documentar senhas ou alterar DNS por aqui.
- **Senha dos PDFs:** o gate em `materiais.html` usa a senha `nordflux2026`
  (constante `ACCESS_PASSWORD`). É um gate leve, client-side. Não exponha em commits
  públicos como algo "secreto" — é intencionalmente simples.

---

## 5. Receitas (tarefas rotineiras)

### 5.1 — Adicionar um vídeo novo (Instagram Reel)

Local: `index.html`, dentro de `<div class="videos-grid">` (seção `#videos`).

1. **Copie um bloco `<a class="video-card">` inteiro** já existente e cole-o como
   **PRIMEIRO item** dentro de `.videos-grid`.
2. Troque **só 3 coisas**:
   - o `href` → URL do novo Reel;
   - o texto do `<h3 class="video-cover-title">` (use `<span class="accent">palavra</span>`
     para destacar 1 palavra em cobre);
   - o texto do `<p class="video-caption">` (legenda curta).
3. **Não troque** o `<img src="assets/video-cover-5.png">` — é a foto-capa padrão
   (Thiago) usada em todos os cards.
4. **Mantenha 8 cards visíveis:** os vídeos a partir do 9º têm a classe `is-hidden`
   (ficam atrás do botão "Ver mais"). Como você adicionou 1 no topo, adicione
   ` is-hidden` ao card que agora passou a ser o **9º** (era o 8º, visível).
5. Atualize o contador `(+N)` no botão `#videosToggle`
   (`<span class="videos-remaining">(+6)</span>`) somando +1.

Modelo do bloco:
```html
<a href="URL_DO_REEL" target="_blank" rel="noopener" class="video-card">
  <div class="video-cover video-cover--photo">
    <img src="assets/video-cover-5.png" alt="" class="video-cover-img" loading="lazy">
    <h3 class="video-cover-title">Título do vídeo com <span class="accent">destaque</span></h3>
    <div class="video-play" aria-hidden="true">
      <svg viewBox="0 0 24 24" width="32" height="32" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
    </div>
    <span class="video-platform"><!-- ícone Instagram (copie do bloco vizinho) -->Reel</span>
  </div>
  <div class="video-body">
    <p class="video-caption">Legenda curta sobre o vídeo.</p>
    <span class="video-cta">Ver no Instagram <!-- seta (copie do bloco vizinho) --></span>
  </div>
</a>
```
> Os SVGs (ícone do Instagram e a seta) são longos — **copie-os do bloco vizinho**
> em vez de digitar.

### 5.2 — Adicionar um artigo

Local: `index.html`, na grade de artigos (seção `#artigos`). Duplique um
`<a class="artigo-card">` e troque: `href`, `artigo-platform` (LinkedIn/Instagram),
`artigo-date`, `artigo-title`, `artigo-excerpt` e as `<span class="tag">`.

### 5.3 — Adicionar um PDF / material

1. Coloque o arquivo em `pdfs/` (ex.: `pdfs/meu-material.pdf`).
2. Em `materiais.html`, duplique um `<article class="material-card">` e troque:
   `card-year`, `card-title`, `card-subtitle`, `card-desc`, as tags, e os atributos
   do botão: `data-title` e `data-file="meu-material.pdf"`.
3. O modal de senha e o download já funcionam automaticamente via `data-file`.

---

## 6. Como testar antes de commitar

Não há build. Para ver o site, abra `index.html` direto no navegador (ou um servidor
estático simples na raiz, ex.: `python3 -m http.server`).

Checklist rápido antes de commitar:
- [ ] O bloco novo aparece e está visualmente idêntico aos vizinhos.
- [ ] Links abrem corretamente.
- [ ] Nenhum estilo/cor novo foi introduzido (só `var(--...)`).
- [ ] Vídeos: 8 cards visíveis + botão "Ver mais" com contador correto.
- [ ] Sem erros no console do navegador.

---

## 7. Pra mudanças de VISUAL/LAYOUT

Alterações de design (novo layout, nova seção, mudança de estilo) **não** são feitas
direto aqui por improviso. Elas vêm desenhadas no Claude (ambiente de design) e
chegam como arquivos/handoff. O papel do Claude Code nesses casos é **integrar
fielmente** o que foi entregue, não recriar do zero.
