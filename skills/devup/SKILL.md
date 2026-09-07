---
name: devup
description: O comando `devup` sobe / derruba / mostra os projetos de desenvolvimento local da pessoa — 1 ou vários — com um disparo só. Use quando ela pedir "devup", "sobe o ambiente local", "status dos projetos", "derruba o <projeto>", ou nomear um projeto para rodar/parar. E quando ela criar ou clonar um projeto de desenvolvimento: ofereça registrá-lo no devup.
---

# devup — subir os projetos de dev local

`devup` é um atalho pessoal e fluido para subir os projetos de desenvolvimento
local de UMA pessoa. Cada projeto é um bloco de config no `devup.sh`; o resto do
script é genérico e serve para 1 ou para vários. **Não é memória de projeto** — é
só startup simplificado, sem nada de produção.

Origem: o repo-template `sh-devup/`. Versionados: `devup.sh.example`,
`setup.sh.example`, este `SKILL.md`, o `CLAUDE.md`. Por-pessoa e no `.gitignore`:
`devup.sh`, `setup.sh`, `.devup/`.

## Comandos

    devup                 status   (◆ no ar  ·  ◇ parado)
    devup up   [projeto]  sobe tudo, ou só um
    devup down [projeto]  derruba tudo, ou só um
    devup help

Dispara e sai. Já no ar → pulado, nunca duplica. Projeto do tipo `process` (um dev
server, p.ex.) roda destacado (`setsid`) e sobrevive ao terminal.

## Onde as coisas ficam

- **O script:** `readlink -f "$(command -v devup)"` → `<clone>/sh-devup/devup.sh`
  (no Windows nativo, fora do WSL, é um `devup.ps1` no mesmo repo).
- **Projetos registrados agora:** rode `devup`, ou leia `<sh-devup>/.devup/projects.tsv`.
- **Config de projeto:** só na seção `PROJETOS` no topo do `devup.sh`. Dois tipos:
  - `docker`  → `TYPE_x=docker`,  `COMPOSE_x`, `SERVICES_x` (vazio = todos os serviços; liste só alguns para deixar o de produção de fora), `URL_x`
  - `process` → `TYPE_x=process`, `DIR_x`, `CMD_x`, `PORT_x`, `URL_x`
  mais o nome em `PROJECTS`. Nada de nome de projeto fora dessa seção.
- Caminhos são relativos a `$ROOT` (a pasta-base dos projetos; `DEVUP_ROOT` sobrescreve).

## Quando a pessoa criar/clonar um projeto novo

Ofereça **registrá-lo no devup**. Se ela topar: descubra como o projeto sobe (dev
server e porta? `docker compose`? quais serviços?), acrescente um bloco de config
no `devup.sh`, ajuste `PROJECTS`, e confira com `sh -n devup.sh` e `devup`. Nunca
inclua serviço de produção (túnel, deploy).

## Bootstrap (primeiro uso do repo, ainda sem `devup.sh`)

Siga o `CLAUDE.md` do repo `sh-devup/`: pergunte a pasta-base e/ou os projetos,
gere `devup.sh` do `.example` (seção PROJETOS), **escreva** um `setup.sh` dedicado
à máquina (SO + PATH da pessoa; esqueleto no `.example`), rode `setup.sh`.

## Este skill tem duas cópias, conteúdo idêntico

- instalada: `~/.claude/skills/devup/SKILL.md` — a que carrega nas sessões
- no repo:   `<clone>/sh-devup/skills/devup/SKILL.md` — versionada

**Alterou uma, altere a outra** — `diff` entre as duas deve ser vazio. `setup.sh`
copia a do repo por cima da instalada.
