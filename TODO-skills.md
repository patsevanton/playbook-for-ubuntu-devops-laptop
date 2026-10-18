# TODO-skills: глобальные skills и AGENTS.md для opencode

Отдельный трекер под **глобальные skills для opencode** и **глобальный `AGENTS.md`** —
то, что должно действовать на уровне пользователя (`~/.config/opencode/`),
а не в отдельных проектах.

Источник — каталог старой машины `/home/user/`: там лежат установленные skills
и глобальный `AGENTS.md`, которые нужно перенести на ноутбук под пользователем
`magnit` и зафиксировать в Ansible-роли `opencode`.

> **Статус доступа:** на 2026-10-18 к `/home/user/` доступа нет —
> `ls: cannot open directory '/home/user/': Permission denied`
> (каталог `drwxr-x--- user user`, `sudo` требует интерактивный пароль).
> Поэтому конкретный список skills и текст `AGENTS.md` пока не вычитаны:
> ниже — заготовка с путями, чеклистом переноса и местом под инвентаризацию.
> Как только доступ появится — заполнить раздел 2 и таблицу в разделе 7.

> **Статус 2026-10-18 (позже):** установка глобальных skills уже реализована —
> ролью `skills` этого репозитория (`roles/skills/`): список воспроизведён по
> lock-файлу рабочей машины `~/.agents/.skill-lock.json` (68 skills из
> 16 git-репозиториев), ставится через `npx skills add … -g -y -a opencode`.
> Разделы 1–2 и 7 (перенос skills) закрыты ролью; открытым остаётся только
> глобальный `AGENTS.md` (разделы про него).

Как читать:

- `- [ ]` — задача ещё не сделана.
- Команды — как есть для Ubuntu 26.04 / x86_64.
- «Глобально» = для пользователя `magnit`, каталог `~/.config/opencode/`.

---

## 0. Получить доступ к `/home/user/` и провести инвентаризацию

- [ ] Открыть каталог (пароль от `sudo`, или попросить владельца выдать права):

  ```
  sudo ls -la /home/user/
  sudo ls -la /home/user/.config/opencode/
  ```

- [ ] Найти все skills (и opencode-стиль `SKILL.md`, и возможные claude-стиль
      `.claude/skills/`, `.opencode/skills/`):

  ```
  sudo find /home/user -maxdepth 6 -name SKILL.md -not -path '*/node_modules/*'
  sudo find /home/user -maxdepth 6 -type d -name skills -not -path '*/node_modules/*'
  ```

- [ ] Найти глобальный `AGENTS.md` (и заодно глобальные subagents/commands):

  ```
  sudo find /home/user -maxdepth 4 -iname 'AGENTS.md'
  sudo ls -la /home/user/.config/opencode/agent/ /home/user/.config/opencode/command/ 2>/dev/null
  ```

- [ ] Забрать содержимое во временное место (`/tmp/opencode-skills-import/`),
      вычитать `SKILL.md` каждого skill (frontmatter `name`, `description`,
      зависимости: `scripts/`, `references/`, `assets/`) и заполнить таблицу
      в разделе 7.

- [ ] Вычистить приватное перед переносом: токены, ключи, corp-URL с креденшелами,
      упоминания `/home/user/` внутри скриптов.

## 1. Куда класть глобально (opencode V2)

| Что | Глобальный путь |
|---|---|
| Skill | `~/.config/opencode/skills/<skill-name>/SKILL.md` (+ файлы skill'а рядом) |
| Subagent | `~/.config/opencode/agent/<name>.md` |
| Command | `~/.config/opencode/command/<name>.md` |
| Глобальные инструкции | `~/.config/opencode/AGENTS.md` |
| Плагины, MCP, модели | `~/.config/opencode/opencode.jsonc` (уже есть) |
| Настройки TUI/клавиш | `~/.config/opencode/cli.json` (сюда не путать с серверной конфигурацией) |

- [ ] Создать каталоги: `mkdir -p ~/.config/opencode/{skills,agent,command}`.
- [ ] Сверить имена путей и полей с доками V2 перед применением:
      [opencode.ai/v2/docs/skills](https://opencode.ai/v2/docs/skills),
      [opencode.ai/v2/docs/config](https://opencode.ai/v2/docs/config) —
      на момент написания сайта недоступны (таймаут), источники не сверены.

## 2. Перенос skills из `/home/user/`

- [ ] Скопировать каждый каталог skill целиком (не только `SKILL.md` —
      внутри обычно `scripts/` и `references/`):

  ```
  sudo cp -a /home/user/.config/opencode/skills/. /home/magnit/.config/opencode/skills/
  sudo chown -R magnit:magnit /home/magnit/.config/opencode/skills
  ```

- [ ] Внутри перенесённых файлов поправить хардкод: `/home/user/` → `/home/magnit/`,
      имя пользователя, `~`-пути, пути к бинарям (`{{ bin_dir }}` — вне Ansible:
      фактический `~/bin` или `/usr/local/bin`).

- [ ] Пройтись по зависимостям skill'ов: какие CLI нужны (`ripgrep`, `gh`,
      `jq` и т.п.) — проверить, что их ставит плейбук, иначе завести задачу
      в `TODO-awesome-github.md` или в соответствующую роль.

- [ ] Проверить, что skills подхватились opencode (каталог виден без
      перезапуска сервиса, но при сомнении — `opencode service restart`,
      см. [troubleshooting](https://opencode.ai/v2/docs/troubleshooting)):

  ```
  opencode service status
  tail -f ~/.local/share/opencode/log/opencode.log
  ```

- [ ] Smoke: открыть TUI в тестовом каталоге, убедиться, что skills
      применяются по описанию (description в frontmatter — это то, по чему
      агент решает, что skill подходит под задачу).

## 3. Глобальный `AGENTS.md`

- [ ] Скопировать найденный в `/home/user/` глобальный `AGENTS.md`:

  ```
  sudo cp /home/user/<путь-к-найденному>/AGENTS.md /home/magnit/.config/opencode/AGENTS.md
  sudo chown magnit:magnit /home/magnit/.config/opencode/AGENTS.md
  ```

- [ ] Подключить его глобально в `~/.config/opencode/opencode.jsonc` полем
      `instructions` (см. [конфиг V2](https://opencode.ai/v2/docs/config)) —
      чтобы он не «всплывал» только в тех проектах, где лежит `AGENTS.md`.

  ```jsonc
  {
    "$schema": "https://opencode.ai/config.json",
    "instructions": ["AGENTS.md"],
    // ... остальные настройки не трогать (provider, mcp)
  }
  ```

- [ ] Вычитать текст под ноутбук: убрать машинные пути `/home/user/`,
      требования к corp-стеку, лишние жёсткие ограничения; сохранить то,
      что относится ко всем проектам (стиль, правила безопасности, формат).

- [ ] Проверить порядок наложения: глобальный `AGENTS.md` не должен
      затирать проектные `AGENTS.md` (проектные — приоритетнее/дополняют).

## 4. Глобальные subagents и commands (если были в `/home/user/`)

- [ ] Перенести `~/.config/opencode/agent/*.md` (свой subagent = markdown с
      frontmatter: имя, description, модель, разрешённые инструменты).
- [ ] Перенести `~/.config/opencode/command/*.md`, если были.
- [ ] Для каждого — сверить с доками V2:
      [agents](https://opencode.ai/v2/docs/agents),
      [commands](https://opencode.ai/v2/docs/commands).

## 5. Автоматизация в Ansible (роль `opencode`)

Сейчас роль `roles/opencode/` ставит только бинарник (`tasks/main.yml`,
`defaults/main.yml`). Добавить глобальную конфигурацию — идемпотентно:

- [ ] В `roles/opencode/defaults/main.yml` завести переменные:

  ```yaml
  opencode_config_dir: "{{ ansible_env.HOME }}/.config/opencode"
  # skills, которые всегда ставятся глобально (пусто — пока не вычитан /home/user)
  opencode_skills: []
  #   - name: <skill-name>
  #     src: files/skills/<skill-name>   # или git-URL
  opencode_global_agents_md: ""          # путь к AGENTS.md в files/ роли, "" — не трогать
  opencode_global_agents: []             # subagents из files/agent/
  ```

- [ ] В `roles/opencode/tasks/main.yml` (или `tasks/config.yml`) добавить блок:
      `file: state=directory` для `skills/`, `agent/`, `command/`; `copy`/`template`
      для skills, `AGENTS.md` и subagents; `notify` на перезапуск сервиса не нужен —
      opencode перечитывает конфиг сам, но проверить это на прогоне.
- [ ] Исходники держать в репозитории: `roles/opencode/files/skills/<name>/…`,
      `roles/opencode/files/AGENTS.md`, `roles/opencode/files/agent/<name>.md` —
      и не тянуть из `/home/user/` при каждом прогоне.
- [ ] Секреты в skills/`AGENTS.md` не хранить: только `{env:...}`-подстановки,
      как уже сделано для API-ключей в `opencode.jsonc`.
- [ ] Прогнать `ansible-playbook playbook.yml` (или только роль `opencode`),
      убедиться, что повторный прогон не меняет ничего (`changed=0`).

## 6. Проверка после установки

- [ ] `opencode --version`, `opencode service status` — без ошибок.
- [ ] `ls ~/.config/opencode/skills/` — все ожидаемые skills на месте,
      в каждом есть `SKILL.md`.
- [ ] Тестовый проект: `~/.config/opencode/AGENTS.md` подхватывается,
      проектный `AGENTS.md` его дополняет, а не отменяет.
- [ ] Лог `~/.local/share/opencode/log/opencode.log` — нет ошибок загрузки
      skills/агентов (`role=server`).

## 7. Инвентаризация переносимого (заполнить, когда откроют `/home/user/`)

| # | Skill | Источник в `/home/user/` | Куда в `~/.config/opencode/skills/` | Зависимости | Статус |
|---|---|---|---|---|---|
| 1 | — | — | — | — | `- [ ]` |

Отдельно:

- [ ] Глобальный `AGENTS.md` — источник: `/home/user/<...>/AGENTS.md`
      → `~/.config/opencode/AGENTS.md`.
- [ ] Subagents — источники: `/home/user/.config/opencode/agent/` →
      `~/.config/opencode/agent/`.
- [ ] Commands — источники: `/home/user/.config/opencode/command/` →
      `~/.config/opencode/command/`.

## 8. Если каталог `/home/user/` так и не откроют

- [ ] Спросить у владельца машины список skills и текст `AGENTS.md`
      (хотя бы `ls` + `cat SKILL.md`).
- [ ] Либо собрать набор заново: официальные skills —
      [anthropics/skills](https://github.com/anthropics/skills),
      встроенные skills opencode (`opencode`, `report`) уже доступны без установки.
- [ ] Глобальный `AGENTS.md` написать с нуля: общие правила для всех проектов
      (язык ответов, стиль кода, запрет на секреты, требования к коммитам).

---

Связанное: [`TODO.md`](TODO.md) — сводка по кандидатам; [`TODO-awesome-github.md`](TODO-awesome-github.md) —
утилиты для плейбука; роль `opencode` в `roles/opencode/` — установка бинарника.
