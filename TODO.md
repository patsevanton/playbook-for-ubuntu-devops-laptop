# TODO: сводка по кандидатам — что уже есть и что ставить не нужно

Канонический список кандидатов в утилиты для плейбука — в
[`TODO-awesome-github.md`](TODO-awesome-github.md): сгруппирован по темам,
со ссылками на репозитории и рекомендациями «что выбрать из дублей».
Уникальные тренды 2026 — в [`TODO-trend-github.md`](TODO-trend-github.md),
отдельный трекер под NORA (artifact registry) + локальный TLS-сертификат
(mkcert) — в [`TODO-nora.md`](TODO-nora.md), глобальные skills и `AGENTS.md`
для opencode — в [`TODO-skills.md`](TODO-skills.md), аналоги pitwall для Ubuntu
с поддержкой opencode (на случай, если pitwall не устроит) —
в [`TODO-pitwall.md`](TODO-pitwall.md), отложенная установка herdr и rtk —
в [`TODO-herdr-rtk.md`](TODO-herdr-rtk.md).

Источник подборки: все репозитории со ⭐ аккаунта (2057 шт.), из них отобраны
~570 похожих на локально устанавливаемые утилиты и вручную вычищены до списков
в файлах выше. Полные данные обхода (все 2057 звёзд + 571 кандидат + ссылки на
релизы): `/home/magnit/Public/starred-tools/data/` (`install_hints.tsv`,
`install_report.md`).

Дедупликация 2026-10-18: каждый кандидат описан ровно в одном файле-трекере;
дубли из этого файла и из `TODO-trend-github.md` перенесены в
`TODO-awesome-github.md` вместе с блоками «⭐ Рекомендую». Здесь остались
справочные списки: что уже ставит плейбук, что не ставится на ноутбук и что
является просто подборками.

---

## Уже устанавливается плейбуком — НЕ дублировать

`gh`, `glab`, `gitlabcloner`, `glci`, `pre-commit`, `reposcan`, `ansible-lint`,
`molecule`, `tflint`, `terraform-docs`, `packer`, `driftctl`, `tgsieve`, `tfenv`,
`tgenv`, `pgcenter`, `kubectl`, `helm`, `k9s`, `kustomize`, `kubectx`, `flux`,
`flux9s`, `stern`, `flate`, `flux-tools`, `cty`, `diffyml`, `kubectl-k8i`,
`kubespy`, `kubecolor`, `kube-capacity`, `ktop`, `kubefetch`, `vault`,
`gitleaks`, `starship`, `bat`, `eza`, `zoxide`, `fzf`, `ripgrep`, `fd-find`,
`direnv`, `uv`, `yq`, `nmap`, `tcpdump`, `httpie`, `opencode`, `pitwall`,
`mcommander`, `HushMic`, `rkn-block-checker`, `gai`, `dnsglobe`, `docker`,
`guardrails-llm-filter`.

`ansible-core` ставится один раз вручную при bootstrap (см. `README.md`).

Аналоги `pitwall` с поддержкой `opencode` (claude-squad, Omnara и отсеянные
варианты) описаны отдельно — в [`TODO-pitwall.md`](TODO-pitwall.md).

Кандидаты, которые дублируют этот список и потому не берутся (в
`TODO-awesome-github.md` помечены ⛔): `ktail` (≈ `stern`), `kube-switch`
(≈ `kubectx`), `dyff` (≈ `diffyml`).

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

Ранее здесь числились `VictoriaMetrics` и `coroot` — перенесены в кандидаты
(`TODO-awesome-github.md`, раздел «Локальный мониторинг»): у обоих есть
single-binary режим для локального запуска.

## Списки и обучающие материалы (не утилиты)

`awesome`, `system-design-primer`, `awesome-sysadmin`, `90DaysOfDevOps`,
`DevOps-Roadmap`, `awesome-gh-extensions`, `awesome-tf` — полезны как подборки,
но устанавливать нечего.

Остальные подборки (`devops-exercises`, `DevOps-Bash-tools`, `kubetools`,
`modern-unix`, `awesome-tunneling`, `awesome-prometheus-alerts`) — со своими
описаниями и ссылками, как кандидаты, в `TODO-awesome-github.md` (раздел
«Обучающие материалы и подборки»).
