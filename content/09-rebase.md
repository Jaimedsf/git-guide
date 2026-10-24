---
title: "Rebase"
description: "Reescrever a base de um branch para um histórico linear, e quando não fazer isso."
weight: 9
objetivo: "o que o rebase faz, quando usá-lo em vez do merge, como resolver conflitos durante um rebase e a regra de ouro que evita problemas na equipe."
---

## Conceito

Existem duas formas de integrar o trabalho de um branch que divergiu: **merge** e **rebase**.

- **Merge** preserva a história exatamente como aconteceu e cria um commit de merge.
- **Rebase** "replanta" os seus commits em cima da ponta do outro branch, como se você tivesse começado o trabalho depois. O histórico fica **linear**.

```text
Antes:        A ── B ── E  (main)
                   └── C ── D  (sorvete)

Merge:        A ── B ── E ──────── M  (main)
                   └── C ── D ───┘

Rebase:       A ── B ── E ── C' ── D'  (sorvete)
```

Repare no `C'` e `D'`: são commits **novos**, com o mesmo conteúdo de `C` e `D` mas outro hash. O rebase **reescreve histórico**.

### O que acontece por dentro

Ao rodar `git rebase main` estando em `sorvete`, o Git:

1. encontra o ancestral comum (`B`);
2. guarda temporariamente os commits exclusivos de `sorvete` (`C`, `D`);
3. move `sorvete` para a ponta de `main` (`E`);
4. reaplica os commits guardados, um por um, criando `C'` e `D'`;
5. aponta `sorvete` para o último commit reaplicado.

Depois disso, um `git merge sorvete` no `main` é um simples fast-forward.

### A regra de ouro

<blockquote class="aviso">
<p><strong>Nunca faça rebase de commits que já foram enviados e que outras pessoas podem estar usando.</strong></p>
<p>Como o rebase troca os commits por cópias novas, quem já tinha os antigos fica com um histórico diferente do seu, e a sincronização vira bagunça.</p>
</blockquote>

Na prática: rebase é ótimo para **organizar o seu branch local** antes de compartilhar. Em branches compartilhados (como `main`), use merge.

### Merge ou rebase?

| Situação | Use |
|---|---|
| Atualizar seu branch pessoal com o que entrou no `main` | rebase |
| Integrar trabalho em um branch compartilhado | merge |
| Quer histórico fiel ao que aconteceu | merge |
| Quer histórico linear e fácil de ler | rebase |
| Não tem certeza | merge (é sempre seguro) |

## Mão na massa

Monte divergência:

```bash
git switch main
git switch -c sorvete
echo "## Sorvete de manga" >> sobremesas.md
git add sobremesas.md
git commit -m "adiciona sorvete de manga"

git switch main
echo "## Farofa" >> salgados.md
git commit -am "adiciona farofa"
```

Faça o rebase de `sorvete` sobre `main`:

```bash
git switch sorvete
git rebase main
git log --oneline --graph --all
```

```text
* 7e8f9a0 (HEAD -> sorvete) adiciona sorvete de manga
* 3b4c5d6 (main) adiciona farofa
* ...
```

Linear. Agora integre com fast-forward:

```bash
git switch main
git merge sorvete
```

### Conflitos durante o rebase

Como o rebase reaplica commit a commit, um conflito pode aparecer em qualquer um deles. O processo:

```bash
# 1. resolva o arquivo como no capítulo anterior
git add <arquivo>
git rebase --continue    # segue para o próximo commit
```

Outras saídas:

```bash
git rebase --skip     # descarta o commit atual e segue
git rebase --abort    # desiste e volta ao estado de antes do rebase
```

> **Note:** durante o rebase, `HEAD` (no topo do conflito) é o branch base (`main`) e a versão "que chega" é o seu commit. É o contrário do merge. Leia com atenção antes de escolher.

### Pull com rebase

Em vez de criar um commit de merge toda vez que você puxa alterações:

```bash
git pull --rebase
```

Seus commits locais (ainda não enviados) são replantados sobre o que veio do remoto. Para tornar padrão:

```bash
git config --global pull.rebase true
```

### Bônus: arrumando commits com rebase interativo

Antes de enviar, você pode juntar, reordenar ou renomear commits locais:

```bash
git rebase -i HEAD~3
```

O editor abre a lista dos 3 últimos commits; troque `pick` por `squash` (junta ao anterior), `reword` (renomeia) ou `drop` (descarta). Vale a mesma regra de ouro: só em commits ainda não compartilhados.

## O que observar

- Os hashes dos commits mudam depois do rebase.
- Se você já tinha enviado o branch, o próximo `git push` será recusado. Só force (`git push --force-with-lease`) se o branch for **só seu**.

## Resumo

| Comando | Para quê |
|---|---|
| `git rebase <base>` | replanta o branch atual sobre `<base>` |
| `git rebase --continue` / `--skip` / `--abort` | controla um rebase com conflito |
| `git pull --rebase` | pull sem commit de merge |
| `git rebase -i HEAD~N` | edita os N últimos commits |
| `git push --force-with-lease` | envia histórico reescrito (só em branch seu) |

## Teste-se

<details><summary>Por que o rebase muda os hashes?</summary>
Porque o hash depende do conteúdo e do commit pai. Ao reaplicar sobre outra base, o pai muda e o Git cria commits novos.
</details>

<details><summary>Seu colega já baixou o branch feature. Pode fazer rebase dele?</summary>
Não deveria (regra de ouro). Use merge para trazer as atualizações, ou combine com o colega antes.
</details>
