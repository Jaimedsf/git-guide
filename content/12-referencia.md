---
title: "Referência rápida"
description: "Todos os comandos do guia em um só lugar, e o fluxo do dia a dia."
weight: 12
objetivo: "onde encontrar rapidamente cada comando visto no guia."
---

## Fluxo do dia a dia

```bash
git switch main && git pull          # 1. comece atualizado
git switch -c minha-tarefa           # 2. um branch por tarefa
# ... edita arquivos ...
git status                           # 3. confira
git add <arquivos>                   # 4. prepare
git commit -m "descreve a mudança"   # 5. registre
git push -u origin minha-tarefa      # 6. envie e abra um PR
```

## Configuração

| Comando | Descrição |
|---|---|
| `git config --global user.name "Nome"` | define seu nome |
| `git config --global user.email "e@mail"` | define seu e-mail |
| `git config --global init.defaultBranch main` | branch inicial padrão |
| `git config --global core.editor "code --wait"` | editor padrão |
| `git config --global pull.rebase true` | pull com rebase por padrão |
| `git config --global push.autoSetupRemote true` | push cria upstream sozinho |
| `git config --list` | lista configurações |

## Repositório e commits

| Comando | Descrição |
|---|---|
| `git init` | cria repositório na pasta atual |
| `git status` | estado dos arquivos |
| `git add <arquivo>` / `git add .` | prepara alterações |
| `git commit -m "msg"` | cria commit |
| `git commit -am "msg"` | prepara arquivos rastreados e commita |
| `git log --oneline --graph --all` | histórico desenhado |
| `git show <hash>` | detalhes de um commit |
| `git diff` / `git diff --staged` | alterações não preparadas / preparadas |

## Branches e merge

| Comando | Descrição |
|---|---|
| `git branch` | lista branches locais |
| `git branch -a` | inclui remotos |
| `git branch -vv` | mostra upstreams |
| `git switch <b>` | muda de branch |
| `git switch -c <b>` | cria e muda |
| `git switch -` | volta ao branch anterior |
| `git switch --detach <hash>` | visita commit antigo |
| `git merge <b>` | integra `<b>` no branch atual |
| `git merge --abort` | cancela merge com conflito |
| `git branch -d <b>` / `-D` | exclui branch / força exclusão |

## Remotos

| Comando | Descrição |
|---|---|
| `git clone <url>` | copia repositório remoto |
| `git remote add origin <url>` | registra remoto |
| `git remote -v` | lista remotos |
| `git push -u origin <b>` | primeiro envio de um branch |
| `git push` | envia commits |
| `git fetch` | baixa sem integrar |
| `git fetch --prune` | limpa branches remotos apagados |
| `git pull` / `git pull --rebase` | baixa e integra |
| `git push origin --delete <b>` | apaga branch remoto |

## Rebase

| Comando | Descrição |
|---|---|
| `git rebase <base>` | replanta branch atual sobre `<base>` |
| `git rebase --continue` | segue após resolver conflito |
| `git rebase --skip` | pula o commit atual |
| `git rebase --abort` | cancela o rebase |
| `git rebase -i HEAD~N` | edita os N últimos commits |
| `git push --force-with-lease` | envia histórico reescrito (só branch seu) |

## Desfazendo

| Comando | Descrição |
|---|---|
| `git restore <arquivo>` | descarta alterações não preparadas |
| `git restore --staged <arquivo>` | tira da preparação |
| `git commit --amend` | refaz o último commit |
| `git revert <hash>` | cria commit que desfaz outro |
| `git reset --soft\|--mixed\|--hard <ref>` | move o branch para trás |
| `git stash` / `git stash pop` | guarda / recupera alterações |
| `git reflog` | histórico de onde o HEAD passou |

## Glossário

| Termo | Significado |
|---|---|
| **Repositório** | pasta com histórico Git (`.git`) |
| **Commit** | foto do projeto com autor, data, mensagem e pai |
| **Hash** | identificador único de um commit |
| **Staging area / index** | área de preparação do próximo commit |
| **Branch** | ponteiro móvel para um commit |
| **HEAD** | onde você está agora |
| **Remoto / origin** | cópia do repositório em outro lugar / nome padrão dela |
| **Upstream** | branch remoto ligado ao seu branch local |
| **Fast-forward** | merge que só move o ponteiro |
| **Conflito** | alterações concorrentes nas mesmas linhas |
| **Rebase** | reaplicar commits sobre outra base |
| **Pull request** | pedido de revisão e merge no serviço de hospedagem |
| **Fork** | cópia de um repositório na sua conta do serviço |

## Para ir além

- [Pro Git](https://git-scm.com/book/pt-br/v2), livro oficial e gratuito, em português.
- [Learn Git Branching](https://learngitbranching.js.org/?locale=pt_BR), exercícios visuais interativos.
- `git help <comando>`, manual completo de qualquer comando.
