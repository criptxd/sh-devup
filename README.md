```text
███████╗██╗  ██╗   ██████╗ ███████╗██╗   ██╗██╗   ██╗██████╗
██╔════╝██║  ██║   ██╔══██╗██╔════╝██║   ██║██║   ██║██╔══██╗
███████╗███████║   ██║  ██║█████╗  ██║   ██║██║   ██║██████╔╝
╚════██║██╔══██║   ██║  ██║██╔══╝  ╚██╗ ██╔╝██║   ██║██╔═══╝
███████║██║  ██║██╗██████╔╝███████╗ ╚████╔╝ ╚██████╔╝██║
╚══════╝╚═╝  ╚═╝╚═╝╚═════╝ ╚══════╝  ╚═══╝   ╚═════╝ ╚═╝
```

`devup` mostra quais dos teus projetos de desenvolvimento estão rodando na tua
máquina e sobe ou derruba eles — um ou todos, num comando só.

```console
$ devup

  ◆ web    http://localhost:5173
  ◆ api    http://localhost:8000
  ◇ admin  http://localhost:4173
```

Este repo é um **template**. Você não escreve o `devup.sh` — o Claude escreve pra
ti. Abra o Claude Code na pasta do repo, cole o prompt abaixo, e ele descobre
teus projetos e teu sistema (SO, onde o `PATH` aponta), pergunta o que faltar e
gera um `devup.sh` sob medida, mais um `setup.sh` que registra o comando `devup`.
É só pra rodar as coisas localmente — nunca sobe túnel nem produção.

## O prompt

Abra o Claude Code na pasta do repo e cole:

```
Leia o CLAUDE.md e faça o bootstrap do devup pra esta máquina.
```

## Como usar

**1.** Clone e abra o Claude:

```sh
git clone https://github.com/criptxd/sh-devup && cd sh-devup
claude
```

**2.** Cole o prompt. Responda as perguntas do Claude — onde ficam teus projetos,
em que porta cada um sobe, o que é produção pra deixar de fora. Ele escreve
`devup.sh` + `setup.sh` e roda o setup.

**3.** Reabra o terminal. Agora:

```sh
devup            # o que está no ar
devup up         # sobe tudo o que está parado
devup up api     # sobe só um
devup down       # derruba tudo
devup down web   # derruba só um
```

Criou um projeto novo depois? Peça pro Claude adicionar no `devup.sh` — a skill
fica instalada e ele sabe como.

## Exemplo

Alguém com um front (`npm run dev`, porta 5173) e uma API em `docker compose`
(porta 8000). O bootstrap é uma conversa curta:

```text
você   › Leia o CLAUDE.md e faça o bootstrap do devup pra esta máquina.
claude › Achei dois projetos. O front tem script "dev" — que porta?
você   › 5173
claude › A API tem compose. Algum serviço é de produção?
você   › não, pode subir tudo
claude › Escrevi devup.sh e setup.sh e rodei o setup.
         ~/.local/bin já está no PATH — devup funciona agora.
```

Daí em diante:

```console
$ devup up
  ◆ web  iniciado
  ◆ api  iniciado

$ devup
  ◆ web  http://localhost:5173
  ◆ api  http://localhost:8000
```

## O que fica no repo

Versionado (o template): os `*.example`, `skills/devup/SKILL.md`, `CLAUDE.md`.
Gerado pra ti e fora do git: `devup.sh`, `setup.sh`, `.devup/` (logs e pids).
