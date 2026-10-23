# TODO: бинарник не запускается на Ubuntu 26.04

Кандидаты, у которых готовый бинарник в GitHub-релизе есть, но на
Ubuntu 26.04 / x86_64 он не работает: собран против старой OpenSSL/glibc,
требует библиотеку, которой в дистрибутиве нет, или опубликован не для этой
платформы.

В плейбук такие утилиты пока не берём. Пересматривать при каждом новом релизе
апстрима (и при обновлении Ubuntu): если появилась сборка под 26.04 или
бинарник стал статическим — взять кандидата в работу и поставить ролью
`binaries`.

Как читать: `- [ ]` — ждёт подходящего релиза; в заметке — что именно
сломано, ссылка на issue апстрима и чем утилита покрыта сейчас.

Заброшенные проекты (архив GitHub, годы без обновлений) сюда не входят: они
описаны только в [`TODO-abandoned.md`](TODO-abandoned.md).

---

## Секреты

- [ ] **teller** — облачно-независимый менеджер секретов для разработчика.
      *(`cargo`)* — [tellerops/teller](https://github.com/tellerops/teller)
      — ⛔ не запускается: последний релиз (v2.0.7, 2024) собран динамически
      против `libssl.so.1.1`, которого нет в Ubuntu 26.04; статических сборок
      и сборок под OpenSSL 3 апстрим не публикует. Отслеживается в
      [tellerops/teller#290](https://github.com/tellerops/teller/issues/290)
      (OPEN; комментарий с проверкой на 26.04 добавлен 2026-10-22).
      Покрывается `sops` + `vault` + `medusa`. Пересмотреть при новом релизе.

## Kubernetes

- [ ] **KubeDiagrams** — диаграммы архитектуры Kubernetes из манифестов,
      kustomize, Helm и состояния кластера. *(`pipx`)* —
      [philippemerle/KubeDiagrams](https://github.com/philippemerle/KubeDiagrams)
      — ⛔ не устанавливается: зависимость `pygraphviz==1.14` публикуется только
      исходниками и не собирается на Python 3.14 (`Failed building wheel for
      pygraphviz`). Покрывается `kubectl-tree` (дерево объектов) и веб-UI
      `headlamp`/`radar`. Пересмотреть, если выйдет колесо под 3.14.
- [ ] **PowerfulSeal** — chaos-тестирование Kubernetes. *(`pipx`)* —
      [bloomberg/powerfulseal](https://github.com/bloomberg/powerfulseal)
      — ⛔ не устанавливается: пакет 2020 года требует `PyYAML<6`, который
      падает на сборке под Python 3.14 (`AttributeError: 'build_ext' object
      has no attribute 'cython_sources'`). Покрывается `pumba` (chaos для
      контейнеров). Пересмотреть при обновлении зависимостей.
- [ ] **Netchecks** — проверки сетевых условий из кластера. *(`pipx`)* —
      [hardbyte/netchecks](https://github.com/hardbyte/netchecks)
      — ⛔ не запускается: пакет `netcheck` ставится, но CLI падает на Python
      3.14 с `TypeError: Secondary flag is not valid for non-boolean flag`
      (несовместимость Typer 0.9 и Click). Покрывается `knetvis` и
      `kube-burner`. Пересмотреть после обновления Typer.
- [ ] **IceKube** — поиск путей атак в кластере. *(`pipx`)* —
      [WithSecureLabs/IceKube](https://github.com/WithSecureLabs/IceKube)
      — ⛔ не запускается: CLI падает на Python 3.14 с тем же
      `TypeError: Secondary flag is not valid for non-boolean flag`
      (Typer 0.9 + Click). Покрывается `kubehound` (граф путей атак) и
      `rbac-tool`. Пересмотреть после обновления Typer.
- [ ] **kubestriker** — быстрый security-аудит Kubernetes. *(`pipx`)* —
      [vchinnipilli/kubestriker](https://github.com/vchinnipilli/kubestriker)
      — ⛔ не устанавливается: пакет 2021 года с жёсткими пинами
      (`figlet==0.0.1`, `PyYAML==5.3.1`, `prompt-toolkit==1.0.15`) — под
      Python 3.14 `pip install kubestriker` падает («No matching distribution
      found for figlet==0.0.1»). Отслеживается в
      [vchinnipilli/kubestriker#36](https://github.com/vchinnipilli/kubestriker/issues/36)
      (OPEN; про устаревший `prompt-toolkit` — [#29](https://github.com/vchinnipilli/kubestriker/issues/29)).
      Покрывается `kube-linter`, `kubeconform`, `marvin` и `kubehound`.
      Пересмотреть, если апстрим снимет пины.

## Безопасность

- [ ] **vulncov** — корреляция Semgrep-сканов с покрытием тестами. *(`pipx`)* —
      [mllamazares/vulncov](https://github.com/mllamazares/vulncov)
      — ⛔ не устанавливается: пин `faiss-cpu==1.9.0` не имеет колёс под Python
      3.14 (доступны только 1.12+), `pip install git+https://github.com/mllamazares/vulncov.git`
      падает на сборке зависимостей; пакета на PyPI
      нет. Заведено
      [mllamazares/vulncov#1](https://github.com/mllamazares/vulncov/issues/1)
      (OPEN, 2026-10-22). Пересмотреть, если апстрим ослабит пин.
