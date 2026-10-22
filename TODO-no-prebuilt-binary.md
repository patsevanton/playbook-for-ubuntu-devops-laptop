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
списке. Исключение — уже добавленные `gatus` и `entropy`: они собираются
ролью `go_apps` и пока остаются в плейбуке.

Как читать: `- [ ]` — ждёт релизного бинарника; в заметке — что именно
отсутствует.

Серверные приложения и Helm-чарты (opencost, dagda, scantron, terrapod,
k3s-ansible и т.п.) сюда не входят: они не устанавливаются на ноутбук по
другой причине — см. [`TODO-server-apps.md`](TODO-server-apps.md).

---

## IaC и инфраструктура

- [ ] **knee** — интерактивный CLI поверх Ansible: полуавтоматический setup
      инфраструктуры. *(`pipx`)* —
      [shurutech/knee](https://github.com/shurutech/knee)
      — пакета на PyPI нет: установка из клона (`pip install -r
      requirements.txt` с пинами ansible-lint 6.22.2 и typer 0.9.0), в релизе
      v0.0.1 ассетов нет. Пересмотреть, если появится пакет или бинарник.

## Kubernetes

Нодовые агенты (DaemonSet, static Pod) — отдельный список:
[`TODO-node-tools.md`](TODO-node-tools.md). Здесь остаются кластерные
инструменты, у которых просто нет бинарника.

- [ ] **kondense** — автоматический подбор requests/limits для контейнеров.
      *(`go`)* — [unagex/kondense](https://github.com/unagex/kondense)
      — в релизах нет ассетов, только исходники (`go build`).

- [ ] **kubernetes-rbac-audit** — аудит RBAC в кластере. *(`pipx`)* —
      [cyberark/kubernetes-rbac-audit](https://github.com/cyberark/kubernetes-rbac-audit)
      — один скрипт `ExtensiveRoleCheck.py` без упаковки: работает офлайн по
      заранее выгруженным JSON ролей. Покрывается `rbac-wizard` и `kubehound`
      (роль `secrets`).

## Сеть

- [ ] **EternalTerminal** — переподключаемый удалённый shell (сессия переживает
      обрыв). *(`apt` из репо ET; в latest .deb только arm64)* —
      [MisterTea/EternalTerminal](https://github.com/MisterTea/EternalTerminal)
      — в релизе нет `.deb` под amd64 (только arm64 и исходники); amd64
      ставится из apt-репозитория ET, на 26.04 не проверялся.
- [ ] **wondershaper** — простое ограничение полосы пропускания адаптера.
      *(`make`)* — [magnific0/wondershaper](https://github.com/magnific0/wondershaper)
      — релизов нет: bash-скрипт, ставится копированием/`make install`.

## Диагностика и железо

- [ ] **linnix** — eBPF-наблюдаемость с AI-детекцией инцидентов. *(`deb`)* —
      [linnix-os/linnix](https://github.com/linnix-os/linnix)
      — релиз v0.2.0 опубликован без ассетов: ни `.deb`, ни Linux-бинарника,
      только исходники и docker-образ.
- [ ] **sysinfo** — скрипты сбора подробной системной информации.
      *(`pipx` / `git clone`)* — [peterbay/sysinfo](https://github.com/peterbay/sysinfo)
      — набор bash/python-скриптов без упаковки (ни PyPI, ни релизов). Похожие
      данные дают `procs`, `fastfetch` и `jc`.
- [ ] **mcelog** — userspace-логирование аппаратных ошибок памяти CPU (MCE).
      *(`make`)* — [andikleen/mcelog](https://github.com/andikleen/mcelog)
      — релизов нет: сборка из исходников (`make`).
- [ ] **bench-scripts** — набор скриптов бенчмарка Linux-сервера. *(`git clone`)* —
      [haydenjames/bench-scripts](https://github.com/haydenjames/bench-scripts)
      — релизов нет: набор shell-скриптов, запускается из клона.

## Безопасность

- [ ] **flawz** — TUI-браузер CVE/уязвимостей. *(`cargo`)* —
      [orhun/flawz](https://github.com/orhun/flawz)
      — в релизах только macOS/Windows и `flawz-installer.sh`; готового
      Linux-бинарника нет (`cargo build`).

## Контейнеры и реестры

- [ ] **registry-cli** — манипуляции с docker-registry из командной строки.
      *(`pipx`)* — [andrey-pohilko/registry-cli](https://github.com/andrey-pohilko/registry-cli)
      — одиночный `registry.py` плюс `pip install -r requirements-*.txt`;
      ни пакета на PyPI, ни релизных артефактов нет. Альтернатива — готовый
      docker-образ `anoxis/registry-cli`.
- [ ] **gitlab-registry-images-cleaner** — чистка старых образов в GitLab
      registry. *(`pipx`)* —
      [n0madic/gitlab-registry-images-cleaner](https://github.com/n0madic/gitlab-registry-images-cleaner)
      — одиночный python-скрипт с `requirements.txt`, пакета на PyPI и
      релизных ассетов нет.

## Пакетные менеджеры

- [ ] **stew** — независимый пакетный менеджер для скомпилированных бинарников.
      *(`go`)* — [alxrem/stew](https://github.com/alxrem/stew)
      — релизов нет, только исходники (`go install`).
