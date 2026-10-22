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

---

| Раздел | Кандидат | Что это | В каком виде поставляется |
| --- | --- | --- | --- |
| IaC и инфраструктура | [knee](https://github.com/shurutech/knee) | Интерактивный CLI поверх Ansible: полуавтоматический setup инфраструктуры | Клон репозитория и `pip install -r requirements.txt` (с пинами ansible-lint 6.22.2 и typer 0.9.0); пакета на PyPI нет, в релизе v0.0.1 ассетов нет |
| Kubernetes | [kondense](https://github.com/unagex/kondense) | Автоматический подбор requests/limits для контейнеров | Только исходники (`go build`); релизы без ассетов |
| Kubernetes | [kubernetes-rbac-audit](https://github.com/cyberark/kubernetes-rbac-audit) | Аудит RBAC в кластере, в том числе офлайн по заранее выгруженным JSON ролей | Один скрипт `ExtensiveRoleCheck.py` без упаковки, ни PyPI, ни релизных артефактов; покрывается `kubehound` (роль `kubernetes`) |
| Сеть | [EternalTerminal](https://github.com/MisterTea/EternalTerminal) | Переподключаемый удалённый shell: сессия переживает обрыв | В релизе `.deb` только под arm64 плюс исходники; amd64 ставится из apt-репозитория ET, на 26.04 не проверялся |
| Сеть | [wondershaper](https://github.com/magnific0/wondershaper) | Простое ограничение полосы пропускания адаптера | bash-скрипт, ставится копированием или `make install`; релизов нет |
| Диагностика и железо | [linnix](https://github.com/linnix-os/linnix) | eBPF-наблюдаемость с AI-детекцией инцидентов | Исходники и docker-образ; релиз v0.2.0 опубликован без ассетов — ни `.deb`, ни Linux-бинарника |
| Диагностика и железо | [sysinfo](https://github.com/peterbay/sysinfo) | Скрипты сбора подробной системной информации | Набор bash/python-скриптов без упаковки: ни PyPI, ни релизов; похожие данные дают `procs`, `fastfetch` и `jc` |
| Диагностика и железо | [mcelog](https://github.com/andikleen/mcelog) | userspace-логирование аппаратных ошибок памяти CPU (MCE) | Сборка из исходников (`make`); релизов нет |
| Диагностика и железо | [bench-scripts](https://github.com/haydenjames/bench-scripts) | Набор скриптов бенчмарка Linux-сервера | Набор shell-скриптов, запускается из клона репозитория; релизов нет |
| Безопасность | [flawz](https://github.com/orhun/flawz) | TUI-браузер CVE/уязвимостей | В релизах только macOS/Windows и `flawz-installer.sh`; готового Linux-бинарника нет (`cargo build`) |
| Контейнеры и реестры | [registry-cli](https://github.com/andrey-pohilko/registry-cli) | Манипуляции с docker-registry из командной строки | Одиночный `registry.py` плюс `pip install -r requirements-*.txt`; ни пакета на PyPI, ни релизных артефактов. Альтернатива — готовый docker-образ `anoxis/registry-cli` |
| Контейнеры и реестры | [gitlab-registry-images-cleaner](https://github.com/n0madic/gitlab-registry-images-cleaner) | Чистка старых образов в GitLab registry | Одиночный python-скрипт с `requirements.txt`; пакета на PyPI и релизных ассетов нет |
| Пакетные менеджеры | [stew](https://github.com/alxrem/stew) | Независимый пакетный менеджер для скомпилированных бинарников | Только исходники (`go install`); релизов нет |
