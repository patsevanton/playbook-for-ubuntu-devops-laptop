# TODO-awesome-github: awesome-утилиты для Ubuntu DevOps-ноутбука

Отобранный вручную список локальных утилит, которые имеет смысл поставить на
Ubuntu 26.04 / x86_64 рядом с плейбуком. Источник — все репозитории со ⭐
аккаунта (2057 шт.); кандидаты и полный обход лежат в
`/home/magnit/Public/starred-tools/data/` (`enriched.json`, `install_hints.tsv`,
`install_report.md`).

Это канонический список кандидатов (дедупликация 2026-10-18): дубли из
`TODO.md` и `TODO-trend-github.md` перенесены сюда и удалены оттуда, вместе с
ними перенесены рекомендации «что выбрать из дублей» (см. таблицу ниже).
Уникальные тренды 2026 остались в [`TODO-trend-github.md`](TODO-trend-github.md).
Утилиты, которые уже ставит плейбук, перечислены в [`TODO.md`](TODO.md) — их
дублировать не нужно
(см. `README.md`).

Как читать:

- `- [ ]` — кандидат, ещё не рассмотрен.
- `- [x]` — рассмотрен и добавлен в плейбук (или стал не нужен) — см. заметку
  у пункта; что уже ставит плейбук — в [`TODO.md`](TODO.md).
- В скобках — способ установки для Ubuntu 26.04 / x86_64:
  `deb` — готовый `.deb` в GitHub-релизе, `AppImage`, `apt`, `snap`,
  `release` — готовый бинарник/архив из релиза,
  `go` / `cargo` / `pipx` / `npm` — сборка или установка из исходников,
  `script` — официальный installer.sh, `make` — сборка из исходников.
- Ссылка ведёт на репозиторий проекта.

---

## Дубли по функционалу — что выбрать

У группы с дублями берём одну утилиту (⭐ в таблице и в списке ниже), остальные
держим как запасные. Отдельно: **дубли уже установленного плейбуком** — не брать
(полный список уже устанавливаемого — в [`TODO.md`](TODO.md)): `ktail`≈`stern`,
`kube-switch`≈`kubectx`, `dyff`≈`diffyml` — в списке помечены ⛔.

| Группа | Рекомендую ⭐ | Альтернативы | Почему |
| --- | --- | --- | --- |
| История shell | **mcfly** | atuin, hstr | atuin конфликтует с VTE (баг GNOME/vte#2951); mcfly/hstr локальные, но проще |
| Шпаргалки команд | **navi** | cheat, tldr | объединяет свои и community-шпаргалки и работает поверх уже установленного `fzf` |
| Терминальные мультиплексоры | **zellij** | 3mux | развитее и активнее; 3mux интересен только ради i3-навигации |
| Анализ занятого места | **gdu** | dusage | быстрее и удобнее в TUI |
| LLM-сообщения коммитов | **opencommit** | lumen, gptcommit, CodeGPT | проще всего ставится (npm) и активно развивается; остальные — варианты под предпочтения |
| DNS-клиенты | **q** | dog | поддерживает DoH/DoT/DoQ, ставится одним `.deb` |
| eBPF-снифферы трафика | **kyanos** | oryx, pktstat-bpf | даёт и латентность, и payload, а не просто байты |
| Просмотр X.509-сертификатов | **showcert** | certificate-ripper, gossl | человекочитаемый разбор, ставится через `pipx`, без Java |
| Терминал в браузере | **sshx** | gotty | шаринг одной командой, без разворачивания сервиса |
| Мониторинг полосы трафика | **bandwhich** | sniffnet | разбивка по процессам и соединениям прямо в терминале |
| K8s-дашборды | **headlamp** | kubebox, kubewall | самый функциональный, есть desktop-режим |
| Обзор ресурсов кластера | **kube-capacity** | ktop | показывает requests/limits против фактического потребления |
| Слои docker-образов | **dive** | docker-image-layer-explorer | интерактивный, с оценкой эффективности образа |
| Интерактивный просмотр JSON | **fx** | jnv | работа из коробки, не требует отдельного `jq` |
| Просмотр JSON-логов | **hl** | json-log-viewer, logdy-core | быстрый TUI, умеет не только JSON |
| Установщики бинарников | **eget** | binup, stew, dra | проще всех, один бинарник и понятный синтаксис |

---

## Ansible и управление конфигурацией

- [x] **molecule** — фреймворк тестирования Ansible-ролей (прогон ролей в
      контейнерах/VM). *(`pipx`; ⭐ 4.2k)* — [ansible/molecule](https://github.com/ansible/molecule)
      — ✅ добавлен в плейбук (роль `base`: pipx + драйвер docker).
- [ ] **cloud.terraform** — Ansible-коллекция: запуск Terraform/OpenTofu из
      плейбуков (plan/apply/state как задачи). *(`ansible-galaxy collection`)* —
      [ansible-collections/cloud.terraform](https://github.com/ansible-collections/cloud.terraform)
- [ ] **ansible-role-docker** — проверенная роль установки Docker (geerlingguy).
      *(`ansible-galaxy role`)* — [geerlingguy/ansible-role-docker](https://github.com/geerlingguy/ansible-role-docker)
- [ ] **ansible-role-wireguard** — роль WireGuard-VPN для Ubuntu/Debian/RHEL.
      *(`ansible-galaxy role`)* — [githubixx/ansible-role-wireguard](https://github.com/githubixx/ansible-role-wireguard)
- [ ] **ansible-collection-smallstep** — коллекция для Smallstep CLI и `step-ca`
      (PKI/сертификаты). *(`ansible-galaxy collection`)* —
      [maxhoesel-ansible/ansible-collection-smallstep](https://github.com/maxhoesel-ansible/ansible-collection-smallstep)
- [ ] **ansible-haproxy** — роль установки актуального HAProxy. *(`ansible-galaxy role`)* —
      [Oefenweb/ansible-haproxy](https://github.com/Oefenweb/ansible-haproxy)
- [ ] **java_role** / **nexus-role** — роли idealista: Java (OpenJDK/Corretto) и
      Sonatype Nexus. *(`ansible-galaxy role`)* —
      [idealista/java_role](https://github.com/idealista/java_role),
      [idealista/nexus-role](https://github.com/idealista/nexus-role)
- [x] **ansible-role-pgcenter** — роль установки `pgCenter` (мониторинг Postgres).
      *(`ansible-galaxy role`)* — [vitabaks/ansible-role-pgcenter](https://github.com/vitabaks/ansible-role-pgcenter)
      — ✅ не нужна: сам `pgcenter` уже ставится плейбуком (роль `postgres`).
- [ ] **knee** — интерактивный CLI поверх Ansible: полуавтоматический setup
      инфраструктуры. *(`pipx`)* — [shurutech/knee](https://github.com/shurutech/knee)
- [ ] **k3s-ansible** — готовый плейбук HA-кластера k3s (kube-vip, MetalLB).
      *(`git clone`)* — [timothystewart6/k3s-ansible](https://github.com/timothystewart6/k3s-ansible)
- [ ] **scantron** — распределённый фреймворк nmap/masscan с расписанием и API
      (Ansible-разворачивание). *(`pipx` / `make`)* — [rackerlabs/scantron](https://github.com/rackerlabs/scantron)

## Terraform / Terragrunt / IaC

- [ ] **tfvar** — генерирует шаблон `variables.tf` из Terraform-кода.
      *(`deb` / `go`)* — [shihanng/tfvar](https://github.com/shihanng/tfvar)
- [ ] **tpm** — менеджер провайдеров Terraform/OpenTofu. *(`go`)* —
      [Madh93/tpm](https://github.com/Madh93/tpm)
- [ ] **terraform-tui** — TUI для работы с Terraform-стейтом. *(`pipx`)* —
      [idoavrah/terraform-tui](https://github.com/idoavrah/terraform-tui)
- [ ] **prettyplan** — форматирует большие `terraform plan` для ревью.
      *(`npm` / статический билд)* — [chrislewisdev/prettyplan](https://github.com/chrislewisdev/prettyplan)
- [ ] **config-lint** — валидация конфигов (HCL/Terraform и др.). *(`go`)* —
      [stelligent/config-lint](https://github.com/stelligent/config-lint)
- [ ] **gitlab-terraform-manager** — управление GitLab Terraform backend.
      *(`go`)* — [CRASH-Tech/gitlab-terraform-manager](https://github.com/CRASH-Tech/gitlab-terraform-manager)
- [ ] **terrapod** — self-hosted замена Terraform Enterprise (если понадобится
      свой backend/runner). *(`make`)* — [mattrobinsonsre/terrapod](https://github.com/mattrobinsonsre/terrapod)
- [ ] **tf-mirror** — self-hosted зеркало Terraform Provider Registry и бинарников
      HashiCorp (air-gapped/CI). *(`go`)* — [kashtan404/tf-mirror](https://github.com/kashtan404/tf-mirror)
- [ ] **gruntwork-io/pre-commit** — набор pre-commit-хуков для Terraform.
      *(`pre-commit`)* — [gruntwork-io/pre-commit](https://github.com/gruntwork-io/pre-commit)

> `terraform` и `terragrunt` плейбук не ставит напрямую — они управляются через
> `tfenv`/`tgenv` (см. `TODO.md` и `README.md`).

## Kubernetes — повседневные CLI-утилиты

- [x] **kubecolor** — цветной, читаемый вывод `kubectl`. *(`deb`)* —
      [kubecolor/kubecolor](https://github.com/kubecolor/kubecolor)
      — ✅ добавлен в плейбук (роль `kubernetes`; вызывается отдельной командой,
      алиасы на `kubectl` не навешиваем).
- [x] ⭐ **kube-capacity** — requests/limits и реальная загрузка кластера.
      *(`go` / `release`)* — [robscott/kube-capacity](https://github.com/robscott/kube-capacity)
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [x] **ktop** — `top`-подобный монитор метрик нод и подов. *(`go`)* —
      [vladimirvivien/ktop](https://github.com/vladimirvivien/ktop)
      — ✅ добавлен в плейбук (роль `kubernetes`; плагин `kubectl-ktop` не ставим).
- [x] **kubefetch** — neofetch для Kubernetes-кластера. *(`go`)* —
      [jkulzer/kubefetch](https://github.com/jkulzer/kubefetch)
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [ ] **ktail** — tail логов подов с фильтрами (родственник stern). *(`go`)* —
      [atombender/ktail](https://github.com/atombender/ktail) — ⛔ дубль уже
      установленного плейбуком **stern**, не брать.
- [ ] **kube-switch** — маленький bash-переключатель кластеров. *(`git clone`)* —
      [josefkorbel/kube-switch](https://github.com/josefkorbel/kube-switch) — ⛔ дубль
      уже установленного плейбуком **kubectx**, не брать.
- [x] **kubectl-view-secret** — плагин: декодирование секретов. *(`go` / `krew`)* —
      [elsesiy/kubectl-view-secret](https://github.com/elsesiy/kubectl-view-secret)
      — ✅ добавлен в плейбук (роль `kubernetes`, бинарником как `view-secret`).
- [x] **kubectl-df-pv** — плагин: `df` по PersistentVolume. *(`go` / `krew`)* —
      [yashbhutwala/kubectl-df-pv](https://github.com/yashbhutwala/kubectl-df-pv)
      — ✅ добавлен в плейбук (роль `kubernetes`, бинарником как `df-pv`).
- [x] **kubectl-browse-pvc** — плагин: просмотр PVC из CLI. *(`go` / `krew`)* —
      [clbx/kubectl-browse-pvc](https://github.com/clbx/kubectl-browse-pvc)
      — ✅ добавлен в плейбук (роль `kubernetes`, бинарником как `browse-pvc`).
- [x] **pv-migrate** — миграция и backup/restore PersistentVolume. *(`go`)* —
      [utkuozdemir/pv-migrate](https://github.com/utkuozdemir/pv-migrate)
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [x] **pluto** — находит устаревшие/удалённые `apiVersion` в манифестах.
      *(`go` / `release`)* — [FairwindsOps/pluto](https://github.com/FairwindsOps/pluto)
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [ ] **dyff** — структурный diff YAML (иногда JSON). *(`go`)* —
      [homeport/dyff](https://github.com/homeport/dyff) — ⛔ дубль уже
      установленного плейбуком **diffyml**, не брать.
- [ ] **kubevpn** — cloud-native dev-окружение и VPN в кластер. *(`go`)* —
      [kubenetworks/kubevpn](https://github.com/kubenetworks/kubevpn)
- [x] **kftray** — менеджер `kubectl port-forward` и reverse-туннелей (GUI+TUI).
      *(`AppImage` / `tar.gz`; ⭐ 1.6k)* — [hcavarsan/kftray](https://github.com/hcavarsan/kftray)
      — ✅ добавлен в плейбук (роль `kubernetes`, TUI-бинарник `kftui`).
- [ ] **txeh** — управление `/etc/hosts` из CLI. *(`deb`)* —
      [txn2/txeh](https://github.com/txn2/txeh)
- [ ] **netobserv-cli** — сетевая наблюдаемость Kubernetes (eBPF, без агента).
      *(`go`)* — [netobserv/netobserv-cli](https://github.com/netobserv/netobserv-cli)
- [x] **marvin** — сканер кластера CEL-выражениями: проблемы, мисконфиги,
      уязвимости. *(`go` / `release`)* — [undistro/marvin](https://github.com/undistro/marvin)
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [ ] **kondense** — автоматический подбор requests/limits для контейнеров.
      *(`go`)* — [unagex/kondense](https://github.com/unagex/kondense)
- [ ] **netfetch** — скан кластера на незащищённые NetworkPolicy ворклоады.
      *(`go`)* — [deggja/netfetch](https://github.com/deggja/netfetch)

> Плагины kubectl (`kubectl-view-secret`, `kubectl-df-pv`, `kubectl-browse-pvc`)
> ставит плейбук бинарниками (без префикса `kubectl-`): `view-secret`, `df-pv`,
> `browse-pvc` — см. роль `kubernetes`. Сам **krew** (менеджер плагинов kubectl)
> тоже ставится плейбуком на случай ручной установки плагинов потом.

## Kubernetes — UI, Helm и GitOps

- [x] ⭐ **headlamp** — полноценный web/desktop UI для Kubernetes. *(`deb` / `AppImage`)* —
      [kubernetes-sigs/headlamp](https://github.com/kubernetes-sigs/headlamp)
      — ✅ добавлен в плейбук (роль `kubernetes`, `.deb` из релиза, desktop UI).
- [ ] **kubewall** — dashboard одним бинарником на несколько кластеров.
      *(`deb`)* — [kubewall/kubewall](https://github.com/kubewall/kubewall)
- [ ] **radar** — Kubernetes UI со встроенным MCP-сервером для AI-агентов.
      *(`deb`)* — [skyhook-io/radar](https://github.com/skyhook-io/radar)
- [ ] **kubebox** — терминальная и веб-консоль для Kubernetes. *(`npm`)* —
      [astefanutti/kubebox](https://github.com/astefanutti/kubebox)
- [ ] **kubeview** — визуализация ресурсов кластера и связей между ними.
      *(`go` / `make`)* — [benc-uk/kubeview](https://github.com/benc-uk/kubeview)
- [x] **ytt** — шаблонизатор YAML, работающий со структурой, а не текстом.
      *(`release`)* — [carvel-dev/ytt](https://github.com/carvel-dev/ytt)
      — ✅ добавлен в плейбук (роль `kubernetes`, одиночный бинарник
      ytt-linux-amd64 из релиза).
- [ ] **kbld** — встраивает образы в манифесты (build/resolve/push).
      *(`go`)* — [carvel-dev/kbld](https://github.com/carvel-dev/kbld)
- [ ] **timoni** — пакетный менеджер Kubernetes на CUE. *(`go`)* —
      [stefanprodan/timoni](https://github.com/stefanprodan/timoni)
- [x] **chart-testing** — линтинг и тестирование Helm-чартов (`ct`). *(`tar.gz` linux_amd64 / `go`; ⭐ 1.6k)* —
      [helm/chart-testing](https://github.com/helm/chart-testing)
      — ✅ добавлен в плейбук (роль `kubernetes`, бинарник `ct`).
- [ ] **helm-chartsnap** — snapshot-тестирование Helm-чартов. *(`go`)* —
      [jlandowner/helm-chartsnap](https://github.com/jlandowner/helm-chartsnap)
- [ ] **charts-syncer** — синхронизация Helm-репозиториев. *(`go`)* —
      [bitnami/charts-syncer](https://github.com/bitnami/charts-syncer)
- [ ] **capacitor** — UI для FluxCD. *(`npm`)* —
      [gimlet-io/capacitor](https://github.com/gimlet-io/capacitor)
- [ ] **goldpinger** — проверка связности между нодами кластера. *(`go`)* —
      [bloomberg/goldpinger](https://github.com/bloomberg/goldpinger)

## Docker и контейнеры

- [x] ⭐ **dive** — анализ слоёв docker-образа, размер и мусор. *(`deb`)* —
      [wagoodman/dive](https://github.com/wagoodman/dive)
      — ✅ добавлен в плейбук (роль `docker`, бинарником из тарболла).
- [x] **dry** — терминальный менеджер Docker-контейнеров, образов, сетей.
      *(`go` / `release`)* — [moncho/dry](https://github.com/moncho/dry)
      — ✅ добавлен в плейбук (роль `docker`, бинарник из тарболла).
- [x] ⭐ **ctop** — `top` для метрик контейнеров. *(`go` / `release`; ⭐ 17.8k)* —
      [bcicen/ctop](https://github.com/bcicen/ctop)
      — ✅ добавлен в плейбук (роль `docker`, одиночный бинарник ctop-<ver>-linux-amd64).
- [x] **lazydocker** — TUI-менеджер Docker/Podman (от автора lazygit). *(`go`)* —
      [jesseduffield/lazydocker](https://github.com/jesseduffield/lazydocker)
      — ✅ добавлен в плейбук (роль `docker`, бинарником из тарболла).
- [ ] **docker-image-layer-explorer** — просмотр и распаковка слоёв образа.
      *(`pipx`)* — [Kartik1397/docker-image-layer-explorer](https://github.com/Kartik1397/docker-image-layer-explorer)
- [ ] **decompose** — reverse-engineering docker-окружений (сети, тома, образы).
      *(`deb`)* — [s0rg/decompose](https://github.com/s0rg/decompose)
- [ ] **copacetic** — патчинг уязвимостей прямо в контейнерных образах.
      *(`go`)* — [project-copacetic/copacetic](https://github.com/project-copacetic/copacetic)
- [x] **oras** — клиент OCI-реестров для произвольных артефактов. *(`go`)* —
      [oras-project/oras](https://github.com/oras-project/oras)
      — ✅ добавлен в плейбук (роль `docker`, бинарником из тарболла).
- [x] **s5cmd** — очень быстрый параллельный клиент S3. *(`deb`)* —
      [peak/s5cmd](https://github.com/peak/s5cmd)
      — ✅ добавлен в плейбук (роль `iac`, бинарником из тарболла).
- [x] **nora** — лёгкий мультиформатный artifact registry (Docker, npm, PyPI,
      Terraform, Ansible Galaxy и др.). *(`deb`)* —
      [getnora-io/nora](https://github.com/getnora-io/nora) — ставится ролью
      `nora` (см. `README.md`, раздел «NORA artifact registry»)
- [ ] **registry-cli** — манипуляции с docker-registry из командной строки.
      *(`pipx`)* — [andrey-pohilko/registry-cli](https://github.com/andrey-pohilko/registry-cli)
- [ ] **gitlab-registry-images-cleaner** — чистка старых образов в GitLab
      registry. *(`pipx`)* — [n0madic/gitlab-registry-images-cleaner](https://github.com/n0madic/gitlab-registry-images-cleaner)

## Логи, JSON, YAML, данные

- [x] **dasel** — запросы и правка JSON/YAML/TOML/XML/CSV/HCL одной утилитой.
      *(`go` / `release`)* — [TomWright/dasel](https://github.com/TomWright/dasel)
      — ✅ добавлен в плейбук (роль `shell`, одиночный бинарник dasel_linux_amd64).
- [ ] **jc** — конвертирует вывод обычных CLI-утилит в JSON. *(`deb` / `pipx`)* —
      [kellyjonbrazil/jc](https://github.com/kellyjonbrazil/jc)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла).
- [x] ⭐ **fx** — интерактивный просмотрщик и обработчик JSON. *(`npm`)* —
      [antonmedv/fx](https://github.com/antonmedv/fx)
      — ✅ добавлен в плейбук (роль `shell`, одиночный бинарник fx_linux_amd64).
- [x] **jnv** — интерактивный фильтр JSON (jq + fzf). *(`cargo`, нужен `jq`)* —
      [ynqa/jnv](https://github.com/ynqa/jnv)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из tar.xz; `jq` уже
      в `base`).
- [x] **octosql** — SQL-запросы к JSON/CSV/Parquet-файлам. *(`go`)* —
      [cube2222/octosql](https://github.com/cube2222/octosql)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла).
- [x] **gomplate** — шаблонизатор конфигов из разных источников (env, файлы, API).
      *(`go`)* — [hairyhenderson/gomplate](https://github.com/hairyhenderson/gomplate)
      — ✅ добавлен в плейбук (роль `shell`, одиночный бинарник).
- [ ] **gonzo** — TUI-анализатор логов с графиками по столбцам. *(`go`)* —
      [control-theory/gonzo](https://github.com/control-theory/gonzo)
- [x] ⭐ **hl** — быстрый просмотрщик и процессор логов (JSON/logfmt). *(`cargo`)* —
      [pamburus/hl](https://github.com/pamburus/hl)
      — ✅ добавлен в плейбук (роль `shell`, бинарник hl-linux-x86_64-gnu).
- [x] **lazyjournal** — TUI для логов journald, auditd, Docker, файлов.
      *(`release`)* — [Lifailon/lazyjournal](https://github.com/Lifailon/lazyjournal)
      — ✅ добавлен в плейбук (роль `shell`, одиночный бинарник
      lazyjournal-<ver>-linux-amd64 из релиза).
- [ ] **json-log-viewer** — интерактивный просмотр JSON-логов. *(`deb`)* —
      [hedhyw/json-log-viewer](https://github.com/hedhyw/json-log-viewer)
- [ ] **logdy-core** — realtime веб-просмотр логов (`tail -f` + браузерный UI).
      *(`go`)* — [logdyhq/logdy-core](https://github.com/logdyhq/logdy-core)
- [ ] **otel-tui** — терминальный просмотрщик OpenTelemetry-трейсов/логов/метрик.
      *(`go`)* — [ymtdzzz/otel-tui](https://github.com/ymtdzzz/otel-tui)
- [ ] **jl** — работа с структурированным JSON-логированием. *(`go`)* —
      [koenbollen/jl](https://github.com/koenbollen/jl)
- [x] **yamlfmt** — форматтер YAML. *(`go`)* —
      [google/yamlfmt](https://github.com/google/yamlfmt)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из тарболла).
- [ ] **yamlfixer** — автоматическое исправление замечаний `yamllint`. *(`pipx`)* —
      [opt-nc/yamlfixer](https://github.com/opt-nc/yamlfixer)
- [x] ⭐ **grex** — генерирует регулярные выражения из примеров строк.
      *(`cargo` / `pipx`)* — [pemistahl/grex](https://github.com/pemistahl/grex)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла).
- [ ] **sd** — интуитивная замена `sed` для поиска/замены. *(`cargo`)* —
      [chmln/sd](https://github.com/chmln/sd)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла).

## eBPF и системная наблюдаемость

- [x] **bpftop** — real-time обзор запущенных eBPF-программ и их CPU. *(`deb`)* —
      [jfernandez/bpftop](https://github.com/jfernandez/bpftop)
      — ✅ добавлен в плейбук (роль `ebpf`, одиночный бинарник bpftop-x86_64).
- [x] ⭐ **ecapture** — захват TLS/SSL plaintext через eBPF без MITM-прокси.
      *(`deb`; ⭐ 15.5k)* — [gojue/ecapture](https://github.com/gojue/ecapture)
      — ✅ добавлен в плейбук (роль `network`, бинарник ecapture-<ver>-linux-amd64).
- [ ] **linnix** — eBPF-наблюдаемость с AI-детекцией инцидентов. *(`deb`)* —
      [linnix-os/linnix](https://github.com/linnix-os/linnix)
- [x] **oryx** — TUI-сниффер трафика на eBPF. *(`cargo`, нужен BPF)* —
      [pythops/oryx](https://github.com/pythops/oryx)
      — ✅ добавлен в плейбук (роль `ebpf`, одиночный бинарник oryx-x86_64-musl).
- [x] ⭐ **kyanos** — анализ сетевых запросов через eBPF (латентность, payload).
      *(`go`)* — [hengyoush/kyanos](https://github.com/hengyoush/kyanos)
      — ✅ добавлен в плейбук (роль `network`, бинарник kyanos_*_linux_amd64).
- [x] **pktstat-bpf** — монитор Ethernet-трафика на eBPF (TC/XDP/kprobes).
      *(`deb`)* — [dkorunic/pktstat-bpf](https://github.com/dkorunic/pktstat-bpf)
      — ✅ добавлен в плейбук (роль `ebpf`, бинарник из тарболла pktstat-bpf_Linux_x86_64).
- [x] **pwru** — eBPF-отладчик сетевого стека Linux («где потерялся пакет»).
      *(`go`)* — [cilium/pwru](https://github.com/cilium/pwru)
      — ✅ добавлен в плейбук (роль `ebpf`, бинарник из тарболла pwru-linux-amd64).
- [x] **tracee** — runtime-security и форензика Linux на eBPF. *(`go`)* —
      [aquasecurity/tracee](https://github.com/aquasecurity/tracee)
      — ✅ добавлен в плейбук (роль `ebpf`, бинарник + сигнатуры в `/opt/tracee`).

## Сеть и сетевая диагностика

- [x] ⭐ **bandwhich** — TUI: текущая полоса трафика по процессам и
      соединениям. *(`deb` из релиза / `tar.gz` + setcap; ⭐ 12k)* —
      [imsnif/bandwhich](https://github.com/imsnif/bandwhich)
      — ✅ добавлен в плейбук (роль `network`, бинарник + setcap CAP_NET_RAW).
- [x] ⭐ **sniffnet** — кроссплатформенный GUI-мониторинг сетевого трафика.
      *(`deb`; ⭐ 41.6k)* — [GyulyVGC/sniffnet](https://github.com/GyulyVGC/sniffnet)
      — ✅ добавлен в плейбук (роль `network`, официальный .deb Sniffnet_LinuxDEB_amd64.deb).
- [x] **trippy** — TUI traceroute с историей и статистикой по хопам.
      *(`deb` / `snap`)* — [fujiapple852/trippy](https://github.com/fujiapple852/trippy)
      — ✅ добавлен в плейбук (роль `network`, бинарник trip под именем trippy).
- [x] ⭐ **q** — крошечный DNS-клиент: UDP/TCP/DoH/DoT/DoQ/ODoH. *(`deb`)* —
      [natesales/q](https://github.com/natesales/q)
      — ✅ добавлен в плейбук (роль `network`, бинарником из тарболла).
- [ ] **dog** — DNS-клиент с человекочитаемым цветным выводом. *(`cargo`)* —
      [ogham/dog](https://github.com/ogham/dog)
      — ⛔ не ставится: upstream мёртв с 2021 (последний релиз v0.1.0), готовый
      бинарник динамически связан с `libssl.so.1.1`, которого нет в Ubuntu 26.04.
      Покрывается уже добавленным `q`. Живой аналог — **doggo**
      (tar.gz, статический бинарник) — добавлен в плейбук (роль `network`).
- [x] **tcping** — `ping` поверх TCP-портов. *(`deb`)* —
      [pouriyajamshidi/tcping](https://github.com/pouriyajamshidi/tcping)
      — ✅ добавлен в плейбук (роль `network`, статическим бинарником из тарболла).
- [x] ⭐ **termshark** — TUI-интерфейс к tshark/Wireshark. *(`go`; ⭐ 10.0k)* —
      [gcla/termshark](https://github.com/gcla/termshark)
      — ✅ добавлен в плейбук (роль `network`, бинарник termshark + зависимость tshark).
- [x] ⭐ **websocat** — netcat/curl для WebSockets. *(`deb`; ⭐ 8.7k)* —
      [vi/websocat](https://github.com/vi/websocat)
      — ✅ добавлен в плейбук (роль `network`, бинарник websocat.x86_64-unknown-linux-musl).
- [x] **hurl** — запуск и тестирование HTTP-запросов из plain-text файлов.
      *(`deb`; ⭐ 19.3k)* — [Orange-OpenSource/hurl](https://github.com/Orange-OpenSource/hurl)
      — ✅ добавлен в плейбук (роль `network`, бинарником из тарболла).
- [x] **xh** — быстрый и дружелюбный HTTP-клиент (замена httpie). *(`deb`; ⭐ 8.1k)* —
      [ducaale/xh](https://github.com/ducaale/xh)
      — ✅ добавлен в плейбук (роль `network`, бинарником из тарболла).
- [x] **mkcert** — локально доверенные dev-сертификаты без настройки. *(`go`)* —
      [FiloSottile/mkcert](https://github.com/FiloSottile/mkcert) — ставится ролью
      `nora` (см. `README.md`, раздел «NORA artifact registry»)
- [x] **httptap** — разбор HTTP-запроса по слоям: DNS, connect, TLS, TTFB.
      *(`pipx`)* — [ozeranskii/httptap](https://github.com/ozeranskii/httptap)
      — ✅ добавлен в плейбук (роль `network`, через pipx из PyPI).
- [ ] **asn** — справка по IP/домену: ASN, RPKI, BGP, WHOIS, трейс. *(`git clone`)* —
      [nitefood/asn](https://github.com/nitefood/asn)
- [x] ⭐ **showcert** — «OpenSSL для людей»: разбор X.509-сертификатов. *(`pipx`)* —
      [yaroslaff/showcert](https://github.com/yaroslaff/showcert)
      — ✅ добавлен в плейбук (роль `network`, через pipx из PyPI).
- [ ] **certificate-ripper** — извлекает цепочки сертификатов с удалённых
      серверов. *(`jar` + `apt install openjdk`)* —
      [Hakky54/certificate-ripper](https://github.com/Hakky54/certificate-ripper)
- [ ] **gossl** — CLI-проверка SSL-сертификатов. *(`go`)* —
      [vvrnv/gossl](https://github.com/vvrnv/gossl)
- [x] **ssh-audit** — аудит конфигурации SSH-серверов и клиентов. *(`pipx`)* —
      [jtesta/ssh-audit](https://github.com/jtesta/ssh-audit)
      — ✅ добавлен в плейбук (роль `network`, через pipx из PyPI).
- [ ] **lexicon** — единый способ управления DNS-записями у разных провайдеров.
      *(`pipx`)* — [AnalogJ/lexicon](https://github.com/AnalogJ/lexicon)
- [ ] **netutils-linux** — набор утилит для тюнинга и диагностики сетевого стека
      Linux. *(`pipx` / `make`)* — [strizhechenko/netutils-linux](https://github.com/strizhechenko/netutils-linux)
- [ ] **reachable** — быстрая проверка доступности домена. *(`go`)* —
      [italolelis/reachable](https://github.com/italolelis/reachable)

## Удалённый доступ и шаринг терминала

- [x] ⭐ **sshx** — совместный терминал в браузере (мгновенный шаринг сессии).
      *(`script` / `cargo`)* — [ekzhang/sshx](https://github.com/ekzhang/sshx)
      — ✅ добавлен в плейбук (роль `network`, тарболл с S3).
- [ ] **EternalTerminal** — переподключаемый удалённый shell (сессия переживает
      обрыв). *(`apt` из репо ET; в latest .deb только arm64)* —
      [MisterTea/EternalTerminal](https://github.com/MisterTea/EternalTerminal)
- [x] **gotty** — делится терминалом как веб-приложением. *(`go` / `release`)* —
      [yudai/gotty](https://github.com/yudai/gotty)
      — ✅ добавлен в плейбук (роль `network`, бинарником из тарболла).
- [ ] **assh** — «умный» ssh-клиент: прокси, jump-хосты, генерация конфигов.
      *(`deb` / `go`)* — [moul/assh](https://github.com/moul/assh)

## Секреты и безопасность

- [x] **sops** — шифрование секретов прямо в Git (KMS/age/PGP). Дополняет
      `vault`. *(`deb`)* — [getsops/sops](https://github.com/getsops/sops)
      — ✅ добавлен в плейбук (роль `secrets`, бинарником из релиза).
- [x] **grype** — сканер уязвимостей контейнерных образов и ФС. *(`deb`)* —
      [anchore/grype](https://github.com/anchore/grype)
      — ✅ добавлен в плейбук (роль `secrets`, бинарником из тарболла).
- [ ] **flawz** — TUI-браузер CVE/уязвимостей. *(`cargo`)* —
      [orhun/flawz](https://github.com/orhun/flawz)
- [x] **mfa** — терминальный менеджер 2FA/TOTP. *(`release`)* —
      [LexSerest/mfa](https://github.com/LexSerest/mfa) — ✅ добавлен в плейбук
      (роль `secrets`, бинарник mfa из тарболла релиза).
- [ ] **teller** — облачно-независимый менеджер секретов для разработчика.
      *(`cargo`)* — [tellerops/teller](https://github.com/tellerops/teller)
- [ ] **bank-vaults** — швейцарский нож для Vault: init, unseal, конфигурация.
      *(`go`)* — [bank-vaults/bank-vaults](https://github.com/bank-vaults/bank-vaults)
- [ ] **medusa** — импорт/экспорт секретов HashiCorp Vault. *(`go`)* —
      [jonasvinther/medusa](https://github.com/jonasvinther/medusa)
- [ ] **entropy** — ищет в кодовой базе строки с высокой энтропией (секреты).
      *(`go`)* — [EwenQuim/entropy](https://github.com/EwenQuim/entropy)
- [ ] **shellclear** — находит секреты и опасные команды в истории shell.
      *(`cargo`)* — [rusty-ferris-club/shellclear](https://github.com/rusty-ferris-club/shellclear)
- [ ] **horusec** — SAST-сканер уязвимостей одной командой. *(`deb`)* —
      [ZupIT/horusec](https://github.com/ZupIT/horusec)
- [ ] **dependency-check** — OWASP SCA: уязвимые зависимости проекта. *(`java`)* —
      [dependency-check/DependencyCheck](https://github.com/dependency-check/DependencyCheck)
- [ ] **dagda** — статический анализ образов/контейнеров на malware и уязвимости.
      *(`pipx`)* — [eliasgranderubio/dagda](https://github.com/eliasgranderubio/dagda)
- [ ] **ochrona-cli** — детект уязвимостей Python-зависимостей + безопасные
      установки. *(`pipx`)* — [ochronasec/ochrona-cli](https://github.com/ochronasec/ochrona-cli)
- [ ] **vulncov** — корреляция Semgrep-сканов с покрытием тестами. *(`pipx`)* —
      [mllamazares/vulncov](https://github.com/mllamazares/vulncov)
- [x] **osquery** — SQL-подобная инструментация и мониторинг ОС. *(`deb`)* —
      [osquery/osquery](https://github.com/osquery/osquery)
      — ✅ добавлен в плейбук (роль `secrets`, `.deb` из релиза; демон не поднимаем).
- [ ] **KubeHound** — построение путей атак в Kubernetes. *(`go`)* —
      [DataDog/KubeHound](https://github.com/DataDog/KubeHound)
- [ ] **kubestriker** — быстрый security-аудит Kubernetes. *(`pipx`)* —
      [vchinnipilli/kubestriker](https://github.com/vchinnipilli/kubestriker)
- [ ] **kubernetes-rbac-audit** — аудит RBAC в кластере. *(`pipx`)* —
      [cyberark/kubernetes-rbac-audit](https://github.com/cyberark/kubernetes-rbac-audit)
- [ ] **rbac-wizard** — UI для визуализации RBAC-ресурсов. *(`go`)* —
      [pehlicd/rbac-wizard](https://github.com/pehlicd/rbac-wizard)

## Git, GitHub/GitLab и CI/CD

- [x] **lazygit** — TUI для git: статус, коммиты, rebase, конфликты.
      *(`release` / `go`)* — [jesseduffield/lazygit](https://github.com/jesseduffield/lazygit)
      — ✅ добавлен в плейбук (роль `git`).
- [ ] **lumen** — просмотр git diff + генерация коммитов через LLM. *(`cargo`)* —
      [jnsahaj/lumen](https://github.com/jnsahaj/lumen)
- [ ] ⭐ **opencommit** — генерация осмысленных git-сообщений через GPT.
      *(`npm`)* — [di-sukharev/opencommit](https://github.com/di-sukharev/opencommit)
      — ❌ не устанавливаем: то же самое умеет opencode (генерирует сообщение
      коммита по диффу). Отдельный CLI не нужен.
- [ ] **gptcommit** — git-хук `prepare-commit-msg`: сообщения через LLM.
      *(`cargo` + hook)* — [zurawiki/gptcommit](https://github.com/zurawiki/gptcommit)
- [ ] **CodeGPT** — CLI-генерация сообщений коммитов и ревью через LLM.
      *(`go`)* — [appleboy/CodeGPT](https://github.com/appleboy/CodeGPT)
- [ ] **ghgrab** — TUI для просмотра и скачивания файлов из GitHub/GitLab/Gitea.
      *(`cargo`)* — [abhixdd/ghgrab](https://github.com/abhixdd/ghgrab)
- [ ] **glaball** — массовое управление self-hosted GitLab-инстансами. *(`go`)* —
      [flant/glaball](https://github.com/flant/glaball)
- [ ] **gitlabform** — configuration-as-code для GitLab (группы, проекты,
      настройки). *(`pipx`)* — [gitlabform/gitlabform](https://github.com/gitlabform/gitlabform)
- [x] **worktrunk** — CLI для git worktree: параллельные ветки без переключения.
      *(`cargo`)* — [max-sixty/worktrunk](https://github.com/max-sixty/worktrunk)
      — ✅ добавлен в плейбук (роль `git`, бинарник `wt` из tar.xz).
- [x] **changie** — автоматизация changelog и подготовки релизов. *(`release`)* —
      [miniscruff/changie](https://github.com/miniscruff/changie) — ✅ добавлен
      в плейбук (роль `git`, бинарник changie из тарболла релиза).
- [x] ⭐ **reviewdog** — автоматическое code review с любыми линтерами. *(`go`)* —
      [reviewdog/reviewdog](https://github.com/reviewdog/reviewdog)
      — ✅ добавлен в плейбук (роль `git`, бинарник из тарболла).

## Терминал, shell, продуктивность

- [x] ⭐ **zellij** — терминальный мультиплексор (сессии/вкладки/панели) с
      плагинами. *(`release` / `cargo`)* — [zellij-org/zellij](https://github.com/zellij-org/zellij)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла).
- [x] ⭐ **mcfly** — умная история shell: SQLite, приоритизация по контексту, Ctrl-R.
      *(`deb` / `script`)* — [cantino/mcfly](https://github.com/cantino/mcfly)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла). Выбран
      вместо atuin, который конфликтовал с VTE (баг GNOME/vte#2951 — см.
      [`not-install.md`](not-install.md)).
- [x] ⭐ **navi** — интерактивные шпаргалки для команд (поверх fzf). *(`cargo`)* —
      [denisidoro/navi](https://github.com/denisidoro/navi)
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла).
- [x] **cheat** — создание и просмотр собственных шпаргалок команд. *(`go`)* —
      [cheat/cheat](https://github.com/cheat/cheat)
      — ✅ добавлен в плейбук (роль `shell`, отдельной задачей — релизный `.gz`).
- [x] **tldr** — краткие примеры использования команд. *(`npm`)* —
      [tldr-pages/tldr](https://github.com/tldr-pages/tldr)
      — ✅ добавлен в плейбук (роль `shell`, `npm install -g`).
- [x] **gum** — «красивые» примитивы для shell-скриптов. *(`deb`)* —
      [charmbracelet/gum](https://github.com/charmbracelet/gum)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из тарболла).
- [ ] **shellcheck** — статический анализ shell-скриптов на баги. *(`apt`)* —
      [koalaman/shellcheck](https://github.com/koalaman/shellcheck)
      — ✅ добавлен в плейбук (роль `shell`, пакетом из репозитория Ubuntu).
- [ ] **hstr** — TUI-подсказки из истории bash/zsh. *(`deb`)* —
      [dvorka/hstr](https://github.com/dvorka/hstr)
- [ ] **spacer** — вставляет разделители между выводами команд длинного пайплайна.
      *(`cargo`)* — [samwho/spacer](https://github.com/samwho/spacer)
- [ ] **3mux** — мультиплексор терминала в стиле i3. *(`go`)* —
      [aaronjanse/3mux](https://github.com/aaronjanse/3mux)
- [x] **difftastic** — структурный diff (понимает синтаксис языка).
      *(`tar.gz` / `cargo`; ⭐ 26k)* —
      [Wilfred/difftastic](https://github.com/Wilfred/difftastic)
      — ✅ добавлен в плейбук (роль `shell`).
- [x] **delta** — улучшенный просмотр git-diff (side-by-side, подсветка; не
      путать с apt-пакетом `delta` — это другая утилита). *(`deb` git-delta /
      `cargo`; ⭐ 32.5k)* — [dandavison/delta](https://github.com/dandavison/delta)
      — ✅ добавлен в плейбук (роль `shell`).
- [x] **dust** — наглядный `du` с деревом и графиком. *(`deb` / `cargo`; ⭐ 12.5k)* —
      [bootandy/dust](https://github.com/bootandy/dust)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из тарболла).

## Файлы, диск, система

- [ ] **dusage** — наглядный подсчёт занятого места на диске. *(`deb`)* —
      [mihaigalos/dusage](https://github.com/mihaigalos/dusage)
- [x] ⭐ **gdu** — быстрый анализатор диска с интерактивным TUI. *(`deb`)* —
      [dundee/gdu](https://github.com/dundee/gdu)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из тарболла).
- [x] **lf** — терминальный файловый менеджер с vim-подобными клавишами.
      *(`go`)* — [gokcehan/lf](https://github.com/gokcehan/lf)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из тарболла).
- [x] **fastfetch** — быстрый neofetch-подобный вывод информации о системе.
      *(`deb`)* — [fastfetch-cli/fastfetch](https://github.com/fastfetch-cli/fastfetch)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из тарболла).
- [x] **procs** — современная замена `ps` на Rust (дерево, цвета, поиск).
      *(`cargo`)* — [dalance/procs](https://github.com/dalance/procs)
      — ✅ добавлен в плейбук (роль `shell`, бинарником из zip-архива).
- [ ] **auto-cpufreq** — автооптимизация частоты CPU и энергопотребления ноутбука.
      *(`snap` / `script`)* — [AdnanHodzic/auto-cpufreq](https://github.com/AdnanHodzic/auto-cpufreq)
      — ❌ не устанавливаем: хватает того, что в Ubuntu есть из коробки
      (`power-profiles-daemon` + `amd-pstate`/`intel_pstate`).
- [ ] **LACT** — GUI для настройки и мониторинга GPU (AMD/Intel/NVIDIA).
      *(`deb`)* — [ilya-zlobintsev/LACT](https://github.com/ilya-zlobintsev/LACT)
      — ❌ не устанавливаем: сборки под Ubuntu 26.04 в релизах пока нет
      (только debian-12, ubuntu-2204, ubuntu-2404).
- [ ] **mcelog** — userspace-логирование аппаратных ошибок памяти CPU (MCE).
      *(`make`)* — [andikleen/mcelog](https://github.com/andikleen/mcelog)
- [ ] **wondershaper** — простое ограничение полосы пропускания адаптера.
      *(`make`)* — [magnific0/wondershaper](https://github.com/magnific0/wondershaper)
- [ ] **sysinfo** — скрипты сбора подробной системной информации. *(`pipx` / `git clone`)* —
      [peterbay/sysinfo](https://github.com/peterbay/sysinfo)
- [ ] **nxs-backup** — создание, доставка и ротация бэкапов GNU/Linux.
      *(`go`)* — [nixys/nxs-backup](https://github.com/nixys/nxs-backup)
- [ ] **bench-scripts** — набор скриптов бенчмарка Linux-сервера. *(`git clone`)* —
      [haydenjames/bench-scripts](https://github.com/haydenjames/bench-scripts)

## Установщики бинарников (мета-инструменты)

- [x] ⭐ **eget** — установка готовых бинарников прямо из GitHub-релизов. *(`go`)* —
      [zyedidia/eget](https://github.com/zyedidia/eget)
      — ✅ добавлен в плейбук (роль `base`).
- [ ] **binup** — автоматическая установка приложений из GitHub-релизов.
      *(`cargo`)* — [KonishchevDmitry/binup](https://github.com/KonishchevDmitry/binup)
- [ ] **stew** — независимый пакетный менеджер для скомпилированных бинарников.
      *(`go`)* — [alxrem/stew](https://github.com/alxrem/stew)
- [ ] **dra** — скачивание релизных ассетов GitHub по имени. *(`deb`)* —
      [devmatteini/dra](https://github.com/devmatteini/dra)

## Локальный мониторинг (обычно в кластере, но есть CLI)

- [x] **VictoriaMetrics** — быстрая и экономичная TSDB/мониторинг (single-binary
      для локального запуска). *(`go` / `release`)* —
      [VictoriaMetrics/VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics)
      — ✅ частично: ставится только **vmalert-tool** (офлайн-проверка и
      unit-тесты алерт-правил, роль `monitoring`). Сама TSDB и остальные
      бинарники (`vmctl`, `vmagent`, `vmalert`, `vmauth`, `vmbackup`,
      `vmrestore`) на ноуте не нужны — им требуется локальная приёмка/скрейп
      метрик в VictoriaMetrics.
- [ ] **coroot** — observability и APM с root-cause анализом. *(`go`)* —
      [coroot/coroot](https://github.com/coroot/coroot)
- [ ] **opencost** — мониторинг стоимости ворклоадов Kubernetes и облака.
      *(`go`)* — [opencost/opencost](https://github.com/opencost/opencost)
- [ ] **krr** — рекомендации ресурсов на основе Prometheus. *(`pipx`)* —
      [robusta-dev/krr](https://github.com/robusta-dev/krr)
- [ ] **sloth** — генератор Prometheus SLO/алертов из простого описания.
      *(`go`)* — [slok/sloth](https://github.com/slok/sloth)
- [ ] **gatus** — status page и проверки доступности из YAML. *(`go`)* —
      [TwiN/gatus](https://github.com/TwiN/gatus)
- [ ] **cloudprober** — активный мониторинг сбоев до того, как их заметят
      пользователи. *(`go`)* — [cloudprober/cloudprober](https://github.com/cloudprober/cloudprober)
- [ ] **canary-checker** — health-check платформа (Kubernetes-native).
      *(`go`)* — [flanksource/canary-checker](https://github.com/flanksource/canary-checker)

## Обучающие материалы и подборки (не утилиты)

- [ ] **devops-exercises** — вопросы и задания по Linux, Ansible, Terraform,
      Kubernetes, SRE. — [bregman-arie/devops-exercises](https://github.com/bregman-arie/devops-exercises)
- [ ] **DevOps-Bash-tools** — 1200+ скриптов для AWS/GCP/K8s/Docker/CI/CD и
      продвинутые dotfiles. — [HariSekhon/DevOps-Bash-tools](https://github.com/HariSekhon/DevOps-Bash-tools)
- [ ] **kubetools** — курируемый список инструментов Kubernetes. —
      [collabnix/kubetools](https://github.com/collabnix/kubetools)
      (инструменты оттуда, которых нет в этом файле, разобраны в
      [`TODO-kubetools.md`](TODO-kubetools.md))
- [ ] **modern-unix** — современные замены классических unix-команд. —
      [ibraheemdev/modern-unix](https://github.com/ibraheemdev/modern-unix)
- [ ] **awesome-tunneling** — альтернативы ngrok/Cloudflare Tunnel/Tailscale. —
      [anderspitman/awesome-tunneling](https://github.com/anderspitman/awesome-tunneling)
- [ ] **awesome-prometheus-alerts** — коллекция правил алертинга Prometheus. —
      [samber/awesome-prometheus-alerts](https://github.com/samber/awesome-prometheus-alerts)
- [ ] **container-security-checklist** — чек-лист практик container security. —
      [krol3/container-security-checklist](https://github.com/krol3/container-security-checklist)
- [ ] **linux-network-performance-parameters** — разбор sysctl-параметров
      сетевого стека Linux. —
      [leandromoreira/linux-network-performance-parameters](https://github.com/leandromoreira/linux-network-performance-parameters)
- [ ] **boilerplates** — шаблоны конфигов для homelab/self-hosted инфраструктуры. —
      [ChristianLempa/boilerplates](https://github.com/ChristianLempa/boilerplates)
- [ ] **home-ops** / **homelab-gitops-k3s** — примеры GitOps-репозиториев
      (Kubernetes + Flux). — [joryirving/home-ops](https://github.com/joryirving/home-ops),
      [ahgraber/homelab-gitops-k3s](https://github.com/ahgraber/homelab-gitops-k3s)

---

## Уже устанавливается плейбуком — НЕ дублировать

Полный список уже устанавливаемого плейбуком (и кандидатов-дублей, которые
поэтому не берутся) — в [`TODO.md`](TODO.md). `ansible-core` ставится один раз
вручную при bootstrap (см. `README.md`).

---

## Не для локальной установки (сервер/платформа) — только если понадобится

Из звёзд также попали self-hosted-платформы и серверные сервисы — они не
ставятся «на ноутбук» одной командой; их список — в [`TODO.md`](TODO.md).
