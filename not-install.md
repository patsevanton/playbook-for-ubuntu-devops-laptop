# not-install: утилиты, которые плейбук сознательно НЕ ставит

Сводка инструментов, от которых отказались при сборке ноутбука, и причина
отказа (обычно — незакрытый баг/issue). Если причина исчезла (issue закрыт,
вышла фиксирующая версия), утилиту можно вернуть в плейбук — актуальный статус
удобно сверять по ссылке на issue.

| Утилита | Зачем была | Причина отказа | Issue |
| --- | --- | --- | --- |
| HolmesGPT | on-call AI-ассистент для алертов Prometheus | последний релиз `0.45.0` требует Python `<3.14`; в Ubuntu 26.04 системный Python 3.14, а `pipx` ставит только старую `0.34.0` | [HolmesGPT/holmesgpt#2649](https://github.com/HolmesGPT/holmesgpt/issues/2649) |
| Lens IDE | desktop-клиент для нескольких кластеров Kubernetes | платный продукт | — |
| K8Studio | IDE-клиент для кластеров Kubernetes | платный продукт | — |
| Datree | CLI против Kubernetes-мисконфигураций | проект в архиве (`datreeio/datree`) | — |
| kcmsu | использование ConfigMaps и Secrets в кластере | устанавливает компонент в кластер, а не локальную утилиту | — |
| atuin | синхронизируемая история команд (SQLite, fuzzy-поиск, статистика) | баг VTE: preexec-хук печатается буквально перед каждой командой в gnome-terminal | [GNOME/vte#2951](https://gitlab.gnome.org/GNOME/vte/-/work_items/2951), [rcaloras/bash-preexec#186](https://github.com/rcaloras/bash-preexec/issues/186) |
| victoria-metrics | single-node TSDB (локальный скрейп метрик + Web UI) | не нужна без приёмки/скрейпа метрик в локальную VM; от релизов VM оставлен только `vmalert-tool` | — |
| auto-cpufreq | авто-оптимизация частоты CPU и энергопотребления | не нужен: хватает того, что в Ubuntu есть из коробки (`power-profiles-daemon` + `amd-pstate`/`intel_pstate`) | — |
| mcelog | userspace-логирование аппаратных ошибок памяти и CPU (MCE) | не нужен: MCE видны в `dmesg` и `journalctl -k`, отдельный демон на ноутбуке не требуется | — |
| kubernetes-rbac-audit | аудит RBAC в кластере, в том числе офлайн по заранее выгруженным JSON ролей | не нужен: есть аналоги с готовыми бинарниками — `kubectl-who-can`, `kubeaudit`, `rbac-tool`, `kubehound` | — |
| registry-cli | манипуляции с docker-registry из командной строки | не нужен: заменяет `regctl` из `regclient/regclient` — одиночный бинарник `regctl-linux-amd64` | — |
| gitlab-registry-images-cleaner | чистка старых образов в GitLab registry | не нужен: штатная GitLab cleanup policy плюс `gitlab-container-registry-cleaner` (npm) или `glab api` | — |
| argocd (Argo CD CLI) | CLI для GitOps-деплоя в Argo CD | Argo CD не используется: сервер не разворачивается, локальный CLI без него не нужен | — |
| pulumi | IaC на языках программирования (альтернатива Terraform) | не используется: стек IaC на Terraform (`tfenv`/`tgenv`) | — |
| knee | интерактивный CLI над Ansible: спрашивает окружение и стек, генерирует inventory/group_vars и запускает один из 13 готовых плейбуков (postgres/mysql/mongodb/redis, python/ruby/node/golang webserver) | не нужен: инфраструктура поднимается своими Ansible-плейбуками, а knee — обёртка над чужими плейбуками из своего репозитория | — |
| 3mux | терминальный мультиплексор с тайлингом в стиле i3 | не нужен: используется стандартное окружение Ubuntu, а мультиплексор уже закрывает `zellij` | — |
| asn | справка по IP/домену: ASN, RPKI, BGP, WHOIS, трейс | не нужен: распространяется только bash-скриптом, без релизных артефактов | — |
| kubetail | логи нескольких подов одним потоком (как `kubectl logs -f` для группы подов) | не нужен: то же самое делает `stern` из роли `kubernetes` | — |
| fubectl | набор bash/zsh-функций над kubectl: fuzzy-выбор подов, `kexec`/`kshell`, события, `ktree` | не нужен: распространяется только bash-скриптом | — |
| kube-ps1 | текущий контекст и namespace Kubernetes в prompt bash/zsh/fish | не нужен: распространяется только bash-скриптом | — |
| q-as-kubectl | bash-функции `q`/`qq` для коротких частых команд kubectl по нескольким кластерам | не нужен: распространяется только bash-скриптом | — |
| helm-ssm | плагин Helm: подстановка расшифрованных секретов из AWS SSM Parameter Store | не нужен: распространяется только bash-скриптом | — |
| kube-dump | бэкап кластера в YAML-манифесты без служебных метаданных | не нужен: распространяется только bash-скриптом | — |
| cri-purge | чистка кэша образов на ноде через `crictl` с сохранением свежих версий | не нужен: распространяется только bash-скриптом | — |
| kubebuilder | SDK и скаффолдинг Kubernetes-операторов и CRD на Go | не используется: Kubernetes-операторы не создаются | — |
| minikube | локальный одноузловой Kubernetes | не нужен: работаем с облачными кластерами, для локальных тестов уже есть `kind` | — |
| crictl (cri-tools) | CLI к CRI для отладки контейнеров и образов на нодах | не нужен на ноутбуке: Docker не предоставляет CRI-эндпоинт, а containerd Docker-движка запущен без CRI-плагина | — |
| helmwave | декларативный набор Helm-релизов из `helmwave.yml` (аналог `helmfile`) | не ставим на ноутбук: используется в CI/CD, локально дублирует `helmfile` | — |
| kapp | деплой набора Kubernetes-ресурсов по метке (Carvel) | не используется: деплой через Helm/Flux, kapp нужен только для готового YAML без Helm | — |
| aiac | генерация Kubernetes-манифестов запросом к LLM | не нужен: генерация манифестов через LLM не используется | — |
| Octant | web-UI/дашборд кластера | не нужен: проект в архиве (vmware-archive, 2023), дашборд дублирует `headlamp` и `radar` | — |
| Kubefirst | разворачивание готовой GitOps-платформы «с нуля» | не используется: платформа на Argo CD, а Argo CD в плейбуке не применяется | — |
| Kubexer | desktop-клиент Kubernetes (ресурсы, топология, GitOps) | закрытое ПО: нет публичного релиза, версию и артефакт не проверить | — |
| kube-shell | интерактивный shell для kubectl | заброшен с 2017 (последний релиз 0.0.23), современные API не покрывает; интерактивный режим уже даёт `kube-prompt` | — |
| Finch | CLI контейнерной разработки (Lima/nerdctl) | не нужен: дублирует Docker с Compose, который уже ставится плейбуком | — |
| kor | поиск неиспользуемых ресурсов кластера | не ставим: `releases/latest` ведёт на релиз Helm-чарта, CLI-бинарники лежат под другим тегом — версию без GitHub API не определить | — |
| kubectl-foreach | параллельный запуск kubectl по контекстам | не нужен: распространяется только bash-скриптом, релизных артефактов нет | — |
| Colima | контейнерные рантаймы на macOS/Linux | не нужен: дублирует Docker с Compose, который уже ставится | — |
| Operator SDK | SDK для Kubernetes-операторов | не используется: операторы не создаются (как и `kubebuilder`) | — |
| Arkade | установка бинарников и Helm-чартов одной командой | не нужен: дублирует `eget`, `binup` и `dra` из роли `base` | — |
| kubectl-debug | отладочный контейнер в работающем поде | не нужен: `kubectl debug` встроен в kubectl | — |
| kuttl | декларативное тестирование операторов | не используется: операторы не создаются | — |
| kubectl-kubesec | krew-плагин `kubectl scan` поверх kubesec | не нужен: ставится только krew-плагином с `plugin.yaml`, а `kubesec` уже есть | — |
| k8s-mcp-server | MCP-сервер с kubectl/helm/istioctl | репозиторий в архиве; MCP-сервер Kubernetes уже есть (`kubernetes-mcp-server`) | — |
| kubectl-mcp-server | MCP-сервер Kubernetes для AI-инструментов | не нужен: дублирует `kubernetes-mcp-server` | — |
| Hardeneks, MKAT, eksdemo | проверки, аудит и демо для Amazon EKS | не нужны: заточены под EKS, работа идёт с другими облаками | — |
| Radius | cloud-native платформа приложений | не нужна на ноутбуке: требует развёрнутого control plane | — |
| Scaphandre | агент метрик энергопотребления хоста | не нужен: это демон на хосте, а не утилита для Kubernetes | — |
| mcp-for-argocd, Argazer | MCP-сервер и мониторинг чартов для Argo CD | Argo CD не используется | — |
| Jenkins X | CI/CD-платформа для Kubernetes | ставится в кластер как платформа, а не на ноутбук | — |
| werf, kubedog, odo, Krane | сборка/доставка, CI-наблюдение, разработка и RBAC-анализ | в GitHub-релизах нет готовых бинарников (trdl/install-скрипт или `go install`) | — |

---

## atuin — баг VTE (GNOME/vte#2951)

`atuin init bash` подключает `bash-preexec`, который на bash ≥ 5.3 вешает
preexec-хук в `PS0` строкой `${ ... }`. Интеграция VTE
(`/etc/profile.d/vte-2.91.sh`, gnome-terminal) заканчивает свой `PS0`
OSC-терминатором `\e\\` (одиночный `\` на конце), и prompt-экранирование
превращает `$` хука в литеральный текст — строка
`${ __bp_invoke_preexec_from_ps0 "..." >&2; }` печатается буквально перед
каждой командой.

Баг на стороне VTE, исправлен только в vte 0.84:
- [GNOME/vte#2951](https://gitlab.gnome.org/GNOME/vte/-/work_items/2951)
- [rcaloras/bash-preexec#186](https://github.com/rcaloras/bash-preexec/issues/186)

**Замена в плейбуке**: [mcfly](https://github.com/cantino/mcfly) — тоже умная
история (Ctrl-R с приоритизацией по контексту), но хук ставится через
`PROMPT_COMMAND`, не трогая `PS0`, поэтому бага VTE не имеет.

**Очистка**: роль `shell` удаляет бинарник `/usr/local/bin/atuin` и каталоги
`~/.config/atuin`, `~/.local/share/atuin`, `~/.atuin` (см.
`roles/shell/tasks/main.yml`).

Вернуть atuin можно после обновления VTE до 0.84+ — либо оставить mcfly как
есть (он локальный, без синхронизации; atuin давал синхронизацию `atuin sync`).

## Argo CD, Pulumi и Kubebuilder — не используются

Argo CD не применяется ни как сервер, ни как локальный CLI `argocd` —
GitOps закрывают Flux (`flux`, `flux9s`, `flate`, `flux-tools`) и `helmfile`.

Pulumi не ставится, потому что IaC-стек уже на Terraform: `tfenv`/`tgenv`,
`tflint`, `terraform-docs`, `tftui` и остальные инструменты роли `iac`.

Kubebuilder не ставится, потому что Kubernetes-операторы не создаются.
Если понадобится писать оператор/CRD на Go — вернуть в
`roles/kubernetes/defaults/main.yml` одной записью.

## knee — свои плейбуки

knee — интерактивный CLI над Ansible: спрашивает окружение (local/staging/
production) и стек, генерирует inventory и group_vars, а затем запускает один
из 13 плейбуков, которые лежат в его репозитории (`playbooks/*.yml`:
PostgreSQL/MySQL/MongoDB/Redis, python/ruby/node/golang webserver,
`user_management`, `webserver_base`). Запуск — через `ansible-runner`, то есть
CLI только выбирает файл и подставляет переменные.

Не ставим: инфраструктура поднимается своими Ansible-плейбуками, чужие
плейбуки внутри knee не нужны. Ставился бы он всё равно из клона —
пакета на PyPI нет, а в релизе v0.0.1 нет ассетов.

## minikube — облачные кластеры и kind

Работа идёт с облачными кластерами, поэтому локальный кластер на ноутбуке
не нужен. Для быстрых локальных тестов уже установлен `kind` (кластер в
Docker-контейнерах), он покрывает тот же сценарий без отдельного драйвера
виртуальной машины. minikube не ставим.

## crictl — CRI на ноутбуке нет

`crictl` разговаривает с CRI-сервисом (gRPC) контейнерного рантайма. На этом
ноутбуке Docker Engine не предоставляет CRI: `/var/run/docker.sock` отвечает
`unknown service runtime.v1.RuntimeService`, а containerd, который запускает
Docker, работает без включённого CRI-плагина (`/run/containerd/containerd.sock`
даёт ту же ошибку). Поэтому контейнер, запущенный через `docker run`, в
`crictl ps` не появится — проверено.

Для отладки узлов облачных кластеров `crictl` ставится на сам узел или
запускается через `kubectl debug node/...`; на ноутбуке он бесполезен.
Исключение — если ноутбук станет kubelet-нодой с containerd и включённым CRI,
но это не сценарий этого плейбука.

## Helmwave, kapp и aiac — не используются

Helmwave — декларативное описание набора Helm-релизов, по смыслу аналог
`helmfile`. Основной сценарий — CI/CD. На ноутбуке он дублирует уже
установленный `helmfile`, поэтому не ставится. Вернуть можно, если в CI
используется именно helmwave и нужно локально воспроизводить сборку или
дифф.

kapp — деплойер готовых Kubernetes-манифестов из набора Carvel: метки
приложения, diff, ожидание готовности, удаление группой. Не ставим, потому
что деплой идёт через Helm/Flux; kapp полезен только в связке
`ytt` → `kbld` → `kapp`, которой в плейбуке нет.

aiac — генератор манифестов и IaC-конфигов запросом к LLM. Не нужен:
генерация манифестов через LLM в работе не используется.