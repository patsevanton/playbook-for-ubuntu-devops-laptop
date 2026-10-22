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
│   │                           #   yq, eget, dra, binup, ansible-lint, molecule
│   ├── git/                    # gh, glab, gitlabcloner, glci, pre-commit, reposcan,
│   │                           #   lazygit, wt (worktrunk), changie, ghgrab, glaball,
│   │                           #   reviewdog, typos, act, jj (jujutsu),
│   │                           #   gitui (TUI для git),
│   │                           #   lumen, gptcommit, codegpt (LLM-коммиты),
│   │                           #   gitlabform (pipx)
│   ├── docker/                 # репозиторий Docker, Engine, Compose V2,
│   │                           #   dive, lazydocker, ctop, oras, dry, decompose,
│   │                           #   copa (copacetic), hadolint,
│   │                           #   docker-image-layer-explorer
│   ├── kubernetes/             # kubectl, helm, k9s, kustomize, kubectx, flux, flux9s,
│   │                           #   stern, flate, flux-tools, cty, diffyml,
│   │                           #   kubectl-k8i, kubespy, kubecolor, kube-capacity,
│   │                           #   ktop, kubefetch, krew, view-secret, df-pv,
│   │                           #   browse-pvc, pluto, pv-migrate, marvin,
│   │                           #   kftui, ct (chart-testing), ytt, kbld,
│   │                           #   chartsnap, kubewall, kubevpn, netfetch,
│   │                           #   timoni, kubeview, kubehound, charts-syncer,
│   │                           #   netobserv, capacitor, kubebox (npm),
│   │                           #   kubeconform, kube-linter, helmfile, kind,
│   │                           #   kops, devpod, kompose, ko,
│   │                           #   kubie, kubefwd, kubectl-tree, conftest,
│   │                           #   kube-bench, kube-hunter (pipx),
│   │                           #   cdk8s (npm), kubernetes-mcp-server,
│   │                           #   kubecm, kubectl-slice, helmify, kail,
│   │                           #   kubeaudit, rbac-tool, kluctl, kube-burner,
│   │                           #   kwok/kwokctl, kpexec, groot, k2tf, kubesec,
│   │                           #   kpt, kube-job, kubectl-sniff,
│   │                           #   static-tcpdump, kdash, kubetail, kubepug,
│   │                           #   kube-prompt, rbac-lookup, kubectl-who-can,
│   │                           #   korb, kl, kubectl-doctor, kbom,
│   │                           #   kubectl-modify-secret, silver-surfer,
│   │                           #   kubeprompt, kustomizer, buoy, k9sight,
│   │                           #   hanoi-cli, kzero/kubectl-kzero, ktunnel,
│   │                           #   kubectl-duplicate, kubectl-fd,
│   │                           #   kubectl-ports (тег 1.1.0),
│   │                           #   kubectl-relay, gwctl, kbu, kurt,
│   │                           #   kubediff, kubetrim, kubevious, k8shark,
│   │                           #   node-gizmo, burn, tk, audit2rbac,
│   │                           #   knetvis (pipx), pumba, kopilot,
│   │                           #   kubectl-outagedeck, kptop (pipx),
│   │                           #   keploy, okteto, garden, ig, peirates,
│   │                           #   helmsman, kexp, gefyra, reckoner, copper,
│   │                           #   alterant, crashd, kubescout, k8s-insider,
│   │                           #   patchloom, klustr (.deb), kubectl-fields,
│   │                           #   kosko (npm), headlamp, radar,
│   │                           #   k8sgpt, kubescape, popeye, vcluster,
│   │                           #   skaffold, kubeshark, helm-diff (плагин helm),
│   │                           #   kubectl-ai, k3d, telepresence, tilt, mirrord,
│   │                           #   devspace, kubent, monokle и rubick (UI, .deb)
│   ├── iac/                    # tflint, terraform-docs, packer, driftctl, tgsieve,
│   │                           #   s5cmd, tpm, tfvar, config-lint, tftui (pipx),
│   │                           #   tf-mirror, gitlab-terraform-manager,
│   │                           #   inframap, rover (схемы из state/плана),
│   │                           #   terramate, infracost, graphviz (apt),
│   │                           #   terraformer, terrascan, steampipe,
│   │                           #   checkov (pipx), tfenv, tgenv
│   ├── postgres/               # pgCenter (админка и мониторинг PostgreSQL),
│   │                           #   pgcli (pipx)
│   ├── runtimes/               # Python, uv, Go, Node.js, Java, dagger, devbox,
│   │                           #   flox, Lima (Linux-VM, /opt + симлинки)
│   ├── secrets/                # direnv, vault, gitleaks, sops, grype, trivy,
│   │                           #   age, lynis, osquery, mfa,
│   │                           #   medusa, bank-vaults, horusec,
│   │                           #   trufflehog, syft, cosign,
│   │                           #   dependency-check (/opt + обёртка),
│   │                           #   entropy (go install), ochrona (pipx)
│   ├── shell/                  # eza, zoxide, fzf, starship, bat, difftastic,
│   │                           #   delta, gdu, dust, dasel, jc, sd, shellcheck,
│   │                           #   duf, tv (television), witr,
│   │                           #   jnv, octosql, lazyjournal, gonzo, otel-tui, jl,
│   │                           #   logdy, jlv, spacer,
│   │                           #   lf, gomplate, yamlfmt, yamlfixer (pipx), gum,
│   │                           #   fastfetch, procs, dusage,
│   │                           #   mise, just, yazi, glow, btm (bottom),
│   │                           #   hyperfine, duckdb, vhs, harlequin (pipx),
│   │                           #   lazysql, shellclear, nxs-backup, cheat, tldr,
│   │                           #   JetBrainsMono Nerd Font, mcommander, конфиг shell
│   ├── network/                # nmap, netcat, tcpdump, httpie, dnsutils, telnet,
│   │                           #   gai, dnsglobe, bandwhich, q, xh, hurl, trippy,
│   │                           #   tcping, doggo, gotty, sshx, txeh, assh,
│   │                           #   k6, gping,
│   │                           #   rkn-block-checker (pipx), ssh-audit и httptap (pipx),
│   │                           #   lexicon (pipx), gossl, crip (certificate-ripper),
│   │                           #   ecapture, termshark, websocat, sniffnet,
│   │                           #   rustnet, netutils-linux (pipx), reachable (go install)
│   ├── ebpf/                   # eBPF-наблюдаемость: pwru, bpftop, oryx,
│   │                           #   pktstat-bpf, tracee
│   ├── opencode/               # opencode (из npm-реестра) + глобальный AGENTS.md
│   ├── skills/                 # глобальные skills для opencode (CLI `skills`)
│   ├── audio/                  # HushMic (шумоподавление микрофона, .deb)
│   ├── hardware/               # LACT (настройка и мониторинг GPU, .deb + lactd)
│   ├── guardrails/             # guardrails-llm-filter (systemd-юнит поверх Docker)
│   ├── nora/                   # NORA artifact registry + mkcert TLS + CA клиентам,
│   │                           #   Caddy как TLS-терминатор
│   ├── monitoring/             # vmalert-tool (проверка алерт-правил), sloth (SLO),
│   │                           #   cloudprober, canary-checker, coroot, krr
│   ├── binaries/               # общий движок установки бинарников (вызывается ролями)
│   ├── pipx_apps/              # общий движок установки приложений из PyPI (pipx)
│   └── go_apps/                # общий движок сборки Go-приложений (go install)
├── .ansible-lint
└── README.md
```

Роль `binaries` не тянет инструменты сама: её вызывают роли `base`, `git`,
`kubernetes`, `iac`, `postgres`, `runtimes`, `secrets`, `shell`, `network`,
`opencode`, передавая список того, что нужно поставить. Каждая роль хранит
свой список в `roles/<роль>/defaults/main.yml`. Так же устроены `pipx_apps`
(приложения из PyPI через pipx: `git`, `iac`, `postgres`, `secrets`, `shell`)
и `go_apps` (сборка из исходников: `secrets`, `network`).

Имя `playbook.yml` выбрано осознанно; в чужих проектах точку входа часто называют
`site.yml` — переименование ничего не ломает.

---

## Что делает плейбук

1. Проверяет, что ОС — Ubuntu, архитектура — x86_64, и создаёт служебные каталоги.
2. Ставит пакеты из репозитория Ubuntu (актуальные версии, без пинов),
   включая `pipx`, `ansible-lint` (проверка стиля плейбуков), `molecule`
   (тестирование Ansible-ролей в контейнерах, ставится через pipx), `eget`
   (установщик бинарников из GitHub-релизов), `dra` (скачивание отдельных
   ассетов GitHub-релиза) и `binup` (установка приложений из релизов по
   декларативному манифесту).
3. Ставит клиенты GitHub и GitLab: `gh`, `glab`, `gitlabcloner`, а также `glci`
   (локальный запуск GitLab CI), `pre-commit`, `reposcan`, `lazygit`
   (TUI для git), `wt` (worktrunk — параллельные ветки через git worktree),
   `changie` (фрагменты changelog и подготовка релизов), `ghgrab` (TUI-браузер
   файлов GitHub/GitLab/Gitea), `glaball` (массовые команды на нескольких
   self-hosted GitLab-инстансах), `gitlabform` (GitLab как код, через pipx),
   `typos` (поиск опечаток в коде и документации с автоисправлением), `act`
   (локальный запуск GitHub Actions в Docker — пара к `glci`), `jj`
   (jujutsu — VCS поверх обычного git-репозитория), `gitui` (быстрый TUI для
   git на Rust), а также CLI для
   работы с коммитами через LLM: `lumen` (просмотр диффа и сообщение коммита),
   `gptcommit` (git-хук `prepare-commit-msg`) и `codegpt` (сообщения и
   ревью).
4. Подключает официальный репозиторий Docker и ставит Engine + Compose V2,
   а также `dive` (анализ слоёв образа), `lazydocker` (TUI-менеджер контейнеров),
    `ctop` (`top` для метрик контейнеров), `oras` (клиент OCI-реестров),
    `dry` (терминальный менеджер Docker), `decompose` (карта образов, сетей
    и томов существующего docker-окружения), `copa` (copacetic — патчинг
    CVE прямо в образах по отчёту grype/trivy), `hadolint` (линтер Dockerfile)
    и `docker-image-layer-explorer` (список слоёв образа и распаковка
    выбранного слоя).
5. Ставит клиенты Kubernetes: kubectl, helm, k9s, kustomize, kubectx, flux, flux9s,
   stern, flate, flux-tools (офлайн-рендер и дифф Flux GitOps), cty (sample-YAML
   из CRD), diffyml (структурный дифф YAML), kubectl-k8i (ресурсы нод),
   kubespy (наблюдение за ресурсами в реальном времени), kubecolor (цветной
   вывод kubectl), kube-capacity и ktop (ресурсы и метрики кластера),
   kubefetch (сводка о кластере), krew (менеджер kubectl-плагинов) и три
   утилиты бинарниками — view-secret (декодирование секретов), df-pv
   (`df` по PV), browse-pvc (просмотр PVC) — а также pluto (устаревшие
   apiVersion), pv-migrate (миграция PV), marvin (сканер кластера
   CEL-выражениями), kftui (TUI-менеджер port-forward), ct (линтинг и
   тестирование Helm-чартов), ytt (структурный шаблонизатор YAML), kbld
   (фиксация образов в манифестах), chartsnap (snapshot-тесты Helm-чартов),
   kubewall (дашборд на несколько кластеров), kubevpn (VPN в кластер для
   локальной разработки), netfetch (поиск ворклоадов без NetworkPolicy),
   timoni (пакетный менеджер Kubernetes на CUE), kubeview (граф ресурсов
   кластера), kubehound (поиск путей атак в кластере), charts-syncer
   (синхронизация Helm-репозиториев), netobserv (сетевая наблюдаемость на
   eBPF без агента), capacitor (UI для FluxCD), kubeconform (валидация
   манифестов по схемам), kube-linter (антипаттерны в манифестах), helmfile
   (декларативный набор Helm-релизов), kind (локальный кластер Kubernetes в
   Docker), k3d (k3s в Docker — локальные кластеры из контейнеров),
   kubebox (терминальная и веб консоль Kubernetes, npm),
   headlamp (web/desktop UI Kubernetes) и radar (UI Kubernetes со встроенным
   MCP-сервером для AI-агентов, .deb), а также kops (жизненный цикл
   production-кластеров), devpod (dev-окружения по `devcontainer.json`),
   kompose (docker-compose → Kubernetes),
   ko (сборка Go-образов и деплой в кластер), kubie (изолированные
   kubeconfig по shell/каталогу), kubefwd (массовый port-forward с записями
   в `/etc/hosts`), kubectl-tree (дерево объектов по ownerReferences),
   conftest (политики Rego/OPA для конфигов), kube-bench (CIS Kubernetes
   Benchmark), kube-hunter (поиск уязвимостей кластера, через pipx),
   cdk8s (манифесты кодом, через npm) и kubernetes-mcp-server (MCP-сервер
   для AI-агентов). Дополнительно ставятся kubecm (управление kubeconfig),
   kubectl-slice (разбиение multi-doc YAML), helmify (Helm-чарт из
   манифестов), kail (`tail` логов подов по селектору), kubeaudit (аудит
   security-мисконфигов), rbac-tool (анализ RBAC), kluctl (декларативный
   GitOps-деплой), kube-burner (нагрузочные тесты кластера),
   kwok/kwokctl (симуляция нод без kubelet), kpexec (привилегированный exec
   в контейнере), groot (диагностический архив кластера для RCA), k2tf
   (Kubernetes YAML → Terraform HCL), kubesec (риск-анализ манифестов),
   kpt (пакеты Kubernetes-конфигов) и kube-job (одноразовые Job из CLI).
   В том же наборе — kubectl-sniff и static-tcpdump (сниффинг трафика
   подов), kdash (TUI-дашборд), kubetail (дашборд логов), kubepug
   (устаревшие API), kube-prompt (интерактивный kubectl), rbac-lookup и
   kubectl-who-can (разбор RBAC), korb (перенос PVC), kl (логи нескольких
   контейнеров), kubectl-doctor (триаж кластера), kbom (инвентарь/SBOM
   кластера) и kubectl-modify-secret (правка секретов без base64).
   Следующая волна kubetools: silver-surfer (проверка совместимости
   apiVersion при апгрейде), kubeprompt (контекст кластера в prompt),
   kustomizer (пакеты конфигов как OCI-артефакты), buoy (декларативный
   TUI-дашборд), k9sight (TUI для отладки workload'ов), hanoi-cli (советник
   по ребалансировке подов), kzero/kubectl-kzero (декларативные down/up
   кластера), ktunnel (туннель в кластер без SSH), kubectl-duplicate
   (дублирование ресурсов), kubectl-fd (поиск ресурсов в духе find),
   kubectl-relay (замена port-forward), gwctl (разбор Gateway API) и kbu
   (однопанельный TUI). Новая волна kubetools: kurt (перезапуски подов),
   kubediff (дифф исходников и кластера), kubetrim (чистка kubeconfig),
   kubevious (проверка конфигов), k8shark (снимок и replay), node-gizmo
   (ноды), burn (стоимость), tk (Grafana Tanka), audit2rbac (RBAC из
   audit-логов), knetvis (NetworkPolicy, pipx), pumba (chaos), kopilot
   (AI-ассистент) и kubectl-outagedeck (статус облаков). Ещё через pipx —
   kptop (Prometheus-метрики). Отдельной задачей ставится kubectl-ports —
   плагин `kubectl ports` с таблицей портов подов и сервисов (тег
   зафиксирован на 1.1.0: в последнем релизе апстрима ассетов нет).
   Следующая волна kubetools — самые популярные
   из оставшихся кандидатов: keploy (тест-кейсы
   из реального трафика), okteto (разработка в кластере), garden
   (оркестратор разработки), ig/Inspektor Gadget (eBPF-гаджеты), peirates
   (пентест изнутри кластера), helmsman (Helm-релизы как код), kexp
   (визуальное понимание Kubernetes), gefyra (локальная разработка с
   кластером), reckoner (Helm-релизы из YAML), copper (валидация конфигов),
   alterant (точечные правки YAML), crashd (диагностика сбоев), kubescout
   (тревожные состояния по кластерам), k8s-insider (доступ к сети кластера)
   и patchloom (структурная правка конфигов для AI-агентов).
   Последняя волна: kubectl-fields (иерархия полей ресурса) и kosko
   (манифесты на TypeScript, через npm).
   Добавлены k8sgpt (объяснение проблем кластера через LLM), kubescape
   (профили безопасности NSA/CIS), popeye (мёртвые и неиспользуемые
   ресурсы кластера), vcluster (виртуальные кластеры внутри namespace),
   skaffold (цикл сборка→деплой→проверка с hot-reload), kubeshark (разбор
   трафика подов на L7 через eBPF), `kubectl-ai` (AI-ассистент: генерирует и
   правит kubectl-команды и манифесты), `telepresence` (локальный сервис «как
   будто в кластере»), `tilt` (live-update приложений с веб-UI), `mirrord`
   (локальный процесс в сети, ФС и окружении пода), `devspace` (синхронизация
   файлов и hot-reload в кластере по `devspace.yaml`), `kubent` (что сломается
   при апгрейде: удалённые apiVersion), а также плагин `helm-diff`
   (`helm diff upgrade` — что изменится до применения чарта; ставится
   отдельной задачей).
   Desktop-клиенты `monokle`, `rubick` и
   `klustr` ставятся `.deb`-пакетами. Для запуска `kptop` нужен
   `KPTOP_CONNECTION_METHOD`, для `kopilot` — `KOPILOT_TOKEN`; установка
   от этих переменных не зависит.
6. Ставит инструменты IaC: tflint, terraform-docs, packer, driftctl, tgsieve
   (сжатый вывод `terragrunt plan`), s5cmd (быстрый параллельный клиент S3),
   tpm (менеджер провайдеров Terraform/OpenTofu), tfvar (генерация
   `variables.tf`), config-lint (валидация HCL/JSON/YAML по правилам),
   tftui (TUI для работы с Terraform-стейтом, через pipx), tf-mirror
   (локальное зеркало провайдеров Terraform для air-gapped/CI),
   gitlab-terraform-manager (управление GitLab Terraform backend),
   checkov (SAST для IaC: мисконфиги безопасности Terraform, Kubernetes,
   Dockerfile, Helm, через pipx),
   inframap и rover (схемы из tfstate/плана, см. ниже), terramate
   (оркестрация стэков IaC: change detection и генерация кода), infracost
   (стоимость изменений по `terraform plan`), terraformer (генерация
   Terraform-кода из существующей инфраструктуры), terrascan (проверка IaC по
   политикам OPA/Rego), steampipe (SQL-запросы к облакам и API через плагины),
   graphviz (`dot` для рендера графов),
   плюс tfenv и tgenv.
7. Ставит pgCenter — консольную админку и мониторинг PostgreSQL, и `pgcli`
   (psql с автодополнением и подсветкой, через pipx).
8. Ставит рантаймы: Python (venv/pip), uv, Go, Node.js, Java 21, `dagger`
   (CI-пайплайны как код), `devbox` (изолированные окружения на Nix),
   `flox` (детерминированные окружения на Nix), `limactl`/`lima` (Linux-VM
   поверх Linux; QEMU — пакетами apt, сам архив распаковывается в `/opt/lima`).
9. Ставит direnv, Vault, gitleaks, sops (шифрование секретов в Git), grype
   (сканер уязвимостей образов и ФС), trivy (уязвимости образов, IaC и
   секреты в одном сканере), age (простое шифрование файлов — бэкенд sops),
   lynis (аудит безопасности и харденинга Linux), osquery (SQL-подобная
   инструментация ОС),
   mfa (терминальный менеджер 2FA/TOTP), medusa (импорт/экспорт секретов
   Vault), bank-vaults (init/unseal и конфигурация Vault через YAML),
   horusec (SAST одной командой), dependency-check (OWASP SCA: уязвимые
   зависимости проекта, zip в `/opt` + обёртка), trufflehog (поиск секретов
   в истории git и файлах с проверкой, что ключ живой), syft (SBOM образов и
   каталогов в CycloneDX/SPDX), cosign (подпись и проверка артефактов
   sigstore), entropy (поиск строк с
   высокой энтропией — ключей и токенов — по признаку случайности, а не по
   шаблону; `go install`) и ochrona (SCA для Python-зависимостей: сверка
   версий с базой OSV и безопасные версии, через pipx).
10. Настраивает shell: `~/.config/devops-shell.sh` + одна строка `source` в `~/.bashrc`,
    JetBrainsMono Nerd Font (иконки для prompt и `eza --icons`),
    M-Commander (двухпанельный файловый менеджер) и утилиты просмотра
    difftastic (структурный diff), delta (улучшенный git-diff), gdu
    (анализатор диска), dust (наглядный du), dasel (запросы к
    JSON/YAML/TOML/XML/CSV), jc (конвертация вывода CLI в JSON), sd
    (интуитивная замена sed), shellcheck (статический анализ shell-скриптов),
    duf (`df` «для людей» — таблица дисков и точек монтирования), tv
    (television — fuzzy-finder с готовыми каналами поиска), witr (трассировка
    «почему процесс запущен и кто занял порт/файл»),
    zellij (терминальный мультиплексор), navi (шпаргалки команд поверх fzf),
    fx (интерактивный просмотр JSON), jnv (интерактивный jq-фильтр JSON),
    octosql (SQL-запросы к JSON/CSV/Parquet), hl (просмотр JSON-логов),
    lazyjournal (TUI для journald, auditd, Docker и файлов), gonzo
    (TUI-анализ логов с графиками по столбцам), otel-tui (просмотр
    OpenTelemetry-трейсов/логов/метрик), jl (раскраска JSON-логов),
    logdy (просмотр логов в браузере в реальном времени), jlv
    (интерактивный просмотр JSON-логов), spacer (разделители с таймингом
    между шагами пайплайна),
    mcfly (умная история команд по Ctrl-R), lf (терминальный файловый
    менеджер), gomplate (шаблонизатор конфигов), yamlfmt (форматтер YAML),
    yamlfixer (автоисправление замечаний yamllint, через pipx),
    gum (примитивы для «красивых» shell-скриптов), fastfetch (информация о
    системе), procs (современная замена ps), dusage (наглядный подсчёт
    занятого места), mise (менеджер версий рантаймов), just (командный
    раннер вместо Makefile), yazi (быстрый TUI-файловый менеджер), glow
    (рендер Markdown в терминале), btm (монитор процессов и ресурсов с
    графиками), hyperfine (бенчмарк команд), duckdb (SQL по
    Parquet/CSV/JSON), vhs (запись терминальных GIF/видео из сценария),
    harlequin (SQL-IDE в терминале, через pipx), lazysql (TUI-клиент к MySQL,
    PostgreSQL, MSSQL и SQLite), shellclear (секреты и опасные
    команды в истории shell), nxs-backup (бэкапы по YAML-описанию),
    cheat (собственные шпаргалки команд) и tldr (краткие примеры команд).
11. Ставит opencode (из npm-реестра, в `/usr/local/bin`) и глобальный
    `AGENTS.md` (`~/.config/opencode/AGENTS.md`).
12. Ставит глобальные skills для opencode (CLI `skills`, каталог
    `~/.agents/skills`) — см. раздел «Глобальные skills для opencode».
13. Ставит сетевую диагностику: `gai` (инспектор getaddrinfo), `dnsglobe`
    (проверка распространения DNS), `bandwhich` (трафик по процессам),
    `q` (DNS-клиент с DoH/DoT/DoQ), `doggo` (DNS-клиент с человекочитаемым
    цветным выводом), `xh` (HTTP-клиент), `hurl` (HTTP-запросы
    из plain-text `.hurl`-файлов), `trippy` (TUI traceroute со статистикой),
    `tcping` (`ping` поверх TCP-портов), `gotty` (терминал как веб-приложение),
    `sshx` (совместный терминал в браузере), `txeh` (управление `/etc/hosts`),
    `assh` (генерация SSH-конфигов, jump-хосты и прокси),
    `ecapture` (eBPF-захват TLS/SSL plaintext), `termshark` (TUI к
    Wireshark/tshark), `websocat` (netcat для WebSockets), `rustnet`
    (TUI-мониторинг сети по процессам с разбором пакетов) и `sniffnet`
    (графический TUI-мониторинг трафика, .deb), а также
    `rkn-block-checker` (через pipx), `kyanos` (eBPF-анализ сетевых запросов),
    `showcert` (разбор X.509-сертификатов, через pipx), `ssh-audit`
    (аудит конфигурации SSH-серверов и клиентов, через pipx), `httptap`
    (разбор HTTP-запроса по слоям: DNS, connect, TLS, TTFB, через pipx),
    `lexicon` (управление DNS-записями у разных провайдеров, через pipx),
    `gossl` (проверка SSL-сертификата: цепочка, срок, протоколы), `crip`
    (certificate-ripper — извлечение цепочки сертификатов с хоста),
    `netutils-linux` (утилиты тюнинга и диагностики стека: `network-top`,
    `softirq-top`, `rps`, `link-rate` и др., через pipx), `k6` (нагрузочное
    тестирование сценариями на JS), `gping` (`ping` с графиком задержек) и
    `reachable`
    (проверка доступности домена по резолверам; `go install`).
14. Ставит eBPF-наблюдаемость: `pwru` (eBPF-отладчик сетевого стека), `bpftop`
    (обзор запущенных eBPF-программ), `oryx` (TUI-сниффер трафика), `pktstat-bpf`
    (монитор трафика по процессам) и `tracee` (runtime-security и форензика).
15. Ставит HushMic — шумоподавление микрофона реального времени (PipeWire) —
    и LACT — GUI и демон `lactd` для настройки и мониторинга GPU (AMD/Intel/
    NVIDIA): температура, частоты, вентиляторы, лимиты мощности.
16. Поднимает `guardrails-llm-filter` — systemd-юнит, который запускает
    Docker-контейнер маскировки PII/секретов на пути к LLM.
17. Добавляет пользователя в группу `docker`.
18. Ставит NORA artifact registry: `.deb`-пакет, локальный TLS-сертификат через
    mkcert, автодоверие CA у клиентов (системный store, Docker, npm, pip, Java)
    и Caddy как TLS-терминатор.
19. Ставит `vmalert-tool` (офлайн-проверка и unit-тесты алерт-правил
    Prometheus-совместимых систем), `sloth` (генерация SLO/алертов из
    описания), `cloudprober` (активные пробы), `canary-checker` (health-check
    платформа с проверками из YAML), `coroot` (observability/APM с root-cause
    анализом) и `krr` (рекомендации
    requests/limits по фактическому потреблению) в роли `monitoring`, а также `reviewdog`
    (автоматическое code review) в роли `git` и `grex` (генерация regex из
    примеров) в роли `shell`.
20. В конце проверяет установку и печатает версии всех утилит.

---

## Что устанавливается

### Пакеты apt (актуальные версии из репозитория Ubuntu)

| Роль | Пакеты | Зачем |
|---|---|---|
| base | ca-certificates, curl, wget, git, jq, unzip, zip, tar, gzip, xz-utils, bash-completion, lsb-release, ansible-lint, pipx | база для работы, самого плейбука, проверки стиля и установки molecule |
| runtimes | python3-venv, python3-pip, python3-dev, golang-go, nodejs, npm, openjdk-21-jdk, qemu-system-x86, qemu-utils | языковые рантаймы; QEMU (`qemu-system-x86_64`, `qemu-img`) для VM в Lima |
| iac | graphviz | `dot`: рендер графов Terraform/Terragrunt (DOT → svg/png) |
| postgres | libpq5 | клиентская библиотека PostgreSQL (libpq.so) для psycopg/pgcli |
| secrets | direnv, age, lynis | окружение по каталогам; шифрование файлов для sops; аудит безопасности и харденинга Linux |
| shell | eza, zoxide, fzf, ripgrep, fd-find, fontconfig, shellcheck, duf, ttyd, ffmpeg | удобная работа в терминале, статический анализ shell-скриптов, понятная таблица дисков, ttyd и ffmpeg — запись терминальных демо через vhs |
| network | nmap, netcat-openbsd, tcpdump, httpie, bind9-dnsutils, telnet | сеть и диагностика |
| audio | pipewire, pipewire-bin, pipewire-pulse, wireplumber, hicolor-icon-theme | шумоподавление микрофона (HushMic) |
| docker | docker-ce, docker-ce-cli, containerd.io, docker-compose-plugin | контейнеры (см. ниже) |
| nora | mkcert, libnss3-tools, caddy, apache2-utils | mkcert (локальный CA), Caddy (TLS-терминатор), htpasswd (авторизация) |

Пакетами `.deb` (вне роли `binaries`) ставятся `mcommander` (роль `shell`),
`HushMic` (роль `audio`), `LACT` (роль `hardware`), `headlamp`, `radar`,
`monokle`, `rubick` и `klustr` (роль `kubernetes`), `flox` (роль
`runtimes`).

### Скачиваемые бинарники в `/usr/local/bin`

| Утилита | Источник |
|---|---|
| yq | github.com/mikefarah/yq |
| gh | github.com/cli/cli |
| glab | gitlab.com/gitlab-org/cli |
| gitlabcloner | github.com/Andrey039/gitlabcloner |
| glaball | github.com/flant/glaball |
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
| kftui | github.com/hcavarsan/kftray (TUI) |
| ct | github.com/helm/chart-testing |
| tflint | github.com/terraform-linters/tflint |
| terraform-docs | github.com/terraform-docs/terraform-docs |
| packer | releases.hashicorp.com |
| driftctl | github.com/snyk/driftctl |
| tgsieve | github.com/imcitius/tgsieve |
| inframap | github.com/cycloidio/inframap (тег v0.8.0: в v0.8.1 нет бинарников) |
| terramate | github.com/terramate-io/terramate |
| infracost | github.com/infracost/infracost |
| rover | github.com/im2nguyen/rover |
| s5cmd | github.com/peak/s5cmd |
| pgcenter | github.com/lesovsky/pgcenter |
| vault | releases.hashicorp.com |
| gitleaks | github.com/gitleaks/gitleaks |
| gai | github.com/casablanque-code/gai |
| dnsglobe | github.com/514-labs/dnsglobe |
| bandwhich | github.com/imsnif/bandwhich |
| q | github.com/natesales/q |
| xh | github.com/ducaale/xh |
| hurl | github.com/Orange-OpenSource/hurl |
| trippy | github.com/fujiapple852/trippy |
| tcping | github.com/pouriyajamshidi/tcping |
| doggo | github.com/mr-karan/doggo |
| ecapture | github.com/gojue/ecapture |
| termshark | github.com/gcla/termshark |
| websocat | github.com/vi/websocat |
| pwru | github.com/cilium/pwru |
| bpftop | github.com/jfernandez/bpftop |
| oryx | github.com/pythops/oryx |
| pktstat-bpf | github.com/dkorunic/pktstat-bpf |
| eget | github.com/zyedidia/eget |
| difftastic | github.com/Wilfred/difftastic |
| delta | github.com/dandavison/delta |
| gdu | github.com/dundee/gdu |
| dust | github.com/bootandy/dust |
| dasel | github.com/TomWright/dasel |
| jc | github.com/kellyjonbrazil/jc |
| sd | github.com/chmln/sd |
| lf | github.com/gokcehan/lf |
| gomplate | github.com/hairyhenderson/gomplate |
| yamlfmt | github.com/google/yamlfmt |
| gum | github.com/charmbracelet/gum |
| fastfetch | github.com/fastfetch-cli/fastfetch |
| procs | github.com/dalance/procs |
| cheat | github.com/cheat/cheat |
| tldr | registry.npmjs.org/tldr |
| gotty | github.com/yudai/gotty |
| oras | github.com/oras-project/oras |
| lazygit | github.com/jesseduffield/lazygit |
| lazydocker | github.com/jesseduffield/lazydocker |
| dive | github.com/wagoodman/dive |
| ctop | github.com/bcicen/ctop |
| sops | github.com/getsops/sops |
| grype | github.com/anchore/grype |
| trivy | github.com/aquasecurity/trivy |
| zellij | github.com/zellij-org/zellij |
| navi | github.com/denisidoro/navi |
| mcfly | github.com/cantino/mcfly |
| fx | github.com/antonmedv/fx |
| hl | github.com/pamburus/hl |
| kyanos | github.com/hengyoush/kyanos |
| starship | github.com/starship/starship |
| bat | github.com/sharkdp/bat |
| reviewdog | github.com/reviewdog/reviewdog |
| wt (worktrunk) | github.com/max-sixty/worktrunk |
| grex | github.com/pemistahl/grex |
| jnv | github.com/ynqa/jnv |
| octosql | github.com/cube2222/octosql |
| dry | github.com/moncho/dry |
| lazyjournal | github.com/Lifailon/lazyjournal |
| ytt | github.com/carvel-dev/ytt |
| changie | github.com/miniscruff/changie |
| mfa | github.com/LexSerest/mfa |
| vmalert-tool | github.com/VictoriaMetrics/VictoriaMetrics |
| sloth | github.com/slok/sloth |
| gonzo | github.com/control-theory/gonzo |
| otel-tui | github.com/ymtdzzz/otel-tui |
| jl | github.com/koenbollen/jl |
| kbld | github.com/carvel-dev/kbld |
| chartsnap | github.com/jlandowner/helm-chartsnap |
| kubewall | github.com/kubewall/kubewall |
| tpm | github.com/Madh93/tpm |
| tfvar | github.com/shihanng/tfvar |
| config-lint | github.com/stelligent/config-lint |
| tf-mirror | github.com/kashtan404/tf-mirror |
| gitlab-terraform-manager | github.com/CRASH-Tech/gitlab-terraform-manager |
| txeh | github.com/txn2/txeh |
| assh | github.com/moul/assh |
| decompose | github.com/s0rg/decompose |
| medusa | github.com/jonasvinther/medusa |
| bank-vaults | github.com/bank-vaults/bank-vaults |
| horusec | github.com/ZupIT/horusec |
| opencode | registry.npmjs.org/@opencode/cli-linux-x64 |
| dra | github.com/devmatteini/dra |
| ghgrab | github.com/abhixdd/ghgrab |
| kubevpn | github.com/kubenetworks/kubevpn |
| netfetch | github.com/deggja/netfetch |
| timoni | github.com/stefanprodan/timoni |
| kubeview | github.com/benc-uk/kubeview |
| kubehound | github.com/DataDog/KubeHound |
| charts-syncer | github.com/bitnami/charts-syncer |
| netobserv | github.com/netobserv/netobserv-cli |
| capacitor | github.com/gimlet-io/capacitor |
| copa | github.com/project-copacetic/copacetic |
| logdy | github.com/logdyhq/logdy-core |
| spacer | github.com/samwho/spacer |
| shellclear | github.com/rusty-ferris-club/shellclear |
| nxs-backup | github.com/nixys/nxs-backup |
| cloudprober | github.com/cloudprober/cloudprober |
| canary-checker | github.com/flanksource/canary-checker |
| coroot | github.com/coroot/coroot |
| gossl | github.com/vvrnv/gossl |
| crip (certificate-ripper) | github.com/Hakky54/certificate-ripper |
| jlv (json-log-viewer) | github.com/hedhyw/json-log-viewer |
| dusage | github.com/mihaigalos/dusage |
| binup | github.com/KonishchevDmitry/binup |
| lumen | github.com/jnsahaj/lumen |
| gptcommit | github.com/zurawiki/gptcommit |
| codegpt | github.com/appleboy/CodeGPT |
| typos | github.com/crate-ci/typos |
| act | github.com/nektos/act |
| hadolint | github.com/hadolint/hadolint |
| kubeconform | github.com/yannh/kubeconform |
| kube-linter | github.com/stackrox/kube-linter |
| k8sgpt | github.com/k8sgpt-ai/k8sgpt |
| kubescape | github.com/kubescape/kubescape |
| popeye | github.com/derailed/popeye |
| vcluster | github.com/loft-sh/vcluster |
| skaffold | github.com/GoogleContainerTools/skaffold |
| kubeshark | github.com/kubeshark/kubeshark |
| helmfile | github.com/helmfile/helmfile |
| kubebox | registry.npmjs.org/kubebox |
| mise | github.com/jdx/mise |
| just | github.com/casey/just |
| yazi | github.com/sxyazi/yazi |
| glow | github.com/charmbracelet/glow |
| btm (bottom) | github.com/ClementTsang/bottom |
| hyperfine | github.com/sharkdp/hyperfine |
| duckdb | github.com/duckdb/duckdb |
| tv (television) | github.com/alexpasmantier/television |
| witr | github.com/pranshuparmar/witr |
| jj (jujutsu) | github.com/jj-vcs/jj |
| k6 | github.com/grafana/k6 |
| gping | github.com/orf/gping |
| trufflehog | github.com/trufflesecurity/trufflehog |
| syft | github.com/anchore/syft |
| cosign | github.com/sigstore/cosign |
| kind | github.com/kubernetes-sigs/kind |
| kops | github.com/kubernetes/kops |
| devpod | github.com/loft-sh/devpod |
| kompose | github.com/kubernetes/kompose |
| ko | github.com/google/ko |
| kubie | github.com/kubie-org/kubie |
| kubefwd | github.com/txn2/kubefwd |
| kubectl-tree | github.com/ahmetb/kubectl-tree |
| conftest | github.com/open-policy-agent/conftest |
| kube-bench | github.com/aquasecurity/kube-bench |
| kubernetes-mcp-server | github.com/containers/kubernetes-mcp-server |
| kubecm | github.com/sunny0826/kubecm |
| kubectl-slice | github.com/patrickdappollonio/kubectl-slice |
| kubectl-ports | github.com/widnyana/kubectl-ports-rs (тег 1.1.0) |
| helmify | github.com/arttor/helmify |
| kail | github.com/boz/kail |
| kubeaudit | github.com/Shopify/kubeaudit |
| rbac-tool | github.com/alcideio/rbac-tool |
| kluctl | github.com/kluctl/kluctl |
| kube-burner | github.com/kube-burner/kube-burner |
| kwok, kwokctl | github.com/kubernetes-sigs/kwok |
| kpexec | github.com/ssup2/kpexec |
| groot | github.com/hrodrig/groot |
| k2tf | github.com/sl1pm4t/k2tf |
| kubesec | github.com/controlplaneio/kubesec |
| kpt | github.com/kptdev/kpt |
| kube-job | github.com/h3poteto/kube-job |
| kubectl-fields | github.com/rewanthtammana/kubectl-fields |
| kosko | registry.npmjs.org/kosko |
| silver-surfer | github.com/devtron-labs/silver-surfer |
| kubeprompt | github.com/jlesquembre/kubeprompt |
| kustomizer | github.com/stefanprodan/kustomizer |
| buoy | github.com/everettraven/buoy |
| k9sight | github.com/doganarif/k9sight |
| hanoi-cli | github.com/k-krew/hanoi-cli |
| kzero, kubectl-kzero | github.com/hrodrig/kzero |
| ktunnel | github.com/omrikiei/ktunnel |
| kubectl-duplicate | github.com/Telemaco019/duplik8s |
| kubectl-fd | github.com/alikhil/kubectl-find |
| kubectl-relay | github.com/knight42/krelay |
| gwctl | github.com/kubernetes-sigs/gwctl |
| kbu | github.com/vulcanshen/kbu |
| kurt | github.com/soraro/kurt |
| kubediff | github.com/Ramilito/kubediff |
| kubetrim | github.com/alexellis/kubetrim |
| kubevious | github.com/kubevious/cli |
| k8shark | github.com/phenixblue/k8shark (бинарник `kshrk`) |
| node-gizmo | github.com/Kavinraja-G/node-gizmo (бинарник `nodegizmo`) |
| burn | github.com/tanrikuluozlem/burn |
| tk (Grafana Tanka) | github.com/grafana/tanka |
| audit2rbac | github.com/liggitt/audit2rbac |
| pumba | github.com/alexei-led/pumba |
| kopilot | github.com/knight42/kopilot |
| kubectl-outagedeck | github.com/outagedeck/kubectl-outagedeck |
| kubectl-sniff, static-tcpdump | github.com/eldadru/ksniff |
| kdash | github.com/kdash-rs/kdash |
| kubetail | github.com/kubetail-org/kubetail |
| kubepug | github.com/kubepug/kubepug |
| kube-prompt | github.com/c-bata/kube-prompt |
| rbac-lookup | github.com/FairwindsOps/rbac-lookup |
| kubectl-who-can | github.com/aquasecurity/kubectl-who-can |
| korb | github.com/BeryJu/korb |
| kl | github.com/robinovitch61/kl |
| kubectl-doctor | github.com/emirozer/kubectl-doctor |
| kbom | github.com/rad-security/kbom |
| kubectl-modify-secret | github.com/rajatjindal/kubectl-modify-secret |
| keploy | github.com/keploy/keploy |
| okteto | github.com/okteto/okteto |
| garden | github.com/garden-io/garden |
| ig (Inspektor Gadget) | github.com/inspektor-gadget/inspektor-gadget |
| peirates | github.com/inguardians/peirates |
| helmsman | github.com/mkubaczyk/helmsman |
| kexp | github.com/iximiuz/kexp |
| gefyra | github.com/gefyrahq/gefyra |
| reckoner | github.com/FairwindsOps/reckoner |
| copper | github.com/cloud66-oss/copper |
| alterant | github.com/cloud66-oss/alterant |
| crashd (crash-diagnostics) | github.com/vmware-tanzu/crash-diagnostics |
| kubescout | github.com/ReallyLiri/kubescout |
| k8s-insider | github.com/TrueGoric/k8s-insider |
| patchloom | github.com/patchloom/patchloom |
| gitui | github.com/extrawurst/gitui |
| vhs | github.com/charmbracelet/vhs |
| dagger | github.com/dagger/dagger |
| devbox | github.com/jetify-com/devbox |
| terraformer | github.com/GoogleCloudPlatform/terraformer |
| terrascan | github.com/tenable/terrascan |
| steampipe | github.com/turbot/steampipe |
| kubectl-ai | github.com/GoogleCloudPlatform/kubectl-ai |
| k3d | github.com/k3d-io/k3d |
| telepresence | github.com/telepresenceio/telepresence |
| tilt | github.com/tilt-dev/tilt |
| mirrord | github.com/metalbear-co/mirrord |
| devspace | github.com/devspace-sh/devspace |
| kubent (kube-no-trouble) | github.com/doitintl/kube-no-trouble |
| rustnet | github.com/domcyrus/rustnet |
| lazysql | github.com/jorgerojas26/lazysql |

Отдельно (не через роль `binaries`): `molecule` ставится из PyPI через pipx в
`~/.local/bin` (роль `base`) вместе с драйвером docker (`molecule-plugins`),
`rkn-block-checker`, `showcert`,
`ssh-audit`, `httptap`, `lexicon` и `netutils-linux` — из PyPI через pipx в
`~/.local/bin` (роль `network`), `yamlfixer` — из PyPI через pipx (роль
`shell`), `ochrona` — из PyPI через pipx (роль `secrets`), `checkov` — из
PyPI через pipx (роль `iac`), `kube-hunter`, `knetvis` и `kptop` — из PyPI
через pipx (роль `kubernetes`), `gitlabform`, `tftui`, `pgcli` и `harlequin`
— из PyPI через pipx (общая роль
`pipx_apps`, вызывается из `git`, `iac`, `postgres`, `secrets`, `shell`,
`network` и `kubernetes`), `entropy` (роль `secrets`) и `reachable` (роль `network`) — сборкой из
исходников через `go install` (общая роль `go_apps`, вызывается из
`secrets` и `network`), `krr` — распаковкой PyInstaller-сборки в `/opt/krr` с обёрткой в
`/usr/local/bin` (роль `monitoring`), `dependency-check` — распаковкой
zip-архива в `/opt/dependency-check` с обёрткой в `/usr/local/bin` (роль
`secrets`; база NVD кэшируется в `~/.cache/dependency-check`),
`sshx` — из тарболла S3 (без релиз-ассетов GitHub, роль `network`),
`docker-image-layer-explorer` — одиночным релизным python-скриптом с
зафиксированным тегом `v0.1.0` (у апстрима нет релиза с меткой «Latest»,
роль `docker`), `kubectl-ports` — бинарником из релиза с зафиксированным
тегом `1.1.0` (в последнем релизе `1.1.1` ассетов нет, роль `kubernetes`),
`helm-diff` — helm-плагином (`helm plugin install`, каталог
`~/.local/share/helm/plugins`, роль `kubernetes`),
`headlamp`, `radar`, `monokle`, `rubick` и `klustr` — из
`.deb`-пакетов GitHub-релиза (роль `kubernetes`, desktop UI), `HushMic` — из
`.deb`-пакета (роль `audio`), `mcommander` — из `.deb`-пакетов (роль `shell`),
`sniffnet` — из `.deb`-пакета GitHub-релиза (роль `network`), `flox` — из
`.deb`-пакета с `downloads.flox.dev` (роль `runtimes`, версия из
`stable/LATEST_VERSION`), `cheat` — из
gzip-сжатого одиночного бинарника релиза (роль `shell`), `osquery` — из
`.deb`-пакета GitHub-релиза (роль `secrets`), `tldr`, `kubebox`, `cdk8s` и
`kosko` — через `npm install -g` (роли `shell` и `kubernetes`), `tshark` —
пакетом Ubuntu как зависимость `termshark` (роль `network`), `tracee` — из
тарболла GitHub-релиза вместе с сигнатурами безопасности в `/opt/tracee` +
симлинк в `/usr/local/bin` (роль `ebpf`), `lima` — распаковкой релизного
архива целиком в `/opt/lima` (bin, share/lima с шаблонами, libexec) и
симлинками `limactl`/`lima` в `/usr/local/bin`; QEMU для VM — пакетами apt
(роль `runtimes`),
глобальные skills для opencode — через `npx skills add` (роль `skills`, см.
раздел «Глобальные skills для opencode»).

Скачивание версий идёт без GitHub API (лимит 60 запросов в час): тег последнего
релиза берётся из HTTP-редиректа `releases/latest` — для GitHub это
`github.com/<repo>/releases/latest`, для GitLab (утилиты `glab`, `glci`) —
`gitlab.com/<repo>/-/releases/permalink/latest`.

Роль `binaries` понимает четыре формата архивов: `tar` (`.tar.gz`), `tarxz`
(`.tar.xz`, напр. `dnsglobe`), `tarbz2` (`.tar.bz2`, напр. `binup`) и `zip`;
для одиночных бинарников — `archive: none`.

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
  гоняет тесты в контейнерах Docker (Engine ставится ролью `docker`).
  Коллекции **community.docker** и **ansible.posix**, которыми драйвер
  поднимает контейнеры, плейбук не ставит: molecule доставит их сама в
  `~/.ansible/collections` при первом `molecule test` (`molecule dependency`).

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

Для повседневной работы роль `docker` дополнительно ставит **dry**
(`moncho/dry`) — терминальный менеджер контейнеров, образов, сетей и томов с
мониторингом и поддержкой нескольких Docker-хостов:

```bash
dry                         # интерактивный TUI текущего Docker-хоста
dry -H ssh://user@host      # подключиться к удалённому Docker
```

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
- **wt** (worktrunk, `max-sixty/worktrunk`) — параллельные ветки в отдельных
  каталогах через git worktree: переключение без пересборки рабочего дерева,
  список worktree, удаление отработанных.

```bash
pre-commit install          # включить хуки в текущем репозитории
reposcan ~/src              # показать «грязные» репозитории
glci run                    # прогнать пайплайн локально
wt switch --create feature  # новая ветка в отдельном worktree
wt list                     # все worktree и их состояние
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

## Схемы инфраструктуры: graphviz, inframap, rover

Роль `iac` ставит **graphviz** (`dot`) и два визуализатора Terraform. Прямого
аналога KubeDiagrams (манифесты → схема с иконками) для Terraform + Yandex
Cloud нет: KubeDiagrams читает только k8s, а Terraform-инструменты либо не
знают YC-провайдер, либо не знают про terragrunt (план работ — в
[`TODO-diagrams.md`](TODO-diagrams.md)). Поэтому на большом стеке схема
строится слоями.

**1. Карта юнитов** — `terragrunt dag graph` (v0.93+), рёбра берутся из
`dependency`-блоков. Ни `init`, ни `plan`, ни кредов не нужно.

```bash
terragrunt dag graph > dag.dot && dot -Tsvg dag.dot -o dag.svg
terragrunt dag graph --working-dir live/prod > prod.dot   # по окружению
```

**2. Один юнит** — `inframap` читает tfstate или HCL и выбрасывает служебные
узлы Terraform (провайдеры, переменные, data), поэтому граф читается лучше,
чем от `terraform graph`. Для YC работает общий режим — без группировки по
сетям и без иконок YC.

```bash
cd live/prod/ru-central1-d/k8s
terragrunt run -- show -json > /tmp/k8s-state.json
inframap generate /tmp/k8s-state.json | dot -Tsvg > k8s.svg
```

**3. То же интерактивно** — `rover` берёт JSON плана, показывает ресурсы,
связи и граф в браузере; `-standalone` собирает статический HTML без сервера.
Для terragrunt-юнита план снимается через `terragrunt`, потому что `rover`
запускает `terraform` и в каталоге с одним `terragrunt.hcl` ничего не найдёт.

```bash
terragrunt run -- plan -out=/tmp/k8s.tfplan
terragrunt run -- show -json /tmp/k8s.tfplan > /tmp/k8s-plan.json
rover -planJSONPath /tmp/k8s-plan.json -standalone   # ../rover/index.html
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

## eBPF-наблюдаемость: pwru, bpftop, oryx, pktstat-bpf, tracee

Роль `ebpf` ставит пять локальных eBPF-утилит для отладки ядра, сети и
runtime-безопасности:

- **pwru** (`cilium/pwru`) — eBPF-отладчик сетевого стека Linux: показывает путь
  пакета через ядро и «где он потерялся» (drop/reroute, таблицы маршрутизации,
  iptables, XDP).
- **bpftop** (`jfernandez/bpftop`) — real-time обзор запущенных eBPF-программ:
  сколько CPU они занимают и сколько событий обрабатывают.
- **oryx** (`pythops/oryx`) — TUI-сниффер сетевого трафика на eBPF: tcpdump в
  интерактивном интерфейсе, фильтры по протоколам и направлению.
- **pktstat-bpf** (`dkorunic/pktstat-bpf`) — монитор трафика на eBPF (TC/XDP/
  kprobes): сводка по потокам, процессам и скоростям в терминале.
- **tracee** (`aquasecurity/tracee`) — runtime-security и форензика Linux: сотни
  событий (syscalls, файлы, сеть, контейнеры) и встроенные сигнатуры угроз.

```bash
sudo pwru                                  # где потерялся пакет (можно pcap-filter)
sudo bpftop                                # обзор eBPF-программ в реальном времени
sudo oryx -i eth0                          # TUI-сниффер трафика
sudo pktstat-bpf --tui                     # сводка трафика по процессам
sudo tracee                                # live-события системы
tracee version                             # версия
```

Все они работают с eBPF в ядре и запускаются через `sudo` (нужны
CAP_BPF/CAP_PERFMON/CAP_SYS_ADMIN), поэтому `setcap` на бинарники не навешиваем.
Для работы нужны права на загрузку BPF-программ (на десктопе Ubuntu это доступно
по умолчанию; в системах с `kernel.unprivileged_bpf_disabled=1` потребуется root).

`tracee` ставится не через роль `binaries`: кроме самого бинарника релиз
содержит Go-плагин с сигнатурами безопасности (`dist/signatures/builtin.so`).
Бинарник кладётся в `/opt/tracee/` вместе с каталогом `signatures/`, а в
`/usr/local/bin/tracee` — симлинк (tracee разрешает симлинк через
`os.Executable()` и находит сигнатуры в `<каталог бинарника>/signatures`).

---

## Шпаргалки команд: navi, cheat, tldr

Три взаимодополняющих инструмента для «как запускается команда» ставятся ролью
`shell` (navi — уже была, cheat и tldr добавлены):

- **tldr** (`tldr-pages/tldr`) — каноническая community-база кратких примеров
  для сотен стандартных CLI-утилит. Клиент ставится через `npm install -g`.
- **cheat** (`cheat/cheat`) — собственные шпаргалки: создаёте и редактируете
  `cheatsheets/` и `cheatpaths`, версионируете в git.
- **navi** (`denisidoro/navi`) — интерактивный выбор шпаргалок поверх fzf
  (свои + community-коллекции).

```bash
tldr tar                                   # краткие примеры по утилите
cheat tar                                  # свои шпаргалки по утилите
navi                                       # интерактивный поиск шпаргалки
```

---

## JSON и табличные данные: jnv, octosql

Две утилиты роли `shell` для работы с данными без поднятия БД:

- **jnv** (`ynqa/jnv`) — интерактивный фильтр JSON: набираете jq-выражение и
  сразу видите результат (jq уже ставится ролью `base`).
- **octosql** (`cube2222/octosql`) — SQL-запросы прямо к JSON/CSV/Parquet:
  `SELECT`, `WHERE`, `JOIN` и агрегаты по файлам.

```bash
curl -s https://api.github.com/repos/ynqa/jnv | jnv '.stargazers_count'
octosql 'select name, stars from "repos.json" where stars > 1000'
```

---

## Проверка алерт-правил: vmalert-tool

**vmalert-tool** (`VictoriaMetrics/VictoriaMetrics`, роль `monitoring`) —
офлайн-утилита для валидации и unit-тестов alerting/recording-правил
Prometheus-совместимых систем: проверяет конфиги и гоняет тесты без поднятия
сервиса, без сервера и без Web UI. Единственный нужный бинарник из релизов
VictoriaMetrics на ноутбуке (кладётся из тарболла `vmutils-...` как
`vmalert-tool`).

```bash
vmalert-tool unittest -files rules.yaml -tests tests.yaml   # прогнать unit-тесты правил
```

Остальные бинарники VictoriaMetrics на ноуте не ставятся — им нужна локальная
TSDB/приёмка метрик: `victoria-metrics` (сам сервер + Web UI), `vmctl`
(миграция только **в** VictoriaMetrics), `vmagent` (сбор/пересылка метрик),
`vmalert` (вычисление алертов в реальном времени), `vmauth` (прокси-аутентификация),
`vmbackup`/`vmrestore` (бэкап/восстановление данных VM).

Дополнительно роль `git` ставит **reviewdog** (`reviewdog/reviewdog`) — гоняет
любые линтеры и шлёт их замечания как inline-комментарии в PR/MR
(`reviewdog -f="eslint"`), а роль `shell` — **grex** (`pemistahl/grex`),
генерирующую regex прямо из примеров строк (`grex -f`) — тоже из числа топовых
неустановленных кандидатов.

---

## osquery — SQL-подобная инструментация ОС

Роль `secrets` ставит **osquery** (`osquery/osquery`) официальным `.deb` из
GitHub-релиза. Бинарники `/opt/osquery/bin/osqueryd` (демон) и `osqueryi`
(интерактивная консоль) доступны через симлинки в `/usr/bin`.

```bash
osqueryi "SELECT hostname, cpu_brand, physical_memory FROM system_info;"
osqueryi "SELECT pid, name, path FROM processes WHERE name LIKE '%ssh';"
```

Пакет кладёт и systemd-юнит `osqueryd.service`, но плейбук его не запускает:
без файла конфигурации `/etc/osquery/osquery.conf` демон всё равно не
поднимется, поэтому автозагрузку не трогаем и дефолты не ломаем (запуск — через
`osqueryi` или настройка демона вручную).

---

## gotty — терминал как веб-приложение

Роль `network` ставит **gotty** (`yudai/gotty`) — делится командами в браузере:

```bash
gotty -w htop                              # открыть htop в браузере (читает ввод)
gotty -p 8080 -w top                       # на конкретном порту
```

Служит только на loopback по умолчанию; в отличие от `sshx` (внешний
совместный терминал), gotty разворачивается локально и сам не ходит в сеть —
сервис не поднимаем.

---

## TLS-сертификаты: gossl и crip

Две утилиты роли `network` для работы с X.509 без ручного `openssl s_client`:

- **gossl** (`vvrnv/gossl`) — проверка сертификата хоста: цепочка, срок
  действия, поддерживаемые протоколы; печатает вердикт по каждому пункту.
- **crip** (certificate-ripper, `Hakky54/certificate-ripper`) — вытаскивает
  цепочку сертификатов с удалённого хоста и умеет сразу положить её в PEM/trust
  store. Нативная сборка (GraalVM), JVM не требуется.

```bash
gossl verify --domain example.com
crip print --url https://example.com
crip trust --url https://example.com        # добавить в системный trust store
```

---

## YAML и JSON-логи: yamlfixer и jlv

- **yamlfixer** (`opt-nc/yamlfixer`, роль `shell`, через pipx) — берёт вывод
  `yamllint` и исправляет замечания (отступы, кавычки, пробелы), дополняя
  `yamllint` и `yamlfmt`: `switch`-стиль линтера остаётся, а правки делаются
  автоматически.
- **jlv** (json-log-viewer, `hedhyw/json-log-viewer`, роль `shell`) —
  интерактивный просмотр JSON-логов: строки раскладываются в таблицу с
  фильтрами по полям и переходом к ошибкам. Дополняет `hl`
  (универсальный пейджер логов): `jlv` заточен под навигацию по JSON-потоку.

```bash
yamllint -f parsable . | yamlfixer
jlv app.log                                 # или `kubectl logs ... | jlv`
```

---

## kubebox — консоль Kubernetes по SSH

**kubebox** (`astefanutti/kubebox`, роль `kubernetes`, ставится через npm) —
терминальный dashboard: поды, логи, `exec`, YAML, события; умеет поднимать
веб-режим в браузере. В отличие от `headlamp` (desktop-приложение) работает
прямо в ssh-сессии и не требует GUI.

```bash
kubebox                                     # TUI по текущему контексту
kubebox --server 127.0.0.1:8080             # веб-режим на localhost
```

---

## coroot — локальный observability/APM

**coroot** (`coroot/coroot`, роль `monitoring`, одиночный бинарник) — сервер
и UI: карта сервисов, инспекции, трейсы, логи и подсказки root-cause.
Важно: бинарник `coroot` — только сервер. Данные собирает
`coroot-node-agent` (eBPF, privileged), а хранят ClickHouse и Prometheus с
включённым remote-write receiver. Плейбук ставит только сервер, поэтому без
этих компонентов UI будет пустым — это не «всё в одном».

Минимальный локальный запуск (ClickHouse, Prometheus и агент ставятся
вручную, см. [`TODO-server-apps.md`](TODO-server-apps.md)):

```bash
# 1. Prometheus с приёмом remote write: ARGS="--enable-feature=remote-write-receiver"
# 2. Сервер: SQLite для конфигурации, метрики — в Prometheus, логи — в ClickHouse
coroot --bootstrap-prometheus-url=http://127.0.0.1:9090 \
       --bootstrap-clickhouse-address=127.0.0.1:9000

# 3. Агент на хосте (eBPF, нужен privileged и host PID)
docker run -d --name coroot-node-agent --network host --privileged --pid host \
  -v /sys/kernel/tracing:/sys/kernel/tracing:rw \
  -v /sys/kernel/debug:/sys/kernel/debug:rw \
  -v /sys/fs/cgroup:/host/sys/fs/cgroup:ro \
  ghcr.io/coroot/coroot-node-agent \
  --cgroupfs-root=/host/sys/fs/cgroup \
  --collector-endpoint=http://127.0.0.1:8080
```

UI — <http://localhost:8080>. Для Kubernetes coroot ставится в кластер
Helm-чартом или оператором (не на ноутбук), а смотреть в него удобнее через
`kubectl port-forward`. Требуется ядро Linux ≥ 4.16; Docker-in-Docker
(например, minikube) не поддерживается из-за ограничений eBPF.

---

## pitwall — терминал для coding-агентов

pitwall в плейбук пока не ставится: решение не принято. Блокер снят — issue о
вводе русских букв закрыт 2026-10-19, фикс вышел в `v0.1.0-beta.12`
(<https://github.com/quanticstudios/pitwall/issues/127>). Нужен ли он при
наличии вкладок opencode 2 и `herdr`, разбирается в разделе 6
[`TODO-opencode-orchestration.md`](TODO-opencode-orchestration.md).

Полный список утилит, которые плейбук сознательно не ставит (с причинами и
ссылками на issue) — в [`not-install.md`](not-install.md). Кандидаты, которые
ждут готового бинарника на GitHub, — в
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md), а те, чей бинарник
не запускается на Ubuntu 26.04, — в
[`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md).
План генератора архитектурных схем из Terraform/Terragrunt (аналога
KubeDiagrams для Yandex Cloud), который пока не реализован, — в
[`TODO-diagrams.md`](TODO-diagrams.md).
Утилиты, которые работают агентом на нодах Kubernetes (DaemonSet/static Pod и
не ставятся на ноутбук), собраны в
[`TODO-node-tools.md`](TODO-node-tools.md). Серверные приложения, сервисы и
Helm-чарты (grafana, prometheus, gitea, Jenkins и т.п.), которые плейбук не
ставит, — в [`TODO-server-apps.md`](TODO-server-apps.md). Из Ansible Galaxy
плейбук не ставит
ничего — ни ролей, ни коллекций, ни готовых playbook'ов (почему и как поставить
в конкретном проекте) — в
[`TODO-ansible-galaxy-not-install.md`](TODO-ansible-galaxy-not-install.md).

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

## Глобальные правила для opencode (`AGENTS.md`)

Роль `opencode` кладёт `~/.config/opencode/AGENTS.md` — инструкции, которые
opencode применяет во **всех** проектах (правила безопасности, работа с
Kubernetes и git, доступные зоны и т.п.). Проектный `AGENTS.md` эти правила
дополняет, а не отменяет.

Источник — `roles/opencode/files/AGENTS.md`: правки вносятся в репозиторий,
а плейбук приводит файл на машине к этому состоянию (локальные изменения
перезаписываются). Применить только правила:
`ansible-playbook -K playbook.yml --tags opencode`.

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

## Аудит SSH: ssh-audit

**ssh-audit** (`jtesta/ssh-audit`, роль `network`, через pipx) проверяет
конфигурацию SSH-сервера или клиента: версии протокола, key exchange, шифры,
MAC, известные уязвимости и рекомендации по усилению.

```bash
ssh-audit example.com           # аудит SSH-сервера
ssh-audit -c                    # аудит локального клиента (~/.ssh/config)
ssh-audit -j example.com        # JSON-вывод для автоматизации
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
- `PIPX_HOME` / `PIPX_BIN_DIR`;
- `mcfly init bash` (умная история по Ctrl-R, см. раздел «mcfly — история команд»).

Файл пишется из шаблона `roles/shell/templates/devops-shell.sh.j2` — правки
в самом файле перезапишутся при следующем запуске. Чтобы убрать окружение:
удалите файл и блок между маркерами в `~/.bashrc`.

### Конфиг starship

Тема prompt кладётся из `roles/shell/files/starship.toml` в `~/.config/starship.toml`
(starship читает этот путь по умолчанию). Файл копируется как есть — правки
в `~/.config/starship.toml` перезапишутся при следующем запуске. Меняйте
`roles/shell/files/starship.toml` в репозитории; путь назначения задаётся
переменной `starship_config_file` в `group_vars/all.yml`.

### mcfly — история команд

**mcfly** (роль `shell`, бинарник в `/usr/local/bin`) — умная история команд:
перехватывает Ctrl-R и приоритизирует результаты по контексту (текущий каталог,
недавние команды, частота, код выхода), хранит историю в SQLite
(`~/.local/share/mcfly/history.db`). Хук подключается строкой
`eval "$(mcfly init bash)"` в `~/.config/devops-shell.sh`.

Проверка: `mcfly --version`; история — нажать Ctrl-R после нескольких
выполненных команд (или `mcfly search`).

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
