# TODO: утилиты из GitHub-звёзд — рассмотреть для плейбука

Источник: все репозитории со ⭐ аккаунта (2057 шт.), из них отобраны ~570
похожих на локально устанавливаемые утилиты и вручную вычищены до списка ниже.

Как читать:
- `- [ ]` — кандидат, ещё не рассмотрен.
- В скобках — способ установки для Ubuntu 26.04 / x86_64.
- Полезные ссылки-заметки по каждому инструменту — в колонках:
  `deb` = есть готовый `.deb` в GitHub-релизе, `AppImage`, `apt`, `snap`,
  `cargo` / `go` / `pipx` / `npm` = сборка/установка из исходников.

Полные данные обхода (все 2057 звёзд + 571 кандидат + ссылки на релизы):
`/home/magnit/Public/starred-tools/data/` (`install_hints.tsv`, `install_report.md`).

---

## Терминал, shell, мультиплексоры

- [ ] **zellij** — терминальный мультиплексор (сессии/вкладки/панели) с плагинами; практичная замена tmux. *(`cargo` / бинарный tar.gz)*
- [ ] **atuin** — синхронизируемая история shell: SQLite, fuzzy-поиск, контекст, статистика. *(`installer.sh` / tar.gz)*
- [ ] **mcfly** — fuzzy-поиск по истории команд с учётом контекста. *(`cargo`)*
- [ ] **navi** — интерактивные шпаргалки для команд (поверх fzf). *(`cargo` / `make`)*
- [ ] **cheat** — создание и просмотр собственных шпаргалок команд. *(`go`)*
- [ ] **tldr** — краткие примеры использования команд. *(`npm -g tldr` или клиент `tealdeer` на `cargo`)*
- [ ] **gum** — «красивые» примитивы для shell-скриптов: выбор, ввод, спиннеры, таблицы. *(`deb`)*
- [ ] **shellcheck** — статический анализ shell-скриптов на баги. *(`apt`)*
- [ ] **hstr** — TUI-подсказки из истории bash/zsh (набираешь — предлагает). *(`deb`)*
- [ ] **worktrunk** — CLI для git worktree: параллельные ветки без переключения. *(`cargo`)*
- [ ] **spacer** — вставляет разделители между выводами команд длинного пайплайна. *(`cargo`)*
- [ ] **shellclear** — находит секреты и опасные команды в истории shell. *(`cargo`)*
- [ ] **3mux** — мультиплексор терминала в стиле i3 (быстрая навигация). *(`go`)*

## Файлы, диск, система

- [ ] **dusage** — наглядный подсчёт занятого места на диске. *(`deb`)*
- [ ] **gdu** — быстрый анализатор диска с интерактивным TUI. *(`tar.gz`)*
- [ ] **lf** — терминальный файловый менеджер с vim-подобными клавишами. *(`go`)*
- [ ] **fastfetch** — быстрый neofetch-подобный вывод информации о системе. *(`deb`)*
- [ ] **procs** — современная замена `ps` на Rust (дерево, цвета, поиск). *(`cargo`)*
- [ ] **auto-cpufreq** — автооптимизация частоты CPU и энергопотребления ноутбука. *(`snap` / installer)*
- [ ] **LACT** — GUI для настройки и мониторинга GPU (AMD/Intel/NVIDIA). *(`deb`)*
- [ ] **mcelog** — userspace-логирование аппаратных ошибок памяти CPU (MCE). *(`make`)*
- [ ] **wondershaper** — простое ограничение полосы пропускания сетевого адаптера. *(`make`)*
- [ ] **sysinfo** (peterbay) — скрипты сбора подробной системной информации. *(`python` / `git clone`)*

## Git-помощники

- [ ] **lazygit** — TUI для git: статус, коммиты, rebase, конфликты мышкой/клавишами. *(`release tar.gz` / `go`)*
- [ ] **lumen** — красивый просмотр git diff + генерация сообщений коммитов через LLM. *(`cargo`)*
- [ ] **opencommit** — генерация осмысленных git-сообщений через GPT. *(`npm -g opencommit`)*
- [ ] **gptcommit** — git-хук prepare-commit-msg: коммит-сообщения через LLM. *(`cargo` + hook)*
- [ ] **CodeGPT** — CLI-генерация сообщений коммитов и ревью через LLM. *(`go` / installer)*
- [ ] **ghgrab** — TUI для просмотра и скачивания отдельных файлов из GitHub-репозиториев. *(`cargo`)*
- [ ] **glaball** — массовое управление self-hosted GitLab-инстансами. *(`go`)*

## Сеть и сетевая диагностика

- [ ] **bandwhich** — TUI: текущая полоса по процессам и соединениям. *(`tar.gz` + setcap)*
- [ ] **sniffnet** — кроссплатформенный GUI-мониторинг сетевого трафика. *(`deb`)*
- [ ] **trippy** — TUI traceroute с историей и статистикой по хопам. *(`deb` / `snap`)*
- [ ] **q** (natesales) — крошечный DNS-клиент: UDP/TCP/DoH/DoT/DoQ. *(`deb`)*
- [ ] **dog** — DNS-клиент с человекочитаемым цветным выводом. *(`cargo`)*
- [ ] **tcping** — `ping` поверх TCP-портов. *(`deb`)*
- [ ] **termshark** — TUI-интерфейс к tshark/Wireshark. *(`go` / бинарник)*
- [ ] **kyanos** — анализ сетевых запросов через eBPF (латентность, payload). *(`go`)*
- [ ] **oryx** — TUI-сниффер трафика на eBPF. *(`cargo`, нужен BPF)*
- [ ] **pktstat-bpf** — монитор Ethernet-трафика на eBPF (TC/XDP/kprobes). *(`deb`)*
- [ ] **asn** — справка по IP/домену: ASN, RPKI, BGP, WHOIS, трейс (shell). *(`git clone`)*
- [ ] **certificate-ripper** — извлекает цепочки сертификатов с удалённых серверов. *(`jar` + `apt install openjdk`)*
- [ ] **showcert** — «OpenSSL для людей»: разбор X.509-сертификатов. *(`pipx`)*
- [ ] **gossl** — CLI-проверка SSL-сертификатов. *(`go`)*
- [ ] **ssh-audit** — аудит конфигурации SSH-серверов и ключей. *(`pipx`)*
- [ ] **httptap** — разбор HTTP-запроса по слоям: DNS, connect, TLS, TTFB. *(`pipx`)*

## Удалённый доступ и шаринг терминала

- [ ] **sshx** — совместный терминал в браузере (мгновенный шаринг сессии). *(`installer.sh` / `cargo`)*
- [ ] **EternalTerminal** — переподключаемый удалённый shell (сессия переживает обрыв). *(`apt` из репо ET; в latest .deb только arm64)*
- [ ] **gotty** — делится терминалом как веб-приложением. *(`go` / бинарник)*
- [ ] **websocat** — netcat/curl для WebSockets. *(`deb`)*
- [ ] **assh** — «умный» ssh-клиент: прокси, jump-хосты, генерация конфигов. *(`tar.gz` / `go`)*

## Kubernetes

- [ ] **kubecolor** — цветной, читаемый вывод `kubectl`. *(`deb`)*
- [ ] **kube-capacity** — обзор requests/limits и реальной загрузки кластера. *(`go` / бинарник)*
- [ ] **ktop** — `top`-подобный монитор метрик нод и подов. *(`go`)*
- [ ] **kubefetch** — neofetch для Kubernetes-кластера. *(`go`)*
- [ ] **ktail** — tail логов подов с фильтрами (идейный родственник stern). *(`go`)*
- [ ] **kube-switch** — маленький bash-переключатель между кластерами. *(`git clone`)*
- [ ] **kubebox** — терминальная и веб-консоль для Kubernetes. *(`npm -g kubebox`)*
- [ ] **kubectl-view-secret** — плагин: декодирование Kubernetes-секретов. *(`go` / `krew`)*
- [ ] **kubectl-df-pv** — плагин: «df» по PersistentVolume. *(`go` / `krew`)*
- [ ] **kubectl-browse-pvc** — плагин: просмотр PVC прямо из CLI. *(`go` / `krew`)*
- [ ] **pluto** — находит устаревшие и удалённые apiVersion в манифестах. *(`go` / бинарник)*
- [ ] **dyff** — структурный diff YAML/JSON с учётом семантики. *(`go` / бинарник)*
- [ ] **headlamp** — полноценный web/desktop UI для Kubernetes. *(`deb` / AppImage)*
- [ ] **kubewall** — dashboard с одним бинарником для нескольких кластеров. *(`deb`)*
- [ ] **ytt** (Carvel) — шаблонизатор YAML, работающий со структурой, а не текстом. *(`go` / бинарник)*
- [ ] **kbld** (Carvel) — встраивает образы в манифесты (image build/resolve). *(`go` / бинарник)*
- [ ] **timoni** — пакетный менеджер для Kubernetes на CUE. *(`go` / бинарник)*
- [ ] **kubevpn** — cloud-native dev-окружение и VPN в кластер. *(`go` / installer)*
- [ ] **txeh** — управление `/etc/hosts` из CLI. *(`deb`)*
- [ ] **netobserv-cli** — сетевая наблюдаемость Kubernetes (eBPF, без агента). *(`go` / бинарник)*

## Docker и контейнеры

- [ ] **dive** — анализ слоёв docker-образа, размер и мусор. *(`deb`)*
- [ ] **dry** — терминальный менеджер Docker-контейнеров, образов, сетей. *(`go` / бинарник)*
- [ ] **ctop** — `top` для метрик контейнеров. *(`go` / бинарник)*
- [ ] **docker-image-layer-explorer** — просмотр и распаковка слоёв образа. *(`pipx`)*
- [ ] **decompose** — reverse-engineering docker-окружений (сети, тома, образы). *(`deb`)*
- [ ] **copacetic** — патчинг уязвимостей прямо в контейнерных образах. *(`go` / бинарник)*
- [ ] **oras** — клиент OCI-реестров для произвольных артефактов. *(`go` / бинарник)*
- [ ] **s5cmd** — очень быстрый параллельный клиент S3. *(`deb`)*

## Логи, JSON, YAML

- [ ] **dasel** — запросы и правка JSON/YAML/TOML/XML/CSV из одной утилиты. *(`go` / бинарник)*
- [ ] **jc** — конвертирует вывод обычных CLI-утилит в JSON. *(`deb` / `pipx`)*
- [ ] **fx** — интерактивный просмотрщик и обработчик JSON. *(`npm -g fx`)*
- [ ] **jnv** — интерактивный фильтр JSON (jq + fzf). *(`cargo`, нужен jq)*
- [ ] **octosql** — SQL-запросы к JSON/CSV/Parquet-файлам. *(`go` / бинарник)*
- [ ] **gomplate** — шаблонизатор конфигов из разных источников (env, файлы, API). *(`go` / бинарник)*
- [ ] **gonzo** — TUI-анализатор логов с графиками по столбцам. *(`go`)*
- [ ] **hl** — быстрый просмотрщик и процессор логов (в т.ч. JSON). *(`cargo`)*
- [ ] **lazyjournal** — TUI для логов journald, auditd, Docker, файлов. *(`deb`)*
- [ ] **json-log-viewer** — интерактивный просмотр JSON-логов. *(`deb`)*
- [ ] **logdy-core** — realtime веб-просмотр логов (tail -f + браузерный UI). *(`go` / бинарник)*
- [ ] **otel-tui** — терминальный просмотрщик OpenTelemetry-трейсов/логов/метрик. *(`go`)*

## eBPF и системная наблюдаемость

- [ ] **bpftop** — real-time обзор запущенных eBPF-программ и их CPU. *(`deb`)*
- [ ] **ecapture** — захват TLS/SSL plaintext через eBPF без MITM-прокси. *(`deb`)*
- [ ] **linnix** — eBPF-наблюдаемость с AI-детекцией инцидентов. *(`deb` / сборка)*

## IaC / Terraform

- [ ] **tfvar** — генерация шаблонов `variables.tf` из `terraform`-кода. *(`deb`)*
- [ ] **prettyplan** — форматирование больших `terraform plan` для чтения. *(`npm` / статический билд)*
- [ ] **terraform-tui** — TUI для работы с Terraform-стейтом. *(`pipx`)*
- [ ] **tpm** — менеджер провайдеров Terraform. *(`go` / бинарник)*
- [ ] **gruntwork-io/pre-commit** — набор pre-commit-хуков для Terraform.\
      *(использовать через уже установленный `pre-commit`)*

## Секреты и безопасность

- [ ] **sops** — шифрование секретов прямо в Git (KMS/age/PGP). Дополняет `vault`. *(`deb`)*
- [ ] **grype** — сканер уязвимостей контейнерных образов и файловых систем. *(`deb` / apt-репо Anchore)*
- [ ] **flawz** — TUI-браузер CVE/уязвимостей. *(`cargo`)*
- [ ] **mfa** — терминальный менеджер 2FA/TOTP. *(`deb`)*

## Установщики бинарников (мета-инструменты)

- [ ] **eget** — установка готовых бинарников прямо из GitHub-релизов. *(`go`)*
- [ ] **binup** — автоматическая установка приложений из GitHub-релизов. *(`cargo`)*
- [ ] **stew** — независимый пакетный менеджер для скомпилированных бинарников. *(`go`)*
- [ ] **dra** — скачивание релизных ассетов GitHub по имени. *(`deb`)*

---

## Уже устанавливается плейбуком — НЕ дублировать

`gh`, `glab`, `gitlabcloner`, `glci`, `pre-commit`, `reposcan`, `tflint`,
`terraform-docs`, `packer`, `driftctl`, `tgsieve`, `kubectl`, `helm`, `k9s`,
`kustomize`, `flux`, `flux9s`, `stern`, `flate`, `flux-tools`, `cty`, `diffyml`,
`kubectl-k8i`, `kubespy`, `kubectx`, `vault`, `gitleaks`, `starship`, `bat`,
`eza`, `zoxide`, `fzf`, `ripgrep`, `fd-find`, `direnv`, `uv`, `yq`, `nmap`,
`tcpdump`, `httpie`, `opencode`, `pitwall`, `mcommander`, `HushMic`,
`rkn-block-checker`, `gai`, `dnsglobe`, `tfenv`, `tgenv`, `docker`,
`guardrails-llm-filter`.

## Не для локальной установки (сервер/платформа) — только если понадобится

Из звёзд также попали self-hosted-платформы и серверные сервисы — они не
ставятся «на ноутбук» одной командой, поэтому вынесены отдельно:
`grafana`, `prometheus`, `immich`, `n8n`, `open-webui`, `supabase`, `sentry`,
`jumpserver`, `plausible`, `wg-easy`, `beszel`, `changedetection.io`, `cachet`,
`uptime-kuma`, `headscale`, `pangolin`, `SafeLine`, `meshery`, `coroot`,
`VictoriaMetrics`, `openobserve`, `ClickHouse`, `odigos`, `cilium`,
`rustdesk`, `joplin`, `AFFiNE`, `siyuan`, `logseq`, `excalidraw`, `bruno`.

## Списки и обучающие материалы (не утилиты)

`awesome`, `system-design-primer`, `devops-exercises`, `modern-unix`,
`awesome-tunneling`, `awesome-prometheus-alerts`, `kubetools` — полезны как
подборки, но устанавливать нечего.