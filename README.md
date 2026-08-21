# Loto 7 — site estático (PT / EN / JA)

Site puro em HTML/CSS/JS (sem build, sem dependências) com 3 idiomas, tabela de
resultados, estatísticas e gerador de palpites com salvamento.

## Estrutura (tudo na raiz, sem subpastas)

```
index.html   → versão em português (já vem com este nome, é a página inicial)
en.html      → versão em inglês
ja.html      → versão em japonês
draws.json   → ÚNICA fonte de dados (75 sorteios). As 3 páginas leem daqui.
```

Nenhuma subpasta é necessária — de propósito, para evitar o problema comum de
uploads pelo celular bagunçarem o `/` ao tentar criar uma pasta `data/`.

## Como funciona a sincronização entre os 3 idiomas

As páginas **não têm os números dos sorteios copiados dentro delas**. Toda vez
que uma página abre, ela busca `draws.json` via `fetch()`. Isso significa:

- **Atualizar um sorteio novo = editar `draws.json` uma única vez.** As 3
  páginas passam a mostrá-lo automaticamente, sem precisar tocar em nenhum
  arquivo `.html`.
- O campo `"o"` (observações) dentro de cada sorteio já vem com as 3 traduções
  prontas: `{ "pt": "...", "en": "...", "ja": "..." }`.
- Sempre que você tiver novos sorteios confirmados, é só me mandar aqui no
  chat — eu atualizo o `draws.json` e regenero os 3 arquivos de uma vez.

## Publicar no GitHub Pages (grátis, ~5 minutos)

1. Crie um repositório novo no GitHub (público, para o plano gratuito de
   Pages funcionar sem custo).
2. Suba os 4 arquivos direto na raiz do repositório: `index.html`, `en.html`,
   `ja.html`, `draws.json`. Pela interface web: **Add file → Upload files**,
   arraste os 4, **Commit changes**. Não precisa renomear nada — já vêm com
   os nomes certos.
3. Vá em **Settings → Pages**, em "Source" escolha **Deploy from a branch**,
   branch `main`, pasta `/ (root)`. Salve.
4. Em 1–2 minutos o site estará em `https://seu-usuario.github.io/seu-repo/`.

## Testar localmente antes de publicar

Abrir o `.html` direto no navegador (duplo clique) **não funciona** — o
navegador bloqueia o `fetch()` de `draws.json` por segurança (erro de CORS
com `file://`). Rode um servidor local simples primeiro:

```bash
cd loto7-site
python3 -m http.server 8000
# depois abra http://localhost:8000/index.html
```

## Onde ficam os dados salvos pelo usuário

- Sorteios adicionados manualmente (botão "Adicionar Novo") e palpites
  salvos no gerador usam `localStorage` do navegador — ficam só no aparelho
  de quem usou, não vão para o GitHub nem para outros visitantes.
- Se a página for aberta dentro do Claude (como artifact), ela detecta isso
  automaticamente e usa o armazenamento do Claude em vez do `localStorage`.
  Isso é automático, você não precisa configurar nada.

## Funcionalidade: salvar palpites gerados

Na aba "Gerador de Palpites", depois de gerar uma combinação aparece o botão
**"Salvar este palpite"**. Os palpites salvos ficam listados logo abaixo,
com opção de excluir cada um individualmente.
