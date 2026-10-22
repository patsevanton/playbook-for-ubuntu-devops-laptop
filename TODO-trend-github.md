# TODO: трендовые утилиты из GitHub — кандидаты для плейбука

Список кандидатов, которые **активно развиваются в 2026 году** и ставятся
локально на Ubuntu 26.04 / x86_64 как CLI/TUI-утилиты. Уклон — DevOps: Kubernetes,
Ansible, Terraform/Terragrunt, IaC, секреты, плюс полезное для обычного
Linux-пользователя. Прежний общий список кандидатов разобран полностью и
удалён; дубли с уже установленным плейбуком здесь не повторяются (см. раздел
«Пересечение» ниже).

Как читать:
- `- [ ]` — кандидат, ещё не рассмотрен.
- Звёзды — снимок GitHub на **2026-10-18**; у всех кандидатов есть коммиты
  за последние месяцы (проверено по `pushed_at`).
- В скобках — способ установки: `deb` (готовый пакет в GitHub-релизе),
  `apt` (уже есть в репозитории Ubuntu), `AppImage`, `tar.gz`/`zip` (готовый
  бинарник из релиза, ложится в `bin_dir` через роль `binaries`),
  `pipx` / `npm` / `go` / `cargo` (сборка или установка из исходников),
  `script` (официальный install.sh).

Проверялось: наличие `.deb`/архивов в последнем релизе через `releases/latest`
и `expanded_assets` (без GitHub API), актуальность — по `pushed_at`.

> Приоритет для добавления в плейбук: у кого есть `.deb` или готовый
> `linux-amd64` архив в релизе — те ставятся ролью `binaries` одной строкой.

---

## Kubernetes: диагностика и валидация манифестов

- [ ] **kubectl-ai** — AI-ассистент: генерирует и правит kubectl-команды и манифесты. *(7.6k ⭐, `tar.gz`)*
- [ ] **k8sgpt** — сканирует кластер и объясняет проблемы человеческим языком (LLM). *(8.3k ⭐, `deb`)*
- [x] **kubeconform** — быстрая валидация манифестов против схем (замена `kubeval`). *(3.2k ⭐, `tar.gz`)*
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [x] **kube-linter** — статический анализ YAML на антипаттерны и best practices. *(3.5k ⭐, `tar.gz`)*
      — ✅ добавлен в плейбук (роль `kubernetes`).
- [ ] **kube-score** — оценка манифестов по чек-листу: probes, requests/limits, securityContext. *(3.1k ⭐, `tar.gz`)*
- [ ] **popeye** — «spinach» для кластера: находит мёртвые ресурсы, ошибки конфигов. *(6.5k ⭐, `deb`)*
- [ ] **kubescape** — сканер безопасности кластера по NSA/CIS-профилям. *(11.8k ⭐, `deb`)*
- [ ] **polaris** (FairwindsOps) — аудит и автофикс конфигов на соответствие best practices. *(3.4k ⭐, `tar.gz`)*
- [ ] **nova** (FairwindsOps) — находит Helm-релизы с устаревшими версиями чартов. *(0.9k ⭐, `tar.gz`)*
- [ ] **kubent** (kube-no-trouble) — показывает, что сломается при апгрейде кластера (removed APIs). *(3.7k ⭐, `tar.gz`)*
- [ ] **kubectl-neat** — убирает из `kubectl get -o yaml` служебный мусор (`managedFields`, `status`). *(2.1k ⭐, `tar.gz`)*
- [ ] **kubectl-validate** (kubernetes-sigs) — офлайн-валидация манифестов родными схемами. *(0.3k ⭐, `tar.gz`)*

## Kubernetes: работа с кластером и dev-окружение

- [ ] **vcluster** — виртуальные кластеры внутри неймспейса: изоляция без цены реального кластера. *(11.4k ⭐, `script` install-standalone.sh)*
- [ ] **mirrord** — запускает локальный процесс «как будто он под в кластере» (без сборки образа). *(5.4k ⭐, `zip`)*
- [ ] **glasskube** — пакетный менеджер для Kubernetes с GUI и CLI (dependency-aware). *(3.5k ⭐, `deb`)*
- [ ] **devspace** — деплой и hot-reload приложения прямо в кластер. *(5.2k ⭐, `bin` linux-amd64)*
- [ ] **kubeshark** — Wireshark-подобный анализ трафика внутри кластера (eBPF). *(12.1k ⭐, `bin` linux-amd64)*
- [ ] **k3d** — k3s в Docker: быстрые локальные кластеры для тестов. *(6.6k ⭐, `bin` linux-amd64)*
- [x] **kind** — Kubernetes IN Docker: референсные локальные кластеры. *(15.5k ⭐, `bin` linux-amd64)*
      — ✅ добавлен в плейбук (роль `kubernetes`, одиночный бинарник
      kind-linux-amd64).
- [ ] **telepresence** — проксирование локального сервиса в удалённый кластер. *(7.3k ⭐, `deb`)*
- [ ] **skaffold** — цикл сборка→деплой→проверка для Kubernetes. *(15.9k ⭐, `bin` linux-amd64)*
- [ ] **tilt** — live-update мультисервисных приложений в k8s с веб-UI. *(10.1k ⭐, `tar.gz`)*

## Helm / GitOps

- [x] **helmfile** — декларативное описание набора Helm-релизов (`helmfile.yaml`). *(5.2k ⭐, `tar.gz`)*
      — ✅ добавлен в плейбук (роль `kubernetes`, поверх уже стоящего `helm`).
- [ ] **helm-diff** (databus23) — плагин `helm diff upgrade`: что изменится до применения. *(3.5k ⭐, `tgz`)*
- [ ] **helm-secrets** (jkroepke) — плагин helmfile/helm для шифрования values через sops. *(2k ⭐, `tar.gz`)*

## Ansible

- [ ] **ansible-navigator** — TUI для запуска и разбора плейбуков через execution environments. *(0.6k ⭐, `pipx`)*
- [ ] **ansible-builder** — сборка образов Execution Environment (ansible-builder). *(0.4k ⭐, `pipx`)*
- [ ] **ansible-creator** — генератор каркаса коллекций/ролей/плагинов. *(0.2k ⭐, `pipx`)*
- [ ] **ansible-dev-tools** — мета-пакет: ставит molecule, navigator, builder, creator, lint разом. *(`pipx`)*
- [ ] **ansible-doctor** — генерирует документацию ролей по комментариям и метаданным. *(`pipx`)*
- [ ] **ara** — запись и веб-просмотр прогонов Ansible (что где изменилось). *(2k ⭐, `pipx` + сервер)*
- [ ] **dev-sec hardening collection** — готовая коллекция харденинга Linux/SSH/nginx для плейбуков. *(5.5k ⭐, `ansible-galaxy`)*

## Terraform / OpenTofu / Terragrunt

- [ ] **terramate** — оркестрация стэков IaC: change detection, генерация кода, GitOps. *(3.6k ⭐, `deb`)*
- [ ] **atmos** (Cloud Posse) — Terraform-оркестратор: DRY-конфиги окружений поверх Terragrunt. *(1.4k ⭐, `deb`)*
- [ ] **tenv** — один менеджер версий для tofu/terraform/terragrunt/terramate/atmos. *(1.5k ⭐, `deb`)*
- [ ] **tfupdate** — обновляет пины версий провайдеров/модулей в конфигах. *(`tar.gz`)*
- [ ] **tfsort** (AlexNabokikh) — сортирует блоки `variable`/`output`/`locals`/`terraform` в `.tf`. *(`tar.gz`)*
- [ ] **terraformer** — генерирует Terraform-код из уже существующей инфраструктуры (reverse). *(14.5k ⭐, `bin` terraformer-<cloud>-linux-amd64)*
- [ ] **infracost** — считает стоимость изменений из `terraform plan` прямо в PR. *(12.6k ⭐, `tar.gz`)*
- [x] **checkov** — статический анализ IaC (Terraform/K8s/Docker/CloudFormation) на мисконфиги. *(9.1k ⭐, `pipx` / `zip`)*
      — ✅ добавлен в плейбук (роль `iac`, через pipx из PyPI).
- [ ] **terrascan** — сканер IaC на нарушения политик безопасности. *(5.2k ⭐, `tar.gz`)*
- [ ] **inframap** — рисует граф ресурсов из tfstate/HCL. *(`go build` — релизных бинарников нет)*
- [ ] **terraform-ls** — LSP-сервер для HCL (автодополнение в редакторе). *(`go build` — релизных бинарников нет)*

## Секреты, supply chain, безопасность

- [ ] **trivy** — универсальный сканер: образы, файловые системы, IaC, SBOM, секреты. *(38.5k ⭐, `deb`)*
- [x] **cosign** (sigstore) — подпись и проверка контейнерных образов и артефактов. *(6.4k ⭐, `deb`)*
      — ✅ добавлен в плейбук (роль `secrets`, одиночный бинарник cosign-linux-amd64).
- [x] **syft** — генерация SBOM из образов и директорий. *(9.7k ⭐, `deb`)*
      — ✅ добавлен в плейбук (роль `secrets`, бинарник из тарболла
      syft_{version}_linux_amd64.tar.gz).
- [x] **trufflehog** — поиск и **проверка** живых секретов в git-истории. *(28.6k ⭐, `tar.gz`)*
      — ✅ добавлен в плейбук (роль `secrets`, бинарник из тарболла
      trufflehog_{version}_linux_amd64.tar.gz).
- [ ] **age** — простое современное шифрование файлов (основа sops). *(24k ⭐, `apt` 1.2.1)*
- [x] **hadolint** — линтер Dockerfile (в т.ч. проверка inline-shell через shellcheck). *(12.5k ⭐, `bin` hadolint-linux-x86_64)*
      — ✅ добавлен в плейбук (роль `docker`).
- [ ] **lynis** — аудит безопасности и харденинга Linux-системы. *(16.5k ⭐, `apt`)*

## Терминал, shell, Linux-продуктивность

- [x] **yazi** — очень быстрый TUI-файловый менеджер на Rust (async I/O, превью). *(42.9k ⭐, `deb`)*
      — ✅ добавлен в плейбук (роль `shell`, бинарник из zip-архива
      yazi-x86_64-unknown-linux-gnu.zip).
- [ ] **television** (`tv`) — универсальный fuzzy-finder (замена/дополнение к fzf) с каналами поиска. *(6.4k ⭐, `deb`)*
- [x] **bottom** (`btm`) — кросс-платформенный монитор процессов/CPU/сети с графиками. *(14.1k ⭐, `deb`)*
      — ✅ добавлен в плейбук (роль `shell`, команда `btm`, бинарник из
      тарболла bottom_x86_64-unknown-linux-gnu.tar.gz).
- [ ] **duf** — `df` «для людей»: таблица дисков и точек монтирования. *(15.3k ⭐, `deb` / apt 0.9.1)*
- [x] **gping** — `ping` с графиком задержек в терминале. *(12.7k ⭐, `tar.gz` / apt 1.20.1)*
      — ✅ добавлен в плейбук (роль `network`, бинарник из тарболла
      gping-Linux-musl-x86_64.tar.gz; тег релиза — `gping-v…`).
- [ ] **witr** — трассировка: почему запущен процесс / кто занял порт / чей файл. *(22.7k ⭐, `deb`)*
- [ ] **rustnet** — сетевой мониторинг по процессам в TUI (глубокий разбор пакетов). *(5.1k ⭐, `deb`)*
- [x] **glow** — рендер Markdown прямо в терминале (для README и заметок). *(27.7k ⭐, `deb` / apt 2.1.1)*
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла
      glow_{version}_Linux_x86_64.tar.gz).
- [ ] **vhs** — записывает терминальные GIF/видео из сценария (демо и документация). *(21.1k ⭐, `deb`)*
- [x] **hyperfine** — бенчмарк команд с прогревом и статистикой. *(apt 1.19 / `deb` 2.0)*
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла
      hyperfine-v{version}-x86_64-unknown-linux-musl.tar.gz).
- [ ] **btop** — красивый монитор ресурсов (замена top/htop). *(apt 1.4.6)*
- [ ] **ncdu** — интерактивный анализ занятого места на диске. *(apt 1.22)*

## Рантаймы, сборка, версии

- [x] **mise** — один менеджер версий для node/python/go/terraform/… + задачи и env. *(35.1k ⭐, `tar.gz` linux-x64)*
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла
      mise-v{version}-linux-x64.tar.gz).
- [x] **just** — командный раннер (Makefile без боли): рецепты в `justfile`. *(36.3k ⭐, `apt` / `tar.gz`)*
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла
      just-{version}-x86_64-unknown-linux-musl.tar.gz).
- [x] **typos** — быстрый поиск опечаток в коде с автофиксом. *(4.2k ⭐, `tar.gz`)*
      — ✅ добавлен в плейбук (роль `git`).
- [x] **act** — локальный запуск GitHub Actions в Docker. *(72.4k ⭐, `tar.gz` Linux_x86_64)*
      — ✅ добавлен в плейбук (роль `git`, пара к `glci` для GitLab CI).
- [x] **k6** — нагрузочное тестирование сценариями на JS. *(32k ⭐, `deb`)*
      — ✅ добавлен в плейбук (роль `network`, бинарник из тарболла
      k6-v{version}-linux-amd64.tar.gz).
- [x] **jj** (jujutsu) — новая VCS от авторов Mercurial, совместима с git-репозиториями. *(32.1k ⭐, `tar.gz`)*
      — ✅ добавлен в плейбук (роль `git`, бинарник из тарболла
      jj-v{version}-x86_64-unknown-linux-musl.tar.gz).
- [ ] **gitui** — быстрый TUI для git на Rust. *(22.6k ⭐, `tar.gz` linux-x86_64)*
- [ ] **dagger** — движок CI-пайплайнов как код, запускается локально и в CI. *(16.3k ⭐, `tar.gz`)*
- [ ] **devbox** — изолированные dev-окружения из Nix-пакетов без изучения Nix. *(12.4k ⭐, `tar.gz`)*
- [ ] **flox** — детерминированные окружения на базе Nix. *(4.2k ⭐, `script` — install.flox.dev)*
- [ ] **lima** — Linux-VM поверх Linux (гоняет контейнеры без Docker Desktop). *(22.1k ⭐, `tar.gz`)*

## Данные, логи, API и БД

- [ ] **steampipe** — SQL-запросы к облакам и API без ETL (`select * from aws_s3_bucket`). *(8k ⭐, `deb`)*
- [x] **duckdb** — встраиваемая аналитическая СУБД: SQL по Parquet/CSV/JSON. *(42.2k ⭐, `zip`)*
      — ✅ добавлен в плейбук (роль `shell`, бинарник из тарболла
      duckdb-cli-linux-amd64.tar.gz).
- [x] **doggo** — современный DNS-клиент (DoH/DoT/DoQ, цветной вывод). *(4.5k ⭐, `tar.gz`)*
      — ✅ уже был в плейбуке до этого разбора (роль `network`).
- [ ] **pgcli** — psql с автодополнением и подсветкой. *(13.4k ⭐, `pipx`)*
- [ ] **harlequin** — SQL-IDE в терминале (DuckDB, Postgres, MySQL и др.). *(6.5k ⭐, `pipx`)*
- [ ] **lazysql** — TUI-клиент к нескольким СУБД сразу. *(4.4k ⭐, `tar.gz`)*

---

## Уже устанавливается плейбуком — НЕ дублировать

Полный список уже устанавливаемого плейбуком (и кандидатов-дублей, которые
поэтому не берутся) — в [`TODO.md`](TODO.md).

## Пересечение с уже установленным плейбуком (здесь не повторяются)

При дедупликации 2026-10-18 из этого файла в общий список кандидатов (разобран
и удалён) ушли дубли: `kftray`, `chart-testing`, `molecule`, `delta`,
`difftastic`, `dust`, `hurl`, `xh`. Ранее не повторялись здесь по той же
причине:

`kubecolor`, `kube-capacity`, `ktop`, `kubefetch`, `ktail`, `kubebox`,
`kubectl-view-secret`, `kubectl-df-pv`, `kubectl-browse-pvc`, `pluto`, `dyff`,
`headlamp`, `kubewall`, `ytt`, `kbld`, `timoni`, `kubevpn`, `txeh`, `netobserv-cli`,
`dive`, `dry`, `ctop`, `decompose`, `copacetic`, `oras`, `s5cmd`, `dasel`, `jc`,
`fx`, `jnv`, `octosql`, `gomplate`, `gonzo`, `hl`, `lazyjournal`,
`json-log-viewer`, `logdy-core`, `otel-tui`, `bpftop`, `ecapture`, `linnix`,
`tfvar`, `prettyplan`, `terraform-tui`, `tpm`, `sops`, `grype`, `flawz`, `mfa`,
`eget`, `binup`, `stew`, `dra`, `zellij`, `mcfly`, `navi`, `cheat`,
`tldr`, `gum`, `shellcheck`, `hstr`, `worktrunk`, `spacer`, `shellclear`,
`dusage`, `gdu`, `lf`, `fastfetch`, `procs`, `auto-cpufreq`,
`mcelog`, `wondershaper`, `sysinfo`, `lazygit`, `lumen`, `opencommit`,
`gptcommit`, `CodeGPT`, `ghgrab`, `glaball`, `bandwhich`, `sniffnet`, `trippy`,
`q`, `dog`, `tcping`, `termshark`, `kyanos`, `oryx`, `pktstat-bpf`,
`certificate-ripper`, `showcert`, `gossl`, `ssh-audit`, `httptap`, `sshx`,
`EternalTerminal`, `gotty`, `websocat`, `assh`.

Часть из них после разбора вынесена в отдельные трекеры: `flawz` — в
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md), `dog`, `teller` —
в [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md),
`mcelog` — в [`not-install.md`](not-install.md). `stew`, `wondershaper` и
`EternalTerminal` разобраны в конце `TODO-no-prebuilt-binary.md`.
`LACT` добавлен в плейбук (роль `hardware`) после появления сборки под
Ubuntu 26.04 в релизе v0.10.1.

---

## Не для локальной установки (сервер/платформа)

Попавшие в тренды сервисные платформы (ставятся как сервис, а не как утилита
на ноутбук) перечислены в [`TODO.md`](TODO.md).

## Списки и обучающие материалы (не утилиты)

Список полезных подборок — в [`TODO.md`](TODO.md).
