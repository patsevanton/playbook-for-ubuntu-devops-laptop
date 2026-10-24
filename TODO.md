# TODO: сводка по кандидатам — что уже есть и что ставить не нужно

Кандидаты из трендов GitHub, которые осознанно не попали в плейбук, — в
[`TODO-trend-github-not-added.md`](TODO-trend-github-not-added.md),
новая партия утилит для ноутбука из трендов за последние 3 года (срезка
2026-10-24) — в
[`TODO-trend-github-last-3-years.md`](TODO-trend-github-last-3-years.md),
opencode 2 (один агент
раздаёт задачи другим, включая pitwall и herdr) — в
[`TODO-opencode-orchestration.md`](TODO-opencode-orchestration.md), утилиты,
сознательно не ставимые плейбуком (с причинами и issue — atuin и др.) —
в [`not-install.md`](not-install.md), кандидаты без готового бинарника на
GitHub — в [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md),
кандидаты, чей бинарник не запускается на Ubuntu 26.04, —
в [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md),
серверные приложения, утилиты, Helm-чарты и агенты на нодах Kubernetes
(DaemonSet/static Pod, не на ноутбук) — в
[`TODO-server-apps.md`](TODO-server-apps.md), всё из Ansible Galaxy (роли,
коллекции, готовые playbook'и), которое плейбук не ставит, — в
[`TODO-ansible-galaxy-not-install.md`](TODO-ansible-galaxy-not-install.md),
кандидаты из списка binenv (готовые релизные бинарники), которых в плейбуке
пока нет, — в [`TODO-binenv-not-added.md`](TODO-binenv-not-added.md),
заброшенные проекты (архив GitHub или годы без обновлений) — в
[`TODO-abandoned.md`](TODO-abandoned.md), живые кандидаты из статьи Habr
«Полезные консольные Linux утилиты» (habr.com/ru/articles/553000) — в
[`TODO-habr-553000-not-added.md`](TODO-habr-553000-not-added.md); остальные
утилиты оттуда разложены по своим трекерам — `TODO-abandoned.md`,
`TODO-no-prebuilt-binary.md`, `TODO-server-apps.md`, `not-install.md`.
Не кандидат, а план работ — генератор архитектурных схем из
Terraform/Terragrunt: [`TODO-diagrams.md`](TODO-diagrams.md).

Источник подборки: все репозитории со ⭐ аккаунта (2057 шт.), из них отобраны
~570 похожих на локально устанавливаемые утилиты и вручную вычищены до списков
в файлах выше. Полные данные обхода (все 2057 звёзд + 571 кандидат + ссылки на
релизы): `/home/magnit/Public/starred-tools/data/` (`install_hints.tsv`,
`install_report.md`).

Дедупликация 2026-10-18: каждый кандидат описан ровно в одном файле-трекере.
Общий список кандидатов разобран полностью: всё, что ставится, уже в ролях и
`README.md`, отказы с причинами — в [`not-install.md`](not-install.md) и
отдельных трекерах, поэтому сам файл со списком удалён. Здесь остались
справочные списки: что уже ставит плейбук, что не ставится на ноутбук и что
является просто подборками.

Дедупликация 2026-10-22: серверные приложения, сервисы и Helm-чарты собраны в
[`TODO-server-apps.md`](TODO-server-apps.md) (из этого файла и из
`TODO-no-prebuilt-binary.md`); тогда же нодовые агенты Kubernetes
(DaemonSet/static Pod) вынесены из `TODO-no-prebuilt-binary.md` в отдельный
`TODO-node-tools.md`, чтобы не смешивать их с кластерными сервисами уровня
Deployment и с кандидатами, у которых просто нет бинарника.

Дедупликация 2026-10-23: `TODO-node-tools.md` влит в
[`TODO-server-apps.md`](TODO-server-apps.md) — причина отказа у нодовых агентов
та же, что у остальных кластерных компонентов (не ноутбук), а признак
«DaemonSet на нодах» сохранён пометкой в описании кандидата, а не отдельным
файлом.

Реестр 2026-10-23: заброшенные проекты (архив GitHub, годы без обновлений)
описаны только в [`TODO-abandoned.md`](TODO-abandoned.md). Из остальных
трекеров такие кандидаты убраны, там остались только ссылки на этот файл:
один заброшенный проект не должен быть описан в нескольких местах.

Разбор 2026-10-23: из [`not-install.md`](not-install.md) убраны кандидаты, у
которых причина отказа техническая, а не «не нужен». HolmesGPT (Python 3.14)
ушёл в [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md);
kcmsu, Kubefirst, Radius, Jenkins X, k8s-tun и Scaphandre (агент на хосте) — в
[`TODO-server-apps.md`](TODO-server-apps.md). Krane и Kubes (только gem),
kubectl-kubesec (krew) сначала ушли в
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md), но в тот же день
вернулись в `not-install.md`: у всех трёх есть аналоги среди уже
поставленного. werf и kubedog не рассматриваются, odo перенесён в
[`TODO-abandoned.md`](TODO-abandoned.md) (репозиторий в архиве).
В `not-install.md` остались платные продукты, дубли уже установленного,
bash-скрипты без релизов и отказы из-за багов апстрима.

---

Добавлены 2026-10-22 из трендов GitHub (топ-15
приложений по звёздам среди оставшихся кандидатов; `dev-sec` — коллекция
Ansible Galaxy, а не приложение, поэтому вместо неё в набор вошёл следующий по
звёздам `terrascan`): `gitui`, `vhs`, `dagger`, `limactl`/`lima`,
`pgcli`, `harlequin`, `terraformer`, `terrascan`, `steampipe`, `kubectl-ai`,
`k3d`, `telepresence`, `tilt`, `mirrord`.

Добавлены 2026-10-22 из тех же трендов следующим по звёздам (снова без
`dev-sec`): `devspace`, `rustnet`, `lazysql`, `kubent`. `devbox` и `flox`
удалены: Nix не используется.

Добавлены 2026-10-23 из тех же трендов — следующие 15 по звёздам среди
оставшихся кандидатов (без заброшенных — см.
[`TODO-abandoned.md`](TODO-abandoned.md), мета-пакета `ansible-dev-tools`,
который дублирует уже поставленные части, и коллекции Ansible Galaxy
`dev-sec`): `polaris`, `kube-score`, `kubectl-neat`, `kubectl-validate`,
`nova`, `helm-secrets`, `ansible-navigator`, `ansible-builder`,
`ansible-creator`, `ansible-doctor`, `ara`, `tenv`, `atmos`, `tfupdate`,
`tfsort`. `inframap` уже стоял в плейбуке до этого разбора (роль `iac`, тег
v0.8.0).

Добавлены 2026-10-23 из тех же трендов — последние кандидаты с готовым пакетом
Ubuntu: `btop` (1.4.6) и `ncdu` (1.22), оба ролью `shell` через apt. Трекер
трендов после этого разобран полностью и удалён; кандидаты, которые осознанно
не добавлены (`ansible-dev-tools` — мета-пакет, `dev-sec` — коллекция Galaxy),
перечислены с причинами в
[`TODO-trend-github-not-added.md`](TODO-trend-github-not-added.md). Заброшенные
из этой волны — в [`TODO-abandoned.md`](TODO-abandoned.md).

Добавлены 2026-10-24 из трендов GitHub за последние 3 года: `toolong`,
`ptcpdump`, `superfile` (бинарник `spf`), `rainfrog`, `posting`, `lfk`,
`zizmor`, `landrun`, `lazyssh`. `bluetui` не ставится — причина в
[`not-install.md`](not-install.md).

Добавлены 2026-10-24 по запросу: `dbx` (роль `postgres`), `omnyssh` — TUI
`omny` и GUI `OmnySSH` (роль `network`), `RackPeek` — команда `rpk` (роль
`iac`), `kula` (роль `monitoring`), `tirith` и `dcg` (роль `guardrails`,
dcg дополнительно подключён к OpenCode плагином).

Добавлены 2026-10-24 из трендов GitHub за последние 3 года по запросу:
`serpl` (роль `shell`), `whosthere` (роль `network`), `khi` (роль
`kubernetes`), `sonar` (роль `shell`). Все — одиночные бинарники из
GitHub-релизов, без root и setcap. `sonar` частично дублирует уже
поставленный `witr` (тот же вопрос «кто держит порт»), но добавлен
сознательно: `witr` даёт разовый ответ в CLI/JSON, `sonar` — живой
TUI-дашборд портов с kill и перезапуском, плюс MCP-сервер для
AI-агентов.

Добавлены 2026-10-24 по запросу из классических apt-пакетов (не из трендов
GitHub): `rsync` (роль `base`), `bpftrace` (роль `ebpf`), `socat` и `iperf3`
(роль `network`), `shfmt` (роль `shell`). Эти пробелы не были в трекерах,
потому что `TODO-trend-*` собираются по звёздам GitHub и не учитывают
обычные пакеты Ubuntu.

Добавлены 2026-10-24 по запросу, вторая партия: `lsof` (роль `base`),
`sysstat` (роль `shell`), `ethtool` (роль `network`), `skopeo` (роль
`docker`), `yc` (роль `iac`). `yc` ставится отдельной задачей
`roles/iac/tasks/yc.yml`: апстрим публикует бинарники на своём storage, а не
в GitHub-релизах, поэтому роль `binaries` не подходит. `skopeo`, как и
`rsync` из первой партии, уже упоминался в README как существующий, хотя
ничего его не ставило.

Добавлены 2026-10-24 по запросу, третья партия — пробелы в диагностике и
переносе данных: `strace` и `etckeeper` (роль `base`), `pv` (роль `shell`),
`sshfs` и `rclone` (роль `network`). `strace` дополняет `fatrace` (тот видит
только доступ к файлам и только от root) и eBPF-утилиты (в основном сеть).
`etckeeper` держит `/etc` в локальном git: apt-хук коммитит до и после
операций с пакетами, systemd-таймер добавляет ручные правки; удалённого
репозитория нет, потому что в `/etc` попадаются секреты. `rclone` дополняет
`s5cmd`: тот быстрее на S3, rclone работает с WebDAV/SFTP/Drive/OneDrive.

`smartmontools`, `nvme-cli` и `lm-sensors` из этой же партии уже добавлены
ранее в роль `hardware`.

`pueue` (очередь долгих задач) и `clipse` (история буфера обмена) в этот
набор не вошли: в репозитории Ubuntu 26.04 их нет, а с GitHub пришлось бы
брать два бинарника плюс systemd-юнит (`pueue`/`pueued`) или проверять
Wayland-интеграцию. Пакеты apt дают ту же пользу без этой цены.

Добавлены 2026-10-24 по запросу, четвёртая партия — данные, API и
репозитории с большими файлами: `redis-tools` и `kcat` (роль `shell`),
`grpcurl` и `buf` (роль `network`), `inxi` (роль `hardware`), `git-lfs`
(роль `base`). `redis-cli` и `kcat` — единственные клиенты Redis и Kafka в
плейбуке: `dbx` даёт GUI, но в скриптах и на серверах нужны именно CLI.
`grpcurl` — первый gRPC-клиент (до этого gRPC был только как приёмник OTLP
у `otel-tui`); `buf` идёт в паре с ним для работы с `.proto`. `inxi`
дополняет `smartctl`/`sensors` общей картиной железа. `git-lfs` нужен,
чтобы `git clone` репозиториев с ассетами не оставлял pointer-заглушки.

NetBird (`netbird` + `netbird-ui`) добавлен 2026-10-24 в роль `network` из
официального apt-репозитория `pkgs.netbird.io` (PPA у проекта нет). Остальные
VPN-клиенты (`wireguard-tools`, `openvpn`, плагины NetworkManager,
`sshuttle`) в плейбук пока не добавлены — решение отложено.

Добавлены 2026-10-24 по запросу, четвёртая партия: `postgresql-client` (роль
`postgres`, даёт `psql`/`pg_dump`/`pg_restore`), `docker-buildx-plugin` (роль
`docker`), `pciutils`, `usbutils` и `dmidecode` (роль `hardware`), `perf`
через `linux-tools-generic` (роль `shell`). `postgresql-client`, как `rsync`
и `skopeo` раньше, уже подразумевался в README («pgcli дополняет psql»), но
ничего его не ставило; `docker-buildx-plugin` не входил в `docker-ce-cli`,
поэтому `docker buildx` на чистой машине не работал.

Добавлены 2026-10-24 по разбору трекеров, где кандидаты были перечислены, но
решение по ним не принято ([`TODO-habr-553000-not-added.md`](TODO-habr-553000-not-added.md),
[`TODO-binenv-not-added.md`](TODO-binenv-not-added.md)): `mtr` (роль
`network`) и `sos` (роль `base`) — пакеты apt; `kubestr`, `imgpkg` и `cmctl`
(роль `kubernetes`), `pgmetrics` (роль `postgres`) — бинарники из
GitHub-релизов. `mtr` — единственный инструмент с текстовым отчётом по всем
хопам (у `trippy` и `gping` только TUI/график); `sos` собирает диагностику
одним архивом для тикета; `kubestr` проверяет хранилище кластера (StorageClass,
CSI, реальные IOPS), чего не делает ни одна из утилит роли; `imgpkg`
завершает набор Carvel рядом с `ytt` и `kbld`; `cmctl` заменяет
`kubectl`-простыни по объектам cert-manager; `pgmetrics` снимает состояние
PostgreSQL в текст/JSON, тогда как `pgcenter` — интерактивный монитор.

`HolmesGPT` не ставится: последний релиз требует Python `<3.14`, а в Ubuntu
26.04 системный Python 3.14 — см.
[`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md) и
[issue #2649](https://github.com/HolmesGPT/holmesgpt/issues/2649).

`argocd`, `pulumi` и `kubebuilder` не ставятся: Argo CD не используется, IaC —
на Terraform, Kubernetes-операторы не создаются
(см. [`not-install.md`](not-install.md)).

`ansible-core` ставится один раз вручную при bootstrap (см. `README.md`).

`pitwall` пока не ставится плейбуком, но уже не из-за бага: issue с вводом
русских букв закрыт 2026-10-19, фикс вышел в `v0.1.0-beta.12`. Решение о
добавлении ждёт разбора оркестрации в opencode 2, раздел 6:
[`TODO-opencode-orchestration.md`](TODO-opencode-orchestration.md).

`Slightly Parallel` ([`NMar33/slightly-parallel`](https://github.com/NMar33/slightly-parallel))
— **разобраться, нужен ли он в плейбуке**, решение отложено. Локальный дашборд
для работы с несколькими проектами и агентами сразу: записывает все чаты
(хуки Claude Code/Codex/Cursor, плагин opencode), держит канбан задач, собирает
стендапы и чекпоинты по команде `#scan`, данные — плоские файлы в
`~/.slightly-parallel`, сервер слушает `127.0.0.1:8123` без авторизации
(один человек, один компьютер). Пакетов нет: ни на PyPI (значит, pipx не
подходит), ни `.deb`, ни релизных бинарников — только `git clone` и
`./run.sh`, который сам создаёт `backend/.venv` (нужны Python 3.11+ и
`python3-venv`). При подключении проекта он пишет в его `AGENTS.md`/
`CLAUDE.md`, ставит хуки и запись MCP, поэтому подключение остаётся ручным
шагом в UI, а не задачей плейбука. Если добавлять — только ролью: `git clone`
в `~/opt` + systemd-юнит `--user` (`sp.sh autostart on`), с флагом
`slightly_parallel_install: false` по умолчанию.

`vibeforge` ([`mmm-byte/vibeforge`](https://github.com/mmm-byte/vibeforge))
— **в будущие установки**, решение отложено (2026-10-24). CLI-конвейер для
AI-разработки приложений: семь стадий (`clarify` → `architect` → `scaffold` →
`build` → `harden` → `document` → `ship`), между ними quality gate; семь типов
проектов (webapp, api, cli_tool, bot, extension, data_pipeline, mobile);
LLM-провайдеры — Ollama по умолчанию (офлайн), OpenAI и Anthropic. Лицензия
MIT, Python ≥ 3.9, точка входа — `vibeforge`.

Как ставить: пакет питоновский, поэтому ролью `pipx_apps` из git —
`pipx install git+https://github.com/mmm-byte/vibeforge`. `pip install
vibeforge` из README сейчас не работает: на PyPI пакета нет (404), релизов и
тегов на GitHub тоже нет, последний коммит — 2026-04-09. Отсюда и пауза:
установка из плавающего `main` не пинится и не обновляется, а сам конвейер
генерирует и запускает код в чужих проектах (сборка Docker, тесты,
self-healing), то есть это рабочий инструмент под задачу, а не утилита на
один бинарник. Пересматривать, когда появится релиз на PyPI или хотя бы тег:
тогда ставим `pipx_apps` с зафиксированной версией.

Кандидаты, которые дублируют этот список и потому не берутся: `ktail`
(≈ `stern`), `kube-switch` (≈ `kubectx`), `dyff` (≈ `diffyml`).

## garden — удалён из плейбука, вернуться к рассмотрению

`garden` ([`garden-io/garden`](https://github.com/garden-io/garden)) — оркестратор
разработки для Kubernetes и контейнеров (сборка, деплой, тесты, логи в одном CLI) —
удалён из роли `kubernetes` 2026-10-24. Причина: при каждом запуске он создаёт
каталог `.garden/` в текущем рабочем каталоге (`metadata.json` и журналы с меткой
времени), поэтому даже проверка `garden version` после прогона мусорила в
репозитории плейбука. Обёртка с `mktemp` в `playbook.yml` лечила симптом, а не
причину, и утилита убрана целиком.

Вернуться к вопросу, если понадобится оркестрация dev-окружений в Kubernetes.
Тогда сначала решить, куда писать состояние (каталог запуска или настройка
расположения `.garden/`), и запускать `garden` только из каталога проекта, а не
из домашнего каталога или репозитория плейбука. Пока в плейбуке остаются
`okteto`, `devspace`, `tilt`, `skaffold`, `mirrord` и `telepresence` — они
закрывают ту же задачу «разработка в Kubernetes» без побочных файлов.

## Обновление одиночных .deb — рассмотреть

Разбор 2026-10-23. Плейбук ставит .deb из GitHub-релизов, но `apt` такие
пакеты не обновляет: источник в dpkg не фиксируется, `apt-cache policy` для
них показывает единственную запись `100 /var/lib/dpkg/status`, то есть
кандидат всегда равен установленной версии. Отсюда варианты:

- **deb-get** — каталог ~460 сторонних приложений, `deb-get update` показывает
  ожидающие обновления, `deb-get upgrade` ставит новые .deb и обновляет
  сторонние apt-репозитории. Целесообразен, если появятся приложения, которые
  ставятся руками, вне плейбука: из .deb плейбука в каталоге есть только
  `sniffnet`, остальные пришлось бы описывать в `/etc/deb-get/99-local.d/`
  (bash-рецепты, второй набор определений в чужом формате плюс отдельный
  реестр `/etc/deb-get/installed`, который к тому же не видит пакеты,
  поставленные плейбуком). Проверено: 0.4.7 работает на Ubuntu 26.04
  (`resolute`), поддержка добавлена в PR wimpysworld/deb-get#1781, issue
  wimpysworld/deb-get#1860 закрыт этим релизом.
- **updatecli** — декларативный аналог renovate (`source → condition → target`,
  запуск в CI, PR с бампом версии). Имеет смысл в gitops-репозитории, где
  версии пиннятся; в этом плейбуке пинить нечего — 25 файлов задач берут
  `releases/latest` на момент запуска.
- **topgrade** — оркестратор «обновить всё»: apt, snap, flatpak, brew, pipx,
  npm, cargo плюс свои команды. Рассмотреть как интерактивную команду на
  ноутбуке; .deb с GitHub он не детектит — это уже закрывает
  `roles/kubernetes/tasks/github_deb.yml`.

Решение на 2026-10-23: deb-get и updatecli в плейбук не добавляем, обновление
.deb делает `tasks/github_deb.yml` (сравнение версии из `releases/latest` с
`dpkg-query`). Вернуться к deb-get, если доля приложений, ставящихся вне
плейбука, вырастет.

## Не для локальной установки (сервер/платформа) — только если понадобится

Из звёзд также попали self-hosted-платформы, серверные сервисы и Helm-чарты —
они не ставятся «на ноутбук» одной командой: нужен отдельный хост, Docker
Compose или Helm-релиз. Полный список с описаниями и ссылками — в
[`TODO-server-apps.md`](TODO-server-apps.md).

`VictoriaMetrics` сюда не относится: у него есть single-binary режим для
локального запуска, поэтому он стоит в роли `monitoring` (из VictoriaMetrics —
только `vmalert-tool`).

Нодовые агенты Kubernetes (DaemonSet/static Pod: `goldpinger`, `node-exporter`,
`kured`, eBPF-агенты и т.п.) — в том же
[`TODO-server-apps.md`](TODO-server-apps.md), с пометкой «DaemonSet на нодах».

## Списки и обучающие материалы (не утилиты)

`awesome`, `system-design-primer`, `awesome-sysadmin`, `90DaysOfDevOps`,
`DevOps-Roadmap`, `awesome-gh-extensions`, `awesome-tf` — полезны как подборки,
но устанавливать нечего.

Остальные подборки (`devops-exercises`, `DevOps-Bash-tools`, `kubetools`,
`modern-unix`, `awesome-tunneling`, `awesome-prometheus-alerts`,
`container-security-checklist`, `linux-network-performance-parameters`,
`boilerplates`, `home-ops`/`homelab-gitops-k3s`) ставить нечего — это
материалы для чтения. Подборка `kubetools` разобрана: что подошло — в
`README.md`, причины отказов — в [`not-install.md`](not-install.md) и
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md).
