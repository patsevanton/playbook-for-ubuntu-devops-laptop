# TODO-server-apps: серверные приложения, утилиты и Helm-чарты — не на ноутбук

Список серверных приложений, сервисов и Helm-чартов, которые плейбук
**не устанавливает**. Плейбук настраивает рабочий ноутбук: бинарники в
`/usr/local/bin`, конфиги в `~/.config`, несколько CLI-утилит и агентов.
Всё, что ниже, живёт иначе — как долгоживущий сервис с портами, доменом,
хранилищем, базой и пользователями: Docker Compose, Helm-чарт, Deployment в
кластере или отдельная ВМ.

Ставить такое на ноутбук «одной командой» бессмысленно: без внешних
зависимостей (БД, S3, DNS, TLS) оно не заработает, а место и обновления
требует как сервер. Если понадобится — это отдельный хост, отдельный
`docker compose`/Helm-релиз и отдельная эксплуатация.

Как читать:

- `- [ ]` — не ставим и не планируем в роли плейбука;
- в скобках — чем апстрим распространяется: `helm` — Helm-чарт, `образ` —
  контейнерный образ/манифесты, `сервис` — self-hosted приложение с
  установкой по документации;
- ссылка ведёт на репозиторий проекта.

Смежные файлы с другими причинами отказа:

- [`TODO-node-tools.md`](TODO-node-tools.md) — агенты на ноды (DaemonSet,
  static Pod);
- [`TODO-kubetools.md`](TODO-kubetools.md) — остальные кластерные сервисы из
  подборки kubetools;
- [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md) — у проекта нет
  linux-amd64 бинарника;
- [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md) —
  бинарник не запускается на Ubuntu 26.04;
- [`not-install.md`](not-install.md) — осознанный отказ из-за бага/issue.

---

## 1. Наблюдаемость, метрики и SLO

- [ ] **grafana** — дашборды и визуализация метрик/логов. *(`helm`)* —
      [grafana/grafana](https://github.com/grafana/grafana)
- [ ] **prometheus** — сбор и хранение метрик. *(`helm`)* —
      [prometheus/prometheus](https://github.com/prometheus/prometheus)
- [ ] **alertmanager** — маршрутизация алертов. *(`helm`)* —
      [prometheus/alertmanager](https://github.com/prometheus/alertmanager)
- [ ] **prometheus-operator** — оператор для Prometheus/Alertmanager в
      кластере. *(`helm`)* —
      [prometheus-operator/prometheus-operator](https://github.com/prometheus-operator/prometheus-operator)
- [ ] **netdata** — realtime-мониторинг хостов и приложений с веб-UI.
      *(`сервис`)* — [netdata/netdata](https://github.com/netdata/netdata)
      — на ноутбуке хватает `procs`, `fastfetch` и `jc` (уже стоят).
- [ ] **openobserve** — бэкенд для логов, метрик и трейсов. *(`сервис`)* —
      [openobserve/openobserve](https://github.com/openobserve/openobserve)
- [ ] **beszel** — лёгкий мониторинг серверов (hub + агенты). *(`сервис`)* —
      [henrygd/beszel](https://github.com/henrygd/beszel)
- [ ] **uptime-kuma** — проверки доступности и статус-страница. *(`сервис`)* —
      [louislam/uptime-kuma](https://github.com/louislam/uptime-kuma)
- [ ] **gatus** — health-dashboard и status page из YAML: проверки
      HTTP/ICMP/TCP/DNS/SSH и TLS-сертификатов, алерты, веб-UI. *(`сервис`)* —
      [TwiN/gatus](https://github.com/TwiN/gatus)
      — долгоживущий веб-сервис с портом и историей проверок; на ноутбуке
      разовые проверки закрывают `xh`/`curl`, `tcping`, `doggo` и `cloudprober`.
- [ ] **changedetection.io** — отслеживание изменений на страницах.
      *(`сервис`)* —
      [dgtlmoon/changedetection.io](https://github.com/dgtlmoon/changedetection.io)
- [ ] **cachet** — статус-страница для инцидентов. *(`сервис`)* —
      [cachethq/cachet](https://github.com/cachethq/cachet)
- [ ] **kubewatch** — события Kubernetes в уведомления (Slack и т.п.).
      *(`helm`)* — [robusta-dev/kubewatch](https://github.com/robusta-dev/kubewatch)
- [ ] **kuberhealthy** — синтетические проверки кластера. *(`helm`)* —
      [kuberhealthy/kuberhealthy](https://github.com/kuberhealthy/kuberhealthy)
- [ ] **pixie** — eBPF-наблюдаемость Kubernetes без агентов. *(`helm`)* —
      [pixie-io/pixie](https://github.com/pixie-io/pixie)
- [ ] **odigos** — авто-инструментирование приложений для OTel.
      *(`helm`)* — [odigos-io/odigos](https://github.com/odigos-io/odigos)
- [ ] **opencost** — стоимость ворклоадов Kubernetes и облака. *(`helm`)* —
      [opencost/opencost](https://github.com/opencost/opencost)
      — в релизах только SBOM; распространяется контейнерным образом.
      На ноутбуке уместен только CLI `kubectl-cost` к уже существующему
      серверу.
- [ ] **Kubecost** — распределение затрат по ворклоадам. *(`helm`)* —
      [kubecost/cost-model](https://github.com/kubecost/cost-model)

## 2. Данные, БД и реестры артефактов

- [ ] **ClickHouse** — колоночная OLAP-СУБД. *(`сервис`)* —
      [ClickHouse/ClickHouse](https://github.com/ClickHouse/ClickHouse)
- [ ] **cloudnative-pg** — оператор PostgreSQL в Kubernetes. *(`helm`)* —
      [cloudnative-pg/cloudnative-pg](https://github.com/cloudnative-pg/cloudnative-pg)
- [ ] **supabase** — self-hosted BaaS (Postgres + auth + storage + API).
      *(`сервис`)* — [supabase/supabase](https://github.com/supabase/supabase)
- [ ] **bytebase** — UI для миграций и ревью схем БД. *(`сервис`)* —
      [bytebase/bytebase](https://github.com/bytebase/bytebase)
- [ ] **immich** — self-hosted фото/видео с мобильными клиентами.
      *(`сервис`)* — [immich-app/immich](https://github.com/immich-app/immich)
- [ ] **sentry** — сервер сбора ошибок приложений. *(`сервис`)* —
      [getsentry/sentry](https://github.com/getsentry/sentry)
- [ ] **aptly** — репозиторий deb-пакетов. *(`сервис`)* —
      [aptly-dev/aptly](https://github.com/aptly-dev/aptly)
- [ ] **artifact-keeper** — self-hosted сервис для хранения артефактов
      сборки. *(`сервис`)* — на ноутбуке заменяется готовыми реестрами и
      `gh`/`glab`.

## 3. CI/CD, GitOps и IaC

- [ ] **Jenkins** — сервер CI. *(`сервис`)* —
      [jenkinsci/jenkins](https://github.com/jenkinsci/jenkins)
      — локально уже есть `glci`/`pre-commit`.
- [ ] **Argo CD** — GitOps-контроллер Kubernetes. *(`helm`)* —
      [argoproj/argo-cd](https://github.com/argoproj/argo-cd)
      — если понадобится работа с сервером с ноутбука, нужен только CLI
      `argocd`; в плейбуке его пока нет.
- [ ] **gitea** — self-hosted Git-сервер. *(`сервис`)* —
      [go-gitea/gitea](https://github.com/go-gitea/gitea)
- [ ] **Dokploy** — self-hosted PaaS поверх Docker. *(`сервис`)* —
      [Dokploy/dokploy](https://github.com/Dokploy/dokploy)
- [ ] **dokku** — мини-PaaS на одном сервере. *(`сервис`)* —
      [dokku/dokku](https://github.com/dokku/dokku)
- [ ] **kestra** — оркестратор пайплайнов и расписаний. *(`сервис`)* —
      [kestra-io/kestra](https://github.com/kestra-io/kestra)
- [ ] **semaphore** — веб-UI для запуска Ansible-плейбуков. *(`сервис`)* —
      [semaphoreui/semaphore](https://github.com/semaphoreui/semaphore)
- [ ] **atlantis** — PR-автоматизация Terraform. *(`сервис`)* —
      [runatlantis/atlantis](https://github.com/runatlantis/atlantis)
- [ ] **Terrakube** — self-hosted замена Terraform Enterprise. *(`helm`)* —
      [terrakube-io/terrakube](https://github.com/terrakube-io/terrakube)
- [ ] **terrapod** — ещё одна self-hosted замена Terraform Enterprise.
      *(`исходники`)* —
      [mattrobinsonsre/terrapod](https://github.com/mattrobinsonsre/terrapod)
      — в релизах только Terraform-провайдер, сервер собирается из исходников.
- [ ] **coder** — удалённые dev-окружения в кластере. *(`helm`)* —
      [coder/coder](https://github.com/coder/coder)
- [ ] **Unleash** — сервер feature-флагов. *(`сервис`)* —
      [Unleash/unleash](https://github.com/Unleash/unleash)
- [ ] **Crossplane** — control plane для управления облаками из Kubernetes.
      *(`helm`)* — [crossplane/crossplane](https://github.com/crossplane/crossplane)
- [ ] **Floci** — self-hosted сервис из подборки звёзд. *(`образ`)*
      — поднимается контейнером, а не ставится бинарником.
- [ ] **k3s-ansible** — готовый плейбук HA-кластера k3s (kube-vip,
      MetalLB). *(`git clone`)* —
      [timothystewart6/k3s-ansible](https://github.com/timothystewart6/k3s-ansible)
      — набор Ansible-плейбуков для серверов, релизов нет.

## 4. Сеть, VPN и API-шлюзы

- [ ] **wg-easy** — веб-UI для WireGuard. *(`сервис`)* —
      [wg-easy/wg-easy](https://github.com/wg-easy/wg-easy)
- [ ] **headscale** — self-hosted control-plane для Tailscale-клиентов.
      *(`сервис`)* — [juanfont/headscale](https://github.com/juanfont/headscale)
- [ ] **pangolin** — reverse-proxy и туннели с веб-UI. *(`сервис`)* —
      [fosrl/pangolin](https://github.com/fosrl/pangolin)
- [ ] **netmaker** — управляемая WireGuard-сеть. *(`helm`)* —
      [gravitl/netmaker](https://github.com/gravitl/netmaker)
- [ ] **APISIX** — API-шлюз. *(`helm`)* —
      [apache/apisix](https://github.com/apache/apisix)
- [ ] **Kong** — API-шлюз и service mesh. *(`helm`)* —
      [Kong/kong](https://github.com/Kong/kong)
- [ ] **cilium** — CNI и eBPF-платформа кластера. *(`helm`)* —
      [cilium/cilium](https://github.com/cilium/cilium)
- [ ] **meshery** — управление service mesh. *(`helm`)* —
      [meshery/meshery](https://github.com/meshery/meshery)

## 5. Безопасность

- [ ] **jumpserver** — bastion/PAM с веб-UI. *(`сервис`)* —
      [jumpserver/jumpserver](https://github.com/jumpserver/jumpserver)
- [ ] **warpgate** — bastion для SSH/MySQL/Postgres/Kubernetes. *(`сервис`)* —
      [warp-tech/warpgate](https://github.com/warp-tech/warpgate)
- [ ] **SafeLine** — WAF с веб-UI. *(`сервис`)* —
      [chaitin/SafeLine](https://github.com/chaitin/SafeLine)
- [ ] **threatmapper** — платформа поиска уязвимостей в образах и
      ворклоадах. *(`helm`)* —
      [deepfence/ThreatMapper](https://github.com/deepfence/ThreatMapper)
- [ ] **dagda** — статический анализ образов на malware и уязвимости.
      *(`сервис`)* —
      [eliasgranderubio/dagda](https://github.com/eliasgranderubio/dagda)
      — сервер (Django + MongoDB + docker-образ), а не CLI. На ноутбуке его
      функции закрывают `grype` и `copa`.
- [ ] **scantron** — распределённый nmap/masscan с расписанием и API.
      *(`сервис`)* —
      [rackerlabs/scantron](https://github.com/rackerlabs/scantron)
      — набор Ansible-плейбуков с Django-сервером, очередью и БД. Локальный
      скан делается обычным `nmap` (уже стоит).

## 6. Kubernetes-платформы и ОС

- [ ] **kubesphere** — платформа управления Kubernetes с UI. *(`helm`)* —
      [kubesphere/kubesphere](https://github.com/kubesphere/kubesphere)
- [ ] **talos** — immutable-ОС для Kubernetes-нод. *(`образ`)* —
      [siderolabs/talos](https://github.com/siderolabs/talos)
- [ ] **cozystack** — PaaS на Kubernetes. *(`helm`)* —
      [aenix-io/cozystack](https://github.com/aenix-io/cozystack)
- [ ] **kagent** — AI-агенты для операций в Kubernetes. *(`helm`)* —
      [kagent-dev/kagent](https://github.com/kagent-dev/kagent)

## 7. Знания, совместная работа и удалённый доступ

- [ ] **n8n** — автоматизация workflow с веб-UI. *(`сервис`)* —
      [n8n-io/n8n](https://github.com/n8n-io/n8n)
- [ ] **open-webui** — веб-UI для LLM и RAG. *(`сервис`)* —
      [open-webui/open-webui](https://github.com/open-webui/open-webui)
- [ ] **plausible** — self-hosted веб-аналитика. *(`сервис`)* —
      [plausible/analytics](https://github.com/plausible/analytics)
- [ ] **rustdesk** — self-hosted relay для удалённого доступа. *(`сервис`)* —
      [rustdesk/rustdesk](https://github.com/rustdesk/rustdesk)
- [ ] **joplin** — сервер синхронизации заметок. *(`сервис`)* —
      [laurent22/joplin](https://github.com/laurent22/joplin)
- [ ] **AFFiNE** — рабочее пространство с документами и досками.
      *(`сервис`)* — [toeverything/AFFiNE](https://github.com/toeverything/AFFiNE)
- [ ] **siyuan** — заметки с self-hosted синхронизацией. *(`сервис`)* —
      [siyuan-note/siyuan](https://github.com/siyuan-note/siyuan)
- [ ] **logseq** — заметки; десктоп-приложение, серверного пакета для
      ноутбука нет. *(`клиент`)* —
      [logseq/logseq](https://github.com/logseq/logseq)
- [ ] **excalidraw** — доски; self-hosted вариант — отдельный сервис.
      *(`сервис`)* — [excalidraw/excalidraw](https://github.com/excalidraw/excalidraw)
- [ ] **bruno** — GUI-клиент для API; ставится вручную (deb/AppImage), в
      плейбуке его нет. *(`клиент`)* —
      [usebruno/bruno](https://github.com/usebruno/bruno)

---

## Что дальше

- [ ] В плейбук ничего из этого списка не берём: он про ноутбук, а не про
      серверы и кластеры.
- [ ] Если понадобится поднять что-то из списка — отдельный хост, отдельный
      `docker compose` или Helm-релиз, а не роль `binaries`.
- [ ] Сверять при обновлении списка звёзд: у части проектов может появиться
      standalone-режим или удобный CLI, тогда кандидат переезжает в
      [`TODO-trend-github.md`](TODO-trend-github.md).
- [ ] `VictoriaMetrics` и `coroot` сюда **не** относятся: у обоих есть
      single-binary режим для локального запуска, поэтому они стоят в роли
      `monitoring` (из VictoriaMetrics — только `vmalert-tool`).
