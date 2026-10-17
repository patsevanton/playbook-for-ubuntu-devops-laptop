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
│   ├── shell/                  # eza, zoxide, fzf, starship, atuin, bat, конфиг shell
│   ├── network/                # nmap, netcat, tcpdump, httpie, dnsutils, telnet
│   ├── opencode/               # opencode (из npm-реестра)
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
10. Добавляет пользователя в группу `docker`.
11. В конце проверяет установку и печатает версии всех утилит.

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

Отдельно и без пинов: `ansible-core`, `python3-apt` (нужны для запуска самого плейбука).

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
| atuin | github.com/atuinsh/atuin |
| bat | github.com/sharkdp/bat |
| opencode | registry.npmjs.org/@opencode/cli-linux-x64 |

Для справки, версии, которые ставились при последней проверке плейбука
(2026-10-17; **не фиксируются**, у вас будут актуальные):
yq 4.54.1, kubectl 1.37.1, helm 4.3.1, k9s 0.51.0, kustomize 5.8.3, tflint 0.64.0,
terraform-docs 0.24.0, packer 1.16.1, vault 2.2.0, starship 1.27.0, atuin 18.24.1,
bat 0.26.1, opencode 2.0.32.

### Менеджеры версий и установщики

| Утилита | Как ставится |
|---|---|
| tfenv | `git clone` в `~/.tfenv` (только если его ещё нет) |
| tgenv | `git clone` в `~/.tgenv` (только если его ещё нет) |
| uv | официальный installer Astral в `/usr/local/bin` |
| opencode | TGZ из npm-реестра → `/usr/local/bin/opencode` |

---

## Как определяется «последняя версия»

Версия запрашивается **только для тех инструментов, которых ещё нет** в
`/usr/local/bin` — если всё установлено, плейбук вообще не ходит в сеть.

GitHub API не используется: у него лимит 60 неавторизованных запросов в час,
и плейбук легко в него упирается. Вместо этого:

- **GitHub-релизы** (`source: github`): берётся HTTP-редирект
  `https://github.com/<repo>/releases/latest` и из заголовка `Location`
  вытаскивается тег последнего релиза. Лимитов нет.
- **Текстовые endpoint'ы** (`source: text`): версия читается напрямую, например
  `https://dl.k8s.io/release/stable.txt` (kubectl) и
  `https://get.helm.sh/helm-latest-version` (helm).
- **npm** (opencode): `https://registry.npmjs.org/@opencode/cli-linux-x64/latest`.
- **uv**: installer без версии в URL — всегда актуальная версия.

Как поменять источник или добавить инструмент: правьте соответствующий
список в `roles/<роль>/defaults/main.yml`. Формат элемента описан в
`roles/binaries/defaults/main.yml`:

```yaml
- name: k9s                                # имя файла в /usr/local/bin
  source: github                           # github | text
  repo: derailed/k9s                       # owner/repo (source=github)
  url: "https://github.com/derailed/k9s/releases/download/{tag}/k9s_Linux_amd64.tar.gz"
  archive: tar                             # none | tar | zip
  member: k9s                              # путь внутри архива
```

У `bat` это выглядит так: `member: bat-v{version}-x86_64-unknown-linux-gnu/bat` —
в `url` и `member` подставляются `{tag}` (тег релиза) и `{version}` (тот же тег
без префикса `v`). Это нужно, например, для `bat`, у которого бинарник лежит
во вложенном каталоге с версией в имени.

---

## Обновление и идемпотентность

Идемпотентность везде простая: **нет файла — ставим, есть файл — не трогаем.**

- apt-задачи идемпотентны и не понижают уже установленные пакеты.
- Бинарник в `/usr/local/bin` не переустанавливается, если файл уже существует.
  Актуальная версия при этом даже не запрашивается — лишних сетевых обращений нет.
- Обновить инструмент = удалить файл и запустить плейбук снова:

  ```bash
  sudo rm /usr/local/bin/kubectl
  ansible-playbook -K playbook.yml --tags kubernetes
  ```

- `uv` и `opencode` ставятся, только если их нет; `uv` умеет обновлять себя сам
  (`uv self update`), у `opencode` для обновления удалите файл.
- `tfenv`/`tgenv` клонируются один раз (если нет `~/.tfenv/bin/tfenv` / `~/.tgenv/bin/tgenv`).
- Скачанные архивы остаются в `/var/tmp/devops-install` (каталог можно чистить вручную).

Если нужно **зафиксировать** версии (по умолчанию не нужно), есть варианты:

- apt: обычный `apt-mark hold <пакет>` или файл в `/etc/apt/preferences.d/`;
- бинарники: подставить конкретный тег вместо шаблона `{tag}` в `url`
  соответствующей роли.

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
- `direnv hook bash`, `zoxide init bash`, `atuin init bash`, `starship init bash`;
- алиасы `ls`/`ll`/`la`/`lt` на `eza`;
- алиас `catbat` и тема для `bat`;
- алиас `fd` → `fdfind` (в Ubuntu бинарь `fd-find` называется `fdfind`);
- completion для `kubectl`, `helm`, `kubectx`, алиас `k = kubectl`;
- `PIPX_HOME` / `PIPX_BIN_DIR`.

Файл пишется из шаблона `roles/shell/templates/devops-shell.sh.j2` — правки
в самом файле перезапишутся при следующем запуске. Чтобы убрать окружение:
удалите файл и блок между маркерами в `~/.bashrc`.

---

## Проверка установки

Плейбук в конце сам печатает версии всех утилит. Вручную:

```bash
kubectl version --client
helm version --short
k9s version -s
terraform-docs --version
docker compose version
uv --version
opencode --version
tfenv --version
```

---

## Переменные проекта

Все настройки — в `group_vars/all.yml`:

| Переменная | По умолчанию | Смысл |
|---|---|---|
| `user_name` | текущий пользователь | чей `~/.bashrc` и каталоги правим |
| `home_dir` | `$HOME` пользователя | домашний каталог |
| `bin_dir` | `/usr/local/bin` | куда ставятся скачанные бинарники |
| `tmp_dir` | `/var/tmp/devops-install` | загрузки и распаковка |
| `shell_file` | `~/.config/devops-shell.sh` | файл окружения |
| `http_user_agent` | `ansible-devops-playbook` | User-Agent для запросов |

Переопределить можно на лету, без правки файлов:

```bash
ansible-playbook -K playbook.yml -e bin_dir=/usr/bin
```

Списки устанавливаемого лежат в `roles/<роль>/defaults/main.yml`.

Переменные ролей названы с префиксом роли (`docker_packages`,
`kubernetes_binaries`, `tfenv_dir`), общие переменные проекта — без префикса
и живут только в `group_vars/all.yml`.

---

## Что сознательно НЕ ставится

| Утилита | Почему |
|---|---|
| terraform, terragrunt | управляются вручную через tfenv/tgenv |
| tmux, htop, tree, vim | не выбраны при сборе требований |
| build-essential | не выбрано |
| gnupg | не нужен: ключ Docker подключается как ASCII-armored `.asc` |
| awscli, azure-cli, gcloud | не выбраны |
| sops, age, pass | не выбраны |
| wireshark | не выбрано |
| kind, kubens | не выбраны (из kubectx-набора только `kubectx`) |
| zsh, oh-my-zsh | не выбраны; shell остаётся bash |
| docker-compose V1 | EOL, отсутствует в официальных источниках |

---

## Лицензия

Apache License 2.0 (см. `LICENSE`).
