# TODO: нет готового бинарника на GitHub

Кандидаты, для которых апстрим не публикует готовый бинарник под Linux x86_64:
только исходники (`make`, `go build`, `cargo build`, `git clone`), Helm-чарт,
контейнерный образ или сборка под другую архитектуру.

В плейбук такие утилиты пока не берём. Пересматривать, когда:

- апстрим начнёт публиковать релизные бинарники — тогда берём кандидата в
  работу и ставим ролью `binaries`;
- появится повод собирать из исходников — `go install` (роль `go_apps`),
  pipx (роль `pipx_apps`) или пакет Ubuntu.

Установка из PyPI/npm/Ansible Galaxy сюда не относится: это не «нет
бинарника», а штатный способ установки — такие кандидаты остаются в основном
списке. Исключение — уже добавленный `entropy`: он собирается ролью `go_apps`
и пока остаётся в плейбуке.

Как читать таблицу: «Что это» — назначение утилиты, «В каком виде
поставляется» — что именно отдаёт апстрим и чего не хватает для установки
ролью `binaries`. Все кандидаты ниже ждут релизного linux-amd64 бинарника.

Нодовые агенты (DaemonSet, static Pod), серверные приложения и Helm-чарты
(opencost, dagda, scantron, terrapod, k3s-ansible и т.п.) сюда не входят: они
не устанавливаются на ноутбук по другой причине — см.
[`TODO-server-apps.md`](TODO-server-apps.md). Заброшенные проекты (архив GitHub,
годы без обновлений) описаны только в [`TODO-abandoned.md`](TODO-abandoned.md).

Способ установки сверен 2026-10-22 (строки werf/kubedog/odo, Kubes и
kubectl-kubesec добавлены 2026-10-23 из `not-install.md`): для GitHub-проектов
— ассеты последнего релиза, для остальных — PyPI/apt. Перенесённые и отброшенные кандидаты — в
конце файла.

---

| Раздел | Кандидат | Что это | В каком виде поставляется |
| --- | --- | --- | --- |
| Диагностика и железо | [linnix](https://github.com/linnix-os/linnix) | eBPF-наблюдаемость с AI-детекцией инцидентов | Исходники и docker-образ; amd64 `.deb` был только в v0.1.0 (`cognitod_0.1.0_amd64.deb`), в v0.1.1 и v0.2.0 ассетов нет. Запрошено в [#124](https://github.com/linnix-os/linnix/issues/124) |
| Безопасность | [flawz](https://github.com/orhun/flawz) | TUI-браузер CVE/уязвимостей | В релизе v0.4.1 только macOS/Windows и `flawz-installer.sh`; готового Linux-бинарника нет (`cargo build`). Запрошено в [#113](https://github.com/orhun/flawz/issues/113) |
| Kubernetes | [scan2html](https://github.com/fatihtokus/scan2html) | плагин Trivy: интерактивный HTML-отчёт | Пакета на PyPI нет, только исходники |
| Kubernetes | [mokctl](https://github.com/bashtools/mokctl) | conformant multi-node кластеры в контейнерах | Релизы без ассетов, установка shell-скриптами |
| Kubernetes | [werf](https://github.com/werf/werf), [kubedog](https://github.com/werf/kubedog), [odo](https://github.com/redhat-developer/odo), Krane | сборка и доставка, CI-наблюдение, разработка в кластере и RBAC-анализ | В GitHub-релизах нет готовых бинарников: trdl/install-скрипт или `go install` |
| Kubernetes | [Kubes](https://github.com/boltops-tools/kubes) | сборка образа, компиляция YAML и деплой в кластер | Ставится только как gem, релизных бинарников нет |
| Kubernetes | kubectl-kubesec | krew-плагин `kubectl scan` поверх kubesec | Только krew, ассетов в релизе нет; сам `kubesec` в плейбуке уже стоит |

---

## Установлены без релизного бинарника

Эти утилиты не попали в таблицу выше, потому что для них есть штатный
способ установки без сборки из исходников:

- **kptop** — добавлен в роль `kubernetes` через pipx. Standalone-бинарников
  в GitHub-релизах нет; запрошены в
  [#85](https://github.com/eslam-gomaa/kptop/issues/85).
- **knetvis** — добавлен в роль `kubernetes` через pipx. В GitHub-релизе
  `v0.1.2` нет ассетов, а на PyPI пока только `0.1.1`; публикация `0.1.2`
  запрошена в
  [#10](https://github.com/samuelarogbonlo/knetvis/issues/10).

---

## Убрано после проверки 2026-10-22

- **kubectl-ports-rs** — добавлен в роль `kubernetes` (`kubectl-ports`).
  Оказалось, что готовый бинарник всё же есть: в последнем релизе `1.1.1`
  ассеты не собрались, но в `1.1.0` лежит статический
  `kubectl-ports_1.1.0-x86_64-unknown-linux-musl.tar.gz`. Ставится отдельной
  задачей с зафиксированным тегом.
- **Kubexer**, **Finch**, **kor**, **kubectl-foreach** — рассмотрены вместе с
  очередной волной kubetools и не ставятся: причины в
  [`not-install.md`](not-install.md). **Kubefirst** — не на ноутбук, см.
  [`TODO-server-apps.md`](TODO-server-apps.md).
- **werf**, **kubedog**, **odo**, **Krane**, **Kubes**, **kubectl-kubesec** —
  перенесены из `not-install.md` 2026-10-23: причина отказа техническая
  (trdl/install-скрипт, `go install`, gem, только krew), а не «не нужен».
  Теперь они в таблице выше.
- **knee** — не нужен: инфраструктура поднимается своими Ansible-плейбуками,
  а knee — обёртка над 13 плейбуками из своего репозитория. См.
  [`not-install.md`](not-install.md).
- **wondershaper**, **sysinfo**, **bench-scripts** — bash-скрипты без
  упаковки. Решение: shell-скрипты в плейбук не берём, даже если апстрим
  когда-нибудь начнёт их релизить.
- **kubectl-sudo** — перенесён в [`not-install.md`](not-install.md) 2026-10-23:
  в релизе `v1.1.0` ассетов нет, ставится только через krew, а сам плагин —
  шелл-обёртка `#!/bin/sh` (`bash/kubectl-sudo`), которая вызывает `kubectl
  --as=... --as-group=system:masters`. Та же причина, что у
  wondershaper/sysinfo/bench-scripts.
- **Click** — перенесён в [`not-install.md`](not-install.md) 2026-10-23: не
  представляет интереса, работу с кластером закрывают `k9s`, `kdash` и
  `kubectl`. Ставить его всё равно пришлось бы из исходников: релизных
  бинарников нет, только `cargo`.
- **kubecui** — перенесён в [`not-install.md`](not-install.md) 2026-10-23: не
  представляет интереса, то же место занимают `k9s` и `kdash`. К тому же в
  PyPI его нет и релизов нет — ставить пришлось бы из клона.
- **Nelm** — перенесён в [`not-install.md`](not-install.md) 2026-10-23: нужен
  только в CI/CD, локально дублирует `helm` и `helmfile`. Причина «нет
  бинарника» тоже есть: в GitHub-релизах ассетов нет, установка через
  trdl/install-скрипт.
- **Nighthawk** (layer5io/getnighthawk) — перенесён в
  [`not-install.md`](not-install.md) 2026-10-23: аналоги лучше. HTTP-нагрузку
  закрывает `k6` (роль `network`), нагрузку на кластер — `kube-burner` (роль
  `kubernetes`). Плюс с 2021 года релизы идут без ассетов: бинарники были
  только в `v1.0.0`–`v1.0.1`, дальше — Docker-образ и Makefile.
- **kondense** — не утилита для ноутбука: работает сайдкаром
  `kondense/kondense:1.1.0` внутри подов и правит им requests/limits.
  Кандидат для кластерных сервисов, рядом с Goldilocks и VPA.
- **EternalTerminal** — под 26.04 собирается amd64-пакет
  `et 7.0.0-resolute1` в PPA `ppa:jgmath2000/et`; проверено на 26.04.1:
  зависимости ставятся, `et --version` отвечает `7.0.0`. В релизе GitHub
  по-прежнему только `et_7.0.0-stable1_arm64.deb`. Оговорка: это PPA, а
  плейбук сейчас подключает сторонний репозиторий только для Docker.
- **stew** — у апстрима [marwanhawari/stew](https://github.com/marwanhawari/stew)
  есть `stew-v0.6.0-linux-amd64.tar.gz` и `stew_0.6.0_linux_amd64.deb`.
  В файле стояла ссылка на [alxrem/stew](https://github.com/alxrem/stew) —
  это форк без собственных релизов.
- **mcelog** — не нужен: MCE читаются через `dmesg` и `journalctl -k`,
  отдельный демон на ноутбуке не требуется. См. [`not-install.md`](not-install.md).
- **kubernetes-rbac-audit** — не нужен: есть аналоги с готовыми бинарниками
  (`kubectl-who-can`, `kubeaudit`, `rbac-tool`, `kubehound`). См.
  [`not-install.md`](not-install.md).
- **registry-cli** — не нужен: заменяет `regctl` из
  [regclient/regclient](https://github.com/regclient/regclient) (одиночный
  `regctl-linux-amd64` в релизах). Docker-образ `anoxis/registry-cli` не
  нужен. См. [`not-install.md`](not-install.md).
- **gitlab-registry-images-cleaner** — не нужен: есть штатная GitLab cleanup
  policy и CLI
  [gitlab-container-registry-cleaner](https://github.com/gitlab-container-registry-cleaner/gitlab-container-registry-cleaner)
  (npm). Разовые операции — через `glab api`. См.
  [`not-install.md`](not-install.md).
