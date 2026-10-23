# TODO: сводка по кандидатам — что уже есть и что ставить не нужно

Оставшиеся кандидаты в утилиты для плейбука — в
[`TODO-trend-github.md`](TODO-trend-github.md),
opencode 2 (один агент
раздаёт задачи другим, включая pitwall и herdr) — в
[`TODO-opencode-orchestration.md`](TODO-opencode-orchestration.md), утилиты,
сознательно не ставимые плейбуком (с причинами и issue — atuin и др.) —
в [`not-install.md`](not-install.md), кандидаты без готового бинарника на
GitHub — в [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md),
кандидаты, чей бинарник не запускается на Ubuntu 26.04, —
в [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md),
утилиты для нод Kubernetes
(DaemonSet/агенты, не на ноутбук) — в
[`TODO-node-tools.md`](TODO-node-tools.md), серверные приложения, утилиты и
Helm-чарты (не на ноутбук) — в
[`TODO-server-apps.md`](TODO-server-apps.md), всё из Ansible Galaxy (роли,
коллекции, готовые playbook'и), которое плейбук не ставит, — в
[`TODO-ansible-galaxy-not-install.md`](TODO-ansible-galaxy-not-install.md).
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

Дедупликация 2026-10-22: нодовые агенты Kubernetes (DaemonSet/static Pod)
вынесены из `TODO-no-prebuilt-binary.md` в отдельный
[`TODO-node-tools.md`](TODO-node-tools.md) — чтобы не смешивать их с
кластерными сервисами уровня Deployment и с кандидатами, у которых просто нет
бинарника. Тогда же серверные приложения, сервисы и Helm-чарты собраны в
[`TODO-server-apps.md`](TODO-server-apps.md) (из этого файла и из
`TODO-no-prebuilt-binary.md`).

---

## Уже устанавливается плейбуком — НЕ дублировать

`gh`, `glab`, `gitlabcloner`, `glci`, `pre-commit`, `reposcan`, `ansible-lint`,
`molecule`, `tflint`, `terraform-docs`, `packer`, `driftctl`, `tgsieve`, `tfenv`,
`tgenv`, `pgcenter`, `kubectl`, `helm`, `k9s`, `kustomize`, `kubectx`, `flux`,
`flux9s`, `stern`, `flate`, `flux-tools`, `cty`, `diffyml`, `kubectl-k8i`,
`kubespy`, `kubecolor`, `kube-capacity`, `ktop`, `kubefetch`, `vault`,
`gitleaks`, `starship`, `bat`, `eza`, `zoxide`, `fzf`, `ripgrep`, `fd-find`,
`direnv`, `uv`, `yq`, `nmap`, `tcpdump`, `httpie`, `opencode`,
`herdr`, `rtk`, `T3 Code`, `ghostty`, `opengrep`,
`mcommander`, `HushMic`, `LACT`, `rkn-block-checker`, `gai`, `dnsglobe`,
`docker`, `guardrails-llm-filter`, `kops`, `devpod`, `kompose`,
`ko`, `kubie`, `kubefwd`, `kubectl-tree`, `conftest`, `kube-bench`,
`kube-hunter`, `cdk8s`, `kubernetes-mcp-server`, `kubecm`,
`kubectl-slice`, `helmify`, `kail`, `kubeaudit`, `rbac-tool`,
`inframap`, `rover`, `graphviz`,
`kluctl`, `kube-burner`, `kwok`, `kwokctl`, `kpexec`,
`groot`, `k2tf`, `kubesec`, `kpt`, `kube-job`, `kubectl-sniff`,
`static-tcpdump`, `kdash`, `kubetail`, `kubepug`, `kube-prompt`,
`rbac-lookup`, `kubectl-who-can`, `korb`, `kl`, `kubectl-doctor`,
`kbom`, `kubectl-modify-secret`, `silver-surfer`, `kubeprompt`,
`kustomizer`, `buoy`, `k9sight`, `hanoi-cli`, `kzero`, `kubectl-kzero`,
`ktunnel`, `kubectl-duplicate`, `kubectl-fd`, `kubectl-relay`, `gwctl`,
`kbu`, `kurt`, `kubediff`, `kubetrim`, `kubevious`, `k8shark`,
`node-gizmo`, `burn`, `tk`, `audit2rbac`, `knetvis`, `pumba`, `kopilot`,
`kubectl-outagedeck`, `kptop`, `kubectl-fields`, `kosko`,
`monokle`, `rubick`, `klustr`, `devspace`, `kubent`, `rustnet`,
`lazysql`, `polaris`, `kube-score`, `kubectl-neat`, `kubectl-validate`,
`nova`, `helm-secrets`, `ansible-navigator`, `ansible-builder`,
`ansible-creator`, `ansible-doctor`, `ara`, `tenv`, `atmos`, `tfupdate`,
`tfsort`.

Добавлены 2026-10-22 из [`TODO-trend-github.md`](TODO-trend-github.md) (топ-15
приложений по звёздам среди оставшихся кандидатов; `dev-sec` — коллекция
Ansible Galaxy, а не приложение, поэтому вместо неё в набор вошёл следующий по
звёздам `terrascan`): `gitui`, `vhs`, `dagger`, `limactl`/`lima`,
`pgcli`, `harlequin`, `terraformer`, `terrascan`, `steampipe`, `kubectl-ai`,
`k3d`, `telepresence`, `tilt`, `mirrord`.

Добавлены 2026-10-22 из того же файла следующим по звёздам (снова без
`dev-sec`): `devspace`, `rustnet`, `lazysql`, `kubent`. `devbox` и `flox`
удалены: Nix не используется.

Добавлены 2026-10-23 из того же файла — следующие 15 по звёздам среди
оставшихся кандидатов (без архивного `glasskube`, мета-пакета
`ansible-dev-tools`, который дублирует уже поставленные части, и коллекции
Ansible Galaxy `dev-sec`): `polaris`, `kube-score`, `kubectl-neat`,
`kubectl-validate`, `nova`, `helm-secrets`, `ansible-navigator`,
`ansible-builder`, `ansible-creator`, `ansible-doctor`, `ara`, `tenv`,
`atmos`, `tfupdate`, `tfsort`. `glasskube` не ставится: репозиторий
заархивирован 2026-06-14. `inframap` в трекере отмечен как уже стоящий
(роль `iac`, тег v0.8.0).

Добавлены 2026-10-23 из того же файла — последние кандидаты с готовым пакетом
Ubuntu: `btop` (1.4.6) и `ncdu` (1.22), оба ролью `shell` через apt. Остаток
трекера, который осознанно не добавлен (`glasskube` — архив, `ansible-dev-tools`
— мета-пакет, `dev-sec` — коллекция Galaxy, `terraform-ls` — только исходники),
вынесен в [`TODO-trend-github-not-added.md`](TODO-trend-github-not-added.md).

`HolmesGPT` не ставится: последний релиз требует Python `<3.14`, а в Ubuntu
26.04 системный Python 3.14 — см. [`not-install.md`](not-install.md) и
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

Кандидаты, которые дублируют этот список и потому не берутся: `ktail`
(≈ `stern`), `kube-switch` (≈ `kubectx`), `dyff` (≈ `diffyml`).

## Не для локальной установки (сервер/платформа) — только если понадобится

Из звёзд также попали self-hosted-платформы, серверные сервисы и Helm-чарты —
они не ставятся «на ноутбук» одной командой: нужен отдельный хост, Docker
Compose или Helm-релиз. Полный список с описаниями и ссылками — в
[`TODO-server-apps.md`](TODO-server-apps.md).

`VictoriaMetrics` сюда не относится: у него есть single-binary режим для
локального запуска, поэтому он стоит в роли `monitoring` (из VictoriaMetrics —
только `vmalert-tool`).

Нодовые агенты Kubernetes (DaemonSet/static Pod: `goldpinger`,
`node-exporter`, `kured`, eBPF-агенты и т.п.) — отдельный список:
[`TODO-node-tools.md`](TODO-node-tools.md).

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
