# TODO-node-tools: утилиты для нод Kubernetes — не на ноутбук

Кандидаты, которые работают **на каждой ноде кластера**: DaemonSet, static Pod
или systemd-юнит на хосте. Это проверка связности нод, метрики самого хоста,
eBPF-агенты, кэш образов и т.п. На ноутбуке они бессмысленны: без kubelet и
кластера им нечего обслуживать, а «установка» — это манифест, Helm-чарт или
контейнерный образ в кластере, а не бинарник в `/usr/local/bin`.

Файл отдельный, потому что причина отказа другая, чем в соседних трекерах:

- [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md) — у проекта нет
  linux-amd64 бинарника (вопрос релиза);
- [`TODO-server-apps.md`](TODO-server-apps.md) — серверные приложения, сервисы
  и Helm-чарты (не на ноутбук);
- [`not-install.md`](not-install.md) — осознанный отказ из-за бага/issue.

Здесь — именно нодовые агенты, независимо от того, публикует проект бинарник
или только образ.

Как читать:

- `- [ ]` — кандидат, ещё не рассмотрен.
- В скобках — что поставляет апстрим: `helm` — только Helm-чарт, `image` —
  только контейнерный образ/манифест, `release` — есть linux-amd64 бинарник, но
  он ставится на ноду (или это CLI к агенту).
- Ссылка ведёт на репозиторий проекта.

Способ установки сверен с ассетами последнего релиза 2026-10-22
(`releases/latest` + `releases/expanded_assets`); Scaphandre добавлен
2026-10-23. Платформы целиком (Calico,
Cilium, Longhorn, OpenEBS, MetalLB и т.п.) здесь не дублируются: они уже
отнесены к серверным в [`TODO.md`](TODO.md).

---

## 1. Связность и сеть нод

- [ ] **goldpinger** — проверка связности между нодами кластера. *(`helm`)* —
      [bloomberg/goldpinger](https://github.com/bloomberg/goldpinger)
      — в релизе только Helm-чарт `goldpinger-1.2.0.tgz`; распространяется
      контейнерным образом (DaemonSet).
- [ ] **k8s-netchecker-server** — server + agent: проверка сети между нодами,
      ближайший аналог goldpinger. *(`image`)* —
      [mirantis/k8s-netchecker-server](https://github.com/mirantis/k8s-netchecker-server)
      — релиз без ассетов; server и agent ставятся манифестами в кластер.
- [ ] **kube-network-policies** — eBPF-реализация NetworkPolicy без CNI.
      *(`image`)* —
      [kubernetes-sigs/kube-network-policies](https://github.com/kubernetes-sigs/kube-network-policies)
      — релиз без ассетов; агент работает DaemonSet на каждой ноде.
- [ ] **kube-router** — CNI, service proxy вместо kube-proxy и network policy
      в одном DaemonSet.
      *(`release`)* — [cloudnativelabs/kube-router](https://github.com/cloudnativelabs/kube-router)
      — есть `kube-router_2.11.1_linux_amd64.tar.gz`, но бинарник запускается
      агентом на ноде, а не на ноутбуке.
- [ ] **Submariner** — связность подов и сервисов между кластерами.
      *(`image`)* — [submariner-io/submariner](https://github.com/submariner-io/submariner)
      — релиз без ассетов; gateway ставится DaemonSet через оператор/Helm.
- [ ] **k8spacket** — визуализация трафика подов на уровне ноды. *(`image`)* —
      [k8spacket/k8spacket](https://github.com/k8spacket/k8spacket)
      — релиз без ассетов; DaemonSet с eBPF-агентами и веб-интерфейсом.
- [ ] **KubeSkoop** — диагностика сети для разных CNI и облачных провайдеров.
      *(`image`)* — [alibaba/kubeskoop](https://github.com/alibaba/kubeskoop)
      — релиз без ассетов; skoop-agent работает DaemonSet на нодах.

## 2. Метрики и здоровье ноды

- [ ] **node_exporter** — метрики хоста (CPU, память, диск, сеть) для
      Prometheus. *(`release`)* —
      [prometheus/node_exporter](https://github.com/prometheus/node_exporter)
      — есть `node_exporter-1.12.1.linux-amd64.tar.gz`; в кластере ставится
      DaemonSet'ом, локально на ноутбуке смысла не имеет.
- [ ] **node-problem-detector** — детекторы проблем ноды (ядро, диск, память).
      *(`release`)* —
      [kubernetes/node-problem-detector](https://github.com/kubernetes/node-problem-detector)
      — есть `node-problem-detector-v1.37.0-linux_amd64.tar.gz`; DaemonSet на
      каждой ноде.
- [ ] **kured** — перезагрузка нод после обновлений ядра/ОС. *(`release`)* —
      [kubereboot/kured](https://github.com/kubereboot/kured)
      — есть `kured_1.23.0_linux_amd64`; DaemonSet, следит за reboot-required.
- [ ] **node-feature-discovery** — разметка нод по возможностям железа
      (CPU-флаги, GPU, SR-IOV). *(`helm`)* —
      [kubernetes-sigs/node-feature-discovery](https://github.com/kubernetes-sigs/node-feature-discovery)
      — в релизе только чарт `node-feature-discovery-chart-0.20.0.tgz`.
- [ ] **NVIDIA k8s-device-plugin** — доступ подов к GPU на нодах. *(`helm`)* —
      [NVIDIA/k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin)
      — в релизе только чарты `nvidia-device-plugin` и
      `gpu-feature-discovery`.

## 3. Энергопотребление ноды

- [ ] **kepler** — оценка энергопотребления ворклоадов через eBPF и
      Prometheus-метрики. *(`release` / `helm`)* —
      [sustainable-computing-io/kepler](https://github.com/sustainable-computing-io/kepler)
      — есть `kepler-0.12.0.linux-amd64.tar.gz` и чарт; в кластере работает
      DaemonSet на нодах.
- [ ] **Scaphandre** — агент метрик энергопотребления хоста (RAPL/powercap).
      *(`image`)* — [hubblo-org/scaphandre](https://github.com/hubblo-org/scaphandre)
      — в релизе `v1.0.3` ассетов нет (deb-пакеты и контейнерный образ);
      работает systemd-юнитом или DaemonSet и отдаёт метрики в Prometheus.
      На ноутбуке не нужен: хватает `power-profiles-daemon` и
      `amd-pstate`/`intel_pstate`.

## 4. eBPF: наблюдаемость и безопасность

- [ ] **Retina** — распределённая сетевая наблюдаемость для Kubernetes.
      *(`helm` / `image`)* — [microsoft/retina](https://github.com/microsoft/retina)
      — агент ставится чартом/оператором; в релизе есть только CLI
      `kubectl-retina-linux-amd64-v1.3.0.tar.gz`.
- [ ] **Tetragon** — eBPF-наблюдаемость и runtime-enforcement. *(`helm`)* —
      [cilium/tetragon](https://github.com/cilium/tetragon)
      — агент ставится чартом; в релизе есть только CLI
      `tetra-linux-amd64.tar.gz`.
- [ ] **DeepFlow** — eBPF-трейсинг и профилирование для Kubernetes.
      *(`image`)* — [deepflowio/deepflow](https://github.com/deepflowio/deepflow)
      — релиз без ассетов; агент работает DaemonSet на нодах.
- [ ] **Alaz** — eBPF-агент наблюдаемости сервисных взаимодействий.
      *(`image`)* — [getanteon/alaz](https://github.com/getanteon/alaz)
      — релиз без ассетов; DaemonSet.

## 5. Кэш образов и хранилище на ноде

- [ ] **kube-fledged** — кэш контейнерных образов прямо на нодах.
      *(`image`)* — [senthilrch/kube-fledged](https://github.com/senthilrch/kube-fledged)
      — релиз без ассетов; DaemonSet + контроллер.
- [ ] **Kubernetes Image Puller** — предзагрузка образов на ноды через
      DaemonSet. *(`image`)* —
      [che-incubator/kubernetes-image-puller](https://github.com/che-incubator/kubernetes-image-puller)
      — ставится манифестами/оператором в кластер.
- [ ] **TopoLVM** — CSI-плагин с учётом ёмкости локальных дисков ноды.
      *(`helm`)* — [topolvm/topolvm](https://github.com/topolvm/topolvm)
      — в релизе только чарт `topolvm-17.3.0.tgz`; node-компонент — DaemonSet.
- [ ] **kube-fencing** — очистка ресурсов упавших нод, чтобы не блокировать
      восстановление. *(`image`)* — [kvaps/kube-fencing](https://github.com/kvaps/kube-fencing)
      — релиз без ассетов; DaemonSet на control-plane нодах.

## 6. Не сюда: кластерные, но не нодовые

Эти утилиты тоже не ставятся на ноутбук, но живут как Deployment/Job в
кластере, а не агентом на ноде, поэтому остаются в других трекерах:

- **opencost**, **Kubecost** — стоимость ворклоадов; оба в
  [`TODO-server-apps.md`](TODO-server-apps.md).
- **kube-state-metrics** — метрики состояния кластера; **metrics-server** и
  **prometheus-adapter** — серверы метрик для API/HPA. В трекерах пока не
  описаны (metrics-server упоминается в `README.md` как зависимость `ktop`).
- **descheduler**, **Autoscaler/VPA**, **Goldilocks**, **kube-green** —
  контроллеры правки requests/limits и расписаний.
- **Chaos Mesh**, **chaoskube**, **kubeinvaders** — chaos-платформы.

Локальный CLI к кластерному OpenCost — **`kubectl-cost`**
([opencost/kubectl-cost](https://github.com/opencost/kubectl-cost)) — наоборот,
уместен на ноутбуке как плагин kubectl; упомянут в
[`TODO-server-apps.md`](TODO-server-apps.md) как локальная альтернатива
серверу.

## 7. Что дальше

- [ ] В плейбук ничего из этого списка не берём: он настраивает ноутбук, а не
      кластер. Файл — справочник на случай, если понадобится поднять агента
      в кластере.
- [ ] Если понадобится — оформлять отдельной ролью с Helm/манифестами, а не
      ролью `binaries`.
- [ ] Периодически сверять релизы: у части проектов может появиться
      standalone-режим, тогда кандидата логично поставить ролью `binaries`.

Связанное: [`TODO.md`](TODO.md) — что ставится и что серверное;
[`TODO-server-apps.md`](TODO-server-apps.md) — серверные приложения и
Helm-чарты;
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md) — нет бинарника;
[`not-install.md`](not-install.md) — отказы из-за багов.
