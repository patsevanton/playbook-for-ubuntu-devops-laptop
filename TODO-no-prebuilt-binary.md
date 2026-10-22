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

Нодовые агенты (DaemonSet, static Pod) — отдельный список:
[`TODO-node-tools.md`](TODO-node-tools.md). Серверные приложения и Helm-чарты
(opencost, dagda, scantron, terrapod, k3s-ansible и т.п.) сюда тоже не входят:
они не устанавливаются на ноутбук по другой причине — см.
[`TODO-server-apps.md`](TODO-server-apps.md).

Способ установки сверен 2026-10-22: для GitHub-проектов — ассеты последнего
релиза, для остальных — PyPI/apt. Перенесённые и отброшенные кандидаты — в
конце файла.

---

| Раздел | Кандидат | Что это | В каком виде поставляется |
| --- | --- | --- | --- |
| Диагностика и железо | [linnix](https://github.com/linnix-os/linnix) | eBPF-наблюдаемость с AI-детекцией инцидентов | Исходники и docker-образ; amd64 `.deb` был только в v0.1.0 (`cognitod_0.1.0_amd64.deb`), в v0.1.1 и v0.2.0 ассетов нет. Запрошено в [#124](https://github.com/linnix-os/linnix/issues/124) |
| Безопасность | [flawz](https://github.com/orhun/flawz) | TUI-браузер CVE/уязвимостей | В релизе v0.4.1 только macOS/Windows и `flawz-installer.sh`; готового Linux-бинарника нет (`cargo build`). Запрошено в [#113](https://github.com/orhun/flawz/issues/113) |

---

## Убрано после проверки 2026-10-22

- **knee** — не нужен: инфраструктура поднимается своими Ansible-плейбуками,
  а knee — обёртка над 13 плейбуками из своего репозитория. См.
  [`not-install.md`](not-install.md).
- **wondershaper**, **sysinfo**, **bench-scripts** — bash-скрипты без
  упаковки. Решение: shell-скрипты в плейбук не берём, даже если апстрим
  когда-нибудь начнёт их релизить.
- **kondense** — не утилита для ноутбука: работает сайдкаром
  `kondense/kondense:1.1.0` внутри подов и правит им requests/limits.
  Кандидат для [`TODO-kubetools.md`](TODO-kubetools.md), рядом с Goldilocks
  и VPA.
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
