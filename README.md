# Git na Prática

Guia didático em português para aprender Git do zero: commits, branches, merge, repositórios remotos, conflitos, rebase, pull requests e como desfazer erros.

Site: https://jaimedsf.github.io/git-guide/

## Rodando localmente

Requer [Hugo](https://gohugo.io) (extended).

```bash
hugo server
```

Abra http://localhost:1313/git-guide/.

## Estrutura

- `content/` capítulos em Markdown (ordem definida por `weight`)
- `layouts/` templates do site
- `static/css/style.css` estilos
- `.github/workflows/hugo.yml` build e deploy no GitHub Pages a cada push em `gh-pages`
