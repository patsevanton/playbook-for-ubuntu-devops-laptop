# TODO-kubetools: кандидаты из collabnix/kubetools, которых ещё нет в репозитории

Отдельный трекер под подборку инструментов Kubernetes
[collabnix/kubetools](https://github.com/collabnix/kubetools) — курируемый список
инструментов Kubernetes (520 уникальных записей на 2026-10-18, в README
автора — более 500). Здесь собраны те инструменты, которые **ещё не описаны**
ни в одном файле-трекере плейбука.

Дедупликация 2026-10-18: из списка исключены те, что уже есть в
[`TODO-awesome-github.md`](TODO-awesome-github.md),
[`TODO-trend-github.md`](TODO-trend-github.md), [`TODO.md`](TODO.md) (в т.ч.
уже устанавливаемое плейбуком и отнесённое к серверным/платформенным) и других
`TODO-*.md`. Полный список совпадений — в разделе «Уже описано в репозитории».

Как читать:

- `- [ ]` — кандидат, ещё не рассмотрен.
- `- [x]` — рассмотрен (добавлен в плейбук или отброшен) — с заметкой у пункта.
- В скобках — **предварительный** способ установки для Ubuntu 26.04 / x86_64:
  `deb` — готовый `.deb` в GitHub-релизе, `AppImage`, `apt`, `snap`,
  `release` — готовый бинарник/архив из релиза, `go` / `cargo` / `pipx` /
  `npm` / `gem` / `script` / `make` — сборка или установка из исходников,
  `krew` — плагин kubectl через менеджер плагинов `krew`,
  `helm` — ставится в кластер Helm-чартом.
- Способы установки **не сверялись с релизами** (в отличие от
  `TODO-trend-github.md`, где проверялись): при добавлении в плейбук уточнять
  через редирект `releases/latest` без GitHub API (см. `README.md`).
- Ссылка ведёт на репозиторий проекта.

Порядок разделов — по категориям kubetools. Сначала идут локальные CLI/TUI
(ставятся на ноутбук и работают с кластером через kubeconfig), затем —
кластерные операторы/контроллеры и self-hosted платформы (в кластер или как
сервис, «не на ноутбук»).

---

## 1. Локальные CLI/TUI — кандидаты

### Pods

- [ ] **kubetail** (johanhaleby) — bash-скрипт: tail логов нескольких подов
      сразу. *(`script`)* — [johanhaleby/kubetail](https://github.com/johanhaleby/kubetail)
- [ ] **kubectl-ports-rs** — плагин kubectl: список открытых портов ресурсов
      Pod. *(`krew`)* — [widnyana/kubectl-ports-rs](https://github.com/widnyana/kubectl-ports-rs)

### Cluster Management

- [ ] **Monokle** — desktop-инструмент для авторства, анализа и деплоя
      Kubernetes-конфигов. *(`deb` / `AppImage`)* —
      [kubeshop/monokle](https://github.com/kubeshop/monokle)
- [ ] **kops** — установка, апгрейд и управление production-кластерами K8s.
      *(`release`)* — [kubernetes/kops](https://github.com/kubernetes/kops)
- [ ] **silver-surfer** — проверка совместимости `apiVersion` и путь миграции
      при апгрейде кластера. *(`release` / `go`)* —
      [devtron-labs/silver-surfer](https://github.com/devtron-labs/silver-surfer)
- [ ] **kubeprompt** — информация о текущем кластере/контексте в prompt shell.
      *(`release`?)* — [jlesquembre/kubeprompt](https://github.com/jlesquembre/kubeprompt)
- [ ] **kubectl-snapshot** — плагин kubectl: снимки состояния кластера.
      *(`krew`)* — [fbrubbo/kubectl-snapshot](https://github.com/fbrubbo/kubectl-snapshot)
- [ ] **kapp** — деплой «Kubernetes-приложений» (набор ресурсов по метке).
      *(`release` / `go`)* — [k14s/kapp](https://github.com/k14s/kapp)
- [ ] **Octant** — web-UI для понимания сложности кластера (архивный, но
      полезен как локальный дашборд). *(`release`)* —
      [vmware-tanzu/octant](https://github.com/vmware-tanzu/octant)
- [ ] **xlskubectl** — управление кластером из «электронной таблицы».
      *(`pipx`)* — [learnk8s/xlskubectl](https://github.com/learnk8s/xlskubectl)
- [ ] **Kubefirst** — CLI: разворачивает готовую GitOps-платформу с нуля.
      *(`script`)* — [kubefirst/kubefirst](https://github.com/kubefirst/kubefirst)
- [ ] **kustomizer** — экспериментальный менеджер пакетов k8s-конфигов как OCI
      артефактов. *(`release`)* — [stefanprodan/kustomizer](https://github.com/stefanprodan/kustomizer)
- [ ] **KindScaler** — управление нодами KinD-кластера без пересоздания.
      *(`go`)* — [lobuhi/kindscaler](https://github.com/lobuhi/kindscaler)
- [ ] **buoy** — декларативный Kubernetes-дашборд в терминале. *(`release`)* —
      [everettraven/buoy](https://github.com/everettraven/buoy)
- [ ] **k9sight** — быстрый keyboard-driven TUI для отладки workload'ов.
      *(`release` / `go`)* — [doganarif/k9sight](https://github.com/doganarif/k9sight)
- [ ] **Hanoi-CLI** — интерактивный советник по ребалансировке подов, симулирует
      отказ нод. *(`release`)* — [k-krew/hanoi-cli](https://github.com/k-krew/hanoi-cli)
- [ ] **Kubexer Kubernetes IDE** — кроссплатформенный desktop-клиент: ресурсы,
      топология, helm, GitOps (ArgoCD/Flux). *(`deb`)* —
      [kubexer.com](https://kubexer.com) (закрытый; ссылка на сайт)
- [ ] **Rubick** — k8s desktop-клиент (Tauri + Rust + React), RBAC-aware.
      *(`deb`)* — [Dudude-bit/rubick](https://github.com/Dudude-bit/rubick)
- [ ] **kzero** — декларативные down/up/reset кластера для bastion-обслуживания.
      *(`release`)* — [hrodrig/kzero](https://github.com/hrodrig/kzero)

### Cluster with Core CLI tools

- [ ] **kube-shell** — интегрированный интерактивный shell для kubectl с
      автодополнением. *(`pipx`)* —
      [cloudnativelabs/kube-shell](https://github.com/cloudnativelabs/kube-shell)
- [ ] **kuttle** — обёртка kubectl для sshuttle без SSH. *(`pipx`)* —
      [kayrus/kuttle](https://github.com/kayrus/kuttle)
- [ ] **kubectl-sudo** — запуск k8s-команд с привилегиями другого пользователя.
      *(`krew`)* — [postfinance/kubectl-sudo](https://github.com/postfinance/kubectl-sudo)
- [ ] **Ktunnel** — публикация локальных ресурсов в Kubernetes (туннель).
      *(`go` / `release`)* — [omrikiei/ktunnel](https://github.com/omrikiei/ktunnel)
- [ ] **Vimkubectl** — управление любыми k8s-ресурсами прямо из Vim.
      *(`vim plugin`)* — [rottencandy/vimkubectl](https://github.com/rottencandy/vimkubectl)
- [ ] **fubectl** — набор bash-функций: меньше повторяющихся команд kubectl.
      *(`script`)* — [kubermatic/fubectl](https://github.com/kubermatic/fubectl)
- [ ] **kube-ps1** — prompt с текущим контекстом/namespace Kubernetes.
      *(`script`)* — [jonmosco/kube-ps1](https://github.com/jonmosco/kube-ps1)
- [ ] **Conftest** — тесты структурированных конфигов на языке Rego (OPA).
      *(`deb` / `release`)* — [open-policy-agent/conftest](https://github.com/open-policy-agent/conftest)
- [ ] **Kube-prompt** — интерактивный kubectl-клиент с автодополнением.
      *(`release` / `go`)* — [c-bata/kube-prompt](https://github.com/c-bata/kube-prompt)
- [ ] **Click** — интерактивный CLI-контроллер для Kubernetes. *(`pipx`)* —
      [databricks/click](https://github.com/databricks/click)
- [ ] **Kubie** — более мощная альтернатива `kubectx`/`kubens` (изолированные
      kubeconfig по shell). *(`cargo` / `release`)* — [sbstp/kubie](https://github.com/sbstp/kubie)
- [ ] **Finch** — CLI-клиент контейнерной разработки (обёртка над Lima/ncrun).
      *(`deb`)* — [runfinch/finch](https://github.com/runfinch/finch)
- [ ] **kor** — поиск неиспользуемых ресурсов Kubernetes. *(`release` / `go`)* —
      [yonahd/kor](https://github.com/yonahd/kor)
- [ ] **image-builder** — сборка VM-образов Kubernetes для разных провайдеров.
      *(`script` / `pipx`)* — [kubernetes-sigs/image-builder](https://github.com/kubernetes-sigs/image-builder)
- [ ] **Duplik8s** — плагин kubectl: дублирование ресурсов кластера. *(`krew`)* —
      [Telemaco019/duplik8s](https://github.com/Telemaco019/duplik8s)
- [ ] **kl** — интерактивный TUI для логов нескольких контейнеров/кластеров.
      *(`release` / `go`)* — [robinovitch61/kl](https://github.com/robinovitch61/kl)
- [ ] **kubectl-foreach** — параллельный запуск kubectl по всем/некоторым
      контекстам. *(`script` / `krew`)* — [ahmetb/kubectl-foreach](https://github.com/ahmetb/kubectl-foreach)
- [ ] **kubecui** — делает kubectl дружелюбнее (TUI-обёртка). *(`pipx`)* —
      [pymag09/kubecui](https://github.com/pymag09/kubecui)
- [ ] **q-as-kubectl** — ещё один способ управлять командами k8s через `q`.
      *(`script`)* — [ildar-shaimordanov/q-as-kubectl](https://github.com/ildar-shaimordanov/q-as-kubectl)
- [ ] **kubectl-find** — UNIX-find-подобный плагин kubectl: поиск и действия над
      ресурсами. *(`krew`)* — [alikhil/kubectl-find](https://github.com/alikhil/kubectl-find)
- [ ] **kubefwd** — массовый port-forward сервисов с уникальными IP и записями в
      `/etc/hosts`. *(`release` / `go`)* — [txn2/kubefwd](https://github.com/txn2/kubefwd)
- [ ] **kubecm** — удобное управление `kubeconfig`. *(`release` / `go`)* —
      [sunny0826/kubecm](https://github.com/sunny0826/kubecm)
- [ ] **krelay** — альтернатива `kubectl port-forward`: TCP/UDP до IP/хоста внутри
      кластера. *(`krew`)* — [knight42/krelay](https://github.com/knight42/krelay)
- [ ] **gwctl** — управление и разбор ресурсов Gateway API. *(`go` / `release`)* —
      [kubernetes-sigs/gwctl](https://github.com/kubernetes-sigs/gwctl)
- [ ] **kbu** — однопанельный TUI Kubernetes (навигация Tab/Space/Enter/Esc).
      *(`release` / `go`)* — [vulcanshen/kbu](https://github.com/vulcanshen/kbu)

### Alert and Monitoring

- [ ] **Kube-Scout** — поиск тревожных проблем по кластерам. *(`release` / `go`)* —
      [ReallyLiri/kubescout](https://github.com/ReallyLiri/kubescout)
- [ ] **Ksniff** — плагин kubectl: сниффинг трафика подов через tcpdump/wireshark.
      *(`krew`)* — [eldadru/ksniff](https://github.com/eldadru/ksniff)
- [ ] **kptop** — мониторинг нод, подов, контейнеров и PVC через метрики
      Prometheus в терминале. *(`release`)* — [eslam-gomaa/kptop](https://github.com/eslam-gomaa/kptop)
- [ ] **kurt** — плагин kubectl: контекст «что перезапускается» в кластере.
      *(`release`)* — [soraro/kurt](https://github.com/soraro/kurt)
- [ ] **HolmesGPT** — on-call ассистент для алертов Prometheus (AI-расследование).
      *(`pipx`)* — [robusta-dev/holmesgpt](https://github.com/robusta-dev/holmesgpt)
- [ ] **kcmsu** — использование ConfigMaps и Secrets в кластере. *(`release` / `go`)* —
      [cristian98149/kcmsu](https://github.com/cristian98149/kcmsu)
- [ ] **kubectl-outagedeck** — плагин kubectl: статус облачных провайдеров по
      аннотациям workload'ов. *(`krew` / `release`)* —
      [outagedeck/kubectl-outagedeck](https://github.com/outagedeck/kubectl-outagedeck)

### Logging and Tracing

- [ ] **kubectl-mtail** — плагин kubectl: tail логов подов по label selector.
      *(`go`)* — [grzesuav/kubectl-mtail](https://gitlab.com/grzesuav/kubectl-mtail)
- [ ] **kail** — просмотрщик логов Kubernetes (`tail -f` по кластеру). *(`release`)* —
      [boz/kail](https://github.com/boz/kail)
- [ ] **kubetail** (kubetail-org) — real-time logging dashboard: терминал или
      браузер. *(`release`)* — [kubetail-org/kubetail](https://github.com/kubetail-org/kubetail)

### Troubleshooting / Debugging

- [ ] **kubectl-debug** — запуск отладочного контейнера со всеми утилитами в
      работающем поде (предшественник `kubectl debug`). *(`release`)* —
      [aylei/kubectl-debug](https://github.com/aylei/kubectl-debug)
- [ ] **PowerfulSeal** — мощный инструмент тестирования кластеров (chaos).
      *(`pipx`)* — [bloomberg/powerfulseal](https://github.com/bloomberg/powerfulseal)
- [ ] **crash-diagnostics** — расследование неотвечающих/упавших кластеров.
      *(`release`)* — [vmware-tanzu/crash-diagnostics](https://github.com/vmware-tanzu/crash-diagnostics)
- [ ] **kubectl-doctor** — плагин-триаж кластера («brew doctor» для k8s).
      *(`krew`)* — [emirozer/kubectl-doctor](https://github.com/emirozer/kubectl-doctor)
- [ ] **kpexec** — запуск команд в контейнере с повышенными привилегиями.
      *(`release`)* — [ssup2/kpexec](https://github.com/ssup2/kpexec)
- [ ] **Inspektor Gadget** — набор eBPF-инструментов для отладки и инспекции
      ресурсов/приложений. *(`deb` / `release`)* —
      [inspektor-gadget/inspektor-gadget](https://github.com/inspektor-gadget/inspektor-gadget)
- [ ] **kuttl** — декларативное тестирование Kubernetes-операторов. *(`release`)* —
      [kudobuilder/kuttl](https://github.com/kudobuilder/kuttl)
- [ ] **kubediff** — сравнение исходников и задеплоенного (что изменилось между
      окружениями). *(`go`)* — [Ramilito/kubediff](https://github.com/Ramilito/kubediff)
- [ ] **k8shark** — снимок состояния кластера в архив + replay через mock API
      server. *(`release`)* — [phenixblue/k8shark](https://github.com/phenixblue/k8shark)
- [ ] **groot** — read-only диагностика: логи подов, события и контекст API в один
      `.tar.gz` для RCA. *(`release`)* — [hrodrig/groot](https://github.com/hrodrig/groot)

### Development Tools/Kit

- [ ] **Okteto** — разработка и тестирование кода прямо в Kubernetes.
      *(`script`)* — [okteto/okteto](https://github.com/okteto/okteto)
- [ ] **Garden** — оркестратор разработки для k8s, контейнеров и функций.
      *(`release`)* — [garden-io/garden](https://github.com/garden-io/garden)
- [ ] **Copper** — валидатор конфигов Kubernetes. *(`release`)* —
      [cloud66-oss/copper](https://github.com/cloud66-oss/copper)
- [ ] **ko** — сборка и деплой Go-приложений в Kubernetes. *(`go`)* —
      [google/ko](https://github.com/google/ko)
- [ ] **Lens IDE** — мощный desktop-клиент для нескольких кластеров (сейчас
      Mirror.xyz, есть Linux). *(`deb` / `AppImage`)* —
      [k8slens.dev](https://k8slens.dev/)
- [ ] **Kosko** — организация k8s-манифестов на JavaScript. *(`npm`)* —
      [kosko.dev](https://kosko.dev/)
- [ ] **KuberEz** — графическое моделирование k8s-манифестов. *(`release`)* —
      [uengine-oss/kuber-ez](https://github.com/uengine-oss/kuber-ez)
- [ ] **Kubes** — деплой-инструмент: сборка образа, компиляция YAML, apply.
      *(`gem`)* — [boltops-tools/kubes](https://github.com/boltops-tools/kubes)
- [ ] **Kubebuilder** — SDK для построения Kubernetes API на CRD. *(`release`)* —
      [kubernetes-sigs/kubebuilder](https://github.com/kubernetes-sigs/kubebuilder)
- [ ] **Operator-sdk** — SDK для операторов Kubernetes. *(`release`)* —
      [operator-framework/operator-sdk](https://github.com/operator-framework/operator-sdk)
- [ ] **cdk8s** — k8s-приложения и абстракции на языках программирования.
      *(`npm`)* — [cdk8s-team/cdk8s](https://github.com/cdk8s-team/cdk8s)
- [ ] **Arkade** — «магазин» готовых установок (в т.ч. k8s-утилит) одной командой.
      *(`script`)* — [alexellis/arkade](https://github.com/alexellis/arkade)
- [ ] **k2tf** — конвертер Kubernetes YAML → Terraform HCL. *(`release`)* —
      [sl1pm4t/k2tf](https://github.com/sl1pm4t/k2tf)
- [ ] **Kompose** — конвертация docker-compose в ресурсы Kubernetes. *(`release`)* —
      [kubernetes/kompose](https://github.com/kubernetes/kompose)
- [ ] **Gefyra** — быстрая локальная разработка приложений с Kubernetes.
      *(`script`)* — [gefyrahq/gefyra](https://github.com/gefyrahq/gefyra)
- [ ] **odo** — CLI для быстрой итеративной разработки на k8s и Podman.
      *(`release`)* — [redhat-developer/odo](https://github.com/redhat-developer/odo)
- [ ] **k8tz** — CLI и admission-controller: инъекция таймзон в Pods/CronJobs.
      *(`release`)* — [k8tz/k8tz](https://github.com/k8tz/k8tz)
- [ ] **KDash** — простой и быстрый дашборд Kubernetes на Rust. *(`cargo` / `release`)* —
      [kdash-rs/kdash](https://github.com/kdash-rs/kdash)
- [ ] **K8Studio** — кроссплатформенный IDE-клиент для кластеров Kubernetes.
      *(`deb` / `AppImage`)* — [guiqui/k8Studio](https://github.com/guiqui/k8Studio)
- [ ] **Radius** — портативная cloud-native платформа приложений. *(`script`)* —
      [radius-project/radius](https://github.com/radius-project/radius)
- [ ] **k'exp** — визуальное понимание Kubernetes (учебный web-UI, локально).
      *(`release`)* — [iximiuz/kexp](https://github.com/iximiuz/kexp)
- [ ] **k8s-insider** — zero-config доступ к сети кластера. *(`release`)* —
      [TrueGoric/k8s-insider](https://github.com/TrueGoric/k8s-insider)
- [ ] **Helmify** — создание Helm-чарта из Kubernetes YAML. *(`go` / `release`)* —
      [arttor/helmify](https://github.com/arttor/helmify)
- [ ] **Reckoner** — декларативная установка и управление релизами Helm.
      *(`release`)* — [FairwindsOps/reckoner](https://github.com/FairwindsOps/reckoner)
- [ ] **Katenary** — конвертация docker-compose в настраиваемый Helm-чарт.
      *(`pipx`)* — [metal3d/katenary](https://github.com/metal3d/katenary)
- [ ] **node-gizmo** — CLI-утилита по нодам Kubernetes. *(`release`)* —
      [Kavinraja-G/node-gizmo](https://github.com/Kavinraja-G/node-gizmo)
- [ ] **kubectl-fields** — плагин kubectl: разбор иерархии ресурсов. *(`krew`)* —
      [rewanthtammana/kubectl-fields](https://github.com/rewanthtammana/kubectl-fields)
- [ ] **scaf** — каркас нового проекта на Kubernetes. *(`pipx`)* —
      [sixfeetup/scaf](https://github.com/sixfeetup/scaf)
- [ ] **KubePug** — pre-upgrade checker: устаревшие API в манифестах. *(`release`)* —
      [kubepug/kubepug](https://github.com/kubepug/kubepug)
- [ ] **kubectl-slice** — плагин kubectl: разбиение multi-YAML на отдельные файлы.
      *(`krew` / `release`)* — [patrickdappollonio/kubectl-slice](https://github.com/patrickdappollonio/kubectl-slice)
- [ ] **KubeDiagrams** — генерация диаграмм архитектуры из манифестов, kustomize,
      Helm и состояния кластера. *(`pipx`)* — [philippemerle/KubeDiagrams](https://github.com/philippemerle/KubeDiagrams)
- [ ] **Grafana-Tanka** — гибкая и переиспользуемая конфигурация Kubernetes
      (Jsonnet). *(`go` / `release`)* — [grafana/tanka](https://github.com/grafana/tanka)
- [ ] **Klustr** — нативный cross-platform desktop-клиент по kubeconfig (без
      установки в кластер). *(`deb`)* — [SametKUM/klustr](https://github.com/SametKUM/klustr)

### Alternative Tools for Development

- [ ] **minikube** — локальный одноузловой Kubernetes. *(`deb` / `release`)* —
      [kubernetes/minikube](https://github.com/kubernetes/minikube)
- [ ] **mokctl** — сборка conformant multi-node кластеров в контейнерах.
      *(`release`)* — [bashtools/mokctl](https://github.com/bashtools/mokctl)

### CI/CD integration Tools

- [ ] **werf** — GitOps-доставка приложений в k8s с интеграцией GitLab/CI.
      *(`deb` / `release`)* — [werf/werf](https://github.com/werf/werf)
- [ ] **Argo CD CLI** (`argocd`) — CLI для declarative continuous deployment (сам
      сервер Argo CD отнесён к платформенным в [`TODO.md`](TODO.md); здесь именно
      локальный CLI). *(`release`)* — [argoproj/argo-cd](https://github.com/argoproj/argo-cd)
- [ ] **Jenkins X** (`jx`) — CI+CD для Kubernetes с preview-окружениями на PR.
      *(`release`)* — [jenkins-x/jx](https://github.com/jenkins-x/jx)
- [ ] **Helmwave** — helm3-native деплой чартов. *(`release`)* —
      [helmwave/helmwave](https://github.com/helmwave/helmwave)
- [ ] **Helmsman** — «Helm Charts as Code». *(`release`)* —
      [Praqma/helmsman](https://github.com/Praqma/helmsman)
- [ ] **Kluctl** — templated declarative деплой (push/pull GitOps). *(`release`)* —
      [kluctl/kluctl](https://github.com/kluctl/kluctl)
- [ ] **kubedog** — наблюдение за ресурсами в CI/CD пайплайнах. *(`release`)* —
      [werf/kubedog](https://github.com/werf/kubedog)
- [ ] **Nelm** — современная альтернатива Helm (исправляет болячки Helm).
      *(`release`)* — [werf/nelm](https://github.com/werf/nelm)
- [ ] **mcp-for-argocd** — MCP-сервер для Argo CD (AI-ассистенты + Argo CD).
      *(`go`)* — [argoproj-labs/mcp-for-argocd](https://github.com/argoproj-labs/mcp-for-argocd)
- [ ] **Argazer** — мониторинг обновлений Helm-чартов в ArgoCD-приложениях.
      *(`release`)* — [k-krew/argazer](https://github.com/k-krew/argazer)

### Security Tools

- [ ] **Kubevious CLI** — предотвращение cross-manifest ошибок и нарушений best
      practices в YAML/Helm/кластере. *(`npm` / `script`)* —
      [kubevious/cli](https://github.com/kubevious/cli)
- [ ] **klum** — «Kubernetes Lazy User Manager». *(`go`)* —
      [ibuildthecloud/klum](https://github.com/ibuildthecloud/klum)
- [ ] **kube-bench** — проверки кластера по CIS Kubernetes Benchmark. *(`release`)* —
      [aquasecurity/kube-bench](https://github.com/aquasecurity/kube-bench)
- [ ] **kube-hunter** — пентест: поиск слабых мест кластера. *(`pipx`)* —
      [aquasecurity/kube-hunter](https://github.com/aquasecurity/kube-hunter)
- [ ] **kubectl-who-can** — плагин kubectl: кто имеет RBAC-права на действия.
      *(`krew`)* — [aquasecurity/kubectl-who-can](https://github.com/aquasecurity/kubectl-who-can)
- [ ] **rbac-lookup** — поиск ролей/clusterRole у пользователей, SA и групп.
      *(`release` / `go`)* — [FairwindsOps/rbac-lookup](https://github.com/FairwindsOps/rbac-lookup)
- [ ] **kubeaudit** — аудит кластера против типовых security-контролей.
      *(`release` / `go`)* — [Shopify/kubeaudit](https://github.com/Shopify/kubeaudit)
- [ ] **audit2rbac** — автогенерация RBAC-политик из audit-логов. *(`go`)* —
      [liggitt/audit2rbac](https://github.com/liggitt/audit2rbac)
- [ ] **Chartsec** — сканер безопасности Helm-чартов. *(`release`)* —
      [banzaicloud/chartsec](https://github.com/banzaicloud/chartsec)
- [ ] **Datree** — CLI против k8s-мисконфигураций по best practices и политикам.
      *(`script`)* — [datreeio/datree](https://github.com/datreeio/datree)
- [ ] **Krane** — статический анализ и визуализация RBAC. *(`release`)* —
      [appvia/krane](https://github.com/appvia/krane)
- [ ] **kubesec** — анализ рисков безопасности k8s-ресурсов. *(`release`)* —
      [controlplaneio/kubesec](https://github.com/controlplaneio/kubesec)
- [ ] **Hardeneks** — проверки соответствия EKS best practices. *(`pipx`)* —
      [aws-samples/hardeneks](https://github.com/aws-samples/hardeneks)
- [ ] **Hubble** — сетевая/сервисная/security-наблюдаемость на eBPF (CLI).
      *(`release`)* — [cilium/hubble](https://github.com/cilium/hubble)
- [ ] **Peirates** — пентест-инструмент Kubernetes. *(`release`)* —
      [inguardians/peirates](https://github.com/inguardians/peirates)
- [ ] **kubectl-kubesec** — плагин kubectl: анализ рисков ресурсов. *(`krew`)* —
      [controlplaneio/kubectl-kubesec](https://github.com/controlplaneio/kubectl-kubesec)
- [ ] **Netchecks** — набор инструментов проверки сетевых условий.
      *(`pipx`)* — [hardbyte/netchecks](https://github.com/hardbyte/netchecks)
- [ ] **IceKube** — поиск путей атак в кластере от низкопривилегированной точки.
      *(`pipx`)* — [WithSecureLabs/IceKube](https://github.com/WithSecureLabs/IceKube)
- [ ] **kubectl-modify-secret** — плагин: правка секретов без base64.
      *(`krew`)* — [rajatjindal/kubectl-modify-secret](https://github.com/rajatjindal/kubectl-modify-secret)
- [ ] **rbac-tool** — RBAC Power Toys: визуализация, анализ, генерация, запросы.
      *(`release` / `go`)* — [alcideio/rbac-tool](https://github.com/alcideio/rbac-tool)
- [ ] **MKAT** — all-in-one аудит-тулкит для managed Kubernetes (пока EKS).
      *(`pipx`)* — [DataDog/managed-kubernetes-auditing-toolkit](https://github.com/DataDog/managed-kubernetes-auditing-toolkit)
- [ ] **scan2html** — плагин Trivy: вывод сканов в интерактивный HTML.
      *(`pipx`)* — [fatihtokus/scan2html](https://github.com/fatihtokus/scan2html)

### Network Policies

- [ ] **k8s-tun** — доступ к pod IP / service IP кластера. *(`script`)* —
      [yzxiu/k8s-tun](https://github.com/yzxiu/k8s-tun)
- [ ] **knetvis** — CLI: визуализация и тестирование NetworkPolicy. *(`release` / `go`)* —
      [samuelarogbonlo/knetvis](https://github.com/samuelarogbonlo/knetvis)

### Testing Tools

- [ ] **kube-burner** — стресс-тестирование кластеров Kubernetes. *(`release`)* —
      [kube-burner/kube-burner](https://github.com/kube-burner/kube-burner)
- [ ] **KWOK** — «Kubernetes WithOut Kubelet»: симуляция тысяч нод и кластеров.
      *(`release`)* — [kubernetes-sigs/kwok](https://github.com/kubernetes-sigs/kwok)
- [ ] **Keploy** — конвертация пользовательского трафика в тест-кейсы и заглушки.
      *(`script`)* — [keploy/keploy](https://github.com/keploy/keploy)
- [ ] **Nighthawk** — HTTP load-tester (constant & adaptive rates, insights).
      *(`release`)* — [layer5io/getnighthawk](https://github.com/layer5io/getnighthawk)
- [ ] **helm-hook-preflight** — CLI: проблемы порядка создания Job/Pod из
      отрендеренного Helm YAML. *(`pipx`)* —
      [ruslan-shaydullin/helm-hook-preflight](https://github.com/ruslan-shaydullin/helm-hook-preflight)

### Observability

- [ ] **cri-tools** (`crictl`) — CLI и валидация для CRI Kubelet. *(`release`)* —
      [kubernetes-sigs/cri-tools](https://github.com/kubernetes-sigs/cri-tools)
- [ ] **kubectl-tree** — плагин kubectl: иерархия объектов по ownerReferences.
      *(`krew`)* — [ahmetb/kubectl-tree](https://github.com/ahmetb/kubectl-tree)
- [ ] **Pumba** — chaos-тестирование, эмуляция сети и стресс для контейнеров.
      *(`release`)* — [alexei-led/pumba](https://github.com/alexei-led/pumba)

### Kubernetes Tools for Specific Cloud

- [ ] **helm-ssm** — получение и инъекция секретов из AWS SSM в Helm.
      *(`script`)* — [totango/helm-ssm](https://github.com/totango/helm-ssm)
- [ ] **Skupper** — мультиоблачная коммуникация для Kubernetes. *(`release`)* —
      [skupper.io](https://skupper.io/)
- [ ] **eksdemo** — «easy button» для изучения и демонстрации Amazon EKS.
      *(`release`)* — [awslabs/eksdemo](https://github.com/awslabs/eksdemo)

### Backup Tools

- [ ] **kube-dump** — бэкап кластера в YAML-манифесты. *(`script`)* —
      [WoozyMasta/kube-dump](https://github.com/WoozyMasta/kube-dump)
- [ ] **cnpg-drill** — проверка бэкапов CloudNativePG (restore + read-only SQL).
      *(`release`)* — [danielgaskins/cnpg-drill](https://github.com/danielgaskins/cnpg-drill)

### Multiple Tools Repo

- [ ] **Pulumi** — IaC на любом языке и любом облаке. *(`script`)* —
      [pulumi/pulumi](https://github.com/pulumi/pulumi)
- [ ] **Colima** — контейнерные рантаймы на macOS (и Linux) с минимальной
      настройкой. *(`release`)* — [abiosoft/colima](https://github.com/abiosoft/colima)
- [ ] **korb** — перенос PVC между StorageClass и namespace. *(`release`)* —
      [BeryJu/korb](https://github.com/BeryJu/korb)

### Cost Optimisation

- [ ] **KBOM** — Kubernetes Bill of Materials. *(`release` / `go`)* —
      [rad-security/kbom](https://github.com/rad-security/kbom)
- [ ] **Burn** — CLI: анализ стоимости k8s по namespace, реальные цены облака,
      AI-рекомендации. *(`release`)* — [tanrikuluozlem/burn](https://github.com/tanrikuluozlem/burn)
- [ ] **Patchloom** — agent-friendly поиск и правка структурированного кода/конфигов.
      *(`release`)* — [patchloom/patchloom](https://github.com/patchloom/patchloom)

### Artificial Intelligence

- [ ] **kopilot** — AI-ассистент Kubernetes. *(`release` / `go`)* —
      [knight42/kopilot](https://github.com/knight42/kopilot)
- [ ] **kopylot** — AI-ассистент для разработчиков Kubernetes. *(`pipx`)* —
      [avsthiago/kopylot](https://github.com/avsthiago/kopylot)
- [ ] **aiac** — генератор k8s-манифестов и пакетов через AI. *(`go` / `release`)* —
      [gofireflyio/aiac](https://github.com/gofireflyio/aiac)
- [ ] **mico** — AI-помощник kubectl. *(`go`)* —
      [tahtaciburak/mico](https://github.com/tahtaciburak/mico)
- [ ] **kubectl-assistant** — генерация kubectl-команд через GPT3 API. *(`pipx`)* —
      [dkoshkin/kubectl-assistant](https://github.com/dkoshkin/kubectl-assistant)
- [ ] **kubernetes-mcp-server** — MCP-сервер для Kubernetes/OpenShift.
      *(`release`)* — [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)
- [ ] **kubectl-mcp-server** — MCP-сервер Kubernetes для AI-инструментов.
      *(`npm`)* — [rohitg00/kubectl-mcp-server](https://github.com/rohitg00/kubectl-mcp-server)
- [ ] **k8s-mcp-server** — MCP-сервер: безопасный запуск kubectl/helm/istioctl/argocd.
      *(`pipx`)* — [alexei-led/k8s-mcp-server](https://github.com/alexei-led/k8s-mcp-server)

### Clients

- [ ] **DevPod** — client-only инструмент: воспроизводимые dev-окружения по
      `devcontainer.json` на любом бэкенде. *(`release`)* —
      [loft-sh/devpod](https://github.com/loft-sh/devpod)

### Cleanup

- [ ] **CRI-Purge** — очистка кэшированных CRI-образов Kubernetes.
      *(`script`)* — [reefland/cri-purge](https://github.com/reefland/cri-purge)
- [ ] **Kubetrim** — CLI: автоочистка `KUBECONFIG`. *(`release` / `go`)* —
      [alexellis/kubetrim](https://github.com/alexellis/kubetrim)

### Non-Categorize

- [ ] **Function** — CLI для работы с функциями (function.io). *(`release`)* —
      [functionio/function](https://github.com/functionio/function)
- [ ] **Alterant** — простой модификатор Kubernetes-конфигурации. *(`release`)* —
      [cloud66-oss/alterant](https://github.com/cloud66-oss/alterant)
- [ ] **kpt** — управление, кастомизация и применение k8s-конфигов.
      *(`release`)* — [GoogleContainerTools/kpt](https://github.com/GoogleContainerTools/kpt)
- [ ] **yupd** — обновление YAML-файлов «по-GitOps». *(`release`)* —
      [srozange/yupd](https://github.com/srozange/yupd)
- [ ] **kube-job** — запуск одноразовых job в Kubernetes из CLI. *(`release`)* —
      [h3poteto/kube-job](https://github.com/h3poteto/kube-job)
- [ ] **Scaphandre** — агент метрик энергопотребления. *(`cargo` / `apt`)* —
      [hubblo-org/scaphandre](https://github.com/hubblo-org/scaphandre)

---

## 2. Кластерные операторы/контроллеры и платформы — не для ноутбука

Ставятся **в кластер** (Helm/манифест/оператор) или как self-hosted сервис,
а не «на ноутбук» одной командой. Держим как справочник — если понадобится
поднять в кластере, а не локально. Дубли с [`TODO.md`](TODO.md) (раздел «Не для
локальной установки») сюда уже не попали. Способ установки не указываем:
зависит от кластера (обычно Helm-чарт или манифест).

**Pods**

- kube-s3 — Kubernetes pods used shared S3 storage. — [github.com/freegroup/kube-s3](https://github.com/freegroup/kube-s3)

**Cluster Management**

- OpenKruise/Kruise — Automate application workloads management on Kubernetes. — [github.com/openkruise/kruise](https://github.com/openkruise/kruise)
- Portainer — Portainer внутри окружения Kubernetes. — [github.com/portainer/k8s](https://github.com/portainer/k8s)
- Kube-ops-view — Kubernetes Operational View - read-only system dashboard for multiple K8s clusters. — [github.com/hjacobs/kube-ops-view](https://github.com/hjacobs/kube-ops-view)
- Metalk8s — An opinionated Kubernetes distribution with a focus on long-term on-prem deployments. — [github.com/scality/metalk8s](https://github.com/scality/metalk8s)
- Clusterman — Cluster Autoscaler for Kubernetes and Mesos. — [github.com/yelp/clusterman](https://github.com/yelp/clusterman)
- Cert-manager — Automatically provision and manage TLS certificates. — [github.com/jetstack/cert-manager](https://github.com/jetstack/cert-manager)
- Goldilocks — Get your resource requests "Just Right". — [github.com/fairwindsops/goldilocks](https://github.com/fairwindsops/goldilocks)
- katafygio — Dump, or continuously backup Kubernetes objects as yaml files in git. — [github.com/bpineau/katafygio](https://github.com/bpineau/katafygio)
- Rancher — Complete container management platform. — [github.com/rancher/rancher](https://github.com/rancher/rancher)
- Sealed Secrets — A Kubernetes controller and tool for one-way encrypted Secrets. — [github.com/bitnami-labs/sealed-secrets](https://github.com/bitnami-labs/sealed-secrets)
- KEDA — Event-driven autoscaler for Kubernetes. — [https://keda.sh/](https://keda.sh/)
- Gardener — Deliver fully-managed clusters at scale everywhere with your own Kubernetes-as-a-Service. — [https://gardener.cloud/](https://gardener.cloud/)
- Kamaji — Build and operate Kubernetes at scale with a fraction of operational burden. — [github.com/clastix/kamaji](https://github.com/clastix/kamaji)
- Kudo — declarative approach to building production-grade Kubernetes Operators. — [github.com/kudobuilder/kudo](https://github.com/kudobuilder/kudo)
- node-problem-detector — This is a place for various problem detectors running on the Kubernetes nodes. — [github.com/kubernetes/node-problem-detector](https://github.com/kubernetes/node-problem-detector)
- k0s — k0s is an all-inclusive Kubernetes distribution, which is configured with all of the features needed to build a Kubernetes cluster and packaged as a single binary for ease of use. — [github.com/k0sproject/k0s](https://github.com/k0sproject/k0s)
- k0smotron — Deploy and run Kubernetes control planes powered by k0s on any existing cluster. — [github.com/k0sproject/k0smotron](https://github.com/k0sproject/k0smotron)
- ClusterPedia — The Encyclopedia of Kubernetes clusters. — [github.com/clusterpedia-io/clusterpedia](https://github.com/clusterpedia-io/clusterpedia)
- kubean — 🌱 Kubernetes lifecycle management operator based on kubespray. — [github.com/kubean-io/kubean](https://github.com/kubean-io/kubean)
- punq — Self-hosted Kubernetes workload manager with team collaboration. — [github.com/mogenius/punq](https://github.com/mogenius/punq)
- Claudie — Deploy and operate Kubernetes clusters with each nodepool in a different provider or on-prem. — [github.com/berops/claudie](https://github.com/berops/claudie)
- Kubemarine — Management tool for Kubernetes cluster deployment and maintenance. — [github.com/netcracker/kubemarine](https://github.com/netcracker/kubemarine)
- k8s-duplicator — Kubernetes controller that copies secrets into other namespaces. — [github.com/nick-triller/k8s-duplicator](https://github.com/nick-triller/k8s-duplicator)
- KubeStellar — a flexible solution for challenges associated with multi-cluster configuration management for edge, multi-cloud, and hybrid clou. — [github.com/kubestellar/kubestellar](https://github.com/kubestellar/kubestellar)
- Trolley — Trolley is a multi cloud Kubernetes management system. — [github.com/trolley-mgmt/trolleymgmt](https://github.com/trolley-mgmt/trolleymgmt)
- Kunobi — Rust Kubernetes management from your desktop, with built-in MCP server. — [https://kunobi.ninja](https://kunobi.ninja)
- Cyclops — a powerful user interface for managing and interacting with Kubernetes clusters. — [github.com/cyclops-ui/cyclops](https://github.com/cyclops-ui/cyclops)
- Liqo — an open-source project that enables dynamic and seamless Kubernetes multi-cluster topologies, supporting heterogeneous on-premise, cloud and edge infrastructures. — [github.com/liqotech/liqo](https://github.com/liqotech/liqo)
- Guard — 🔑 Kubernetes Authentication & Authorization WebHook Server. — [github.com/kubeguard/guard](https://github.com/kubeguard/guard)
- cluster-template — A template for deploying a Kubernetes cluster with k3s or Talos. — [github.com/onedr0p/cluster-template](https://github.com/onedr0p/cluster-template)
- Karmada — Open, Multi-Cloud, Multi-Cluster Kubernetes Orchestration. — [github.com/karmada-io/karmada](https://github.com/karmada-io/karmada)
- Kubeadmiral — Multi-Cluster Kubernetes Orchestration. — [github.com/kubewharf/kubeadmiral](https://github.com/kubewharf/kubeadmiral)
- Kubeseal-Webgui — Kubeseal-Webgui is a simple web ui for Bitnami Sealed Secrets. — [github.com/jaydee94/kubeseal-webgui](https://github.com/jaydee94/kubeseal-webgui)
- Klutch — Klutch extends Crossplane to manage resources across multiple Kubernetes clusters. — [github.com/anynines/klutchio](https://github.com/anynines/klutchio)
- Kanvas — A comprehensive suite of collaborative tools for designing, deploying and managing cloud-native infrastructure. — [https://kanvas.new](https://kanvas.new)
- ReleaseRun K8s Deprecation Checker — Scan Kubernetes manifests for deprecated and removed APIs across versions 1.16-1.35, with auto-fix YAML output and upgrade timeline. — [https://releaserun.com/tools/k8s-deprecation-checker/](https://releaserun.com/tools/k8s-deprecation-checker/)
- okd-metal-installer — Ansible-driven bare-metal OKD provisioning via static ISOs and Bootstrap-in-Place. — [github.com/tosin2013/okd-metal-installer](https://github.com/tosin2013/okd-metal-installer)
- KubeStellar Console — CNCF Sandbox multi-cluster Kubernetes dashboard with AI-powered operations, real-time observability, and GitOps-native deploy workflows across edge and cloud clusters. — [github.com/kubestellar/console](https://github.com/kubestellar/console)
- Krust — Native macOS Kubernetes dashboard for resources, logs, YAML, Helm, CRDs, topology, port forwarding, metrics, security checks, and AI diagnostics. — [https://krust.io/](https://krust.io/)

**Cluster with Core CLI tools**

- Bootkube — bootkube - Launch a self-hosted Kubernetes cluster. — [github.com/kubernetes-sigs/bootkube](https://github.com/kubernetes-sigs/bootkube)
- KubeOperator — Run kubectl command in Web Browser. https://kubeoperator.io/. — [github.com/kubeoperator/webkubectl](https://github.com/kubeoperator/webkubectl)
- KubeHelper — KubeHelper - simplifies many daily Kubernetes cluster tasks through a web interface. — [github.com/kubehelper/kubehelper](https://github.com/kubehelper/kubehelper)
- Kueue — Kueue is a set of APIs and controller for job queueing. — [github.com/kubernetes-sigs/kueue](https://github.com/kubernetes-sigs/kueue)
- Autoscaler — autoscaler Autoscaling components for Kubernetes. — [github.com/kubernetes/autoscaler](https://github.com/kubernetes/autoscaler)
- Karpenter — Kubernetes Node Autoscaler built for flexibility, performance, and simplicity. — [github.com/aws/karpenter](https://github.com/aws/karpenter)
- Kured — Kubernetes Reboot Daemon. — [github.com/kubereboot/kured](https://github.com/kubereboot/kured)
- ksync — Sync files between your local system and a kubernetes cluster. — [github.com/ksync/ksync](https://github.com/ksync/ksync)
- fleet — Manage large fleets of Kubernetes clusters. — [github.com/rancher/fleet](https://github.com/rancher/fleet)
- stash — Backup your Kubernetes Stateful Applications. — [github.com/stashed/stash](https://github.com/stashed/stash)
- Descheduler — Descheduler for Kubernetes. — [github.com/kubernetes-sigs/descheduler](https://github.com/kubernetes-sigs/descheduler)
- cloudtty — A Friendly Kubernetes CloudShell (Web Terminal) !. — [github.com/cloudtty/cloudtty](https://github.com/cloudtty/cloudtty)
- Kubernetes Image Puller — Caching images on a cluster by creating a DaemonSet downloading and running the container images on each node. — [github.com/che-incubator/kubernetes-image-puller](https://github.com/che-incubator/kubernetes-image-puller)
- Kubed — Kubed with a intuitive interactive interface within Emacs help us work with our Kubernetes clusters and deployments with the full power of kubectl. — [github.com/eshelyaron/kubed](https://github.com/eshelyaron/kubed)

**Alert and Monitoring**

- Thanos — Highly available Prometheus setup with long term storage capabilities. CNCF Sandbox project. https://thanos.io. — [github.com/thanos-io/thanos](https://github.com/thanos-io/thanos)
- Searchlight — Alerts for Kubernetes. — [github.com/searchlight/searchlight](https://github.com/searchlight/searchlight)
- linkerd2 Monitoring Mixin for Grafana — Grafana dashboards for linkerd2 monitoring and can work in standalone (default) or in multi cluster setup. — [github.com/andrew-waters/linkerd2-mixin](https://github.com/andrew-waters/linkerd2-mixin)
- kuberhaus — Kubernetes resource dashboard with node/pod layout and resource requests. — [github.com/stevelacy/kuberhaus](https://github.com/stevelacy/kuberhaus)
- Kubernetes Job/CronJob Notifier — This tool sends an alert to slack whenever there is a Kubernetes cronJob/Job failure/success. — [github.com/sukeesh/k8s-job-notify](https://github.com/sukeesh/k8s-job-notify)
- Argus — This tool monitors changes in the filesystem on specified paths. — [https://clustergarage.io/argus/docs/overview](https://clustergarage.io/argus/docs/overview)
- Kwatch — monitor & detect crashes in your Kubernetes(K8s) cluster instantly. — [github.com/abahmed/kwatch](https://github.com/abahmed/kwatch)
- Scope — Monitoring, visualisation & management for Docker & Kubernetes. — [github.com/weaveworks/scope](https://github.com/weaveworks/scope)
- KlusterView — Get instant insights on your Kubernetes clusters with our lightweight, plug-and-play performance monitoring tool. — [github.com/oslabs-beta/klusterview](https://github.com/oslabs-beta/klusterview)
- X.509 Certificate Exporter — Watch certificates for expiration in Kubernetes Secrets and control-plane files. Alerts and dashboard available. — [github.com/enix/x509-certificate-exporter](https://github.com/enix/x509-certificate-exporter)
- VpK — VpK - Visually presented Kubernetes View k8s in graphical fashion. — [github.com/k8svisual/vpk](https://github.com/k8svisual/vpk)
- k8s-collector — A Kubernetes Job to collect resources, logs and events from a Kubernetes cluster. — [github.com/gianlucam76/k8s_collector](https://github.com/gianlucam76/k8s_collector)
- WatchAlert — Lightweight cloud-native multi-data source monitoring and alerting engine. — [github.com/opsre/watchalert](https://github.com/opsre/watchalert)

**Logging and Tracing**

- Jaeger — CNCF Jaeger, a Distributed Tracing Platform. — [github.com/jaegertracing/jaeger](https://github.com/jaegertracing/jaeger)
- Kiali — Kiali project, observability for the Istio service mesh. — [github.com/kiali/kiali](https://github.com/kiali/kiali)
- ELK — Elasticsearch, Logstash, Kibana. — [https://github.com/elastic](https://github.com/elastic)
- fluentbit — Fast and Lightweight Log processor and forwarder for Linux, BSD and OSX. — [github.com/fluent/fluent-bit](https://github.com/fluent/fluent-bit)
- Loki — Like Prometheus, but for logs. — [github.com/grafana/loki](https://github.com/grafana/loki)
- k8spacket — packets traffic visualization for kubernetes. — [github.com/k8spacket/k8spacket](https://github.com/k8spacket/k8spacket)
- Otterize network mapper — Map Kubernetes in-cluster traffic and export as text, intents, or an image. — [github.com/otterize/network-mapper](https://github.com/otterize/network-mapper)
- kube-audit-rest — Kubernetes audit logging, when you don't control the control plane. — [github.com/richardoc/kube-audit-rest](https://github.com/richardoc/kube-audit-rest)
- retina — eBPF distributed networking observability tool for Kubernetes. — [github.com/microsoft/retina](https://github.com/microsoft/retina)
- sablier — Starting containers on demand and close automatically when not in use. — [github.com/acouvreur/sablier](https://github.com/acouvreur/sablier)

**Troubleshooting / Debugging**

- Knative Inspect — A light-weight debugging tool for Knative's system components. — [github.com/nimakaviani/knative-inspect](https://github.com/nimakaviani/knative-inspect)
- Kubeman — To find information from Kubernetes clusters, and to investigate issues related to Kubernetes and Istio. — [github.com/walmartlabs/kubeman](https://github.com/walmartlabs/kubeman)
- Koolkits — 🧰 Opinionated, language-specific, batteries-included debug container images for Kubernetes. — [github.com/lightrun-platform/koolkits](https://github.com/lightrun-platform/koolkits)
- KubeSkoop — kubernetes networking diagnose tool for different CNI plug-ins and IAAS providers. — [github.com/alibaba/kubeskoop](https://github.com/alibaba/kubeskoop)
- kdave — The kdave (Kubernetes Deprecated API Versions Exporter) checks for any deprecated or removed apiVersions in the cluster and exports them in a Prometheus metrics format. — [github.com/wayfair-incubator/kdave](https://github.com/wayfair-incubator/kdave)
- kubebuddy — A PowerShell tool for monitoring and managing Kubernetes clusters, supporting JSON, TXT, and HTML output formats with a CLI option for streamlined operations, including health check scoring for cluster assessment. — [github.com/kubedeckio/kubebuddy](https://github.com/kubedeckio/kubebuddy)

**Development Tools/Kit**

- KuberNix — Single dependency Kubernetes clusters for local testing, experimenting and development. — [github.com/saschagrunert/kubernix](https://github.com/saschagrunert/kubernix)
- Dekorate — Java annotation processors for Kubernetes. — [github.com/dekorateio/dekorate](https://github.com/dekorateio/dekorate)
- Aptakube — A modern, lightweight and multi-cluster desktop client for Kubernetes. — [https://aptakube.com](https://aptakube.com)
- Kaniko — Build Container Images In Kubernetes. — [github.com/googlecontainertools/kaniko](https://github.com/googlecontainertools/kaniko)
- eclipse che IDE for k8s — The Kubernetes-Native IDE for Developer Teams. — [github.com/eclipse/che](https://github.com/eclipse/che)
- Gitkube — Build and deploy docker images to Kubernetes using git push. — [github.com/hasura/gitkube](https://github.com/hasura/gitkube)
- Atomix — A Kubernetes toolkit for building distributed applications using cloud native principles. — [github.com/atomix/atomix](https://github.com/atomix/atomix)
- Brigade — Event-driven scripting for Kubernetes. — [github.com/brigadecore/brigade](https://github.com/brigadecore/brigade)
- Spread — Docker to Kubernetes in one comman. — [github.com/redspread/spread](https://github.com/redspread/spread)
- validkube — ValidKube combines the best open-source tools to help ensure Kubernetes YAML best practices, hygiene & security. — [github.com/komodorio/validkube](https://github.com/komodorio/validkube)
- Config Syncer — Config Syncer by AppsCode keeps ConfigMaps and Secrets synchronized across namespaces and/or clusters. — [github.com/kubeops/config-syncer](https://github.com/kubeops/config-syncer)
- Kubernetes ConfigMap Reload — Simple binary to trigger a reload when a Kubernetes ConfigMap is update. — [github.com/jimmidyson/configmap-reload](https://github.com/jimmidyson/configmap-reload)
- RELOADER — A Kubernetes controller to watch changes in ConfigMap and Secrets and do rolling upgrades on Pods. — [github.com/stakater/reloader](https://github.com/stakater/reloader)
- Ingress Monitor Controller — A Kubernetes controller to watch ingresses and create liveness alerts for your apps/microservices in UptimeRobot, StatusCake, Pingdom, etc. — [github.com/stakater/ingressmonitorcontroller](https://github.com/stakater/ingressmonitorcontroller)
- kronic — The simple Kubernetes CronJob Admin UI. — [github.com/mshade/kronic](https://github.com/mshade/kronic)
- Docketeer — Docketeer is a developer-friendly application that provides a single interface for container and network management as well as metric visualization. — [github.com/open-source-labs/docketeer](https://github.com/open-source-labs/docketeer)
- kubernetes-replicator — Kubernetes controller for synchronizing secrets & config maps across namespaces. — [github.com/mittwald/kubernetes-replicator](https://github.com/mittwald/kubernetes-replicator)
- KubeBlocks — open-source control plane that runs and manages databases, message queues and other data infrastructure on K8s. — [github.com/apecloud/kubeblocks](https://github.com/apecloud/kubeblocks)
- sleepcycles — Define sleep & wake up cycles for your Kubernetes resources. — [github.com/rekuberate-io/sleepcycles](https://github.com/rekuberate-io/sleepcycles)
- mogenius — A self-service platform that enables developers to deploy and manage applications on Kubernetes with ease and safety. — [https://mogenius.com](https://mogenius.com)
- Armada — A multi-cluster batch queuing system for high-throughput workloads on Kubernetes. — [github.com/armadaproject/armada](https://github.com/armadaproject/armada)
- kro — Kube Resource Orchestrator (kro) provides a powerful abstraction layer that allows you to define complex multi-resource constructs as reusable components in your applications and systems. — [github.com/awslabs/kro](https://github.com/awslabs/kro)
- crik — crik is a project that aims to provide checkpoint and restore functionality for Kubernetes pods mainly targeted for node shutdown and restart scenarios. — [github.com/qawolf/crik](https://github.com/qawolf/crik)
- Helm Kanvas Snapshot — Plugin that generates a visual snapshot of Helm charts. — [github.com/meshery/helm-kanvas-snapshot](https://github.com/meshery/helm-kanvas-snapshot)
- k8skonf — Write Kubernetes manifests in TypeScript. — [github.com/konfjs/k8skonf](https://github.com/konfjs/k8skonf)
- helm-controller — A simple way to manage helm charts with Custom Resource Definitions in k8s. — [github.com/k3s-io/helm-controller](https://github.com/k3s-io/helm-controller)
- kubesearch.dev — Kubesearch is a searchable index of popular Helm chart releases in categories such as observability, networking, storage, and automation. — [https://kubesearch.dev/](https://kubesearch.dev/)

**Alternative Tools for Development**

- skippbox — A Desktop application for k8s. — [github.com/skippbox/skippbox](https://github.com/skippbox/skippbox)
- Systemk — Systemk is a systemd backend for the virtual-kubelet. Instead of starting containers, you start systemd units. — [github.com/virtual-kubelet/systemk](https://github.com/virtual-kubelet/systemk)
- Otomi — Self-hosted PaaS for Kubernetes. — [github.com/redkubes/otomi-core](https://github.com/redkubes/otomi-core)
- Rainbond — offers a serverless experience, and allows you to easily manage containerized applications without needing to understand Kubernetes. — [github.com/goodrain/rainbond](https://github.com/goodrain/rainbond)
- MyKube — k8s-installer - One-click k8s single-node cluster installation on your own device. — [github.com/guyst16/mykube](https://github.com/guyst16/mykube)
- K8e — K8e 🚀 - Instantly Ready Kubernetes Enterprise Edition. — [github.com/xiaods/k8e](https://github.com/xiaods/k8e)

**Internal Developer Platform**

- OpenChoreo — A complete, open-source internal developer platform (IDP) for Kubernetes that provides high-level abstractions (components, endpoints, environments) to turn developer intent into Kubernetes resources, with a Backstage-powered portal and built-in CI/CD and GitOps. — [github.com/openchoreo/openchoreo](https://github.com/openchoreo/openchoreo)

**CI/CD integration Tools**

- HybridK8s Droid — Intelligence for your favourite Delivery Platform. — [https://hybridk8s.tech/](https://hybridk8s.tech/)
- Devtron — Software Delivery Workflow for Kubernetes. — [github.com/devtron-labs/devtron](https://github.com/devtron-labs/devtron)
- Apollo — Apollo - The logz.io continuous deployment solution over kubernetes. — [github.com/logzio/apollo](https://github.com/logzio/apollo)
- Helm Cabin — Web UI that visualizes Helm releases in a Kubernetes cluster. — [github.com/nick-triller/helm-cabin](https://github.com/nick-triller/helm-cabin)
- flagger — Progressive delivery Kubernetes operator (Canary, A/B Testing and Blue/Green deployments). — [github.com/weaveworks/flagger](https://github.com/weaveworks/flagger)
- Kubeform — Kubernetes CRDs for Terraform providers https://kubeform.com. — [github.com/kubeform/kubeform](https://github.com/kubeform/kubeform)
- Spinnaker — Spinnaker is an open source, multi-cloud continuous delivery platform for releasing software changes with high velocity and confidence. http://www.spinnaker.io/. — [github.com/spinnaker/spinnaker](https://github.com/spinnaker/spinnaker)
- Tekton — A cloud native continuous integration and delivery (CI/CD) solution. — [github.com/tektoncd/pipeline](https://github.com/tektoncd/pipeline)
- Drone — Drone is a Container-Native, Continuous Delivery Platform. — [github.com/harness/drone](https://github.com/harness/drone)
- PipeCD — The One CD for All {applications, platforms, operations}. — [github.com/pipe-cd/pipecd](https://github.com/pipe-cd/pipecd)
- autoapply — Automatically apply changes to a Kubernetes cluster. — [github.com/autoapply/autoapply](https://github.com/autoapply/autoapply)
- mani-diffy — a hierarchy of Argo CD Application templates, renders Kubernetes manifests from the input templates, and posts the rendered files back for the user to review and validate. — [github.com/chime/mani-diffy](https://github.com/chime/mani-diffy)
- gitops-promoter — GitOps Environment Promotion tool that lets you focus on the "what," not the "how". — [github.com/argoproj-labs/gitops-promoter](https://github.com/argoproj-labs/gitops-promoter)

**Security Tools**

- kube2iam — IAM credentials to containers running inside a kubernetes cluster based on annotations. — [github.com/jtblin/kube2iam](https://github.com/jtblin/kube2iam)
- Kyverno — Kubernetes Native Policy Management https://kyverno.io. — [github.com/kyverno/kyverno](https://github.com/kyverno/kyverno)
- kiosk — kiosk office Multi-Tenancy Extension For Kubernetes - Secure Cluster Sharing & Self-Service Namespace Provisioning. — [github.com/kiosk-sh/kiosk](https://github.com/kiosk-sh/kiosk)
- starboard — Kubernetes-native security toolkit. — [github.com/aquasecurity/starboard](https://github.com/aquasecurity/starboard)
- Simulator — Kubernetes Security Training Platform - Focussing on security mitigation. — [github.com/kubernetes-simulator/simulator](https://github.com/kubernetes-simulator/simulator)
- Gangway — An application that can be used to easily enable authentication flows via OIDC for a kubernetes cluster. — [github.com/heptiolabs/gangway](https://github.com/heptiolabs/gangway)
- Falco — The Falco Project - Cloud-Native runtime security. — [https://falco.org/](https://falco.org/)
- Clair — Vulnerability Static Analysis for Containers. — [github.com/quay/clair](https://github.com/quay/clair)
- M9sweeper — Complete Kubernetes Security Platform. — [github.com/m9sweeper/m9sweeper](https://github.com/m9sweeper/m9sweeper)
- Vault-k8s — First-class support for Vault and Kubernetes. — [github.com/hashicorp/vault-k8s](https://github.com/hashicorp/vault-k8s)
- Paralus — Paralus enables controlled, audited access to Kubernetes infrastructure and Zero trust Kubernetes with zero friction. — [github.com/paralus/paralus](https://github.com/paralus/paralus)
- jsPolicy — jsPolicy is a policy engine for Kubernetes that allows you to write policies in JavaScript or TypeScript. — [github.com/pavan-kumar-99/medium-manifests](https://github.com/pavan-kumar-99/medium-manifests)
- kube-rebac-authorizer — Beyond RBAC: Implementing Relation-based Access Control for Kubernetes with OpenFGA. — [github.com/luxas/kube-rebac-authorizer](https://github.com/luxas/kube-rebac-authorizer)
- MTKPI — 🧰 Multi Tool Kubernetes Pentest Image. — [github.com/r0binak/mtkpi](https://github.com/r0binak/mtkpi)
- KubeZoo — a lightweight kubernetes multi-tenancy gateway. — [github.com/kubewharf/kubezoo](https://github.com/kubewharf/kubezoo)
- KubeClarity — KubeClarity is a tool for detection and management of Software Bill Of Materials (SBOM) and vulnerabilities of container images and filesystems. — [github.com/openclarity/kubeclarity](https://github.com/openclarity/kubeclarity)
- Oligo — Observe application behavior to see what your risks really are – and stop breaches before they begin. — [https://www.oligo.security/](https://www.oligo.security/)
- kube-advisor.io — kube-advisor.io lets you detect misconfigurations and best practice violations of your Kubernetes clusters - continuously and in real-time. — [https://kube-advisor.io/](https://kube-advisor.io/)
- Tetragon — eBPF-based Security Observability and Runtime Enforcement for Kubernetes. — [github.com/cilium/tetragon](https://github.com/cilium/tetragon)
- Darkmoon — Open source (GPLv3) autonomous penetration testing platform: 50 specialist agents over MCP drive 50+ offensive tools across Kubernetes, cloud, web, API and Active Directory, with proof of exploitation behind each finding. Self hosted, with a privacy gateway that tokenizes sensitive target values before any cloud model sees them. — [github.com/ascit31/dark-moon](https://github.com/ascit31/dark-moon)

**Network Policies**

- Calico — Cloud native connectivity and network policy. — [github.com/projectcalico/calico](https://github.com/projectcalico/calico)
- kokotap — Tools for kubernetes pod network tapping. — [github.com/redhat-nfvpe/kokotap](https://github.com/redhat-nfvpe/kokotap)
- Submariner — Connect all your Kubernetes clusters, no matter where they are in the worl. — [github.com/submariner-io/submariner](https://github.com/submariner-io/submariner)
- egress-operator — An operator to produce egress gateway pods and control access to them with network policies. — [github.com/monzo/egress-operator](https://github.com/monzo/egress-operator)
- Kilo (k8s + wg) — A multi-cloud network overlay built on WireGuard and designed for Kubernetes. — [github.com/squat/kilo](https://github.com/squat/kilo)
- Spiderpool — underlay network solution of cloud native, for bare metal, VM and public clou. — [github.com/spidernet-io/spiderpool](https://github.com/spidernet-io/spiderpool)
- LoxiLB — eBPF based cloud-native load-balancer. Powering K8s,Edge,5G,IoT,XaaS Apps. — [github.com/loxilb-io/loxilb](https://github.com/loxilb-io/loxilb)
- Network Policy API — A further work involving Kubernetes network security beyond the NetworkPolicy v1 resource. — [github.com/kubernetes-sigs/network-policy-api](https://github.com/kubernetes-sigs/network-policy-api)
- KubeIP — Assign static public IPs to Kubernetes nodes (GKE, EKS). — [github.com/doitintl/kubeip](https://github.com/doitintl/kubeip)
- MetalLB — MetalLB is a load-balancer implementation for bare metal Kubernetes clusters, using standard routing protocols. — [github.com/metallb/metallb](https://github.com/metallb/metallb)
- kube-vip — Kubernetes Control Plane Virtual IP and Load-Balancer. — [github.com/kube-vip/kube-vip](https://github.com/kube-vip/kube-vip)

**Testing Tools**

- Testkube — Kubernetes-native testing framework for testers and developers. — [github.com/kubeshop/testkube](https://github.com/kubeshop/testkube)
- Network bandwidth and load testingd - — Test suite for Kubernetes. — [github.com/mrahbar/k8s-testsuite](https://github.com/mrahbar/k8s-testsuite)
- test-infrad — Test infrastructure for the Kubernetes project. — [github.com/kubernetes/test-infra](https://github.com/kubernetes/test-infra)
- Litmusd — Cloud-Native Chaos Engineering; Kubernetes-Native Chaos Engineering; Chaos Engineering for Kubernetes. — [github.com/litmuschaos/litmus](https://github.com/litmuschaos/litmus)
- Tracetest — Generate end-to-end and integration tests automatically from your OpenTelemetry traces. — [github.com/kubeshop/tracetest](https://github.com/kubeshop/tracetest)
- Microcks — Kubernetes native tool for mocking and testing API and micro-services. — [github.com/microcks/microcks](https://github.com/microcks/microcks)
- perf-tests — Performance tests and benchmarks. — [github.com/kubernetes/perf-tests](https://github.com/kubernetes/perf-tests)
- Jupyter Notebook Validator Operator — Kubernetes operator for validating Jupyter notebooks in MLOps (Papermill, golden notebooks). — [github.com/tosin2013/jupyter-notebook-validator-operator](https://github.com/tosin2013/jupyter-notebook-validator-operator)
- Omen — Lightweight Kubernetes chaos engineering operator with transparent target selection and optional manual approval. — [github.com/k-krew/omen](https://github.com/k-krew/omen)

**Service Mesh**

- Istio — Connect, secure, control, and observe services. — [github.com/istio/istio](https://github.com/istio/istio)
- Traefik — The Cloud Native Edge Router. — [github.com/containous/traefik](https://github.com/containous/traefik)
- NGINX Ingress Controller — NGINX and NGINX Plus Ingress Controllers for Kubernetes. — [github.com/nginxinc/kubernetes-ingress](https://github.com/nginxinc/kubernetes-ingress)
- Autopilot — THE SERVICE MESH SDK. — [https://docs.solo.io/autopilot/latest](https://docs.solo.io/autopilot/latest)
- linkerd-config — A Kubernetes controller that knows how to reconcile the Linkerd configuration. — [github.com/ihcsim/linkerd-config](https://github.com/ihcsim/linkerd-config)
- OSM — Open Service Mesh (OSM) is a lightweight, extensible, cloud native service mesh. — [github.com/openservicemesh/osm](https://github.com/openservicemesh/osm)
- Gloo Mesh — The Service Mesh Orchestration Platform. — [github.com/solo-io/gloo-mesh](https://github.com/solo-io/gloo-mesh)
- Contour — High performance ingress controller for Kubernetes. — [https://projectcontour.io/](https://projectcontour.io/)
- Kusk Gateway — OpenAPI-driven Ingress Controller for Kubernetes. — [https://kusk.io/kusk-gateway](https://kusk.io/kusk-gateway)
- MerBridge — Use eBPF to speed up your Service Mesh like crossing an Einstein-Rosen Bridge. — [github.com/merbridge/merbridge](https://github.com/merbridge/merbridge)

**Observability**

- kube-state-metrics — Add-on agent to generate and expose cluster-level metrics. — [github.com/kubernetes/kube-state-metrics](https://github.com/kubernetes/kube-state-metrics)
- Sloop — Kubernetes History Visualization. — [github.com/salesforce/sloop](https://github.com/salesforce/sloop)
- chaoskube — chaoskube periodically kills random pods in your Kubernetes cluster. — [github.com/linki/chaoskube](https://github.com/linki/chaoskube)
- BotKube — Helps you monitor your Kubernetes cluster(s), debug critical deployments and gives recommendations for standard practices. — [https://www.botkube.io/](https://www.botkube.io/)
- Kubestone — Kubestone is a benchmarking Operator that can evaluate the performance of Kubernetes installations. — [https://kubestone.io/en/latest](https://kubestone.io/en/latest)
- Chaos Mesh — A Chaos Engineering Platform for Kubernetes. — [github.com/pingcap/chaos-mesh](https://github.com/pingcap/chaos-mesh)
- Lemur — LEMUR: Observability and Context. — [github.com/turbonomic/lemur](https://github.com/turbonomic/lemur)
- kubernetes-event-exporter — Export Kubernetes events to multiple destinations with routing and filtering. — [github.com/opsgenie/kubernetes-event-exporter](https://github.com/opsgenie/kubernetes-event-exporter)
- Kubevious — Kubevious is an app-centric assurance, validation, and introspection platform for Kubernetes. — [https://kubevious.io/](https://kubevious.io/)
- Grafana Tempo — Grafana Tempo is a high volume, minimal dependency distributed tracing backen. — [github.com/grafana/tempo](https://github.com/grafana/tempo)
- kubeinvaders — Gamified Chaos Engineering Tool for Kubernetes. — [github.com/lucky-sideburn/kubeinvaders](https://github.com/lucky-sideburn/kubeinvaders)
- Karpor — Intelligence for Kubernetes - world's most promising Kubernetes Visualization Tool for Developer and Platform Engineering teams. — [github.com/kusionstack/karpor](https://github.com/kusionstack/karpor)
- DeepFlow — eBPF Observability - Distributed Tracing and Profiling for Kubernetes. — [github.com/deepflowio/deepflow](https://github.com/deepflowio/deepflow)
- Alaz — Advanced eBPF Agent for Kubernetes Observability - monitor K8s service interactions and performance metrics. — [github.com/getanteon/alaz](https://github.com/getanteon/alaz)
- OpenTelemetry — High-quality, ubiquitous, and portable telemetry to enable effective observability. — [https://opentelemetry.io/](https://opentelemetry.io/)
- Robusta — Kubernetes observability and automation, with an awesome Prometheus integration. — [github.com/robusta-dev/robusta](https://github.com/robusta-dev/robusta)

**Machine Learning/Deep Learning**

- Nos — nos is an open-source platform to efficiently run AI workloads on Kubernetes, increasing GPU utilization and reducing infrastructure and operational costs. — [github.com/nebuly-ai/nos](https://github.com/nebuly-ai/nos)
- Kubeflow — Machine Learning Toolkit for Kubernetes. — [github.com/kubeflow/kubeflow](https://github.com/kubeflow/kubeflow)
- Volcano — A Kubernetes Native Batch System. — [github.com/volcano-sh/volcano](https://github.com/volcano-sh/volcano)
- piqc — GPU waste scanner for Kubernetes inference clusters — detects tier misplacement, idle capacity, OOM risk, and CPU:GPU imbalance. — [github.com/paralleliq/piqc](https://github.com/paralleliq/piqc)

**Compute Edge Tools**

- KubeEdge — Kubernetes Native Edge Computing Framework. — [github.com/kubeedge/kubeedge](https://github.com/kubeedge/kubeedge)
- OpenYurt — Extending your native Kubernetes to edge. — [github.com/openyurtio/openyurt](https://github.com/openyurtio/openyurt)
- Akri — A Kubernetes Resource Interface for the Edge. — [github.com/project-akri/akri](https://github.com/project-akri/akri)
- SuperEdge — An edge-native container management system for edge computing. — [github.com/superedge/superedge](https://github.com/superedge/superedge)
- Keylime — A CNCF Project to Bootstrap & Maintain Trust on the Edge / Cloud and IoT. — [github.com/keylime/keylime](https://github.com/keylime/keylime)
- Microk8s — A small, fast, single-package Kubernetes for developers, IoT and edge. — [github.com/canonical/microk8s](https://github.com/canonical/microk8s)
- Shifu — Kubernetes native IoT software development framework. — [github.com/edgenesis/shifu](https://github.com/edgenesis/shifu)
- kairos — The immutable Linux meta-distribution for edge Kubernetes. — [github.com/kairos-io/kairos](https://github.com/kairos-io/kairos)

**Kubernetes Tools for Specific Cloud**

- Kubernetes on AWS (kube-aws) — A command-line tool to declaratively manage Kubernetes clusters on AWS. — [github.com/kubernetes-incubator/kube-aws](https://github.com/kubernetes-incubator/kube-aws)
- Draft — A tool for developers to create cloud-native applications on Kubernetes. — [github.com/azure/draft](https://github.com/azure/draft)
- AWS workshop for kubernetes — AWS workshop for kubernetes. — [github.com/aws-samples/aws-workshop-for-kubernetes](https://github.com/aws-samples/aws-workshop-for-kubernetes)
- Sealos — A Kubernetes distribution, a general-purpose Cloud Operating System designed for managing cloud-native applications. — [github.com/labring/sealos](https://github.com/labring/sealos)
- Porter — Kubernetes-powered PaaS that runs in your own clou. — [github.com/porter-dev/porter](https://github.com/porter-dev/porter)
- Kubicorn — Create, manage, snapshot, and scale Kubernetes infrastructure in the public clou. — [github.com/kubicorn/kubicorn](https://github.com/kubicorn/kubicorn)

**Storage Providers**

- ChubaoFS — distributed file system and object storage. — [github.com/chubaofs/chubaofs](https://github.com/chubaofs/chubaofs)
- Longhorn — Cloud-Native distributed block storage built on and for Kubernetes. — [github.com/longhorn/longhorn](https://github.com/longhorn/longhorn)
- OpenEBS — Kubernetes native - hyperconverged block storage with multiple storage engines. — [github.com/openebs/openebs](https://github.com/openebs/openebs)
- Rook — Storage Orchestration for Kubernetes. — [github.com/rook/rook](https://github.com/rook/rook)
- SeaweedFS — Distributed file system supports read-write many volumes. — [github.com/chrislusf/seaweedfs](https://github.com/chrislusf/seaweedfs)
- TiKV — Distributed transactional key-value database. — [github.com/tikv/tikv](https://github.com/tikv/tikv)
- TopoLVM — Capacity-aware CSI plugin for Kubernetes. — [github.com/topolvm/topolvm](https://github.com/topolvm/topolvm)
- velero — Backup and migrate Kubernetes applications and their persistent volumes. — [github.com/vmware-tanzu/velero](https://github.com/vmware-tanzu/velero)
- Vitess — Vitess is a database clustering system for horizontal scaling of MySQL. — [github.com/vitessio/vitess](https://github.com/vitessio/vitess)
- kaDalu — A lightweight Persistent storage solution for Kubernetes / OpenShift using GlusterFS in backgroun. — [github.com/kadalu/kadalu](https://github.com/kadalu/kadalu)
- hwameistor — Hwameistor is an HA local storage system for cloud-native stateful workloads. — [github.com/hwameistor/hwameistor](https://github.com/hwameistor/hwameistor)
- simplyblock — Simplyblock is a distributed block storage engine for Kubernetes persistent volumes. — [https://www.simplyblock.io/kubernetes-storage](https://www.simplyblock.io/kubernetes-storage)
- Veritas infoscale — Manage critical data and applications in Kubernetes environments. — [https://www.veritas.com/availability/infoscale-kubernetes](https://www.veritas.com/availability/infoscale-kubernetes)

**Backup Tools**

- VolSync — Asynchronous data replication for Kubernetes volumes. — [github.com/backube/volsync](https://github.com/backube/volsync)
- Reflector — Custom Kubernetes controller that can be used to replicate secrets, configmaps and certificates. — [github.com/emberstack/kubernetes-reflector](https://github.com/emberstack/kubernetes-reflector)
- SnapScheduler — Scheduled snapshots for Kubernetes persistent volumes. — [github.com/backube/snapscheduler](https://github.com/backube/snapscheduler)
- K8up Backup Operator — K8up is a Kubernetes backup operator based on Restic that will handle PVC and application backups on a Kubernetes or OpenShift cluster. — [github.com/k8up-io/k8up](https://github.com/k8up-io/k8up)
- KubeSnapIt — KubeSnapIt is a PowerShell tool that simplifies managing your Kubernetes resources by providing functionalities for taking snapshots, comparing resources, and restoring them. For Windows, MacOS, and Linux. — [github.com/kubedeckio/kubesnapit](https://github.com/kubedeckio/kubesnapit)

**Multiple Tools Repo**

- Chaos Toolkit Kubernetes Support — Kubernetes driver extension of the Chaos Toolkit probes and actions API. — [github.com/chaostoolkit/chaostoolkit-kubernetes](https://github.com/chaostoolkit/chaostoolkit-kubernetes)
- Gvisor — Application Kernel for Containers. — [github.com/google/gvisor](https://github.com/google/gvisor)
- Krateo Platformops — Open source platform to manage infrastructure, resources and applications via YAML files. — [github.com/krateoplatformops/krateo](https://github.com/krateoplatformops/krateo)
- Kamus — An open source, git-ops, zero-trust secret encryption and decryption solution for Kubernetes applications. — [github.com/soluto/kamus](https://github.com/soluto/kamus)
- KubeDeck Launcher — KubeDeck Launcher is a PowerShell-based UI that integrates KubeTidy and KubeSnapIt, allowing you to manage Kubernetes resources through a single interface. — [github.com/kubedeckio/kubedeck](https://github.com/kubedeckio/kubedeck)

**Cost Optimisation**

- Kubecost — Cross-cloud cost allocation models for workloads running on Kubernetes. — [github.com/kubecost/cost-model](https://github.com/kubecost/cost-model)
- PerfectScale — Reduce Kubernetes costs while improving performance and resilience with data-driven intelligence built for continuous optimization. — [https://www.perfectscale.io](https://www.perfectscale.io)
- kube-green — kube-green is a simple k8s addon that automatically shuts down (some of) your resources when you don't need them. — [github.com/kube-green/kube-green](https://github.com/kube-green/kube-green)
- kepler — Kepler (Kubernetes-based Efficient Power Level Exporter) uses eBPF to probe performance counters and other system stats, use ML models to estimate workload energy consumption based on these stats, and exports them as Prometheus metrics. — [https://github/stars/sustainable-computing-io/kepler](https://github/stars/sustainable-computing-io/kepler)
- StormForge — Autonomous Rightsizing for Kubernetes Workloads. — [https://stormforge.io](https://stormforge.io)
- Attune — Kubernetes operator for safe, in-place pod resource right-sizing using Prometheus metrics. — [github.com/attune-io/attune](https://github.com/attune-io/attune)
- Paralleliq — Model-aware GPU fleet optimization layer for AI infrastructure — detects GPU waste, quantifies cost impact, and delivers actionable recommendations with approval workflows. — [https://www.paralleliq.ai](https://www.paralleliq.ai)
- GoKubeDownscaler — GoKubeDownscaler reduces Kubernetes cluster costs by scaling workloads to zero during off-hours such as nights, weekends and holidays https://kube-downscaler.io. — [github.com/caas-team/gokubedownscaler](https://github.com/caas-team/gokubedownscaler)
- NudgeBee — Self-hosted CloudOps platform that rightsizes Kubernetes workloads and optimizes multi-cloud cost across AWS, Azure and GCP. — [github.com/nudgebee/nudgebee](https://github.com/nudgebee/nudgebee)

**Function as a Service FaaS**

- Knative Serving — Kubernetes-based, scale-to-zero, request-driven compute. — [github.com/knative/serving](https://github.com/knative/serving)
- OpenFaaS — OpenFaaS - Serverless Functions Made Simple. — [github.com/openfaas/faas](https://github.com/openfaas/faas)
- Kubeless — Kubernetes Native Serverless Framework. — [github.com/vmware-archive/kubeless](https://github.com/vmware-archive/kubeless)
- Fission — Fast and Simple Serverless Functions for Kubernetes. — [github.com/fission/fission](https://github.com/fission/fission)
- Nuclio — High-Performance Serverless event and data processing platform. — [github.com/nuclio/nuclio](https://github.com/nuclio/nuclio)
- Apache OpenServerless — Serverless environment based on Apache OpenWhisk with lots of integrated services. — [github.com/apache/openserverless](https://github.com/apache/openserverless)

**Artificial Intelligence**

- kserve — Standardized Serverless ML Inference Platform on Kubernetes. — [github.com/kserve/kserve](https://github.com/kserve/kserve)
- Agentkube — an AI-powered Kubernetes IDE for Developers and DevOps teams. — [github.com/agentkube/agentkube](https://github.com/agentkube/agentkube)
- KubeGraf — Local-first AI SRE Platform for Kubernetes - detects incidents, explains root causes with evidence, and safely previews fixes without SaaS lock-in. — [https://kubegraf.io](https://kubegraf.io)

**Caching**

- kube-fledged — A kubernetes operator for creating and managing a cache of container images directly on the cluster worker nodes. — [github.com/senthilrch/kube-fledged](https://github.com/senthilrch/kube-fledged)
- Tugger — Kubernetes Admission Webhook to enforce pulling of Docker images from the private registry. — [github.com/jainishshah17/tugger](https://github.com/jainishshah17/tugger)
- kube-image-keeper (kuik) — kuik is a container image caching system for Kubernetes. — [github.com/enix/kube-image-keeper](https://github.com/enix/kube-image-keeper)
- harbor — A trusted cloud native registry project that stores,caches, signs, and scans content. — [github.com/goharbor/harbor](https://github.com/goharbor/harbor)

**Clients**

- Go Client for Kubernetes — Official Golang client library for kubernetes. — [github.com/kubernetes-client/go](https://github.com/kubernetes-client/go)
- Java Client for Kubernetes — Official Java client library for kubernetes. — [github.com/kubernetes-client/java](https://github.com/kubernetes-client/java)
- Python client for kubernetes — Official Python client library for kubernetes. — [github.com/kubernetes-client/python](https://github.com/kubernetes-client/python)
- Javascript Client for Kubernetes — Official Javascript client library for kubernetes. — [github.com/kubernetes-client/javascript](https://github.com/kubernetes-client/javascript)
- csharp Client for Kubernetes — Official dotnet client library for kubernetes. — [github.com/kubernetes-client/csharp](https://github.com/kubernetes-client/csharp)
- Dapr — Dapr is a portable, event-driven, runtime for building distributed applications across cloud and edge. — [github.com/dapr/dapr](https://github.com/dapr/dapr)

**Cleanup**

- kube-fencing — cleaning resources from the failed nodes that's blocks any further operation and recovery. — [github.com/kvaps/kube-fencing](https://github.com/kvaps/kube-fencing)
- k8s-cleaner — Cleaner is a Kubernetes controller that helps you maintain a clean and efficient Kubernetes environment by proactively identifying, removing, or updating stale resources. — [github.com/gianlucam76/k8s-cleaner](https://github.com/gianlucam76/k8s-cleaner)
- Pred8tor — Pred8tor is a simple tool to clean up K8s objects that were tagged for expiration. — [github.com/pavelzag/pred8tor](https://github.com/pavelzag/pred8tor)
- KubeTidy — KubeTidy is a PowerShell tool that helps you manage your Kubernetes kubeconfig files by removing unreachable clusters, along with their associated users and contexts. For Windows, Macos, and Linux. — [github.com/kubedeckio/kubetidy](https://github.com/kubedeckio/kubetidy)

**Non-Categorize**

- Rudr — A Kubernetes implementation of the Open Application Model specification. — [github.com/oam-dev/rudr](https://github.com/oam-dev/rudr)
- Keel — Kubernetes Operator to automate Helm, DaemonSet, StatefulSet & Deployment updates. — [https://keel.sh/](https://keel.sh/)
- Cabin, the mobile app for Kubernetes - — The Mobile Dashboard for Kubernetes. — [github.com/bitnami-labs/cabin](https://github.com/bitnami-labs/cabin)
- BUCK — Brigade Universal Controller for Kubernetes. — [github.com/brigadecore/buck](https://github.com/brigadecore/buck)
- capsule — Capsule helps to implement a multi-tenancy and policy-based environment in your Kubernetes cluster. — [github.com/clastix/capsule](https://github.com/clastix/capsule)
- KubeSlice — KubeSlice enables Kubernetes pods and services to communicate seamlessly across clusters, clouds, edges, and data centers by creating logical application boundaries known as Slices. — [https://github.com/kubeslice](https://github.com/kubeslice)
- routernetes — Use Kubernetes to make a home router!. — [github.com/routernetes/routernetes](https://github.com/routernetes/routernetes)
- Symbiosis — Symbiosis is a cloud service provider optimized for Kubernetes. Launch clusters in <2 min with better tools and lower costs. — [https://symbiosis.host/](https://symbiosis.host/)
- Projectsveltos — Kubernetes add-on controller for hundreds of clusters that automatically discovers and classifies clusters, making it easy to manage add-ons at scale. — [github.com/projectsveltos/addon-controller](https://github.com/projectsveltos/addon-controller)
- MayFly — Mayfly is a Kubernetes operator that enables you to create temporary resources on the cluster that will expire after a certain period of time. — [github.com/nccloud/mayfly](https://github.com/nccloud/mayfly)
- paster — This program base64 encodes a file, then pastes chunks it into an activated window using 'Ctrl+V' in the po. — [github.com/bufuuu/paster](https://github.com/bufuuu/paster)
- Mirror — A Kubernetes ConfigMap and Secrets reflector with a strong emphasis on security and performance. — [github.com/lab42/mirror](https://github.com/lab42/mirror)

---

## 3. Уже описано в репозитории — НЕ дублировать

При дедупликации 2026-10-18 из этого файла исключены инструменты kubetools,
которые уже есть в других трекерах. Сверка со всем репозиторием (по ссылке на
репозиторий проекта, а также по имени в `**bold**`/`` `code` ``/строке
«- Имя —»), полный список совпадений — **59**:

- **Уже устанавливает плейбук (роль `kubernetes`, `README.md`) — 8:**
  `k9s`, `kubectx`+`kubens`, `kubecolor`, `stern`, `ktop`, `kube-capacity`,
  `kubespy`, `flux`.
- **Уже есть в [`TODO-awesome-github.md`](TODO-awesome-github.md) — 16:**
  `headlamp`, `pluto`, `kubeview`, `rbac-wizard`, `kftray`, `kubebox`,
  `kubectl-browse-pvc`, `goldpinger`, `netfetch`, `kbld`, `kubestriker`,
  `KubeHound`, `kondense`, `radar`, `krew`, `opencost`.
- **Уже есть в [`TODO-trend-github.md`](TODO-trend-github.md) — 23:**
  `k8sgpt`, `kubectl-ai`, `kind`, `vcluster`, `glasskube`, `kubeshark`, `tilt`,
  `skaffold`, `terrascan`, `trivy`, `kube-linter`, `kubescape`, `kube-score`,
  `popeye`, `kubent`, `mirrord`, `telepresence`, `terraform(er)`, `helmfile`,
  `devspace`, `k3d`, `k6`, `kubeval` (сам kubeval архивирован — актуален
  `kubeconform` из этого же файла).
- **Уже отнесено к серверным/платформенным в [`TODO.md`](TODO.md) — 12:**
  `prometheus`, `grafana`, `cilium`, `argo-cd` (сам сервер; локальный CLI —
  в разделе 1 выше), `apisix`, `kong`, `kubesphere`, `meshery`, `kubewatch`,
  `kuberhealthy`, `pixie`, `odigos`.

Отдельные варианты написания в kubetools считались совпадением с уже описанным:
`k6d` → `k6`, `kube-scored` → `kube-score`, длинное имя `Tilt: …` → `tilt`.
Мета-репозиторий `k14s` не выносим отдельно: его компоненты (`kapp` — раздел 1,
`ytt`/`kbld` — в других файлах) уже учтены.

> Правило дедупликации одинаково для всех `TODO-*.md`: каждый кандидат описан
> ровно в одном файле-трекере. Если инструмент kubetools уже есть в другом
> файле — он остаётся там.

---

## 4. Что дальше

- [ ] Пройтись по разделу 1 и отметить `[x]` то, что реально нужно в плейбуке
      (приоритет — у кого есть `.deb` или готовый `linux-amd64` архив: ставится
      ролью `binaries` одной строкой).
- [ ] Для отобранного — уточнить способ установки через редирект
      `releases/latest` (как в `README.md`) и вынести в соответствующую роль
      (`kubernetes`, `shell` и т.д.).
- [ ] Периодически сверять новый список kubetools с репозиторием и дополнять
      этот файл.

Связанное: [`TODO-awesome-github.md`](TODO-awesome-github.md) — канонический
список локальных кандидатов; [`TODO-trend-github.md`](TODO-trend-github.md) —
тренды 2026; [`TODO.md`](TODO.md) — что уже ставится, что серверное и что
является подборками.
