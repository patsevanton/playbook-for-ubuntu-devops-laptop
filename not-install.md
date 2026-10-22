# not-install: утилиты, которые плейбук сознательно НЕ ставит

Сводка инструментов, от которых отказались при сборке ноутбука, и причина
отказа (обычно — незакрытый баг/issue). Если причина исчезла (issue закрыт,
вышла фиксирующая версия), утилиту можно вернуть в плейбук — актуальный статус
удобно сверять по ссылке на issue.

| Утилита | Зачем была | Причина отказа | Issue |
| --- | --- | --- | --- |
| atuin | синхронизируемая история команд (SQLite, fuzzy-поиск, статистика) | баг VTE: preexec-хук печатается буквально перед каждой командой в gnome-terminal | [GNOME/vte#2951](https://gitlab.gnome.org/GNOME/vte/-/work_items/2951), [rcaloras/bash-preexec#186](https://github.com/rcaloras/bash-preexec/issues/186) |
| pitwall | сетевой файрвол (GUI/TUI) | не работает ввод русских букв | [quanticstudios/pitwall#127](https://github.com/quanticstudios/pitwall/issues/127) |
| victoria-metrics | single-node TSDB (локальный скрейп метрик + Web UI) | не нужна без приёмки/скрейпа метрик в локальную VM; от релизов VM оставлен только `vmalert-tool` | — |
| auto-cpufreq | авто-оптимизация частоты CPU и энергопотребления | не нужен: хватает того, что в Ubuntu есть из коробки (`power-profiles-daemon` + `amd-pstate`/`intel_pstate`) | — |
| argocd (Argo CD CLI) | CLI для GitOps-деплоя в Argo CD | Argo CD не используется: сервер не разворачивается, локальный CLI без него не нужен | — |
| pulumi | IaC на языках программирования (альтернатива Terraform) | не используется: стек IaC на Terraform (`tfenv`/`tgenv`) | — |
| 3mux | терминальный мультиплексор с тайлингом в стиле i3 | не нужен: используется стандартное окружение Ubuntu, а мультиплексор уже закрывает `zellij` | — |
| asn | справка по IP/домену: ASN, RPKI, BGP, WHOIS, трейс | не нужен: распространяется только bash-скриптом, без релизных артефактов | — |
| kubebuilder | SDK и скаффолдинг Kubernetes-операторов и CRD на Go | не используется: Kubernetes-операторы не создаются | — |

---

## atuin — баг VTE (GNOME/vte#2951)

`atuin init bash` подключает `bash-preexec`, который на bash ≥ 5.3 вешает
preexec-хук в `PS0` строкой `${ ... }`. Интеграция VTE
(`/etc/profile.d/vte-2.91.sh`, gnome-terminal) заканчивает свой `PS0`
OSC-терминатором `\e\\` (одиночный `\` на конце), и prompt-экранирование
превращает `$` хука в литеральный текст — строка
`${ __bp_invoke_preexec_from_ps0 "..." >&2; }` печатается буквально перед
каждой командой.

Баг на стороне VTE, исправлен только в vte 0.84:
- [GNOME/vte#2951](https://gitlab.gnome.org/GNOME/vte/-/work_items/2951)
- [rcaloras/bash-preexec#186](https://github.com/rcaloras/bash-preexec/issues/186)

**Замена в плейбуке**: [mcfly](https://github.com/cantino/mcfly) — тоже умная
история (Ctrl-R с приоритизацией по контексту), но хук ставится через
`PROMPT_COMMAND`, не трогая `PS0`, поэтому бага VTE не имеет.

**Очистка**: роль `shell` удаляет бинарник `/usr/local/bin/atuin` и каталоги
`~/.config/atuin`, `~/.local/share/atuin`, `~/.atuin` (см.
`roles/shell/tasks/main.yml`).

Вернуть atuin можно после обновления VTE до 0.84+ — либо оставить mcfly как
есть (он локальный, без синхронизации; atuin давал синхронизацию `atuin sync`).

## pitwall — русские буквы

См. отдельный файл [`TODO-pitwall.md`](TODO-pitwall.md):
не работает ввод русских букв — [quanticstudios/pitwall#127](https://github.com/quanticstudios/pitwall/issues/127).

## Argo CD, Pulumi и Kubebuilder — не используются

Argo CD не применяется ни как сервер, ни как локальный CLI `argocd` —
GitOps закрывают Flux (`flux`, `flux9s`, `flate`, `flux-tools`) и `helmfile`.

Pulumi не ставится, потому что IaC-стек уже на Terraform: `tfenv`/`tgenv`,
`tflint`, `terraform-docs`, `tftui` и остальные инструменты роли `iac`.

Kubebuilder не ставится, потому что Kubernetes-операторы не создаются.
Если понадобится писать оператор/CRD на Go — вернуть в
`roles/kubernetes/defaults/main.yml` одной записью.