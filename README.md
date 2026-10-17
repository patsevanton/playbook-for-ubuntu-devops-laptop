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
│   ├── docker/                 # репозиторий Docker, Engine, Compose V2
│   ├── kubernetes/             # kubectl, helm, k9s, kustomize, kubectx
│   ├── iac/                    # tflint, terraform-docs, packer, tfenv, tgenv
│   ├── runtimes/               # Python, pipx, uv, Go, Node.js, Java
│   ├── secrets/                # direnv, vault
│   ├── shell/                  # eza, zoxide, fzf, starship, bat, конфиг shell
│   ├── network/                # nmap, netcat, tcpdump, httpie, dnsutils, telnet
│   ├── opencode/               # opencode (из npm-реестра)
│   ├── guardrails/             # guardrails-llm-filter (systemd-юнит поверх Docker)
│   └── binaries/               # общий движок установки бинарников (вызывается ролями)
├── .ansible-lint
└── README.md
```

Роль `binaries` не тянет инструменты сама: её вызывают роли `base`, `kubernetes`,
`iac`, `secrets`, `shell`, передавая список того, что нужно поставить.
Каждая роль хранит свой список в `roles/<роль>/defaults/main.yml`.

Имя `playbook.yml` выбрано осознанно; в чужих проектах точку входа часто называют
`site.yml` — переименование ничего не ломает.

---

## Что делает плейбук

1. Проверяет, что ОС — Ubuntu, архитектура — x86_64, и создаёт служебные каталоги.
2. Ставит пакеты из репозитория Ubuntu (актуальные версии, без пинов).
3. Подключает официальный репозиторий Docker и ставит Engine + Compose V2.
4. Ставит клиенты Kubernetes: kubectl, helm, k9s, kustomize, kubectx.
5. Ставит инструменты IaC: tflint, terraform-docs, packer, плюс tfenv и tgenv.
6. Ставит рантаймы: Python (venv/pip/pipx), uv, Go, Node.js, Java 21.
7. Ставит direnv и Vault.
8. Настраивает shell: `~/.config/devops-shell.sh` + одна строка `source` в `~/.bashrc`.
9. Ставит opencode (из npm-реестра, в `/usr/local/bin`).
10. Поднимает `guardrails-llm-filter` — systemd-юнит, который запускает
    Docker-контейнер маскировки PII/секретов на пути к LLM.
11. Добавляет пользователя в группу `docker`.
12. В конце проверяет установку и печатает версии всех утилит.

---

## Что устанавливается

### Пакеты apt (актуальные версии из репозитория Ubuntu)

| Роль | Пакеты | Зачем |
|---|---|---|
| base | ca-certificates, curl, wget, git, jq, unzip, zip, tar, gzip, xz-utils, bash-completion, lsb-release | база для работы и для самого плейбука |
| runtimes | python3-venv, python3-pip, python3-dev, pipx, golang-go, nodejs, npm, openjdk-21-jdk | языковые рантаймы |
| secrets | direnv | окружение по каталогам |
| shell | eza, zoxide, fzf, ripgrep, fd-find | удобная работа в терминале |
| network | nmap, netcat-openbsd, tcpdump, httpie, bind9-dnsutils, telnet | сеть и диагностика |
| docker | docker-ce, docker-ce-cli, containerd.io, docker-compose-plugin | контейнеры (см. ниже) |

### Скачиваемые бинарники в `/usr/local/bin`

| Утилита | Источник |
|---|---|
| yq | github.com/mikefarah/yq |
| kubectl | dl.k8s.io |
| helm | get.helm.sh |
| k9s | github.com/derailed/k9s |
| kustomize | github.com/kubernetes-sigs/kustomize |
| kubectx | github.com/ahmetb/kubectx (shell-скрипт, ветка master) |
| tflint | github.com/terraform-linters/tflint |
| terraform-docs | github.com/terraform-docs/terraform-docs |
| packer | releases.hashicorp.com |
| vault | releases.hashicorp.com |
| starship | github.com/starship/starship |
| bat | github.com/sharkdp/bat |
| opencode | registry.npmjs.org/@opencode/cli-linux-x64 |

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
