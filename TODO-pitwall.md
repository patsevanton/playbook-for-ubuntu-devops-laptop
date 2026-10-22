# pitwall

**Статус 2026-10-22: блокер снят.** Issue о вводе русских букв закрыт
2026-10-19 как completed, фикс — PR #128, вышел в `v0.1.0-beta.12`.
Актуальный релиз — `v0.1.0-beta.17` (2026-10-21).

- Issue: <https://github.com/quanticstudios/pitwall/issues/127>
- PR: <https://github.com/quanticstudios/pitwall/pull/128>
- Релизы: <https://github.com/quanticstudios/pitwall/releases>

Причина бага: Wayland-бэкенд Gio передавал группу раскладки в
`xkb_state_update_mask` трижды; GNOME добавляет локаль к keymap (`us,ru,us`),
и группа «сворачивалась» обратно в первую. Теперь передаётся только как
locked layout.

## Почему всё ещё не в плейбуке

Решение о добавлении не принято. Сейчас уже есть:

- вкладки opencode 2 со статусами и уведомлениями;
- `herdr` (роль `ai_tools`) — управление сессиями/панелями/вкладками
  агентов и их состоянием;
- нативные сабагенты opencode 2.

pitwall даёт то же плюс worktree на задачу, PR/CI в строке вкладки и
`pitwall new/wait/send` для кросс-агентной оркестрации. Нужно понять, что из
этого реально не закрывается без него.

## Что исследовать перед добавлением

- [ ] Работает ли opencode как воркер: `pitwall new -- opencode ...`,
      статусы, `resume`, approvals (поддержка opencode в pitwall беднее, чем
      у Claude Code/Codex: нет side panel, cost и plan limits).
- [ ] `pitwall wait --children` против нативных сабагентов.
- [ ] Стоит ли держать pitwall и herdr одновременно.
- [ ] Способ установки для плейбука: `brew install quanticstudios/tap/pitwall`
      или `scripts/get.sh`; хуки (`pitwall hooks install`), демон, конфиг,
      обновления.
- [ ] Нужен ли GUI-терминал на ноутбуке вообще (pitwall ставится как
      отдельное окно, а не CLI).

Общий трекер по оркестрации opencode 2, включая pitwall как один из
вариантов, — [`TODO-opencode-orchestration.md`](TODO-opencode-orchestration.md).

## Вернуть в not-install?

Не нужно: причина отказа исчезла, а `not-install.md` — про сознательные
отказы. Пока pitwall остаётся кандидатом здесь.
