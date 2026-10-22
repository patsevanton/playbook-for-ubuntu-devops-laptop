# TODO: бинарник не запускается на Ubuntu 26.04

Кандидаты из [`TODO-awesome-github.md`](TODO-awesome-github.md), у которых
готовый бинарник в GitHub-релизе есть, но на Ubuntu 26.04 / x86_64 он не
работает: собран против старой OpenSSL/glibc, требует библиотеку, которой в
дистрибутиве нет, или опубликован не для этой платформы.

В плейбук такие утилиты пока не берём. Пересматривать при каждом новом релизе
апстрима (и при обновлении Ubuntu): если появилась сборка под 26.04 или
бинарник стал статическим — вернуть кандидата в
[`TODO-awesome-github.md`](TODO-awesome-github.md) и поставить ролью
`binaries`.

Как читать: `- [ ]` — ждёт подходящего релиза; в заметке — что именно
сломано, ссылка на issue апстрима и чем утилита покрыта сейчас.

---

## Сеть

- [ ] **dog** — DNS-клиент с человекочитаемым цветным выводом. *(`cargo`)* —
      [ogham/dog](https://github.com/ogham/dog)
      — ⛔ не запускается: upstream мёртв с 2021 (последний релиз v0.1.0),
      бинарник динамически связан с `libssl.so.1.1`, которого нет в Ubuntu
      26.04. Симптом подтверждён в
      [ogham/dog#109](https://github.com/ogham/dog/issues/109); смежные
      открытые issue — [#98](https://github.com/ogham/dog/issues/98) и
      [#122](https://github.com/ogham/dog/issues/122). Покрывается `q` и
      `doggo` (роль `network`). Пересмотреть, если выйдет релиз со статической
      сборкой.

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
