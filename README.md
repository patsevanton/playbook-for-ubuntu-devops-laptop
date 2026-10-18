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
│   ├── base/                   # проверки, каталоги, базовые пакеты (в т.ч. pipx),
│   │                           #   yq, ansible-lint, molecule
│   ├── git/                    # gh, glab, gitlabcloner, glci, pre-commit, reposcan,
│   │                           #   lazygit
│   ├── docker/                 # репозиторий Docker, Engine, Compose V2,
│   │                           #   dive, lazydocker
│   ├── kubernetes/             # kubectl, helm, k9s, kustomize, kubectx, flux, flux9s,
│   │                           #   stern, flate, flux-tools, cty, diffyml,
│   │                           #   kubectl-k8i, kubespy, kubecolor, kube-capacity,
│   │                           #   ktop, kubefetch, krew, view-secret, df-pv,
│   │                           #   browse-pvc, pluto, pv-migrate, marvin
│   ├── iac/                    # tflint, terraform-docs, packer, driftctl, tgsieve,
│   │                           #   tfenv, tgenv
│   ├── postgres/               # pgCenter (админка и мониторинг PostgreSQL)
│   ├── runtimes/               # Python, uv, Go, Node.js, Java
│   ├── secrets/                # direnv, vault, gitleaks, sops, grype
│   ├── shell/                  # eza, zoxide, fzf, starship, bat, difftastic,
│   │                           #   delta, gdu, JetBrainsMono Nerd Font,
│   │                           #   mcommander, конфиг shell
│   ├── network/                # nmap, netcat, tcpdump, httpie, dnsutils, telnet,
│   │                           #   gai, dnsglobe, bandwhich, q, xh,
│   │                           #   rkn-block-checker (pipx)
│   ├── opencode/               # opencode (из npm-реестра)
│   ├── skills/                 # глобальные skills для opencode (CLI `skills`)
│   ├── audio/                  # HushMic (шумоподавление микрофона, .deb)
│   ├── guardrails/             # guardrails-llm-filter (systemd-юнит поверх Docker)
│   ├── nora/                   # NORA artifact registry + mkcert TLS + CA клиентам,
│   │                           #   Caddy как TLS-терминатор
│   └── binaries/               # общий движок установки бинарников (вызывается ролями)
├── .ansible-lint
└── README.md
```

Роль `binaries` не тянет инструменты сама: её вызывают роли `base`, `git`,
`kubernetes`, `iac`, `postgres`, `secrets`, `shell`, `network`, `opencode`,
передавая список того, что нужно поставить. Каждая роль хранит свой список в
`roles/<роль>/defaults/main.yml`.

Имя `playbook.yml` выбрано осознанно; в чужих проектах точку входа часто называют
`site.yml` — переименование ничего не ломает.

---

## Что делает плейбук

1. Проверяет, что ОС — Ubuntu, архитектура — x86_64, и создаёт служебные каталоги.
2. Ставит пакеты из репозитория Ubuntu (актуальные версии, без пинов),
   включая `pipx`, `ansible-lint` (проверка стиля плейбуков), `molecule`
   (тестирование Ansible-ролей в контейнерах, ставится через pipx) и `eget`
   (установщик бинарников из GitHub-релизов).
3. Ставит клиенты GitHub и GitLab: `gh`, `glab`, `gitlabcloner`, а также `glci`
   (локальный запуск GitLab CI), `pre-commit`, `reposcan` и `lazygit`
   (TUI для git).
4. Подключает официальный репозиторий Docker и ставит Engine + Compose V2,
   а также `dive` (анализ слоёв образа) и `lazydocker` (TUI-менеджер контейнеров).
5. Ставит клиенты Kubernetes: kubectl, helm, k9s, kustomize, kubectx, flux, flux9s,
   stern, flate, flux-tools (офлайн-рендер и дифф Flux GitOps), cty (sample-YAML
   из CRD), diffyml (структурный дифф YAML), kubectl-k8i (ресурсы нод),
   kubespy (наблюдение за ресурсами в реальном времени), kubecolor (цветной
   вывод kubectl), kube-capacity и ktop (ресурсы и метрики кластера),
   kubefetch (сводка о кластере), krew (менеджер kubectl-плагинов) и три
   утилиты бинарниками — view-secret (декодирование секретов), df-pv
   (`df` по PV), browse-pvc (просмотр PVC) — а также pluto (устаревшие
   apiVersion), pv-migrate (миграция PV) и marvin (сканер кластера
   CEL-выражениями).
6. Ставит инструменты IaC: tflint, terraform-docs, packer, driftctl, tgsieve
   (сжатый вывод `terragrunt plan`), плюс tfenv и tgenv.
7. Ставит pgCenter — консольную админку и мониторинг PostgreSQL.
8. Ставит рантаймы: Python (venv/pip), uv, Go, Node.js, Java 21.
9. Ставит direnv, Vault, gitleaks, sops (шифрование секретов в Git) и grype
   (сканер уязвимостей образов и ФС).
10. Настраивает shell: `~/.config/devops-shell.sh` + одна строка `source` в `~/.bashrc`,
    JetBrainsMono Nerd Font (иконки для prompt и `eza --icons`),
    M-Commander (двухпанельный файловый менеджер) и утилиты просмотра
    difftastic (структурный diff), delta (улучшенный git-diff) и gdu
    (анализатор диска).
11. Ставит opencode (из npm-реестра, в `/usr/local/bin`).
12. Ставит глобальные skills для opencode (CLI `skills`, каталог
    `~/.agents/skills`) — см. раздел «Глобальные skills для opencode».
13. Ставит сетевую диагностику: `gai` (инспектор getaddrinfo), `dnsglobe`
    (проверка распространения DNS), `bandwhich` (трафик по процессам),
    `q` (DNS-клиент с DoH/DoT/DoQ), `xh` (HTTP-клиент) и `rkn-block-checker`
    (через pipx).
14. Ставит HushMic — шумоподавление микрофона реального времени (PipeWire).
15. Поднимает `guardrails-llm-filter` — systemd-юнит, который запускает
    Docker-контейнер маскировки PII/секретов на пути к LLM.
16. Добавляет пользователя в группу `docker`.
17. Ставит NORA artifact registry: `.deb`-пакет, локальный TLS-сертификат через
    mkcert, автодоверие CA у клиентов (системный store, Docker, npm, pip, Java)
    и Caddy как TLS-терминатор.
18. В конце проверяет установку и печатает версии всех утилит.

---

## Что устанавливается

### Пакеты apt (актуальные версии из репозитория Ubuntu)

| Роль | Пакеты | Зачем |
|---|---|---|
| base | ca-certificates, curl, wget, git, jq, unzip, zip, tar, gzip, xz-utils, bash-completion, lsb-release, ansible-lint, pipx | база для работы, самого плейбука, проверки стиля и установки molecule |
| runtimes | python3-venv, python3-pip, python3-dev, golang-go, nodejs, npm, openjdk-21-jdk | языковые рантаймы |
| secrets | direnv | окружение по каталогам |
| shell | eza, zoxide, fzf, ripgrep, fd-find, fontconfig | удобная работа в терминале |
| network | nmap, netcat-openbsd, tcpdump, httpie, bind9-dnsutils, telnet | сеть и диагностика |
| audio | pipewire, pipewire-bin, pipewire-pulse, wireplumber, hicolor-icon-theme | шумоподавление микрофона (HushMic) |
| docker | docker-ce, docker-ce-cli, containerd.io, docker-compose-plugin | контейнеры (см. ниже) |
| nora | mkcert, libnss3-tools, caddy, apache2-utils | mkcert (локальный CA), Caddy (TLS-терминатор), htpasswd (авторизация) |

Пакетами `.deb` (вне роли `binaries`) ставятся `mcommander` (роль `shell`) и
`HushMic` (роль `audio`).

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
| cty | github.com/Skarlso/crd-to-sample-yaml |
| diffyml | github.com/szhekpisov/diffyml |
| kubectl-k8i | github.com/ViktorUJ/kubectl-k8i |
| kubespy | github.com/pulumi/kubespy |
| kubecolor | github.com/kubecolor/kubecolor |
| kube-capacity | github.com/robscott/kube-capacity |
| ktop | github.com/vladimirvivien/ktop |
| kubefetch | github.com/jkulzer/kubefetch |
| krew | github.com/kubernetes-sigs/krew |
| view-secret | github.com/elsesiy/kubectl-view-secret |
| df-pv | github.com/yashbhutwala/kubectl-df-pv |
| browse-pvc | github.com/clbx/kubectl-browse-pvc |
| pluto | github.com/FairwindsOps/pluto |
| pv-migrate | github.com/utkuozdemir/pv-migrate |
| marvin | github.com/undistro/marvin |
| tflint | github.com/terraform-linters/tflint |
| terraform-docs | github.com/terraform-docs/terraform-docs |
| packer | releases.hashicorp.com |
| driftctl | github.com/snyk/driftctl |
| tgsieve | github.com/imcitius/tgsieve |
| pgcenter | github.com/lesovsky/pgcenter |
| vault | releases.hashicorp.com |
| gitleaks | github.com/gitleaks/gitleaks |
| gai | github.com/casablanque-code/gai |
| dnsglobe | github.com/514-labs/dnsglobe |
| bandwhich | github.com/imsnif/bandwhich |
| q | github.com/natesales/q |
| xh | github.com/ducaale/xh |
| eget | github.com/zyedidia/eget |
| difftastic | github.com/Wilfred/difftastic |
| delta | github.com/dandavison/delta |
| gdu | github.com/dundee/gdu |
| lazygit | github.com/jesseduffield/lazygit |
| lazydocker | github.com/jesseduffield/lazydocker |
| dive | github.com/wagoodman/dive |
| sops | github.com/getsops/sops |
| grype | github.com/anchore/grype |
| starship | github.com/starship/starship |
| bat | github.com/sharkdp/bat |
| opencode | registry.npmjs.org/@opencode/cli-linux-x64 |

Отдельно (не через роль `binaries`): `molecule` ставится из PyPI через pipx в
`~/.local/bin` (роль `base`) вместе с драйвером docker (`molecule-plugins`) и
коллекциями `community.docker`/`ansible.posix`, `rkn-block-checker` — из PyPI
через pipx в `~/.local/bin` (роль `network`), `HushMic` — из `.deb`-пакета
(роль `audio`), `mcommander` — из `.deb`-пакетов (роль `shell`), глобальные
skills для opencode — через `npx skills add` (роль `skills`, см. раздел
«Глобальные skills для opencode»).

Скачивание версий идёт без GitHub API (лимит 60 запросов в час): тег последнего
релиза берётся из HTTP-редиректа `releases/latest` — для GitHub это
`github.com/<repo>/releases/latest`, для GitLab (утилиты `glab`, `glci`) —
`gitlab.com/<repo>/-/releases/permalink/latest`.

Роль `binaries` понимает три формата архивов: `tar` (`.tar.gz`), `tarxz`
(`.tar.xz`, напр. `dnsglobe`) и `zip`; для одиночных бинарников — `archive: none`.

---

## Проверка стиля (ansible-lint)

Роль `base` ставит **ansible-lint** пакетом из репозитория Ubuntu — тот самый,
которым можно проверять этот плейбук:

```bash
ansible-lint                    # профиль production, конфиг .ansible-lint
```

Профиль `production` и список исключений заданы в `.ansible-lint` в корне
репозитория. Единственное исключение — `var-naming[no-role-prefix]`: роль
`binaries` вызывается другими ролями через `include_role` и принимает входную
переменную `binaries_to_install`, которая по правилу должна была бы нести
префикс вызывающей роли.

---

## molecule — тестирование Ansible-ролей

Роль `base` ставит **molecule** (`ansible/molecule`) — фреймворк тестирования
ролей и плейбуков: прогоняет роль в поднятых инстансах (контейнеры, VM),
проверяет идемпотентность и гоняет verify-сценарии. Пакета в apt и релизных
бинарников нет — ставится из PyPI через **pipx** в изолированное окружение
(шим `molecule` — в `~/.local/bin`).

Вместе с ним ставятся:

- **molecule-plugins** с extra `docker` — драйверы сценариев; драйвер `docker`
  гоняет тесты в контейнерах Docker (Engine ставится ролью `docker`),
- коллекции **community.docker** и **ansible.posix** в `~/.ansible/collections`
  — их модулями сам драйвер docker поднимает и разбирает контейнеры.

```bash
molecule drivers                # доступные драйверы: docker, podman, vagrant, ...
molecule init scenario          # каркас сценария ./molecule/default
molecule test                   # прогнать сценарий целиком
molecule converge               # только применить роль
molecule idempotence            # второй прогон — ничего не должен менять
```

Сценарий создаётся в `molecule/<имя>/` внутри каталога роли: `molecule.yml`
(настройки), `converge.yml` (применение роли), `verify.yml` (проверки).
Драйвер задаётся в `molecule.yml` (`driver: name: docker` + `platforms:` с
образами); без явного драйвера используется `default` — инстансы вы поднимаете
сами, а molecule оркестрирует последовательность теста
(`dependency → create → converge → idempotence → verify → destroy`).

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
  `127.0.0.1:8080` (прокси) и `127.0.0.1:9080` (config API и веб-консоль);
- включает аудит запросов (`GUARDRAILS_AUDIT_ENABLED=true`);
- пересылает запросы на `GUARDRAILS_UPSTREAM_BASE_URL=https://polza.ai`;
- при старте применяет политику правил детекции (`ExecStartPost` →
  `/usr/local/sbin/guardrails-apply-policy`) — выключает правила из
  `guardrails_disabled_rules`;
- `Restart=always` — контейнер поднимается автоматически после сбоя или перезагрузки.

Настройки (порты, upstream, аудит, образ) задаются в
`roles/guardrails/defaults/main.yml`.

Политика правил тоже живёт в `roles/guardrails/defaults/main.yml`
(`guardrails_disabled_rules`): по умолчанию выключена маскировка IPv4
(правила `ip-addrs.ipv4*`, IPv6-правила остаются включёнными). Хранилище
guardrails — `in_memory`, поэтому правки через веб-консоль не переживают
рестарт контейнера: политика применяется скриптом при каждом старте сервиса и
синхронизируется при каждом прогоне плейбука. Правила вне списка плейбук не
трогает — их по-прежнему можно выключать в веб-консоли, но они вернутся к
дефолту после рестарта. Чтобы вернуть маскировку IPv4 — убрать правила из
`guardrails_disabled_rules` и перезапустить сервис.

Чтобы видеть **настоящие** креды рядом с **фейковыми** (плейсхолдерами),
включено хранение текстов в аудите (страница «Журнал аудита» в консоли на
`http://127.0.0.1:9080`):

- `GUARDRAILS_AUDIT_STORE_ORIGINAL_TEXTS=plain` — оригинал за плейсхолдером,
  в UI показывается по наведению;
- `GUARDRAILS_AUDIT_STORE_MASKED_TEXTS=true` — маскированный текст запроса;
- `GUARDRAILS_AUDIT_STORE_MASKED_RESPONSE_TEXTS=true` — маскированный текст
  ответа модели.

Значения задаются в `roles/guardrails/defaults/main.yml`
(`guardrails_audit_store_*`) и применяются при следующем старте сервиса.
Без них записи аудита показывают только правило и плейсхолдер, а поле
`original` остаётся пустым.

⚠️ `original_texts=plain` хранит сырые чувствительные данные в памяти
контейнера (`in_memory`, до 24h, до 10000 записей), а config API и консоль не
имеют аутентификации (слушают только loopback). Для шифрования — значение
`encrypted` плюс `GUARDRAILS_STORE_ENCRYPTION_ENABLED=true` и
`GUARDRAILS_STORE_ENCRYPTION_KEY` (см. SECURITY.md upstream-проекта).

Для живой проверки — страница «Песочница» в консоли: вставляете текст и
видите, что ушло модели (замаскировано) и что вернётся клиенту (оригиналы).

Проверка и управление:

```bash
systemctl status guardrails-llm-filter
systemctl restart guardrails-llm-filter
journalctl -u guardrails-llm-filter -f
docker ps --filter name=guardrails
curl -s http://127.0.0.1:9080/    # веб-консоль и config API
curl -s "http://127.0.0.1:9080/v1/audit/records?limit=1"  # аудит: плейсхолдеры и оригиналы
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

## NORA artifact registry + локальный TLS (mkcert)

Роль `nora` ставит **NORA** (`getnora-io/nora`) — self-hosted мультиформатный
artifact registry (Docker, npm, PyPI, Cargo, Go, Maven и др.), выписывает для
него **локальный доверенный сертификат** через **mkcert** и автоматически
добавляет этот CA в trust-сторы клиентов. TLS терминирует **Caddy**.

Что делает роль:

1. Ставит `nora-amd64.deb` из последнего GitHub-релиза (с systemd-юнитом).
   Конфиг — `/etc/nora/nora.env` (из шаблона), данные — `/var/lib/nora`.
2. Ставит `mkcert` + `libnss3-tools`, запускает `mkcert -install` — локальный
   CA ложится в системный store и NSS-сторы браузеров. CA-материал — в
   `~/.local/share/mkcert` (`mkcert -CAROOT`), ключ не раздаётся и не коммитится.
3. Выписывает листовой серт под домен реестра
   (`registry.nora.lan`, `localhost`, `127.0.0.1`) в
   `/etc/nora/tls/nora.crt` + `nora.key` (ключ — `600 root:root`).
4. Раздаёт `rootCA.pem` клиентам: системный store
   (`/usr/local/share/ca-certificates/nora-ca.crt` + `update-ca-certificates` —
   для curl/git/apt/go/rustls), Docker (`/etc/docker/certs.d/registry.nora.lan/ca.crt`),
   npm (`cafile` в `~/.npmrc`), pip (пакет `truststore`), Java
   (`keytool -importcert` в системный `cacerts` — Maven/Gradle).
5. Ставит Caddy как TLS-терминатор: `registry.nora.lan:443` → `127.0.0.1:4000`
   (HTTP).
6. Прописывает `registry.nora.lan` в `/etc/hosts` на `127.0.0.1`.

Серты и ключи в репозиторий плейбука не кладутся (генерируются на месте).
Домен не использует `.local` (конфликт с mDNS).

Проверка после установки:

```bash
curl -fsS https://registry.nora.lan/health        # без -k, серт доверенный
docker pull registry.nora.lan/myapp:1             # CA подхвачен из certs.d
openssl s_client -connect registry.nora.lan:443 -showcerts  # цепочка → rootCA mkcert
```

UI — `https://registry.nora.lan/ui/`. Порты: NORA слушает `0.0.0.0:4000`,
Caddy — `:443`.

### Авторизация NORA

По умолчанию включена (`NORA_AUTH_ENABLED=true`). Пользователи — htpasswd-файл
(`apache2-utils`, bcrypt), программный доступ — API-токены. Создать
пользователя и токен:

```bash
sudo htpasswd -Bc /etc/nora/users.htpasswd admin   # первый пользователь
# токен (показывается один раз):
curl -k -X POST https://registry.nora.lan/api/tokens \
  -H 'Content-Type: application/json' \
  -d '{"username":"admin","password":"...","role":"write","ttl_days":90}'
# docker login с токеном (токен — как пароль):
docker login registry.nora.lan -u token -p nra_...
```

Пока htpasswd-файл не создан, логиниться нельзя (реестр просто без
пользователей). Всё настраивается в `roles/nora/defaults/main.yml`
(`nora_auth_enabled`, `nora_auth_anonymous_read`, `nora_hostname`,
`nora_public_url`, `nora_tls_*`), там же флаги `nora_docker_configure` /
`nora_npm_configure` / `nora_pip_configure` / `nora_java_configure`, если
какой-то trust-store клиента не нужен.

### Обновление и авторотация серта

mkcert сам серты не продлевает. Серт выписывается ролью только при отсутствии
`/etc/nora/tls/nora.crt`; чтобы перевыпустить — удалите его и запустите плейбук
снова. Для автоматической ротации в продакшене используйте `step-ca` с ACME
или Let's Encrypt + Caddy — тогда локальный CA не нужен.

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

## Инструменты для манифестов Kubernetes (cty, diffyml, kubectl-k8i, kubespy)

Роль `kubernetes` дополнительно ставит:

- **cty** (`Skarlso/crd-to-sample-yaml`) — генерирует sample-YAML из
  определения CRD, валидирует sample против схемы CRD и проверяет схему на
  breaking changes между версиями. Удобен для unit-тестов CRD-схем в CI.
- **diffyml** (`szhekpisov/diffyml`) — быстрый структурный дифф YAML с учётом
  специфики Kubernetes (понимает порядок полей, списки, аннотации), умеет
  выдавать аннотации для GitHub/GitLab/Gitea CI.
- **kubectl-k8i** (`ViktorUJ/kubectl-k8i`) — плагин kubectl: таблица ресурсов
  нод (поды, CPU, память, проценты загрузки с цветовой индикацией), метаданные
  (EC2, зона, nodepool), определение автоскейлера (Karpenter/CAS/Spot.io),
  анализ workload'ов. Ставится как `kubectl-k8i`, поэтому доступен и как
  `kubectl k8i`.
- **kubespy** (`pulumi/kubespy`) — наблюдение за изменениями ресурсов
  Kubernetes в реальном времени: `trace` собирает высокоуровневую сводку
  разворачивания, `status` показывает диффы `.status`.

```bash
cty generate -c my-crd.yaml -o sample.yaml     # sample-YAML из CRD
cty validate sample -c my-crd.yaml -s sample.yaml
diffyml old.yaml new.yaml                      # структурный дифф
kubectl k8i nodes                              # ресурсы нод
kubespy trace deployment nginx                 # наблюдение за rollout
```

---

## Обзор кластера: kubecolor, kube-capacity, ktop, kubefetch

Роль `kubernetes` дополнительно ставит утилиты, с которыми кластер понятнее
«на глаз»:

- **kubecolor** (`kubecolor/kubecolor`) — цветной, читаемый вывод kubectl:
  подсвечивает ресурсы, диффы, логи и ошибки. Это обёртка над kubectl, но
  вызывается отдельной командой: алиасы на `kubectl` не навешиваем, сам
  `kubectl` остаётся как есть.
- **kube-capacity** (`robscott/kube-capacity`) — таблица ресурсов кластера:
  requests/limits CPU и памяти по нодам (и подам/контейнерам с флагами `-p`/`-c`)
  против фактического потребления (`--util`), с сортировкой и выводом в
  `csv`/`json`/`yaml`. Для фактических метрик нужен metrics-server.
- **ktop** (`vladimirvivien/ktop`) — `top`-подобный TUI: ноды, поды и их метрики
  CPU/памяти в реальном времени. Из релиза берётся одиночный `ktop` — плагин
  `kubectl-ktop` не ставим.
- **kubefetch** (`jkulzer/kubefetch`) — neofetch для Kubernetes-кластера:
  сводка о кластере, нодах и подах одним экраном.

```bash
kubecolor get pods -A                    # цветной вывод kubectl
kubecolor diff -f old.yaml -f new.yaml   # читаемый дифф манифестов
kube-capacity --util --pods              # requests/limits vs фактическая загрузка
kube-capacity -o json --sort mem.util    # то же в JSON
ktop -A                                  # TUI: метрики нод и подов по всем ns
kubefetch                                # сводка о кластере
```

---

## krew и kubectl-утилиты (view-secret, df-pv, browse-pvc)

Роль `kubernetes` ставит **krew** (`kubernetes-sigs/krew`) — менеджер
kubectl-плагинов (для установки плагинов вручную потом), а также три утилиты,
которые раньше ставились бы как kubectl-плагины, напрямую бинарниками:

- **view-secret** — декодирование секретов: ключи и значения base64, фильтр по
  ключу или всё сразу.
- **df-pv** — `df` по PersistentVolume: занятое место по PVC/PV в стиле
  unix-утилиты `df`.
- **browse-pvc** — просмотр содержимого PVC из CLI: создаёт джобу,
  привязывающуюся к PVC, и заходит в неё.

Все три ставятся бинарниками в `/usr/local/bin` **без префикса `kubectl-`** и
вызываются напрямую (не через `kubectl view-secret`). Версии — актуальные из
GitHub-релизов, фиксации нет.

```bash
krew update                                # обновить индекс плагинов
krew install ctx                           # поставить произвольный плагин вручную
krew list                                  # что уже установлено
view-secret my-secret -a                   # декодировать секрет
view-secret my-secret my-key               # декодировать конкретный ключ
df-pv                                      # занятость PV
browse-pvc my-pvc                          # зайти в PVC
```

Сам `krew` — бинарник в `/usr/local/bin`; `~/.krew/bin` (куда krew кладёт
плагины при `krew install`) добавляется в `PATH` через `devops-shell.sh`.

---

## pluto, pv-migrate и marvin

Роль `kubernetes` дополнительно ставит три самостоятельных бинарника:

- **pluto** (`FairwindsOps/pluto`) — находит устаревшие и удалённые
  `apiVersion` в манифестах Kubernetes: сканирует локальные файлы или кластер
  и показывает, что надо мигрировать.
- **pv-migrate** (`utkuozdemir/pv-migrate`) — миграция и backup/restore
  PersistentVolume между PVC/кластерами (через `rsync` или `skopeo`).
- **marvin** (`undistro/marvin`) — сканер кластера CEL-выражениями: выявляет
  проблемы, мисконфиги и уязвимости по правилам.

```bash
pluto detect-files -d manifests/          # устаревшие apiVersion в файлах
pluto detect-helm -d charts/              # то же в Helm-чартах
pv-migrate migrate src dst                # миграция PV между PVC/кластерами
marvin scan                               # сканировать текущий кластер
```

---

## tgsieve — сжатый вывод `terragrunt plan`

Роль `iac` ставит **tgsieve** (`imcitius/tgsieve`). На большом стеке
`terragrunt plan --all` печатает тысячи строк, в которых нужные изменения
теряются. tgsieve сам запускает план, читает **структурированный** вывод
(`tfplan.json` по юнитам, флаг `--json-out-dir`) вместо текста, отбрасывает
объявленный шум, сворачивает повторяющиеся изменения по юнитам и печатает
только суть — с вложенностью «где → что → какие поля». Есть и `apply`, который
применяет ровно то, что показал план.

```bash
tgsieve plan                    # план для текущего юнита и сжатый отчёт
tgsieve plan --all              # план по всему стеку
tgsieve plan --all --timings    # ещё и самые медленные юниты
```

---

## pgCenter — админка и мониторинг PostgreSQL

Роль `postgres` ставит **pgcenter** (`lesovsky/pgcenter`) — консольную админку
и мониторинг PostgreSQL в духе top/htop. Живая статистика активности, запросов,
блокировок и ожидающих событий, размеры таблиц и индексов, профилировщик wait
events, запись статистики в файл и отчёты по ней. Подключается как обычный
клиент PostgreSQL: параметрами libpq (`-h`, `-p`, `-U` и имя базы) или
переменными окружения `PG*` (включая `~/.pgpass`).

```bash
pgcenter top -h db.example.com -U postgres app      # top-статистика в реальном времени
pgcenter profile -h db.example.com -U postgres app  # профилировщик wait events
pgcenter record -h db.example.com -U postgres app   # запись статистики в файл
pgcenter report -f pgcenter.stat.tar                # отчёт по записанной статистике
```

Для расширенной статистики в самой базе есть `pgcenter config` — ставит и
снимает свою схему сбора статистики.

---

## pitwall — терминал для coding-агентов

pitwall в плейбук не ставится. См. issue о невозможности вводить русские буквы:
<https://github.com/quanticstudios/pitwall/issues/127>.

---

## herdr и rtk — инструменты для AI-агентов

Роль `ai_tools` ставит два CLI, усиливающих coding-агентов (opencode и др.).
Бинарники кладутся в `/usr/local/bin/` через роль `binaries` (версия —
актуальная на момент запуска).

- **herdr** (`herdrdev/herdr`) — TUI-оболочка поверх терминала: управляет
  сессиями/панелями/вкладками coding-агентов, показывает их состояние
  (работает / ждёт ввода / заблокирован), умеет удалённые машины по SSH и
  самообновление (`herdr update`). Одиночный статический бинарник.
- **rtk** (`rtk-ai/rtk`) — прокси-обёртка над утилитами (`rtk git`, `rtk ls`,
  `rtk kubectl`, …): сжимает и фильтрует вывод (логи, diff, таблицы), прежде
  чем он попадёт в контекст LLM.

```bash
herdr --version
rtk --version
```

### Конфиги

- **herdr** — копируется статичный `roles/ai_tools/files/herdr/config.toml`
  в `~/.config/herdr/config.toml`: отключён first-run onboarding
  (`onboarding = false`) и тосты идут через сам herdr
  (`[ui.toast] delivery = "herdr"`). Телеметрии у herdr в конфиге нет.
- **rtk** — генерируется динамически, без хранения готового TOML в `files/`
  (схема конфига может меняться между версиями): если `~/.config/rtk/config.toml`
  нет, запускается `rtk config --create` (актуальный дефолт), затем всегда
  `rtk telemetry disable` — телеметрия выключена и зафиксирован явный отказ от
  согласия (`consent: no`; дефолтный `--create` оставляет `consent: never asked`).

### Интеграции с opencode

Роль `ai_tools` устанавливает обе интеграции (каталог `~/.config/opencode/`
на этот момент уже создан ролью `opencode`):

- **herdr** — `herdr integration install opencode` кладёт в
  `~/.config/opencode/`: `plugins/herdr-agent-state.js` (состояние агента по
  unix-сокету), `herdr-tui-session.js`, `tui.jsonc` и `herdr-opencode/tui.js`.
- **rtk** — `rtk init -g --opencode --no-patch` кладёт `plugins/rtk.ts`:
  перехватывает `tool.execute.before` для команд bash/shell и переписывает их
  через `rtk hook opencode` (движок переписывания — в самом rtk).

Обе команды идемпотентны (перезаписывают свои файлы тем же содержимым) и не
запрашивают подтверждения без TTY.

`rtk init -g --opencode` помимо плагина создаёт побочные файлы для Claude Code
(`~/.claude/CLAUDE.md`, `~/.claude/RTK.md`) и пустой шаблон
`~/.config/rtk/filters.toml` — opencode-плагину они не нужны, поэтому роль
удаляет их отдельными задачами.

---

## Глобальные skills для opencode

Роль `skills` ставит **глобальные skills для opencode** — инструкции-плагины
(каталоги с `SKILL.md`), которые подсказывают агенту, как работать с разными
инструментами и задачами. Skills ставятся CLI [`skills`](https://www.npmjs.com/package/skills)
(через `npx`, без глобальной установки CLI):

```bash
npx skills add <git-репозиторий> -g -y -a opencode -s <skill-1,skill-2,...>
```

Флаги: `-g` — глобально для пользователя, `-y` — без вопросов, `-a opencode` —
с привязкой к агенту opencode, `-s` — конкретные skills из репозитория. CLI
сам клонирует репозиторий источника, поэтому нужны `git` (роль `git`) и `npm`
(роль `runtimes`), а также **SSH-ключ к GitHub** — источники заданы SSH-URL
(как в рабочем окружении, откуда список воспроизведён).

Skills складываются в `~/.agents/skills`, учёт установленного — в
lock-файле `~/.agents/.skill-lock.json`. Идемпотентность: перед установкой
роль читает lock-файл и пропускает источники, чьи skills уже установлены.
Удалить запись из lock-файла = переустановить соответствующий источник.
При установке задаётся `INSTALL_INTERNAL_SKILLS=1`: CLI по умолчанию
пропускает skills с `metadata.internal: true` в frontmatter, и для них
установка не нашла бы имена в списке.

Список в `roles/skills/defaults/main.yml` (`skills_sources`) — реплика
рабочего окружения: **68 skills из 16 репозиториев**:

| Источник | Skills |
|---|---|
| [elastic/agent-skills](https://github.com/elastic/agent-skills) | 24 |
| [obra/superpowers](https://github.com/obra/superpowers) | 15 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | 7 |
| [dietrichgebert/ponytail](https://github.com/dietrichgebert/ponytail) | 6 |
| [victoriametrics/skills](https://github.com/victoriametrics/skills) | 5 |
| [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 1 |
| [artwist-polyakov/polyakov-claude-skills](https://github.com/artwist-polyakov/polyakov-claude-skills) | 1 |
| [blader/humanizer](https://github.com/blader/humanizer) | 1 |
| [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) | 1 |
| [elsvv/yandex-cloud-cli-skill](https://github.com/elsvv/yandex-cloud-cli-skill) | 1 |
| [jeffallan/claude-skills](https://github.com/jeffallan/claude-skills) | 1 |
| [petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop) | 1 |
| [testland/qa](https://github.com/testland/qa) | 1 |
| [theclaymethod/unslop](https://github.com/theclaymethod/unslop) | 1 |
| [vercel-labs/skills](https://github.com/vercel-labs/skills) | 1 |
| [Vladimir-Human/humanizer-ru](https://github.com/Vladimir-Human/humanizer-ru) | 1 |

Проверка после установки:

```bash
ls ~/.agents/skills                    # все ожидаемые skills на месте
npx skills ls -g                       # список установленных глобальных skills
```

Применить только skills: `ansible-playbook -K playbook.yml --tags skills`
(сначала должен быть выполнен тег `runtimes` — нужен `npm`).

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

## Диагностика DNS и разрешения имён (gai, dnsglobe)

Роль `network` дополнительно ставит два инструмента для разбора проблем с DNS:

- **gai** (`casablanque-code/gai`) — инспектор `getaddrinfo`: показывает, каким
  путём ОС на самом деле превращает имя в IP (`/etc/hosts`, mDNS,
  systemd-resolved, DNS), и сверяет его с независимым DNS-запросом. Ловит
  случай, когда `dig` отвечает, а процесс резолвит иначе (например, имя
  перехвачено в `/etc/hosts`). Без LD_PRELOAD/eBPF/ptrace — читает те же
  конфиги, что и ОС.
- **dnsglobe** (`514-labs/dnsglobe`) — TUI-проверка распространения DNS-записи:
  параллельно опрашивает 34 публичных резолвера по миру, сравнивает ответы,
  показывает расхождения и прогресс на карте, умеет watch-режим до полной
  пропагации. В узком терминале рисует вращающийся глобус, в широком — плоскую
  карту.

```bash
gai doctor testhost.local       # разобрать путь разрешения имени и вынести вердикт
gai explain example.com         # только путь разрешения, без вердикта
dnsglobe example.com            # проверка распространения A-записи
dnsglobe example.com A --once   # один прогон текстом (без TTY)
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

### atuin — история команд (не используется)

**atuin** сознательно исключён из плейбука. `atuin init bash` подключает
`bash-preexec`, который на bash ≥ 5.3 вешает preexec-хук в `PS0` строкой
`${ ... }`. Интеграция VTE (`/etc/profile.d/vte-2.91.sh`, gnome-terminal)
заканчивает свой `PS0` последовательностью `\e\\` (OSC-терминатор ST), чей
завершающий `\` экранирует `$` хука — и строка
`${ __bp_invoke_preexec_from_ps0 "..." >&2; }` печатается буквально перед
каждой командой.

Баг на стороне VTE, исправлен только в vte 0.84:

- [GNOME/vte#2951](https://gitlab.gnome.org/GNOME/vte/-/work_items/2951)
- [rcaloras/bash-preexec#186](https://github.com/rcaloras/bash-preexec/issues/186)

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

### M-Commander (двухпанельный файловый менеджер)

Роль `shell` ставит **M-Commander** (`blue-panels/mcommander`) — форк GNU
Midnight Commander: две панели, встроенный редактор и вьюер (со структурным
деревом для JSON/YAML/XML/HTML), встроенный терминал и подключаемые панели для
git, docker, Kubernetes, S3, FTP/SFTP, Samba, systemd и др. Главная команда —
`mcommander`, короткий алиас — `mc6`; ставится рядом с пакетом `mc` из Ubuntu,
не заменяя его.

Ставится официальными `.deb`-пакетами из GitHub-релиза (базовый пакет +
`plugins` + `lua`), сборка под Ubuntu 26.04 (`ubuntu26.04.1`). Версия не
фиксируется — тег последнего релиза берётся из редиректа `releases/latest`.
Все три пакета ставятся одной транзакцией apt (у `plugins`/`lua` зависимость
`mcommander (= версия)`), повторный запуск ничего не переустанавливает.

```bash
mcommander          # запустить файловый менеджер
mc6                 # то же самое (короткий алиас)
mcedit6 file.txt    # сразу редактор
```

Настройки — в `roles/shell/defaults/main.yml` (`mcommander_*`): репозиторий,
список `.deb`-пакетов, суффикс сборки; отключение через
`mcommander_install: false`.
