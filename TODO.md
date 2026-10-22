# TODO: сводка по кандидатам — что уже есть и что ставить не нужно

Оставшиеся кандидаты в утилиты для плейбука — в
[`TODO-trend-github.md`](TODO-trend-github.md),
глобальные skills и `AGENTS.md` для opencode — в
[`TODO-skills.md`](TODO-skills.md), pitwall (не ставится, см. issue
о русских буквах: <https://github.com/quanticstudios/pitwall/issues/127>) —
в [`TODO-pitwall.md`](TODO-pitwall.md), утилиты, сознательно не ставимые
плейбуком (с причинами и issue — atuin, pitwall и др.) —
в [`not-install.md`](not-install.md), кандидаты без готового бинарника на
GitHub — в [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md),
кандидаты, чей бинарник не запускается на Ubuntu 26.04, —
в [`TODO-incompatible-ubuntu-2604.md`](TODO-incompatible-ubuntu-2604.md),
кандидаты из подборки collabnix/kubetools, которых нет в других файлах, —
в [`TODO-kubetools.md`](TODO-kubetools.md), утилиты для нод Kubernetes
(DaemonSet/агенты, не на ноутбук) — в
[`TODO-node-tools.md`](TODO-node-tools.md), всё из Ansible Galaxy (роли,
коллекции, готовые playbook'и), которое плейбук не ставит, — в
[`TODO-ansible-galaxy-not-install.md`](TODO-ansible-galaxy-not-install.md).

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
вынесены из `TODO-kubetools.md` и `TODO-no-prebuilt-binary.md` в отдельный
[`TODO-node-tools.md`](TODO-node-tools.md) — чтобы не смешивать их с
кластерными сервисами уровня Deployment и с кандидатами, у которых просто нет
бинарника.

---

## Уже устанавливается плейбуком — НЕ дублировать

`gh`, `glab`, `gitlabcloner`, `glci`, `pre-commit`, `reposcan`, `ansible-lint`,
`molecule`, `tflint`, `terraform-docs`, `packer`, `driftctl`, `tgsieve`, `tfenv`,
`tgenv`, `pgcenter`, `kubectl`, `helm`, `k9s`, `kustomize`, `kubectx`, `flux`,
`flux9s`, `stern`, `flate`, `flux-tools`, `cty`, `diffyml`, `kubectl-k8i`,
`kubespy`, `kubecolor`, `kube-capacity`, `ktop`, `kubefetch`, `vault`,
`gitleaks`, `starship`, `bat`, `eza`, `zoxide`, `fzf`, `ripgrep`, `fd-find`,
`direnv`, `uv`, `yq`, `nmap`, `tcpdump`, `httpie`, `opencode`,
`herdr`, `rtk`, `mcommander`, `HushMic`, `LACT`, `rkn-block-checker`, `gai`, `dnsglobe`,
`docker`, `guardrails-llm-filter`.

`ansible-core` ставится один раз вручную при bootstrap (см. `README.md`).

`pitwall` не ставится плейбуком из-за issue с вводом русских букв — см.
<https://github.com/quanticstudios/pitwall/issues/127> и
[`TODO-pitwall.md`](TODO-pitwall.md).

Кандидаты, которые дублируют этот список и потому не берутся: `ktail`
(≈ `stern`), `kube-switch` (≈ `kubectx`), `dyff` (≈ `diffyml`).

## Не для локальной установки (сервер/платформа) — только если понадобится

Из звёзд также попали self-hosted-платформы и серверные сервисы — они не
ставятся «на ноутбук» одной командой, поэтому вынесены отдельно:

`grafana`, `prometheus`, `alertmanager`, `immich`, `n8n`, `open-webui`,
`supabase`, `sentry`, `jumpserver`, `plausible`, `wg-easy`, `beszel`,
`changedetection.io`, `cachet`, `uptime-kuma`, `headscale`, `pangolin`,
`SafeLine`, `meshery`, `openobserve`, `ClickHouse`, `odigos`, `cilium`,
`rustdesk`, `joplin`, `AFFiNE`, `siyuan`, `logseq`, `excalidraw`, `bruno`,
`warpgate`, `netmaker`, `talos`, `cozystack`, `artifact-keeper`, `aptly`,
`threatmapper`, `pixie`, `kubewatch`, `kuberhealthy`, `prometheus-operator`,
`netdata`, `gitea`, `Dokploy`, `dokku`, `kestra`, `Floci`, `Jenkins`, `Argo CD`,
`APISIX`, `Kong`, `kubesphere`, `Unleash`, `bytebase`, `coder`, `atlantis`,
`semaphore`, `Terrakube` (замена Terraform Enterprise и аналоги), `Crossplane`,
`kagent`, `cloudnative-pg`.

Нодовые агенты Kubernetes (DaemonSet/static Pod: `goldpinger`,
`node-exporter`, `kured`, eBPF-агенты и т.п.) — отдельный список:
[`TODO-node-tools.md`](TODO-node-tools.md).

`VictoriaMetrics` и `coroot` сюда не относятся: у обоих есть single-binary
режим для локального запуска, поэтому они стоят в роли `monitoring` (из
VictoriaMetrics — только `vmalert-tool`).

## Списки и обучающие материалы (не утилиты)

`awesome`, `system-design-primer`, `awesome-sysadmin`, `90DaysOfDevOps`,
`DevOps-Roadmap`, `awesome-gh-extensions`, `awesome-tf` — полезны как подборки,
но устанавливать нечего.

Остальные подборки (`devops-exercises`, `DevOps-Bash-tools`, `kubetools`,
`modern-unix`, `awesome-tunneling`, `awesome-prometheus-alerts`,
`container-security-checklist`, `linux-network-performance-parameters`,
`boilerplates`, `home-ops`/`homelab-gitops-k3s`) ставить нечего — это
материалы для чтения. Инструменты из подборки `kubetools`, которых ещё нет
в других файлах, разобраны отдельно — в
[`TODO-kubetools.md`](TODO-kubetools.md).
