# not-install: утилиты, которые плейбук сознательно НЕ ставит

Сводка инструментов, от которых отказались при сборке ноутбука, и причина
отказа (обычно — незакрытый баг/issue). Если причина исчезла (issue закрыт,
вышла фиксирующая версия), утилиту можно вернуть в плейбук — актуальный статус
удобно сверять по ссылке на issue.

| Утилита | Зачем была | Причина отказа | Issue |
| --- | --- | --- | --- |
| atuin | синхронизируемая история команд (SQLite, fuzzy-поиск, статистика) | баг VTE: preexec-хук печатается буквально перед каждой командой в gnome-terminal | [GNOME/vte#2951](https://gitlab.gnome.org/GNOME/vte/-/work_items/2951), [rcaloras/bash-preexec#186](https://github.com/rcaloras/bash-preexec/issues/186) |
| pitwall | сетевой файрвол (GUI/TUI) | не работает ввод русских букв | [quanticstudios/pitwall#127](https://github.com/quanticstudios/pitwall/issues/127) |

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