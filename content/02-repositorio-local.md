---
title: "Repositório local e as três áreas"
description: "git init, a pasta .git e o caminho que um arquivo percorre até virar histórico."
weight: 2
objetivo: "criar um repositório e entender as três áreas do Git: diretório de trabalho, área de preparação e repositório."
---

## Conceito

### O que é um repositório

Um **repositório** é uma pasta de projeto cujo histórico o Git está acompanhando. O que transforma uma pasta comum em repositório é uma subpasta oculta chamada **`.git`**: é lá que o Git guarda todos os commits, branches e configurações.

> **Atenção:** nunca edite nem apague a pasta `.git` à mão. Apagar `.git` = apagar todo o histórico.

### As três áreas

Este é o conceito mais importante do guia. Todo arquivo passa por três lugares:

```mermaid
flowchart LR
  WD["Diretório de trabalho<br/>(arquivos que você edita)"] -- "git add" --> ST["Área de preparação<br/>(staging / index)"]
  ST -- "git commit" --> RE["Repositório<br/>(histórico de commits)"]
```

| Área | O que é | Analogia |
|---|---|---|
| **Diretório de trabalho** | os arquivos que você vê e edita no editor | a mesa onde você monta as coisas |
| **Área de preparação** (*staging area* ou *index*) | a lista do que vai entrar no próximo commit | a caixa onde você coloca o que vai ser fotografado |
| **Repositório** | o histórico de commits salvo dentro de `.git` | o álbum de fotos |

Por que existe uma área intermediária? Para você **escolher** o que entra em cada commit. Você pode ter mexido em 5 arquivos e querer registrar só 2 agora.

### Estados de um arquivo

- **Não rastreado** (*untracked*): arquivo novo que o Git nunca viu.
- **Preparado** (*staged*): está na área de preparação, entra no próximo commit.
- **Não modificado**: igual à versão do último commit.
- **Modificado**: rastreado, mas alterado desde o último commit.

## Mão na massa

Dentro da pasta `receitas` criada no capítulo anterior:

```bash
git init
```

Saída esperada: `Initialized empty Git repository in .../receitas/.git/`

Veja a pasta oculta:

```bash
ls -a
# .  ..  .git
```

Agora o comando que você mais vai usar na vida:

```bash
git status
```

```text
On branch main
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

Crie um arquivo com seu editor (ou pelo terminal):

```bash
echo "# Minhas receitas" > receitas.md
git status
```

```text
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        receitas.md
```

Prepare o arquivo:

```bash
git add receitas.md
git status
```

```text
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   receitas.md
```

## O que observar

- `git init` só precisa ser rodado **uma vez** por projeto.
- `git status` diz em qual branch você está e em qual área está cada arquivo. Rode-o o tempo todo; ele também sugere o próximo comando.
- Depois do `git add`, o arquivo saiu de *Untracked* e foi para *Changes to be committed*. Ainda **não há commit**: o arquivo só está preparado.

## Resumo

| Comando | Para quê |
|---|---|
| `git init` | transforma a pasta atual em repositório |
| `git status` | mostra o estado dos arquivos nas três áreas |
| `git add <arquivo>` | move alterações para a área de preparação |

## Teste-se

<details><summary>Você rodou git init na pasta errada. Como desfazer?</summary>
Apague a pasta <code>.git</code> daquela pasta (<code>rm -rf .git</code>). Os seus arquivos continuam lá; só o "rastreamento" some. Faça isso apenas se não houver histórico que você queira manter.
</details>

<details><summary>Para que serve a área de preparação?</summary>
Para montar o próximo commit escolhendo exatamente quais alterações entram nele, em vez de registrar tudo o que mudou de uma vez.
</details>
