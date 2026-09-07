# sh-devup — template do comando `devup`

`devup` sobe / derruba / mostra os projetos de **desenvolvimento local** de UMA
pessoa — 1 ou vários — com um disparo só. Fire-and-forget, nunca duplica, nunca
sobe produção.

Este repo é o **template**. O que é por-pessoa fica no `.gitignore`:
`devup.sh`, `setup.sh`, `.devup/` (estado/logs). Versionado: os `*.example`,
`skills/devup/SKILL.md`, este arquivo.

## Se você é o Claude e acabou de abrir este repo

Faça o bootstrap para esta pessoa. Se `devup.sh` e `setup.sh` já existem, pule para o 4.

1. **Pergunte** (em prosa, sem formulário): a **pasta-base** onde ficam os projetos
   de dev dela (ex.: `~/code`, `~/Sandbox`) e/ou o **nome de cada projeto**. Com só
   a pasta-base, liste os subdiretórios e proponha o tipo de cada um:
   - tem `compose.yaml` / `docker-compose.yml` → tipo **docker** (pergunte se algum
     serviço é de produção, para deixar de fora);
   - tem `package.json` com script `dev` → tipo **process** (`npm run dev`) —
     pergunte em que porta.
2. `cp sh-devup/devup.sh.example sh-devup/devup.sh` e preencha a seção **PROJETOS**:
   ajuste `ROOT` (ou deixe derivar, se o repo está dentro da pasta-base), `PROJECTS`
   e um bloco de config por projeto (modelos comentados no `.example`). Não escreva
   nome de projeto fora dessa seção.
3. **Escreva `sh-devup/setup.sh` dedicado a ESTA máquina** — não copie o `.example`
   cru e não ponha `uname`/ramos de SO nele: ele serve a uma máquina só, então é
   POSIX curto e direto. Ele sempre faz duas coisas — `cp` da skill para
   `~/.claude/skills/devup/` e criar o comando `devup`. Só o "como criar o comando"
   depende do ambiente; **você** decide agora, com a pessoa, e escreve o resultado:
   - `uname -s` + onde o `PATH` dela aponta. Procure pasta gravável já no `PATH`:
     `~/.local/bin`, `~/bin`, `/opt/homebrew/bin` (macOS ARM), `/usr/local/bin`.
     Achou → `ln -sf` do `devup.sh` lá. Fim.
   - `~/.local/bin` não existe → **pergunte** se pode criar (padrão XDG). Sim → o
     setup faz `mkdir -p` e você lembra a pessoa de pôr no `PATH` (rc do shell:
     bash `~/.bashrc`, zsh `~/.zshrc`, fish `fish_add_path`). Não → tente
     `/usr/local/bin` (o `ln` aí pede `sudo` — rode você, ou peça à pessoa).
   - Nada disso serve → o setup só instala a skill; instrua
     `alias devup='<caminho>/sh-devup/devup.sh'`.
   `setup.sh.example` é o esqueleto do caso comum — use como ponto de partida.
   - **WSL conta como Linux** — usa o `devup.sh` normal.

   **Windows nativo (PowerShell, fora do WSL):** não use o `devup.sh`. Escreva um
   `sh-devup/devup.ps1` equivalente — mesmos comandos (sem args = status, `up`,
   `down`, `help`), mesmo `◆` / `◇`, mesma seção PROJETOS no topo, tipos
   `docker` e `process` — usando o `devup.sh` como referência. O "setup" no Windows
   é: copiar `skills\devup\SKILL.md` para `%USERPROFILE%\.claude\skills\devup\` e
   deixar `devup` chamável (função no `$PROFILE`, ou um `devup.cmd` numa pasta do
   `PATH`). Ainda **não há** `.example` de PowerShell; quando escrever o primeiro,
   considere versioná-lo como `devup.ps1.example` / `setup.ps1.example`.
4. `sh sh-devup/setup.sh`.
5. Confira: `sh -n sh-devup/devup.sh` e `devup` (ou o caminho/alias, se sem link).

## Depois

- Projeto novo → mais um bloco de config em `devup.sh` (a skill `devup`, já global,
  lembra o Claude de oferecer isso quando a pessoa cria/clona um projeto).
- Editou `skills/devup/SKILL.md` → replique na cópia instalada (`setup.sh` faz isso),
  e vice-versa. `diff` entre as duas deve ser vazio.
