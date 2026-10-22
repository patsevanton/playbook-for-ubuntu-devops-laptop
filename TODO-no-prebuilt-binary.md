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
| Kubernetes | [kubernetes-rbac-audit](https://github.com/cyberark/kubernetes-rbac-audit) | Аудит RBAC в кластере, в том числе офлайн по заранее выгруженным JSON ролей | Один скрипт `ExtensiveRoleCheck.py` без упаковки: ни PyPI, ни релизов. Покрывается `kubehound` (роль `kubernetes`) |
| Диагностика и железо | [linnix](https://github.com/linnix-os/linnix) | eBPF-наблюдаемость с AI-детекцией инцидентов | Исходники и docker-образ; amd64 `.deb` был только в v0.1.0 (`cognitod_0.1.0_amd64.deb`), в v0.1.1 и v0.2.0 ассетов нет |
| Диагностика и железо | [mcelog](https://github.com/andikleen/mcelog) | userspace-логирование аппаратных ошибок памяти CPU (MCE) | Сборка из исходников (`make`); релизов нет, в репозитории Ubuntu пакета тоже нет |
| Безопасность | [flawz](https://github.com/orhun/flawz) | TUI-браузер CVE/уязвимостей | В релизе v0.4.1 только macOS/Windows и `flawz-installer.sh`; готового Linux-бинарника нет (`cargo build`) |
| Контейнеры и реестры | [registry-cli](https://github.com/andrey-pohilko/registry-cli) | Манипуляции с docker-registry из командной строки | Одиночный `registry.py` плюс `pip install -r requirements-*.txt`; ни пакета на PyPI, ни релизных артефактов. Альтернатива — готовый docker-образ `anoxis/registry-cli` |
| Контейнеры и реестры | [gitlab-registry-images-cleaner](https://github.com/n0madic/gitlab-registry-images-cleaner) | Чистка старых образов в GitLab registry | Одиночный python-скрипт с `requirements.txt`; пакета на PyPI и релизных ассетов нет |

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
