# playbook-for-ubuntu-devops-laptop

Ansible-плейбук, который превращает чистую Ubuntu-ноутбук в рабочее DevOps-окружение:
пакеты, Docker, клиенты Kubernetes, инструменты IaC, секреты и удобный shell.

- Целевая ОС: **Ubuntu 26.04 (resolute)**, архитектура **x86_64**.
- **Версии не фиксируются** — ставится актуальное на момент запуска.
  Исключение: `terraform`/`terragrunt` плейбук вообще не ставит — они управляются
  через `tfenv`/`tgenv`, версии выбираете сами (см. ниже).
- Стандартная структура Ansible-проекта: `ansible.cfg`, `inventory/`, `group_vars/`, `roles/`.
- Запуск на самом ноутбуке (`localhost`, локальное подключение).
- Системные шаги выполняются через `sudo`, пароль вводит пользователь (`-K`).

---

## Требования

- Ubuntu 26.04 (resolute), x86_64.
- Права `sudo` и доступ в интернет.
- `ansible-core` и `python3-apt` — ставятся один раз вручную (см. ниже):
  плейбук не может поставить Ansible самому себе до первого запуска.

---

## Быстрый старт

```bash
# 1. Bootstrap: один раз вручную
sudo apt update
sudo apt install -y ansible-core python3-apt

# 2. Запуск плейбука (пароль sudo спросит сам)
cd playbook-for-ubuntu-devops-laptop
ansible-playbook -K playbook.yml
```

`-K` — это `--ask-become-pass`. Запускать нужно **от обычного пользователя**,
не через `sudo` (иначе домашние установки — tfenv/tgenv и `~/.config` — уйдут в `/root`).

После установки:

```bash
# применить новое окружение и группу docker
exec newgrp docker      # или просто перелогиниться
source ~/.config/devops-shell.sh
```

`docker` без `sudo` заработает после повторного входа: членство в группе
перечитывается при логине.

Полезные режимы запуска:

```bash
ansible-playbook --syntax-check playbook.yml   # проверка синтаксиса
ansible-playbook -K playbook.yml --check       # dry-run
ansible-playbook -K playbook.yml --tags shell  # только роль shell (base — всегда)
ansible-lint                                   # проверка стиля (profile: production)
```

---

## Структура проекта

```
.
├── ansible.cfg                 # настройки: инвентарь, путь к ролям
├── playbook.yml                # точка входа: список ролей + проверка в конце
├── inventory/
│   └── hosts.yml               # localhost с локальным подключением
├── group_vars/
│   └── all.yml                 # общие переменные проекта
├── roles/
│   ├── base/                   # проверки, каталоги, базовые пакеты, yq
│   ├── git/                    # gh, glab, gitlabcloner, glci, pre-commit, reposcan
│   ├── docker/                 # репозиторий Docker, Engine, Compose V2
│   ├── kubernetes/             # kubectl, helm, k9s, kustomize, kubectx, flux, flux9s,
│   │                           #   stern, flate, flux-tools
│   ├── iac/                    # tflint, terraform-docs, packer, driftctl, tfenv, tgenv
│   ├── runtimes/               # Python, pipx, uv, Go, Node.js, Java
│   ├── secrets/                # direnv, vault, gitleaks
│   ├── shell/                  # eza, zoxide, fzf, starship, bat, JetBrainsMono Nerd Font,
│   │                           #   конфиг shell
│   ├── network/                # nmap, netcat, tcpdump, httpie, dnsutils, telnet,
│   │                           #   rkn-block-checker (pipx)
│   ├── opencode/               # opencode (из npm-реестра), pitwall (GUI-терминал)
│   ├── audio/                  # HushMic (шумоподавление микрофона, .deb)
│   ├── guardrails/             # guardrails-llm-filter (systemd-юнит поверх Docker)
│   └── binaries/               # общий движок установки бинарников (вызывается ролями)
├── .ansible-lint
└── README.md
```

Роль `binaries` не тянет инструменты сама: её вызывают роли `base`, `git`,
`kubernetes`, `iac`, `secrets`, `shell`, `opencode`, передавая список того, что
нужно поставить. Каждая роль хранит свой список в `roles/<роль>/defaults/main.yml`.

Имя `playbook.yml` выбрано осознанно; в чужих проектах точку входа часто называют
`site.yml` — переименование ничего не ломает.

---

## Что делает плейбук

1. Проверяет, что ОС — Ubuntu, архитектура — x86_64, и создаёт служебные каталоги.
2. Ставит пакеты из репозитория Ubuntu (актуальные версии, без пинов).
3. Ставит клиенты GitHub и GitLab: `gh`, `glab`, `gitlabcloner`, а также `glci`
   (локальный запуск GitLab CI), `pre-commit` и `reposcan`.
4. Подключает официальный репозиторий Docker и ставит Engine + Compose V2.
5. Ставит клиенты Kubernetes: kubectl, helm, k9s, kustomize, kubectx, flux, flux9s,
   stern, а также flate и flux-tools (офлайн-рендер и дифф Flux GitOps).
6. Ставит инструменты IaC: tflint, terraform-docs, packer, driftctl, плюс tfenv и tgenv.
7. Ставит рантаймы: Python (venv/pip/pipx), uv, Go, Node.js, Java 21.
8. Ставит direnv, Vault и gitleaks.
9. Настраивает shell: `~/.config/devops-shell.sh` + одна строка `source` в `~/.bashrc`,
   ставит JetBrainsMono Nerd Font (иконки для prompt и `eza --icons`).
10. Ставит opencode (из npm-реестра, в `/usr/local/bin`) и pitwall (GUI-терминал
    для coding-агентов, с ярлыком в меню приложений).
11. Ставит сетевую диагностику `rkn-block-checker` (через pipx).
12. Ставит HushMic — шумоподавление микрофона реального времени (PipeWire).
13. Поднимает `guardrails-llm-filter` — systemd-юнит, который запускает
    Docker-контейнер маскировки PII/секретов на пути к LLM.
14. Добавляет пользователя в группу `docker`.
15. В конце проверяет установку и печатает версии всех утилит.

---

## Что устанавливается

### Пакеты apt (актуальные версии из репозитория Ubuntu)

| Роль | Пакеты | Зачем |
|---|---|---|
| base | ca-certificates, curl, wget, git, jq, unzip, zip, tar, gzip, xz-utils, bash-completion, lsb-release | база для работы и для самого плейбука |
| runtimes | python3-venv, python3-pip, python3-dev, pipx, golang-go, nodejs, npm, openjdk-21-jdk | языковые рантаймы |
| secrets | direnv | окружение по каталогам |
| shell | eza, zoxide, fzf, ripgrep, fd-find, fontconfig | удобная работа в терминале |
| network | nmap, netcat-openbsd, tcpdump, httpie, bind9-dnsutils, telnet | сеть и диагностика |
| audio | pipewire, pipewire-bin, pipewire-pulse, wireplumber, hicolor-icon-theme | шумоподавление микрофона (HushMic) |
| docker | docker-ce, docker-ce-cli, containerd.io, docker-compose-plugin | контейнеры (см. ниже) |

### Скачиваемые бинарники в `/usr/local/bin`

| Утилита | Источник |
|---|---|
| yq | github.com/mikefarah/yq |
| gh | github.com/cli/cli |
| glab | gitlab.com/gitlab-org/cli |
| gitlabcloner | github.com/Andrey039/gitlabcloner |
| glci | gitlab.com/gitlab-org/ci-cd/runner-tools/glci (Generic Packages) |
| pre-commit | github.com/pre-commit/pre-commit (самодостаточный `.pyz`) |
| reposcan | github.com/mabd-dev/reposcan |
| kubectl | dl.k8s.io |
| helm | get.helm.sh |
| k9s | github.com/derailed/k9s |
| kustomize | github.com/kubernetes-sigs/kustomize |
| kubectx | github.com/ahmetb/kubectx (shell-скрипт, ветка master) |
| flux | github.com/fluxcd/flux2 |
| flux9s | github.com/dgunzy/flux9s |
| stern | github.com/stern/stern |
| flate | github.com/home-operations/flate |
| flux-tools | github.com/alexandervidyaev/flux-tools |
| tflint | github.com/terraform-linters/tflint |
| terraform-docs | github.com/terraform-docs/terraform-docs |
| packer | releases.hashicorp.com |
| driftctl | github.com/snyk/driftctl |
| vault | releases.hashicorp.com |
| gitleaks | github.com/gitleaks/gitleaks |
| starship | github.com/starship/starship |
| bat | github.com/sharkdp/bat |
| opencode | registry.npmjs.org/@opencode/cli-linux-x64 |
| pitwall | github.com/quanticstudios/pitwall (GUI-терминал, + ярлык и иконка) |

Отдельно (не через роль `binaries`): `rkn-block-checker` ставится из PyPI через
pipx в `~/.local/bin` (роль `network`), `HushMic` — из `.deb`-пакета (роль
`audio`).

Скачивание версий идёт без GitHub API (лимит 60 запросов в час): тег последнего
релиза берётся из HTTP-редиректа `releases/latest` — для GitHub это
`github.com/<repo>/releases/latest`, для GitLab (утилиты `glab`, `glci`) —
`gitlab.com/<repo>/-/releases/permalink/latest`.

---

## Обновление и идемпотентность

Идемпотентность везде простая: **нет файла — ставим, есть файл — не трогаем.**

---

## Docker и Compose

Ставится **только Compose V2** — плагин `docker-compose-plugin`, команда `docker compose`.

Ключ репозитория кладётся как ASCII-armored `.asc` в `/etc/apt/keyrings/docker.asc`
и подключается через `signed-by`, поэтому **`gnupg` не требуется**.

Про старый Compose V1:

- V1 снят с поддержки с июля 2023; в Ubuntu 26.04 пакета `docker-compose` нет,
  а бинарники и тег `v1.29.2` с GitHub удалены — официально поставить его нечем.
- Файлы `docker-compose.yaml` формата `version: "2"` / `version: "3"` работают
  в `docker compose` (ключ `version` игнорируется с предупреждением).
- Легаси-формат v1 (нет ключа `version:`, сервисы в корне файла) не поддерживается —
  нужно один раз обернуть сервисы в `services:`:

```yaml
# было (легаси v1)
web:
  image: nginx

# стало
services:
  web:
    image: nginx
```

Проверка: `docker compose version`.

---

## guardrails-llm-filter (LLM PII/secret masking proxy)

Роль `guardrails` ставит systemd-юнит, который запускает Docker-контейнер
`ghcr.io/cloud-ru-tech/guardrails-llm-filter:latest` — прокси, маскирующий PII и
секреты в запросах к LLM.

Юнит: `/etc/systemd/system/guardrails-llm-filter.service` (кладётся из шаблона
`roles/guardrails/templates/guardrails-llm-filter.service.j2`).

Что делает юнит:

- зависит от `docker.service` (`After`/`Requires`) — поднимается после Docker;
- перед запуском снимает старый контейнер (`docker rm -f guardrails`);
- запускает контейнер с пробросом портов **только на loopback**:
  `127.0.0.1:8080` (прокси) и `127.0.0.1:9080` (метрики);
- включает аудит запросов (`GUARDRAILS_AUDIT_ENABLED=true`);
- пересылает запросы на `GUARDRAILS_UPSTREAM_BASE_URL=https://polza.ai`;
- `Restart=always` — контейнер поднимается автоматически после сбоя или перезагрузки.

Настройки (порты, upstream, аудит, образ) задаются в
`roles/guardrails/defaults/main.yml`.

Проверка и управление:

```bash
systemctl status guardrails-llm-filter
systemctl restart guardrails-llm-filter
journalctl -u guardrails-llm-filter -f
docker ps --filter name=guardrails
curl -s http://127.0.0.1:9080/    # метрики
```

Юнит кладётся из шаблона, поэтому правки в
`/etc/systemd/system/guardrails-llm-filter.service` перезапишутся при следующем
запуске: меняйте шаблон или переменные роли. Изменение файла автоматически
делает `daemon-reload` и перезапускает сервис (образ перетянется — тег `latest`
мог обновиться). Обновить образ вручную:

```bash
docker pull ghcr.io/cloud-ru-tech/guardrails-llm-filter:latest
sudo systemctl restart guardrails-llm-filter
```

---

## Git, GitLab CI и обзор репозиториев

Роль `git` дополнительно ставит:

- **glci** (`github gitlab-org/ci-cd/runner-tools/glci`) — запуск GitLab CI/CD
  пайплайнов локально. Парсит `.gitlab-ci.yml`, поднимает mock GitLab API и
  `gitlab-runner` в контейнерах и выполняет джобы в Docker. Требуется локальный
  Docker (или Podman). Бинарник `glci` лежит в GitLab Generic Packages, поэтому
  версия берётся из редиректа permalink, а файл скачивается по API-пути.
- **pre-commit** (`pre-commit/pre-commit`) — фреймворк git-хуков. Ставится
  самодостаточным `.pyz`-zipapp (со shebang `python3`) прямо в
  `/usr/local/bin/pre-commit`, без pip и virtualenv.
- **reposcan** (`mabd-dev/reposcan`) — быстрый поиск git-репозиториев с
  незакоммиченными/незапушенными изменениями по файловой системе.

```bash
pre-commit install          # включить хуки в текущем репозитории
reposcan ~/src              # показать «грязные» репозитории
glci run                    # прогнать пайплайн локально
```

---

## Flux GitOps без кластера (flate, flux-tools)

Роль `kubernetes` ставит два инструмента для офлайн-рендера и диффа Flux
GitOps-репозитория — без кластера, `kubectl` и внешних `helm`/`kustomize`
(библиотеки встроены в бинарник):

- **flate** (`home-operations/flate`) — Go-переписывание `flux-local`: рендерит
  и диффит GitOps-репозиторий, умеет режим «только изменённое поддерево».
- **flux-tools** (`alexandervidyaev/flux-tools`) — независимая реализация:
  собирает каждую Kustomization, шаблонит каждый HelmRelease, строит unified diff.

```bash
flate build --path ./kubernetes
flate diff  --path ./kubernetes --path-orig ../baseline/kubernetes
flux-tools test  clusters/prod --enable-helm
flux-tools build clusters/prod --enable-helm
```

---

## pitwall — терминал для coding-агентов

Роль `opencode` ставит **pitwall** (`quanticstudios/pitwall`) — GUI-терминал для
нескольких coding-агентов сразу: в сайдбаре видно, кто из агентов работает,
ждёт ввода или закончил, и одна клавиша переводит в нужную сессию. Есть
постоянные сессии, worktree и одобрения с телефона.

Бинарник кладётся в `/usr/local/bin/pitwall`, ярлык — в
`/usr/share/applications/pitwall.desktop`, иконка — в
`/usr/share/icons/hicolor/scalable/apps/pitwall.svg` (после установки pitwall
появится в меню приложений).

```bash
pitwall --version
pitwall -s my-session
```

---

## Аудио: HushMic (шумоподавление микрофона)

Роль `audio` ставит **HushMic** (`Fovty/HushMic`) — шумоподавление микрофона
реального времени поверх PipeWire. Добавляет виртуальный микрофон, который
убирает шум клавиатуры, вентиляторов и фоновые голоса (модель DPDFNet, обработка
на CPU, ~80 мс задержки).

Ставится официальным `.deb`-пакетом из GitHub-релиза (бинарник, LADSPA-плагин,
модели, иконки и пользовательский systemd-юнит). Версия берётся из редиректа
`releases/latest`, повторный запуск не переустанавливает (apt сверяет версию).

В приложениях (Discord, OBS, браузер) выберите устройство ввода **HushMic**.
Управление:

```bash
systemctl --user status hushmic      # пользовательский юнит
systemctl --user restart hushmic
hushmic --help                       # флаги CLI (--headless и др.)
```

Требуются PipeWire + WirePlumber (ставятся ролью) и, для иконки в трее, поддержка
StatusNotifierItem (в GNOME — расширение AppIndicator). Без трея HushMic всё
равно работает — им можно управлять из командной строки.

---

## Диагностика сетевых блокировок (rkn-block-checker)

Роль `network` ставит **rkn-block-checker** (`MayersScott/rkn-block-checker`) —
CLI, который определяет, каким способом провайдер режет доступ: DNS-подмена,
TCP reset, TLS DPI по SNI или заглушка провайдера. Проверка идёт по слоям
(DNS, TCP, TLS, HTTP). Пакета в apt и релизных бинарников нет — ставится из
PyPI через pipx в изолированное окружение, шим `rkn-check` кладётся в
`~/.local/bin`.

```bash
rkn-check                       # проверить список сайтов и вынести вердикт
rkn-check startweb --port 8080  # локальный веб-дашборд (по умолчанию :7777)
```

---

## direnv — инструкция

Хук `direnv` уже прописан в `~/.config/devops-shell.sh`:

```bash
eval "$(direnv hook bash)"
```

Как пользоваться:

1. В каталоге проекта создайте `.envrc`, например:

   ```bash
   export AWS_PROFILE=dev
   export KUBECONFIG=~/.kube/config-dev
   export TF_WORKSPACE=dev
   ```

2. Разрешите его:

   ```bash
   direnv allow
   ```

3. При входе в каталог переменные подхватываются автоматически, при выходе — снимаются.

Важно:

- После **каждого** изменения `.envrc` нужно снова выполнить `direnv allow`
  (direnv следит за хешем файла).
- При первом заходе в каталог с неразрешённым `.envrc` будет сообщение
  `direnv: error .envrc is blocked` — это нормально.
- Не храните в `.envrc` секреты, попадающие в git. Для секретов используйте
  отдельный неотслеживаемый файл и `source_env .envrc.local`.
- Статус: `direnv status`; перезагрузить: `direnv reload`; снять: `direnv deny`.

---

## terraform и terragrunt (через tfenv/tgenv)

Плейбук **не ставит** terraform и terragrunt: он ставит только менеджеры версий
`tfenv` и `tgenv`. Версию выбираете сами:

```bash
tfenv install 1.11.4 && tfenv use 1.11.4       # глобальная версия terraform
tgenv install 0.93.0 && tgenv use 0.93.0       # глобальная версия terragrunt
```

`~/.tfenv/bin` и `~/.tgenv/bin` уже добавлены в `PATH` через `devops-shell.sh`,
там же лежат шимы `terraform` и `terragrunt`.

Основные команды:

```bash
tfenv list / tfenv list-remote      # установленные / доступные версии
tgenv list / tgenv list-remote
```

Версия **по проекту** задаётся файлами в корне репозитория:

```bash
echo 1.11.4 > .terraform-version     # tfenv переключит автоматически
echo 0.93.0 > .terragrunt-version    # tgenv переключит автоматически
```

---

## Shell-конфигурация

Плейбук создаёт `~/.config/devops-shell.sh` и добавляет в `~/.bashrc` один блок:

```bash
# BEGIN DEVOPS PLAYBOOK (ansible role: shell)
[ -f "$HOME/.config/devops-shell.sh" ] && . "$HOME/.config/devops-shell.sh"
# END DEVOPS PLAYBOOK (ansible role: shell)
```

Что внутри `devops-shell.sh`:

- `PATH`: `~/.local/bin`, `~/.tfenv/bin`, `~/.tgenv/bin`;
- `fzf --bash` (keybindings + completion);
- `direnv hook bash`, `zoxide init bash`, `starship init bash`;
- алиасы `ls`/`ll`/`la`/`lt` на `eza`;
- алиас `catbat` и тема для `bat`;
- алиас `fd` → `fdfind` (в Ubuntu бинарь `fd-find` называется `fdfind`);
- completion для `kubectl`, `helm`, `kubectx`, алиас `k = kubectl`;
- `PIPX_HOME` / `PIPX_BIN_DIR`.

Файл пишется из шаблона `roles/shell/templates/devops-shell.sh.j2` — правки
в самом файле перезапишутся при следующем запуске. Чтобы убрать окружение:
удалите файл и блок между маркерами в `~/.bashrc`.

### Конфиг starship

Тема prompt кладётся из `roles/shell/files/starship.toml` в `~/.config/starship.toml`
(starship читает этот путь по умолчанию). Файл копируется как есть — правки
в `~/.config/starship.toml` перезапишутся при следующем запуске. Меняйте
`roles/shell/files/starship.toml` в репозитории; путь назначения задаётся
переменной `starship_config_file` в `group_vars/all.yml`.

### Nerd Font (иконки в prompt и списках файлов)

Starship рисует глифы ветки Git и Kubernetes, а `eza` умеет показывать иконки
файлов (`--icons`) — все они лежат в private use area. В обычных шрифтах таких
глифов нет, и без них в терминале видны «квадратики». Роль `shell` ставит
**полное семейство JetBrainsMono Nerd Font** (все начертания: обычный, Mono,
NL, NLPropo):

- скачивается `JetBrainsMono.tar.xz` (**~7 МБ**) из релизов
  `ryanoasis/nerd-fonts`, распаковываются все `*.ttf` (96 файлов);
- шрифты кладутся в `~/.local/share/fonts/JetBrainsMono/`, затем `fc-cache -f`;
- sudo не нужен, всё в домашнем каталоге.

Иконки уже внутри самого шрифта, поэтому после установки достаточно выбрать
**JetBrainsMono Nerd Font** в настройках терминала — отдельный fallback-шрифт
и fontconfig-конфиг не нужны.

Настройки — в `roles/shell/defaults/main.yml` (`nerd_font_*`): версия, каталог,
отключение через `nerd_font_install: false`. Идемпотентность: если файл-маркер
уже на месте, ничего не скачивается. Обновить версию — поменять
`nerd_font_version` или удалить `~/.local/share/fonts/JetBrainsMono` и запустить
плейбук снова.

> Проверить, что шрифт установился: `fc-list | grep -i "JetBrainsMono Nerd"`,
> а что иконка разрешается в него: `fc-match 'JetBrainsMono Nerd Font:charset=e0a0'`.
