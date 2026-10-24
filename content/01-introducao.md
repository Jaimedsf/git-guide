---
title: "O que é Git e como instalar"
description: "Controle de versão, terminal básico, instalação e configuração inicial."
weight: 1
objetivo: "o que é controle de versão, como usar o terminal o suficiente para o Git e como deixar o Git instalado e configurado."
---

## Conceito

### O problema que o Git resolve

Você já salvou arquivos assim?

```text
trabalho.docx
trabalho_final.docx
trabalho_final_AGORA_VAI.docx
trabalho_final_revisado_joao.docx
```

Isso é controle de versão feito à mão: confuso, frágil e impossível de combinar quando duas pessoas editam ao mesmo tempo. O **Git** é um *sistema de controle de versão*: ele registra cada versão do projeto como uma "foto" (chamada **commit**), deixa você voltar a qualquer foto antiga e junta o trabalho de várias pessoas de forma organizada.

Três ideias para guardar desde já:

1. **Git ≠ GitHub.** Git é o programa que roda no seu computador. GitHub, GitLab e Bitbucket são *sites* que hospedam repositórios Git na internet.
2. **Git é distribuído.** Cada pessoa tem uma cópia completa do histórico. Dá para trabalhar offline.
3. **Git trabalha com qualquer arquivo de texto**, não só código: anotações, documentação, configurações.

### Terminal ou interface gráfica?

Existem interfaces gráficas para Git (VS Code, GitHub Desktop etc.), mas este guia usa a **linha de comando**. Motivo: todo tutorial, documentação e mensagem de erro fala em comandos. Quem entende os comandos consegue usar qualquer interface; o contrário não é verdade.

### O mínimo de terminal que você precisa

| Comando | O que faz |
|---|---|
| `pwd` | mostra em qual pasta você está |
| `ls` (`dir` no CMD do Windows) | lista os arquivos da pasta |
| `cd pasta` | entra em uma pasta |
| `cd ..` | volta uma pasta |
| `mkdir nome` | cria uma pasta |
| `clear` | limpa a tela |

Um comando tem três partes: o **programa**, as **opções** (começam com `-` ou `--`) e os **argumentos**:

```text
git   commit   -m   "minha mensagem"
│     │        │    └─ argumento da opção
│     │        └─ opção
│     └─ subcomando
└─ programa
```

## Mão na massa

### 1. Instale o Git

- **Windows:** baixe em [git-scm.com/download/win](https://git-scm.com/download/win) e instale com as opções padrão. Use o programa **Git Bash** como terminal.
- **macOS:** no Terminal, rode `git --version`. Se não estiver instalado, o sistema oferece instalar as *Command Line Tools*. Alternativa: `brew install git`.
- **Linux (Debian/Ubuntu):** `sudo apt install git`. Fedora: `sudo dnf install git`.

Confira:

```bash
git --version
# git version 2.x.x
```

### 2. Diga ao Git quem é você

Todo commit registra autor e e-mail. Configure uma vez por computador:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "voce@exemplo.com"
```

### 3. Ajustes recomendados

```bash
# nome do branch inicial em novos repositórios
git config --global init.defaultBranch main

# editor usado pelo Git (ex.: VS Code); troque por nano, vim etc. se preferir
git config --global core.editor "code --wait"
```

Veja tudo o que está configurado:

```bash
git config --list
```

> **Dica:** `--global` grava a configuração para o seu usuário em todos os repositórios. Sem `--global`, ela vale só para o repositório atual.

### 4. Crie a pasta do projeto de exemplo

```bash
cd ~
mkdir receitas
cd receitas
pwd
```

## Resumo

- Git guarda o histórico do projeto em *commits* e permite colaboração.
- Git é o programa; GitHub/GitLab são serviços de hospedagem.
- `git config --global user.name` e `user.email` são obrigatórios antes do primeiro commit.

## Teste-se

<details><summary>Qual a diferença entre Git e GitHub?</summary>
Git é o sistema de controle de versão que roda na sua máquina. GitHub é um serviço online que hospeda repositórios Git e adiciona recursos de colaboração (pull requests, issues).
</details>

<details><summary>O que acontece se você esquecer de configurar user.name e user.email?</summary>
O Git recusa o commit (ou tenta adivinhar a partir do sistema e exibe um aviso). O autor do commit fica errado ou o commit não é criado.
</details>
