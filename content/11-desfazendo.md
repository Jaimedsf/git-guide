---
title: "Desfazendo coisas"
description: "restore, amend, revert, reset, stash e reflog: como voltar atrás sem medo."
weight: 11
objetivo: "escolher o comando certo para desfazer cada tipo de erro, sabendo quais são seguros e quais apagam trabalho."
---

## Conceito

A pergunta certa é: **onde está o erro?** Cada área do Git tem seu comando.

| Situação | Comando | Perde trabalho? |
|---|---|---|
| Quero descartar alterações **não preparadas** de um arquivo | `git restore <arquivo>` | **sim**, as alterações somem |
| Preparei (`add`) sem querer | `git restore --staged <arquivo>` | não |
| Errei a mensagem ou esqueci um arquivo no **último commit** (ainda não enviado) | `git commit --amend` | não |
| Quero desfazer um commit **já enviado** | `git revert <hash>` | não (cria commit inverso) |
| Quero apagar commits **locais** | `git reset` | depende da opção |
| Preciso trocar de branch mas não quero commitar ainda | `git stash` | não |
| Fiz besteira e quero achar um commit "perdido" | `git reflog` | não |

## Mão na massa

### restore: arquivos

```bash
git restore receitas.md            # volta o arquivo à última versão commitada
git restore --staged receitas.md   # tira da preparação, mantém a edição
```

### amend: consertar o último commit

```bash
git add arquivo-esquecido.md
git commit --amend -m "mensagem corrigida"
```

O amend **substitui** o último commit por um novo. Só use se ainda não deu push.

### revert: desfazer de forma segura

```bash
git revert e4f5a6b
```

Cria um commit novo que faz o oposto do commit `e4f5a6b`. O histórico fica intacto, por isso é a forma certa de desfazer algo que já está no remoto.

### reset: voltar o ponteiro do branch

```bash
git reset --soft HEAD~1    # desfaz o commit, mantém alterações PREPARADAS
git reset HEAD~1           # (--mixed, padrão) desfaz o commit, mantém alterações NÃO preparadas
git reset --hard HEAD~1    # desfaz o commit E APAGA as alterações
```

`HEAD~1` significa "um commit antes de HEAD".

<blockquote class="aviso">
<p><strong>Cuidado:</strong> <code>git reset --hard</code> descarta alterações não commitadas para sempre. E, como o rebase, <code>reset</code> reescreve histórico: não use em commits já enviados.</p>
</blockquote>

### stash: guardar para depois

```bash
git stash            # guarda alterações e limpa o diretório de trabalho
git switch outro-branch
# ... resolve o que precisava ...
git switch -
git stash pop        # traz as alterações de volta
git stash list       # lista o que está guardado
```

### reflog: a rede de segurança

O Git registra todo lugar por onde o HEAD passou nos últimos meses, inclusive commits "apagados" por reset ou rebase:

```bash
git reflog
```

```text
9a8b7c6 HEAD@{0}: reset: moving to HEAD~1
f1e2d3c HEAD@{1}: commit: adiciona feijoada
```

Para recuperar:

```bash
git branch resgate f1e2d3c
```

## O que observar

- Comandos que **criam** commits (`revert`) são seguros em histórico compartilhado. Comandos que **reescrevem** (`amend`, `reset`, `rebase`) são para histórico local.
- O que nunca foi commitado nem guardado em stash não aparece no reflog. Commite cedo.

## Resumo

Na dúvida, siga esta ordem de preferência: `restore` → `revert` → `reset --soft/--mixed` → `reset --hard` (último recurso). E lembre do `reflog`.

## Teste-se

<details><summary>Um commit com bug já está no main remoto. Como desfazer?</summary>
<code>git revert &lt;hash&gt;</code> e push. Não use reset, que reescreveria um histórico que outras pessoas já têm.
</details>

<details><summary>Qual a diferença entre reset --soft e --hard?</summary>
Ambos movem o branch para trás. <code>--soft</code> mantém as alterações preparadas; <code>--hard</code> as apaga do diretório de trabalho.
</details>
