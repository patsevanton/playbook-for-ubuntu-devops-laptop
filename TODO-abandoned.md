# TODO-abandoned: заброшенные проекты — не ставить

Реестр апстримов, которые заархивированы или годами не обновляются. Сюда
попали только кандидаты, которые рассматривались для ноутбука: из
[`TODO.md`](TODO.md), [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md),
[`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md),
[`TODO-trend-github-not-added.md`](TODO-trend-github-not-added.md) и
[`not-install.md`](not-install.md). Серверные приложения
([`TODO-server-apps.md`](TODO-server-apps.md)) и нодовые агенты
([`TODO-node-tools.md`](TODO-node-tools.md)) сюда не входят: они не ставятся
на ноутбук по другой причине, и смерть апстрима ничего не меняет.

Это сводный реестр статуса апстримов, а не второй трекер: полные описания,
причины отказа и условия пересмотра остаются в исходных файлах. Здесь только
дата последней активности, признак архива и короткая причина.

Как читать:

- `- [ ]` — не ставим;
- «архив» — репозиторий в GitHub Archive;
- «Покрыт: …» — какие инструменты в проекте закрывают ту же роль; «частично» —
  роль закрыта не полностью;
- даты и `archived` проверены 2026-10-23 через GitHub API (`archived`,
  `pushed_at`), последний релиз — из исходного трекера;
- `pushed_at` — дата последнего пуша в репозиторий, а не дата релиза: проект
  может быть активен в коммитах и при этом годами не выпускать бинарники.

Правило возврата одно: проект оживает (архив снят, вышел релиз, появился
linux-amd64 бинарник) — тогда кандидат возвращается в свой трекер и
ставится ролью. Без этого не тратить время.

---

## 1. Репозиторий в архиве

- [ ] **Datree** — CLI против Kubernetes-мисконфигураций. —
      [datreeio/datree](https://github.com/datreeio/datree) — архив; последний
      пуш 2024-04-23. Причина отказа в [`not-install.md`](not-install.md).
      Покрыт: `kubeconform`, `kube-linter`, `polaris`, `kube-score`,
      `kubesec`, `kubescape` (роль `kubernetes`).
- [ ] **glasskube** — пакетный менеджер Kubernetes с GUI и CLI. —
      [glasskube/glasskube](https://github.com/glasskube/glasskube) — архив с
      2026-06-14. Описан в
      [`TODO-trend-github-not-added.md`](TODO-trend-github-not-added.md) и
      [`not-install.md`](not-install.md). Покрыт частично: `helm`, `helmfile`,
      `kluctl`, `kustomizer`, `kpt` плюс UI `headlamp`/`radar`;
      dependency-aware менеджера с GUI нет.
- [ ] **Octant** — web-UI/дашборд кластера. —
      [vmware-archive/octant](https://github.com/vmware-archive/octant) —
      архив (vmware-archive), последний пуш 2023-01-19. Заменён `headlamp` и
      `radar` из роли `kubernetes`. Покрыт: `headlamp`, `radar`; также
      `kubewall`, `kubeview`, `kdash`, `buoy`.
- [ ] **k8s-mcp-server** — MCP-сервер с kubectl/helm/istioctl. —
      [alexei-led/k8s-mcp-server](https://github.com/alexei-led/k8s-mcp-server)
      — архив; последний пуш 2026-02-27. Заменён `kubernetes-mcp-server`.
      Покрыт: `kubernetes-mcp-server` (роль `kubernetes`).

## 2. Не архив, но проект мёртв

- [ ] **dog** — DNS-клиент с человекочитаемым выводом. —
      [ogham/dog](https://github.com/ogham/dog) — последний релиз `v0.1.0`
      (2021), последний пуш 2024-05-29. Бинарник к тому же не запускается на
      Ubuntu 26.04 — см.
      [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md).
      Покрыт: `q` и `doggo` (роль `network`).
- [ ] **Chartsec** — сканер безопасности Helm-чартов. —
      [banzaicloud/chartsec](https://github.com/banzaicloud/chartsec) —
      заброшен с 2019, последний пуш 2019-08-30. Описан в
      [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md). Покрыт
      частично: `kubesec`, `trivy` (роль `secrets`), `kubeaudit` — статический
      скан манифестов, чарт целиком не рендерится.
- [ ] **kubectl-snapshot** — снимки состояния кластера. —
      [fbrubbo/kubectl-snapshot](https://github.com/fbrubbo/kubectl-snapshot) —
      заброшен с 2020, последний пуш 2020-03-24. Описан в
      [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md). Покрыт
      частично: `kbom` (инвентарь состояния в JSON) и `kubectl-slice`; снимка
      с восстановлением нет.
- [ ] **kube-shell** — интерактивный shell для kubectl. —
      [cloudnativelabs/kube-shell](https://github.com/cloudnativelabs/kube-shell)
      — последний релиз `0.0.23` (2017), последний пуш 2022-11-02. Заменён
      `kube-prompt`. Покрыт: `kube-prompt` и `kubeprompt` (роль `kubernetes`),
      также `k9s`.
- [ ] **klum** — управление пользователями и доступом к кластеру. —
      [ibuildthecloud/klum](https://github.com/ibuildthecloud/klum) — не
      обновлялся с 2023, последний пуш 2023-09-21. Описан в
      [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md). Покрыт
      частично: `kubecm` (kubeconfig), `rbac-tool`, `rbac-lookup`,
      `kubectl-who-can`, `audit2rbac`; создания пользователей нет.
- [ ] **KuberEz** — графическое моделирование манифестов. —
      [uengine-oss/kubeez](https://github.com/uengine-oss/kubeez) — переименован
      из `kuber-ez`, заброшен с 2023, последний пуш 2023-11-13. Описан в
      [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md). Покрыт:
      `monokle`, `rubick`, `klustr`, `sextant` (роль `kubernetes`).
- [ ] **kopylot** — AI-аудит, диагностика и чат по ресурсам кластера. —
      [avsthiago/kopylot](https://github.com/avsthiago/kopylot) — не
      обновлялся с 2024, последний пуш 2024-01-24. Причина отказа в
      [`not-install.md`](not-install.md). Покрыт: `kopilot`, `k8sgpt`,
      `kubectl-ai`, `kubernetes-mcp-server` (роль `kubernetes`).
- [ ] **kubectl-mtail** — tail логов подов по label selector. —
      проект живёт на GitLab, ссылки в трекере нет; последний релиз 2020 года,
      ассетов в нём нет. Причина отказа в [`not-install.md`](not-install.md).
      Покрыт: `stern`, `kail`, `kubetail`, `kftui`, `kl` (роль `kubernetes`).

## 3. Формально живы, но два года без активности

Кандидаты без релизного бинарника: последний пуш старше двух лет, но архива
нет и явного «заброшен» в трекере не написано. Пересматривать только вместе
с релизом — сами по себе они не вернутся.

- [ ] **xlskubectl** — управление кластером из «электронной таблицы». —
      [learnk8s/xlskubectl](https://github.com/learnk8s/xlskubectl) — последний
      пуш 2022-09-23; на PyPI не публиковался, релизов нет.
- [ ] **kuttle** — sshuttle-доступ к подам без SSH. —
      [kayrus/kuttle](https://github.com/kayrus/kuttle) — последний пуш
      2023-01-29; релизов нет, имя на PyPI занято другим проектом. Покрыт
      частично: `telepresence`, `kubevpn`, `ktunnel`, `kubectl-relay`,
      `kubefwd`, `gefyra`, `mirrord`; sshuttle-модели нет.
- [ ] **mico** — AI-помощник kubectl. —
      [tahtaciburak/mico](https://github.com/tahtaciburak/mico) — последний
      пуш 2023-03-25; только `go build`. Покрыт: `kubectl-ai` и `k8sgpt`
      (роль `kubernetes`).
- [ ] **KindScaler** — управление нодами KinD-кластера. —
      [lobuhi/kindscaler](https://github.com/lobuhi/kindscaler) — последний
      пуш 2024-06-15; только `go build`, релизов нет.

---

## Что сюда не попало

- **Закрытое ПО** (`Lens IDE`, `K8Studio`, `Kubexer`) — это не заброшенные, а
  платные или закрытые продукты; они остаются в [`not-install.md`](not-install.md).
- **Активные проекты с устаревшими зависимостями** (`HolmesGPT`,
  `KubeDiagrams`, `PowerfulSeal`, `Netchecks`, `IceKube`, `kubestriker`,
  `vulncov`, `teller`) — апстрим жив, ломается совместимость с Ubuntu 26.04 /
  Python 3.14, поэтому они в
  [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md).
- **`devbox` и `flox`** — живые, но удалены из-за отказа от Nix:
  [`TODO-trend-github-not-added.md`](TODO-trend-github-not-added.md).
