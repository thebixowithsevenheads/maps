# maps

Roadmaps de estudo, abertos e mantidos pela comunidade.
Parte de [thebixowithsevenheads.wtf](https://thebixowithsevenheads.wtf/).

Site estático, sem build no CI, sem dependências em produção: são apenas
arquivos HTML, CSS, fontes e imagens servidos direto do repositório.

## Estrutura do repositório

```
.
├── .nojekyll           # serve os arquivos como estão no GitHub Pages
├── index.html          # gerado — lista de todos os roadmaps
├── style.css           # gerado — folha de estilo compartilhada
├── md/
│   └── reverse.md      # FONTE DA VERDADE — é aqui que você edita
├── maps/
│   └── reverse.html    # gerado a partir de md/reverse.md
├── fonts/              # GohuFont (ver GOHUFONT-LICENSE.txt)
├── img/                # fundo e ícones
└── favicon.ico
```

## Como contribuir

**Você edita apenas os arquivos `.md` dentro de `md/`.**

A pasta `maps/` contém os `.html` *gerados*, e o `style.css` também é gerado —
não edite nenhum dos dois, qualquer alteração ali será perdida na próxima
compilação.

1. Faça um fork deste repositório.
2. Edite um roadmap existente em `md/`, ou crie um novo `.md` na mesma pasta.
3. Faça o commit **apenas do `.md`**.
4. Abra um pull request descrevendo o que você mudou ou adicionou.
5. Um mantenedor revisa o conteúdo, compila o HTML e faz o merge.

Não é preciso rodar nada na sua máquina, nem gerar HTML: **a compilação é
responsabilidade do mantenedor.** O conversor não faz parte deste repositório.

### Criando um roadmap novo

Crie `md/nome-do-roadmap.md`. O nome do arquivo vira a URL
(`maps/nome-do-roadmap.html`), então use letras minúsculas, sem espaços
e sem acentos. Ele aparece sozinho na página inicial depois de compilado.

## Estrutura do `.md`

Todo roadmap segue a mesma forma. **Seção → Tópicos → Recursos**, repetido
quantas vezes for preciso:

- **frontmatter** — `title:`, `desc:` e `author:` no topo do arquivo
- `## Seção` — uma etapa do roadmap, na ordem em que se estuda
- `### Tópicos` — **o que** aprender nessa etapa
- `### Recursos` — **onde** aprender: links de cursos, livros, labs

Título, descrição e autor ficam **todos no frontmatter** — nada disso vai no
corpo. Depois do frontmatter, o arquivo começa direto no primeiro `## Seção`.

Copie e preencha:

````markdown
---
title: Nome do Roadmap
desc: Uma linha dizendo do que trata o roadmap.
author: seu-usuario
---

## Primeira Seção

### Tópicos

- Assunto que se estuda aqui
- Outro assunto

### Recursos

- [Nome do material | Autor](https://exemplo.com)
- [Material em inglês (en)](https://exemplo.com)

## Segunda Seção

### Tópicos

- Mais um assunto

### Recursos

- [Outro material | Autor](https://exemplo.com)
````

Regras do conteúdo:

- Título, descrição e autor só no frontmatter — nunca soltos no corpo.
- `desc:` em uma linha só, sem ponto final obrigatório. É o que aparece no índice.
- Seções na ordem de estudo — quem lê de cima para baixo segue o caminho.
- Tópico é assunto, não link. Recurso é sempre link.
- Formato do recurso: `[Nome do material | Autor](url)`.
- Material que não está em português leva `(en)` no fim do nome.
- Uma seção pode ter só `### Tópicos`, ou só `### Recursos`. As duas é o padrão.

### Sintaxe aceita

| Sintaxe | Resultado |
|---|---|
| `---` … `---` no topo | Frontmatter. |
| `title:` no frontmatter | Nome do roadmap: página inicial, `<title>` da aba e cabeçalho. |
| `desc:` no frontmatter | Uma linha sobre o roadmap: cabeçalho da página, cartão do índice e `<meta description>`. |
| `author:` no frontmatter | Seu crédito na página. |
| `# Título` | Alternativa ao `title:`. Vale só se não houver `title:` no frontmatter. |
| `## Seção` | Etapa do roadmap. |
| `### Subtítulo` | Divisão dentro da etapa (`Tópicos`, `Recursos`…). |
| `- item` | Item de lista. |
| `[texto](url)` | Link. Links externos abrem em nova aba. |
| `**texto**` | Negrito. |
| `` `texto` `` | Código. |
| linha em branco | Separa parágrafos. |

O nome do roadmap sai nesta ordem: `title:` do frontmatter, senão o primeiro
`# H1`, senão o nome do arquivo. O `# H1` é sempre consumido como cabeçalho —
ter `title:` e `# H1` no mesmo arquivo não duplica o título na tela.

Não há suporte a tabelas, imagens, blocos de código, citações ou HTML bruto.
Qualquer coisa fora da tabela acima vira texto simples: não quebra a página,
só não vira formatação. HTML escrito no `.md` é escapado e aparece literal,
nunca é executado.

## Para o mantenedor

O conversor (`build.py`, Python 3, sem dependências) fica **apenas na máquina
do mantenedor** e está listado no `.gitignore`. Depois de revisar e aplicar um
pull request:

```sh
python3 build.py
```

Ele lê todo `md/*.md` e reescreve `maps/*.html`, `index.html` e `style.css`.
Um `.html` em `maps/` que não tenha mais o `.md` correspondente é apagado, então
renomear ou remover um roadmap não deixa página órfã no ar. Faça o commit dos
arquivos gerados junto com o `.md` aprovado.

Os links para o GitHub saem do `git remote origin` (SSH ou HTTPS, tanto faz) e
da branch atual — não há nada para configurar. Sem remote configurado, cai em
`REPO_URL_FALLBACK`. Para fixar valores, edite `REPO_URL` e `REPO_BRANCH` no
topo do script.

O botão "voltar à casa" da página inicial sai de `HOME_URL` / `HOME_LABEL`, no
mesmo lugar. `HOME_URL = None` remove o botão.

O arquivo `.nojekyll` desliga o processamento Jekyll do GitHub Pages: o site é
servido exatamente como está no repositório.

## Licença

A fonte GohuFont está em `fonts/` sob a sua própria licença — veja
`fonts/GOHUFONT-LICENSE.txt`.
