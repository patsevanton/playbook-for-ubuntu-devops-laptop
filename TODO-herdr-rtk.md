# TODO-herdr-rtk: herdr и rtk в плейбуке (осталось: интеграции и конфиги)

Трекер под **herdr** (github.com/herdrdev/herdr) и **rtk** (github.com/rtk-ai/rtk).

> **Статус:** установка бинарников уже сделана — роль `ai_tools` в
> `playbook.yml` (после `opencode`, тег `ai-tools`) ставит `herdr` и `rtk` в
> `/usr/local/bin/`, проверки `--version` есть в `post_tasks`, описание — в
> `README.md`. Осталось: интеграции с opencode и конфиги `config.toml`.

> **Контекст:** оба инструмента усиливают opencode (роль `opencode` уже
> ставит бинарники opencode и pitwall). Интеграции пишут файлы в
> `~/.config/opencode/` (каталог `plugins/`), поэтому ставить их нужно
> **после** роли `opencode`. Источник фактов — рабочая машина
> (взято «как есть» оттуда) + GitHub API.
>
> **Статус проверки:** 2026-10-18; актуальные версии взяты из
> `github.com/<repo>/releases/latest` (без GitHub API в плейбуке — см. README,
> раздел «Обновление и идемпотентность»).

Как читать:

- `- [ ]` — задача ещё не сделана; `- [x]` — уже реализовано в плейбуке.
- Команды — как есть для Ubuntu 26.04 / x86_64 (взяты из `~/.bash_history`
  рабочей машины).
- Версии не фиксируются: при установке брать актуальный релиз.

---

## 1. herdr — терминальный workspace-менеджер для AI-агентов

**herdr** (`herdrdev/herdr`) — TUI-оболочка поверх терминала: управляет
сессиями/панелями/вкладками для coding-агентов, показывает их состояние
(работает / ждёт ввода / заблокирован), умеет удалённые машины по SSH,
git-worktree и обновление сам (`herdr update`).

Рабочая машина: версия **0.9.3**, бинарник — статический ELF (static-pie).
Актуально на 2026-10-18: **v0.9.5**. Ассеты релиза:
`herdr-linux-x86_64`, `herdr-linux-aarch64`, macOS, Windows.

Установка (как ставилось вручную):

```bash
sudo eget herdrdev/herdr --to /usr/local/bin/
herdr --version
```

Для роли `binaries` подойдёт запись с `archive: none` (одиночный бинарник):

```yaml
- name: herdr
  source: github
  repo: herdrdev/herdr
  url: "https://github.com/herdrdev/herdr/releases/download/{tag}/herdr-linux-x86_64"
  archive: none
```

Интеграция с opencode (создаёт файлы в `~/.config/opencode/`:
`plugins/herdr-agent-state.js`, `herdr-tui-session.js`, `herdr-opencode/tui.js`):

```bash
herdr integration install opencode
```

В файлы вшиты маркеры `HERDR_INTEGRATION_ID=opencode`,
`HERDR_INTEGRATION_VERSION=13`; сами предупреждают, что перезаписываются при
повторной установке/обновлении интеграции (кастомные правки держать рядом,
не в этих файлах). Интеграция шлёт состояние агента в herdr по unix-сокету
(`/run/user/<uid>/gcr/...`, см. `~/.config/herdr/herdr.sock.agent`).

Конфиг `~/.config/herdr/config.toml` (на рабочей машине):

```toml
onboarding = false

[ui]
status_indicators = "dots"

[ui.sound]
enabled = true

[ui.toast]
delivery = "herdr"
```

- [ ] Добавить herdr в роль (кандидаты: `opencode` или новая роль для
      AI-инструментов) через `binaries` + `herdr integration install opencode`.
- [ ] Положить `config.toml` в `files/` роли и копировать в
      `~/.config/herdr/config.toml`.
- [x] Бинарник herdr — в роли `ai_tools` (`ai_tools_binaries`), после `opencode`.
- [x] Проверка `herdr --version` — в `post_tasks` плейбука.
- [x] Описать в `README.md` (секция «herdr и rtk — инструменты для AI-агентов»).

## 2. rtk — CLI-прокси, экономящий токены

**rtk** (`rtk-ai/rtk`) — прокси-обёртка над утилитами (`rtk git`, `rtk ls`,
`rtk kubectl`, …): сжимает и фильтрует вывод (логи, diff, таблицы) прежде чем
он попадёт в контекст LLM. Есть `rtk gain` (сводка экономии), `rtk rewrite`
(переписывание команд) и `rtk init` (подключение к агентам).

Рабочая машина: версия **0.49.0**, бинарник — статический ELF (static-pie).
Актуально на 2026-10-18: **v1.0.1**. Ассеты релиза (tar.gz): `rtk-x86_64-unknown-linux-musl`
(и aarch64/macOS), плюс варианты `rtk-oss-*` и `rtk-proxy-*` — для плейбука
брать обычный `rtk` без суффиксов.

Установка (как ставилось вручную):

```bash
sudo eget rtk-ai/rtk --to /usr/local/bin/
rtk --version
```

Для роли `binaries` — архив, имя ассета содержит версию без `v`
(`{version}` + `version_strip: ^v`), структуру внутри архива проверить
перед добавлением (`member`):

```yaml
- name: rtk
  source: github
  repo: rtk-ai/rtk
  url: "https://github.com/rtk-ai/rtk/releases/download/{tag}/rtk-{version}-x86_64-unknown-linux-musl.tar.gz"
  archive: tar
  member: rtk      # проверить путь бинарника внутри архива
```

Плагин для opencode (создаёт `~/.config/opencode/plugins/rtk.ts`; в плагине
вписано требование `rtk >= 0.23.0` в PATH):

```bash
rtk init -g --opencode
```

Плагин перехватывает `tool.execute.before` для команд bash/shell и переписывает
их через `rtk rewrite`. Правила переписывания живут в самом rtk
(`src/discover/registry.rs`), файл плагина не править.

Конфиг `~/.config/rtk/config.toml` (на рабочей машине; телеметрия выключена —
сохранить при переносе):

```toml
[tracking]
enabled = true
history_days = 90

[display]
colors = true
emoji = true
max_width = 120

[filters]
ignore_dirs = [".git", "node_modules", "target", "__pycache__", ".venv", "vendor"]
ignore_files = ["*.lock", "*.min.js", "*.min.css"]

[retriever]
mode = "sqlite"
max_entry_bytes = 10485760
max_entries = 200
retention_days = 30
compression = true
tee_max_files = 20
tee_max_file_size = 1048576
tee_on_success = false

[telemetry]
enabled = false
consent_given = false

[hooks]
exclude_commands = []
transparent_prefixes = []

[limits]
grep_max_results = 200
grep_max_per_file = 25
status_max_files = 15
status_max_untracked = 10
passthrough_max_chars = 2000

[awareness]
level = "default"
```

- [ ] Добавить rtk в роль (рядом с herdr) через `binaries` +
      `rtk init -g --opencode`.
- [ ] Положить `config.toml` в `files/` роли и копировать в
      `~/.config/rtk/config.toml` (телеметрия остаётся выключенной).
- [x] Бинарник rtk — в роли `ai_tools` (`ai_tools_binaries`), после `opencode`.
- [x] Проверка `rtk --version` — в `post_tasks` плейбука.
- [x] Описать в `README.md` (секция «herdr и rtk — инструменты для AI-агентов»).

## 3. Общее при добавлении в плейбук

- [x] Обе установки вынесли **после** роли `opencode` в `playbook.yml`
      (роль `ai_tools`, тег `ai-tools`): интеграции пишут в `~/.config/opencode/`.
- [x] Идемпотентность: не переустанавливать бинарник, если он уже есть
      (стиль роли `binaries`); интеграции (`herdr integration install opencode`,
      `rtk init -g --opencode`) перезаписывают свои файлы — это безопасно,
      но проверить, что они не спрашивают подтверждение без TTY.
- [x] Проверки в `post_tasks` плейбука: `herdr --version`, `rtk --version`.
