# Como publicar o site

Guia passo a passo. Sem pressa — leia uma vez antes de executar.

---

## Pré-requisitos (você já tem)

- ✅ Conta GitHub: **thiagonordflux**
- ✅ Repositório: **github.com/thiagonordflux/nordflux-site**
- ✅ Domínio: **nordflux.co** (Cloudflare)
- ✅ Arquivos do site prontos neste projeto

---

## Etapa 1 — Atualizar o GitHub com a versão nova

Você tem o repositório, mas com a versão antiga. Vamos substituir os arquivos.

### Opção mais simples (interface web)

1. Baixe o ZIP do projeto (botão "Baixar projeto" abaixo) e descompacte numa pasta no seu computador
2. Abra **github.com/thiagonordflux/nordflux-site** no navegador
3. Para cada pasta/arquivo da versão antiga (`index.html`, `materiais.html` etc.):
   - Clique no arquivo → ícone de lixeira → "Commit changes"
4. Quando o repositório estiver vazio, clique em **"Add file" → "Upload files"**
5. Arraste **todos os arquivos e pastas** da pasta descompactada
6. Espere terminar o upload (vai demorar alguns minutos — tem muitas fontes)
7. Em "Commit changes", escreva uma mensagem tipo "Versão completa do site" e confirme

> 💡 Se preferir manter histórico limpo, ignore a etapa 3 (apagar) e só sobrescreva os arquivos com nome igual — os antigos sem correspondente continuam lá. Mas pra um site simples, recomeçar limpo é mais seguro.

---

## Etapa 2 — Criar conta no Netlify

1. Acesse **netlify.com**
2. **Sign up** → escolha **"GitHub"** (a sua conta já será conectada)
3. Autorize o acesso quando o GitHub pedir

---

## Etapa 3 — Publicar o site

No painel do Netlify:

1. Clique em **"Add new site"** → **"Import an existing project"**
2. Escolha **GitHub**
3. Procure e clique em **nordflux-site**
4. Na tela de configuração de build, deixe tudo como está:
   - **Branch to deploy**: `main` (ou `master`)
   - **Build command**: vazio
   - **Publish directory**: `.` (ponto)
5. Clique em **"Deploy site"**
6. Espera ~1 minuto. O site fica no ar em algo tipo `random-name-12345.netlify.app`

🎉 **Acesse a URL temporária pra conferir se tudo está funcionando antes de apontar o domínio.**

> Teste: navegação, formulário de contato, abrir um PDF (senha `nordflux2026`), clicar num vídeo.

---

## Etapa 4 — Apontar o domínio nordflux.co

O domínio está no **Cloudflare**. Vamos manter ele lá (DNS) e fazer o Cloudflare apontar pro Netlify (hosting).

### 4.1 — No Netlify

1. No site recém-criado → **Domain settings** → **Add custom domain**
2. Digite `nordflux.co` → **Verify** → **Yes, add domain**
3. Repita para `www.nordflux.co` (dá uma adicionada secundária)
4. O Netlify vai te mostrar 2 valores importantes:
   - Um **endereço apex** (algo como `apex-loadbalancer.netlify.com`)
   - Um **subdomínio** (algo como `seu-site.netlify.app`)
5. Anote esses dois valores

### 4.2 — No Cloudflare

1. Acesse **dash.cloudflare.com** → escolha o domínio `nordflux.co`
2. Menu lateral → **DNS** → **Records**
3. **Apague** os registros A, AAAA ou CNAME existentes que apontam pra `@` (raiz) e `www`
4. Adicione **dois novos registros**:

| Type | Name | Content | Proxy status |
|------|------|---------|--------------|
| CNAME | `@` (ou `nordflux.co`) | `apex-loadbalancer.netlify.com` | **DNS only** (nuvem cinza) |
| CNAME | `www` | `seu-site.netlify.app` | **DNS only** (nuvem cinza) |

> ⚠️ **Importante**: clique na nuvem laranja pra deixar **cinza** (DNS only) nos dois. O proxy do Cloudflare interfere com o SSL do Netlify e dá erro.

5. Salve

### 4.3 — Aguardar propagação

- DNS leva de **5 minutos a 24 horas** pra propagar (geralmente em 30 min)
- Volte ao Netlify → **Domain settings** → quando aparecer ✅ verde em `nordflux.co`, está no ar
- O Netlify ativa o SSL/HTTPS automaticamente (Let's Encrypt) — pode levar mais alguns minutos

🎉 **Pronto. `https://nordflux.co` está no ar.**

---

## Etapa 5 — Configurar notificações de formulário

1. Netlify → **Forms** → você verá `contato` e `mailing-list`
2. Em cada um → **Form notifications** → **Add notification** → **Email notification**
3. Coloque seu e-mail. Toda submissão vai chegar lá.
4. As submissões antigas ficam guardadas no painel — pode exportar como CSV quando quiser.

---

## Como atualizar o site no futuro

### Mudanças pequenas (texto, novo artigo, novo vídeo, novo PDF)

1. Me peça aqui no Claude pra fazer a alteração
2. Eu altero os arquivos
3. Baixe o(s) arquivo(s) alterado(s) — eu te aviso quais
4. No GitHub, abra o arquivo correspondente → ícone do lápis → cole o conteúdo novo → "Commit changes"
5. O Netlify detecta o commit e publica em ~1 minuto. Automático.

### Adicionar um PDF
1. Coloque o arquivo em `pdfs/` no GitHub (drag-drop)
2. Eu atualizo o `materiais.html` com o card novo
3. Commit do `materiais.html` → publica automático

---

## Problemas comuns

| Sintoma | Causa provável | Solução |
|---|---|---|
| Site não abre em `nordflux.co` mas abre em `.netlify.app` | DNS ainda propagando | Espere mais 30 min |
| Erro de certificado SSL | Proxy do Cloudflare está ligado (laranja) | Deixe DNS only (cinza) no Cloudflare |
| Formulário não envia | Esqueceu o `netlify.toml` ou `data-netlify="true"` | Verifique se enviou o `netlify.toml` ao GitHub |
| PDF abre na aba em vez de baixar | Configuração do navegador | Já está forçado download via `netlify.toml` |
| Fonts não carregam | Pasta `fonts/` não foi enviada ao GitHub | Reenvie |

---

## Em caso de dúvida

Me chama aqui no Claude que eu te ajudo com qualquer etapa específica. Pode mandar print de qualquer tela do Netlify ou Cloudflare que eu te digo o que clicar.
