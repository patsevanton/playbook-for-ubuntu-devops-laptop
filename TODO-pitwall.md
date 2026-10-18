# TODO-pitwall: аналоги pitwall для Ubuntu, совместимые с opencode

Отдельный трекер под **аналоги `pitwall`** (github.com/quanticstudios/pitwall) —
на случай, если pitwall не устроит или перестанет развиваться.

**Фильтр отбора: только инструменты с поддержкой `opencode`** — плейбук ставит
`opencode` (роль `opencode`), поэтому кандидаты без него отсеяны (см. раздел 5).
Все кандидаты проверены на наличие **Linux-сборок** и описаны только здесь
(см. правило дедупликации в [`TODO.md`](TODO.md)).

> **Контекст:** сам pitwall плейбук уже ставит (роль `opencode`: бинарник,
> `.desktop`-ярлык, иконка — см. `README.md`, раздел «pitwall»). Дублировать его
> установку не нужно. Здесь — только запасные варианты.
>
> **Статус проверки:** 2026-10-18, теги релизов взяты через GitHub API,
> поддержка opencode — по README/докам самих репозиториев (поисковая
> интеграция в этот день не работала).

Как читать:

- `- [ ]` — задача ещё не сделана.
- Команды — как есть для Ubuntu 26.04 / x86_64.
- «Берём» = кандидат в плейбук; «не берём» = есть причина отказа (⛔).

---

## 1. Сводка кандидатов для Ubuntu + opencode

| Кандидат | Формат | Версия (2026-10-18) | Лицензия | Linux | opencode | Вердикт |
| --- | --- | --- | --- | --- | --- | --- |
| [smtg-ai/claude-squad](https://github.com/smtg-ai/claude-squad) | TUI (терминал), Go | v1.0.20 | AGPL-3.0 | да, amd64/arm64 | **явно поддерживается** (в README и топиках) | **Берём №1** |
| [omnara-ai/omnara](https://github.com/omnara-ai/omnara) | Self-hosted платформа + daemon | omnarad-v0.2.7 | Apache-2.0 | да (Docker + daemon) | заявлен в списке harness-ов, см. §4 | **Опционально** — телефонные approvals |
| [BloopAI/vibe-kanban](https://github.com/BloopAI/vibe-kanban) | Web + Rust | — | Apache-2.0 | да | был в списке 10+ агентов | ⛔ Проект сворачивается («sunsetting» в README) |

Сам pitwall тоже работает на Ubuntu (linux x86_64/arm64) и поддерживает
opencode (статусы Working, Input, Approval, Done и Error + resume) — это текущий
выбор плейбука, из этого трекера он не устанавливается.

## 2. claude-squad — ближайший аналог (TUI)

Управление несколькими coding-агентами в tmux-сессиях, каждая в своём git
worktree, дифф перед коммитом, push/checkout ветки. **opencode поддерживается
штатно** — запускается как обычная программа в сессии (в README перечислен
среди поддерживаемых: Claude Code, Codex, OpenCode, Amp; в топиках репозитория
есть `opencode`). Это TUI в одном терминале, а не GUI: в отличие от pitwall нет
статусов «агент ждёт тебя», Allow/Deny с телефона, PR/CI-чипов, очереди задач и
детектора конфликтов между worktree.

- Зависимости: `tmux` (**плейбук пока не ставит** — добавить), `gh` (уже ставится).
- Лицензия AGPL-3.0 — учитывать, если когда-нибудь будем форкать.

- [ ] Поставить tmux (apt: `tmux`) — отдельной задачей в роль `shell` или
      `opencode`:

  ```
  sudo apt-get install -y tmux
  tmux -V
  ```

- [ ] Поставить claude-squad (в стиле роли `binaries`, тег v1.0.20):

  ```
  curl -fsSLO https://github.com/smtg-ai/claude-squad/releases/download/v1.0.20/claude-squad_1.0.20_linux_amd64.tar.gz
  tar -xzf claude-squad_1.0.20_linux_amd64.tar.gz -C /tmp claude-squad
  sudo install -m 0755 /tmp/claude-squad /usr/local/bin/claude-squad
  sudo ln -sf /usr/local/bin/claude-squad /usr/local/bin/cs
  cs version
  ```

  В формате `binaries_to_install` (роль `binaries`):

  ```yaml
  - name: claude-squad
    source: github
    repo: smtg-ai/claude-squad
    url: "https://github.com/smtg-ai/claude-squad/releases/download/{tag}/claude-squad_{tag}_linux_amd64.tar.gz"
    archive: tar
    member: claude-squad
  ```

  Внимание: в имени архива версия без `v` (`{tag}` = `v1.0.20`,
  файл = `claude-squad_1.0.20_...`) — шаблон `url` нужно подправить или
  ввести отдельную переменную с версией без `v`.

- [ ] Сделать `opencode` дефолтным профилем в `~/.claude-squad/config.json`
      (плейбук ставит именно его; `claude`/`codex`/`gemini` — по желанию):

  ```json
  {
    "default_program": "opencode",
    "profiles": [
      { "name": "opencode", "program": "opencode" },
      { "name": "claude", "program": "claude" },
      { "name": "codex", "program": "codex" }
    ]
  }
  ```

  Разово то же самое без конфига: `cs -p "opencode"`.

- [ ] Проверить сценарий «несколько opencode-агентов»: `cs`, затем `n` — сессия
      в worktree, `N` — сессия с промптом, `tab` — дифф, `s` — commit+push.
- [ ] Проверить, что `cs` уживается с pitwall: оба держат свои tmux/worktree,
      но конфликт возможен на уровне git worktree одного репозитория.

## 3. Omnara — «телефонная» часть и удалённый запуск

Self-hosted платформа запуска агентов (Apache-2.0): дашборд, approvals,
Slack/Discord/GitHub-интеграции, durable-состояние, sandbox-ы. Закрывает ту
часть pitwall, которой нет у конкурентов: **контроль и ответы на approvals с
телефона**. Это не мультиплексер терминалов, а инфраструктура: агенты
запускаются как сервис, а не «вкладками в окне».

- **Статус по opencode:** в README Omnara opencode назван в списке harness-ов
  («Claude Code, the Claude Agent SDK, Codex, OpenCode, or Pi»), но ядро
  агентного цикла у Omnara своё (workers + `agent.yaml`), а MCP-сервером
  документированы Claude Code, Codex и Cursor. Глубина интеграции именно с
  opencode не подтверждена — это первый чеклист-пункт ниже.

- Ставится Docker Compose (docker плейбук уже ставит) либо используется
  Omnara Cloud; на машине нужен только daemon `omnarad`
  (бинари `omnarad-linux-amd64` / `omnarad-linux-arm64` в релизе omnarad-v0.2.7)
  или CLI `npx omnara`.

- [ ] **Проверить интеграцию с opencode до всякой установки**: запускается ли
      opencode как harness Omnara и работает ли opencode как MCP-клиент
      (`https://app.omnara.com/mcp`) для отправки сообщений и approvals.
      Если интеграции нет — пункт «телефон» закрывать штатным телефоном pitwall
      (QR-паринг по LAN, push, Allow/Deny), а Omnara из трекера убрать.
- [ ] Решить формат: self-hosted на ноутбуке (Docker Compose) или «daemon +
      облако». Self-hosted тянет Postgres, Redis, blob storage — это ближе к
      разделу «Не для локальной установки» из [`TODO.md`](TODO.md).
- [ ] Поставить daemon (если выбрали):

  ```
  curl -fsSLO https://github.com/omnara-ai/omnara/releases/download/omnarad-v0.2.7/omnarad-linux-amd64
  sudo install -m 0755 omnarad-linux-amd64 /usr/local/bin/omnarad
  omnarad --help
  ```

- [ ] Проверить security-моменты: API с открытым доступом, self-hosted
      дефолты в README прямо названы «intentionally insecure» — за обратным
      прокси и с авторизацией.

## 4. ⛔ Не берём — и почему

По opencode (главный критерий):

- **sculptor** (imbue-ai/sculptor, MIT, Linux есть) — глубоко встроен только в
  Claude Code и Pi (доки `docs/help/integrated_harnesses.md`); opencode можно
  лишь запустить обычной командой в built-in терминале, без статусов, очереди
  сообщений и ревью. Для opencode не применим.
- **Crystal** (stravu/crystal) — поддерживал Claude Code и Codex, opencode не
  заявлен; плюс депрекейтед с 2026-02.
- **Nimbalyst** (Nimbalyst/nimbalyst, закрытый) — развился из Crystal, opencode
  не заявлен, лицензия закрытая.
- **Conductor** (conductor.build) — только macOS и Claude Code; на Ubuntu и с
  opencode не применим. (Сайт на 2026-10-18 отдаёт 403 автоматическим
  клиентам — детали не проверены.)

По жизнеспособности (opencode был поддержан, но проект уходит):

- **Vibe Kanban** (BloopAI/vibe-kanban) — opencode был в списке 10+ агентов, и
  функционально покрывал большую часть pitwall (канбан, workspaces,
  inline-комментарии к диффам, PR), но в README проекта — «Vibe Kanban is
  sunsetting». На новый ноутбук не ставим; следить, не выйдет ли форк.

Прочее:

- **Самодельный tmux + хуки агентов** — статус-бар с состояниями агентов можно
  собрать на `pitwall notify`-хуках и tmux, но это не утилита, а DIY; отдельной
  задачи не заводим.

## 5. Покрытие фич pitwall — что останется незакрытым

Сравнение только для инструментов с opencode (claude-squad, Omnara):

| Фича pitwall | claude-squad | Omnara |
| --- | --- | --- |
| opencode как агент | да | заявлен, глубина не проверена |
| Несколько opencode-агентов одновременно | да (tmux) | да (сервисно) |
| Изоляция git worktree | да | нет |
| Статусы «Working / Input / Approval / Done» | частично (со скрина) | да (event log) |
| «Одна клавиша — к тому, кто ждёт» | нет | нет |
| Allow/Deny из сайдбара | нет | да (approvals в дашборде) |
| Телефон по LAN без аккаунтов | нет | ⛔ нет (нужен аккаунт/сам-хост) |
| Ревью диффа с комментариями агенту | просмотр диффа | нет |
| PR/CI в интерфейсе, мёрдж | push через `gh` | через GitHub-интеграцию |
| Конфликты между worktree | нет | нет |
| Очередь задач | нет | расписания (cron) |
| Персистентные сессии после ребута | да (tmux) | да (durable) |

Вывод: полной замены нет. Минимальная связка «claude-squad (работа с opencode в
worktree) + Omnara (телефон)», и то без детектора конфликтов, PR-мёрджа из
сайдбара и «одной клавиши к ожидающему агенту».

## 6. Что сделать в репозитории (когда понадобится)

- [ ] Добавить `tmux` в установку (роль `shell` или `opencode`) — нужен и для
      ручной работы, и для claude-squad.
- [ ] Оформить claude-squad в роли `binaries` / `opencode` по образцу pitwall
      (+ симлинк `cs`, дефолтный профиль `opencode`).
- [ ] Проверить интеграцию Omnara с opencode (см. §3); если её нет — вычеркнуть
      Omnara и закрывать «телефон» штатной страницей pitwall.
- [ ] Раз в квартал перепроверять статусы: жив ли pitwall, не появился ли
      полноценный аналог с поддержкой opencode (телефон + статусы + конфликты
      worktree), не вышел ли форк Vibe Kanban.
