# TODO: кандидаты из трендов GitHub, которые не попали в плейбук

Остаток разбора трендовых утилит GitHub: трекер `TODO-trend-github.md` разобран
полностью (последняя партия — 2026-10-23) и удалён, здесь только те кандидаты,
которые осознанно не добавлены — это не приложение либо утилита удалена после
добавления. Свежая партия на рассмотрение — в разделе
[«Тренды GitHub, 2026-10-23»](#тренды-github-2026-10-23), вторая выборка того
же дня — в разделе
[«Партия за последний год, 2026-10-23»](#партия-за-последний-год-2026-10-23).
Добавленные
утилиты перечислены в [`TODO.md`](TODO.md), причины отказов по конкретным
инструментам — в [`not-install.md`](not-install.md).

Проверено 2026-10-23 по `releases/latest` и `expanded_assets`; новая партия —
через `gh api repos/{owner}/{repo}/releases/latest`, потому что нужны точные
имена ассетов. Для apt — по `getPublishedSources` в Launchpad (серия
`resolute`).

Как читать: `- [ ]` — не добавлен; в заметке — причина и условие, при котором
кандидата стоит пересмотреть.

Заброшенные проекты (архив GitHub, годы без обновлений) сюда не входят: они
описаны только в [`TODO-abandoned.md`](TODO-abandoned.md).

---

## Не приложение

- [ ] **ansible-dev-tools** — мета-пакет: molecule, navigator, builder,
      creator, ansible-lint одним `pipx install`. *(`pipx`)* —
      [ansible/ansible-dev-tools](https://github.com/ansible/ansible-dev-tools)
      — ⛔ дублирует уже поставленное по отдельности: `molecule` (роль `base`),
      `ansible-lint` (роль `base`), `ansible-navigator`, `ansible-builder`,
      `ansible-creator` и `ansible-doctor` — тоже роль `base`. Ставить мета-пакет
      поверх них — лишние зависимости и версии, которыми плейбук не управляет.
      Пересматривать, если набор перейдёт на мета-пакет вместо отдельных
      пакетов (тогда — замена, а не добавление).
- [ ] **dev-sec hardening collection** — готовая коллекция харденинга
      Linux/SSH/nginx для плейбуков. *(5.5k ⭐, `ansible-galaxy`)* —
      [dev-sec/ansible-collection-hardening](https://github.com/dev-sec/ansible-collection-hardening)
      — ⛔ это коллекция Ansible Galaxy, а не устанавливаемая утилита: у неё нет
      бинарника, и на ноутбуке она нужна только как зависимость чужих
      плейбуков. Плейбук для себя её не использует. Пересматривать, если
      появятся собственные роли, которым нужен готовый харденинг.

## Удалены после добавления

Эти утилиты успели побывать в плейбуке (роль `runtimes`), но были удалены
осознанно — вместе с решением не использовать Nix.

- [ ] **devbox** — изолированные dev-окружения из Nix-пакетов без изучения
      самого Nix. *(12.4k ⭐, `tar.gz`)* —
      [jetify-com/devbox](https://github.com/jetify-com/devbox)
      — ⛔ добавлялся ролью `runtimes` из тарболла
      `devbox_{version}_linux_amd64.tar.gz`, затем удалён: без Nix окружения
      не собираются, а Nix не используется. Пересматривать только вместе с
      решением ставить Nix — тогда альтернатива `mise` (уже в роли `shell`)
      остаётся проще.
- [ ] **flox** — детерминированные окружения на базе Nix. *(4.2k ⭐,
      `script` — install.flox.dev)* —
      [flox/flox](https://github.com/flox/flox)
      — ⛔ добавлялся ролью `runtimes` (.deb с downloads.flox.dev), затем
      удалён: Nix не используется. Условие пересмотра — то же, что у `devbox`.

---

## Тренды GitHub, 2026-10-23

Новая партия: тренды за день/неделю/месяц плюс свежие репозитории
(`created:>2026-01-01`) по темам `cli`, `tui`, `terminal`, `kubernetes`,
`security`, `devops`, `self-hosted`. По каждому кандидату проверены
`releases/latest` и наличие linux-amd64 артефакта.

Пока ничего не добавлено: сначала нужно решить, нужен ли инструмент, и
проверить лицензию — у части она не MIT/Apache, а проекты молодые (2026 год)
и звёзды у многих раздуты хайпом вокруг агентов. Для каждого кандидата в
заметке указано, при каком условии к нему стоит вернуться.

### Общего назначения

- [ ] **dbx** — клиент к 100+ БД (PostgreSQL, MySQL, SQLite, Redis, Mongo,
      DuckDB) с CLI, desktop-приложением и MCP-сервером. *(27.6k ⭐,
      `.deb`/AppImage)* — [t8y2/dbx](https://github.com/t8y2/dbx)
      — не добавлен: GUI к базам на ноутбуке пока не нужен, а CLI-часть
      закрывают `pgcli`, `harlequin`, `lazysql` и `duckdb`. Пересматривать,
      если понадобится один клиент на все базы сразу, в том числе с MCP.
- [ ] **xerj** — локальный поисковик и MCP-сервер: код, логи, CSV/JSONL,
      PDF, BM25 и вектор, Elasticsearch-совместимый API. *(13.3k ⭐,
      `tar.gz`/`.deb`)* — [xerj-org/xerj](https://github.com/xerj-org/xerj)
      — не добавлен: свежий (`1.0.0-rc.98`) и пересекается с `ripgrep` +
      `fd`; MCP-поиск по репозиторию нужен не каждый день. Пересматривать
      после стабильного 1.0, если понадобится поиск по логам и PDF без
      Elasticsearch.
- [ ] **omnyssh** — SSH-менеджер: GUI и TUI, живые метрики, вкладки, SFTP,
      проброс портов, ключи и сниппеты. *(1.2k ⭐, `.deb`/`tar.gz`/AppImage)*
      — [timhartmann7/omnyssh](https://github.com/timhartmann7/omnyssh)
      — не добавлен: `assh` управляет конфигом, `sshx` — совместной сессией,
      а GUI-менеджер подключений пока не требуется. Пересматривать, если
      станет много хостов и понадобится SFTP/форвардинг из одного окна.
- [ ] **RackPeek** — инвентаризация и документация инфраструктуры и
      хоумлаба из CLI, одиночный бинарник. *(1.8k ⭐, `linux-x64`)* —
      [Timmoth/RackPeek](https://github.com/Timmoth/RackPeek)
      — не добавлен: своей инфраструктуры для документирования на ноутбуке
      нет. Пересматривать при появлении домашней лаборатории или если нужно
      вести инвентарь без NetBox.
- [ ] **Filee** — офлайн-конвертер 255 форматов: изображения, PDF, Office,
      видео, аудио, архивы. *(10.8k ⭐, `linux-x64.tar.gz`)* —
      [KnifeLemon/Filee](https://github.com/KnifeLemon/Filee)
      — не добавлен: это GUI-приложение, а конвертацию в плейбуке закрывают
      `ffmpeg` и разовые CLI-инструменты. Пересматривать, если понадобится
      конвертация «перетащил и готово» без терминала.
- [ ] **VoiceStudio** — полностью локальная диктовка, транскрипция, TTS,
      дубляж и озвучка на 646 языках. *(82.5k ⭐, `.deb`/AppImage)* —
      [debpalash/VoiceStudio](https://github.com/debpalash/VoiceStudio)
      — не добавлен: крупное Electron-приложение, а задача пока не возникает.
      Пересматривать, если понадобится локальная расшифровка встреч или
      диктовка без облака.
- [ ] **googleworkspace/cli** — один CLI на Drive, Gmail, Calendar, Sheets,
      Docs, Chat и Admin. *(31.8k ⭐, `linux-gnu.tar.gz`)* —
      [googleworkspace/cli](https://github.com/googleworkspace/cli)
      — не добавлен: завязан на Google Workspace, которого в сценарии
      ноутбука нет. Пересматривать, если появится рабочий аккаунт Google.
- [ ] **kula** — лёгкий мониторинг Linux с web-UI, один бинарник.
      *(1.3k ⭐, `.deb`/`tar.gz`)* — [c0m4r/kula](https://github.com/c0m4r/kula)
      — не добавлен: интерактивный мониторинг закрывает `btop`, а долгий сбор
      метрик на ноутбуке не нужен. Пересматривать, если понадобится
      веб-панель с историей без Prometheus.

### Безопасность и guardrails

- [ ] **tirith** — защита терминала: homograph-URL, `curl | bash`,
      ANSI-инъекции, обфусцированные команды, вредоносные skills и конфиги.
      *(2.8k ⭐, `.deb`/`linux-gnu.tar.gz`, AGPL-3.0)* —
      [sheeki03/tirith](https://github.com/sheeki03/tirith)
      — не добавлен: лицензия AGPL-3.0, а польза зависит от того, насколько
      часто в терминал попадают чужие install-скрипты. Пересматривать, если
      агенты начнут выполнять команды из внешних источников.
- [ ] **dcg (Destructive Command Guard)** — хук, который блокирует
      `rm -rf`, `git reset --hard`, `push --force` и подобное до выполнения
      агентом. *(6.2k ⭐, `linux-musl.tar.xz`, кастомная лицензия)* —
      [Dicklesworthstone/destructive_command_guard](https://github.com/Dicklesworthstone/destructive_command_guard)
      — не добавлен: лицензию нужно проверить, а правила — согласовать с
      `AGENTS.md`. Сильная сторона: нативная интеграция с OpenCode
      (`dcg install --opencode`). Пересматривать в первую очередь, если
      понадобится техническая страховка от destructive-команд агента.

### Для AI-агентов

- [ ] **hunk** — терминальный diff-viewer для ревью изменений после
      агента: показывает diff как сессию просмотра. *(9.7k ⭐,
      `linux-x64.tar.gz`)* — [modem-dev/hunk](https://github.com/modem-dev/hunk)
      — не добавлен: `delta` и `difftastic` уже стоят. Пересматривать, если
      ревью больших агентских диффов в терминале станет регулярным.
- [ ] **abtop** — `htop` для coding-агентов: сессии, токены, контекст,
      лимиты и порты в реальном времени. *(3.8k ⭐,
      `linux-gnu.tar.xz`)* — [graykode/abtop](https://github.com/graykode/abtop)
      — не добавлен: `rtk` уже считает экономию токенов, а отдельный
      монитор сессий нужен при параллельной работе нескольких агентов.
      Пересматривать вместе с решением по `agent-deck`.
- [ ] **moli** — лёгкий headless-браузер для агентов на Rust.
      *(20.3k ⭐, `linux-gnu.tar.gz`)* —
      [lexmount/moli](https://github.com/lexmount/moli)
      — не добавлен: браузерные задачи агентам пока не ставились, а стек
      Playwright/Chromium на ноутбуке не разворачивался. Пересматривать, если
      понадобится автоматизация браузера без тяжёлых зависимостей.
- [ ] **OfficeCLI** — чтение, редактирование и автоматизация Word, Excel и
      PowerPoint без установленного Office, одиночный бинарник.
      *(32.4k ⭐, `officecli-linux-x64`)* —
      [iOfficeAI/OfficeCLI](https://github.com/iOfficeAI/OfficeCLI)
      — не добавлен: офисные файлы в работе не генерируются. Пересматривать,
      если агентам понадобится править `.docx`/`.xlsx` без LibreOffice.
- [ ] **anydoc** — конвертация Word, PowerPoint, Excel, OpenDocument, RTF,
      EPUB, CSV и PDF в чистый Markdown. *(23.1k ⭐, npm/pip, отдельного
      бинарника нет)* — [firecrawl/anydoc](https://github.com/firecrawl/anydoc)
      — не добавлен: `npx` тянет бинарник при первом запуске, под роль
      `binaries` это не ложится. Пересматривать, если появится npm-путь
      установки или задача массовой конвертации документов.
- [ ] **openwiki** — CLI, который пишет и поддерживает документацию для
      агентов по коду. *(17.4k ⭐, npm, релизных ассетов нет)* —
      [langchain-ai/openwiki](https://github.com/langchain-ai/openwiki)
      — не добавлен: только npm, а документация в репозиториях ведётся
      вручную. Пересматривать, если понадобится автогенерация `AGENTS.md`/
      wiki для чужих проектов.

### Аналоги herdr

- [ ] **agent-deck** — TUI-центр управления сессиями агентов (Claude,
      Codex, Gemini, OpenCode): статусы running/waiting/done, группы, поиск,
      форки, worktrees, учёт стоимости, управление с телефона.
      *(1.1k ⭐, `linux_amd64.tar.gz`)* —
      [asheshgoplani/agent-deck](https://github.com/asheshgoplani/agent-deck)
      — не добавлен: herdr уже стоит и закрывает похожий сценарий.
      Пересматривать, если herdr перестанет хватать для обзора всех сессий
      или понадобится телефонный пульт.
- [ ] **workmux** — git-worktree плюс окно tmux/zellij/kitty на задачу,
      чтобы гонять агентов параллельно без конфликтов. *(2.9k ⭐,
      `linux-amd64.tar.gz`)* — [raine/workmux](https://github.com/raine/workmux)
      — не добавлен: пересекается с `wt` (worktrunk) и `zellij`. Сильная
      сторона — связка worktree + мультиплексор из коробки. Пересматривать,
      если параллельные агенты станут обычной практикой.
- [ ] **alethe-agents** — локальный desktop-воркспейс для нескольких
      агентов и шеллов: настоящие PTY, сплиты, сохраняемые раскладки, история.
      *(974 ⭐, `.deb`/AppImage/`.rpm`)* —
      [Kc1t/alethe-agents](https://github.com/Kc1t/alethe-agents)
      — не добавлен: ещё один GUI-воркспейс поверх уже стоящих herdr,
      `zellij` и T3 Code. Пересматривать, если понадобится именно настольное
      приложение с PTY-сплитами.
- [ ] **cmux** — терминал на Ghostty с вертикальными вкладками и
      уведомлениями под агентов. *(28.4k ⭐, только macOS)* —
      [manaflow-ai/cmux](https://github.com/manaflow-ai/cmux)
      — ⛔ не подходит: сборок под Linux нет, а ноутбук на Ubuntu.

### Аналоги rtk

- [ ] **caveman** — прокси и skill, которые режут около 65% токенов,
      работают с 30+ агентами, включая OpenCode. *(114.5k ⭐,
      `npx @caveman-ai/cli`, Apache-2.0)* —
      [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)
      — не добавлен: rtk уже стоит и делает то же самое, а caveman
      распространяется только через npm. Пересматривать, если rtk перестанет
      покрывать нужные команды или понадобится именно «caveman»-стиль ответов.
- [ ] **context-mode** — прячет вывод инструментов в песочницу (заявлено до
      98%), хранит память сессии и разводит маршрутизацию по 17 платформам
      через MCP и hooks. *(31.9k ⭐, npm/MCP, релизных ассетов нет)* —
      [mksglu/context-mode](https://github.com/mksglu/context-mode)
      — не добавлен: npm + hooks, под роль `binaries` не ложится, а rtk уже
      закрывает сжатие вывода. Пересматривать, если понадобится именно
      песочница для tool-output, а не прокси.
- [ ] **sqz** — точечное сжатие вывода инструментов и дедуп повторов до
      ссылок, Rust, без вызовов LLM. *(660 ⭐,
      `sqz-v*-x86_64-unknown-linux-musl.tar.gz`)* —
      [ojuschugh1/sqz](https://github.com/ojuschugh1/sqz)
      — не добавлен: заметно менее популярен, чем rtk, и решает ту же задачу.
      Пересматривать, если понадобится MCP-вариант сжатия без прокси.
- [ ] **codeburn** — локальный учёт токенов и стоимости по 37 инструментам
      и агентам, разбивка по модели, проекту и задаче. *(11.6k ⭐,
      `npx codeburn`)* — [getagentseal/codeburn](https://github.com/getagentseal/codeburn)
      — не добавлен: это `npx`, и это наблюдение, а не сжатие. Пересматривать,
      если понадобится регулярно смотреть расход по агентам.
- [ ] **cost-xray** — показывает, что именно Claude Code и Codex отправляют
      в API и сколько стоит каждая часть. *(6.1k ⭐, Python)* —
      [tigerless-labs/cost-xray](https://github.com/tigerless-labs/cost-xray)
      — не добавлен: Python-инструмент для разового разбора, а не для
      постоянной установки. Пересматривать при расследовании роста расходов.

### Аналоги T3 Code

- [ ] **orca** — десктоп, мобильный клиент и удалённый рантайм для флота
      агентов, любой агент со своей подпиской. *(103.4k ⭐,
      `.deb`/AppImage/`.rpm`)* — [stablyai/orca](https://github.com/stablyai/orca)
      — не добавлен: T3 Code уже стоит и закрывает тот же сценарий.
      Пересматривать, если понадобится именно удалённый рантайм или
      мобильный клиент лучше, чем у T3.
- [ ] **paperclip** — приложение для управления агентами на работе.
      *(106.4k ⭐, релизных бинарников нет)* —
      [paperclipai/paperclip](https://github.com/paperclipai/paperclip)
      — не добавлен: нет linux-артефакта, только исходники/контейнер.
      Пересматривать, если появится `.deb` или AppImage.
- [ ] **multica** — люди и агенты как одна команда, self-hosted, desktop и
      CLI. *(53.6k ⭐, `.deb`/AppImage/`multica-cli-linux-amd64.tar.gz`)* —
      [multica-ai/multica](https://github.com/multica-ai/multica)
      — не добавлен: это командный сервер, а не пульт к локальным агентам.
      Пересматривать, если понадобится совместная работа с агентами.
- [ ] **cc-switch** — all-in-one переключатель провайдеров и аккаунтов для
      Claude Code, Codex, OpenCode, OpenClaw, Grok Build и Hermes Agent.
      *(148k ⭐, `.deb`/AppImage/`.rpm`)* —
      [farion1231/cc-switch](https://github.com/farion1231/cc-switch)
      — не добавлен: решает другую задачу — смену моделей и аккаунтов, а не
      управление сессиями. Пересматривать, если появится несколько
      провайдеров и нужно быстро переключаться.
- [ ] **atlas** — source control для агентов: несколько агентов, их
      изменения и запросы в одном месте. *(9.6k ⭐,
      `.deb`/AppImage/`linux-x86_64.tar.gz`)* —
      [pacifio/atlas](https://github.com/pacifio/atlas)
      — не добавлен: пересекается с git-воркфлоу и worktree-инструментами.
      Пересматривать, если несколько агентов начнут менять один репозиторий.

### Аналоги Ouroboros

- [ ] **openclaw** — «AI, который реально делает вещи», любая ОС, десктоп и
      несколько платформ. *(392.8k ⭐, `.deb`/AppImage)* —
      [openclaw/openclaw](https://github.com/openclaw/openclaw)
      — не добавлен: Ouroboros уже стоит, а два general-purpose агента на
      ноутбуке избыточны. Пересматривать, если Ouroboros окажется слишком
      тяжёлым или не устроит качество.
- [ ] **nanobot** — лёгкий self-hosted агент: WebUI, инструменты, память,
      MCP, чаты. *(49.3k ⭐, `nanobot-tui-linux-x64.zip`, Python)* —
      [HKUDS/nanobot](https://github.com/HKUDS/nanobot)
      — не добавлен: Python-фреймворк, а не одиночный бинарник; Ouroboros
      покрывает сценарий. Пересматривать, если понадобится свой агент с
      WebUI и минимальными зависимостями.
- [ ] **Octop** — self-hosted ассистент на несколько пользователей и
      агентов. *(10.8k ⭐, `Octop-desktop-linux-amd64.tar.gz`)* —
      [TencentCloud/Octop](https://github.com/TencentCloud/Octop)
      — не добавлен: многопользовательский сервер, на ноутбуке не нужен.
      Пересматривать, если понадобится общий ассистент для команды.
- [ ] **QwenPaw** — персональный ассистент с установкой на свою машину или
      в облако, поддержка нескольких чатов. *(36.0k ⭐, только
      macOS/Windows)* — [agentscope-ai/QwenPaw](https://github.com/agentscope-ai/QwenPaw)
      — ⛔ не подходит: Linux-сборки в релизе нет.
- [ ] **OpenShell** — безопасный приватный рантайм и песочница для
      автономных агентов. *(18.4k ⭐, `.deb`/`tar.gz`)* —
      [NVIDIA/OpenShell](https://github.com/NVIDIA/OpenShell)
      — не добавлен: это не агент, а рантайм под него; отдельного сценария
      на ноутбуке пока нет. Пересматривать, если понадобится изолировать
      агентов с доступом к файлам и сети.
- [ ] **ruflo** — агентский harness со swarm-ами, памятью и RAG.
      *(78.7k ⭐, npm, релизных ассетов нет)* —
      [ruvnet/ruflo](https://github.com/ruvnet/ruflo)
      — не добавлен: npm-пакет и пересекается с opencode/herdr. Пересматривать,
      если понадобятся именно мультиагентные swarm-сценарии.

### Дубли и уже описанные кандидаты

- [ ] **betterleaks** — поиск утёкших секретов везде, где они могут быть.
      *(2.3k ⭐, `linux_x64.tar.gz`)* —
      [betterleaks/betterleaks](https://github.com/betterleaks/betterleaks)
      — ⛔ дублирует `gitleaks` и `trufflehog` из роли `secrets`.
- [ ] **sofka** — Kubernetes TUI на Rust. *(1.8k ⭐,
      `linux-gnu.tar.gz`/`.deb`)* —
      [nklmilojevic/sofka](https://github.com/nklmilojevic/sofka)
      — ⛔ дублирует `k9s` и `kdash`.
- [ ] **treehouse** — управление git-worktree. *(1.9k ⭐,
      `linux-amd64.tar.gz`)* —
      [kunchenguid/treehouse](https://github.com/kunchenguid/treehouse)
      — ⛔ дублирует `wt` (worktrunk).

**Floci** (AWS-эмулятор) в эту партию не заводится: он уже описан в
[`TODO-server-apps.md`](TODO-server-apps.md) как контейнерный сервис.

---

## Партия за последний год, 2026-10-23

Вторая выборка того же дня. Источники: тренды GitHub за день/неделю/месяц
(`github.com/trending`) и поиск `created:>2025-10-23 stars:>500` по темам
`cli`, `tui`, `terminal`, `kubernetes`, `devops`, `docker`, `security`,
`self-hosted`, `monitoring`, `observability`, `git`, `sql`, `dns`, `ssh`,
`terraform` (плюс ключевые слова `cli`/`tui` в имени и описании). По каждому
кандидату проверены лицензия, `releases/latest`, наличие linux-amd64
артефакта, дата последнего пуша и отсутствие в плейбуке (README и все
`TODO-*.md`, `not-install.md`).

Наблюдение по выдаче: в месячных трендах половина — обвязки вокруг
coding-агентов, плюс старые репозитории, которые к «последнему году»
не относятся (Ghidra 2019, Supabase), и уже стоящий `t3code`. Ниже только
утилиты для ноутбука.

### Кандидаты на добавление

- [ ] **revdiff** — TUI для ревью диффов, файлов и документов с аннотациями
      построчно; результат отдаёт в stdout, чтобы передать агенту или скрипту.
      *(936 ⭐, `linux_amd64.tar.gz`/`.deb`, MIT)* —
      [umputun/revdiff](https://github.com/umputun/revdiff)
      — не добавлен: ревью диффов в плейбуке закрывают `difftastic`
      (структурный diff) и `lumen` (diff + сообщение коммита), а сценарий
      «аннотировать и вернуть агенту» пока ручной. Пересматривать, если правки
      агента придётся принимать пачками: `revdiff` отдаёт разметку в stdout,
      и её можно скормить тому же агенту без копипаста.
- [ ] **lazyrsync** — TUI к `rsync`: профили, предпросмотр переноса в виде
      структурированного диффа до запуска, живой прогресс и отмена.
      *(936 ⭐, `x86_64-unknown-linux-gnu.tar.gz`, MIT)* —
      [westpoint-io/lazyrsync](https://github.com/westpoint-io/lazyrsync)
      — не добавлен: `rsync` в плейбуке есть как системная утилита, но
      сценария регулярной синхронизации каталогов на ноутбуке нет, а бэкапы
      описаны `nxs-backup`. Пересматривать, если появится вторая машина или
      NAS: сухой прогон в виде диффа — главное, чего не хватает голому
      `rsync --dry-run`.
- [ ] **tuicr** — ревью кода в терминале с vim-клавишами: сплошной diff,
      комментарии к строке/диапазону/файлу, отправка готового ревью в
      GitHub, GitLab, Gitea, Bitbucket, Azure DevOps и Gerrit; понимает git,
      jj и Mercurial. *(3.4k ⭐, `x86_64-unknown-linux-gnu.tar.gz`, MIT)* —
      [agavra/tuicr](https://github.com/agavra/tuicr)
      — не добавлен: ревью сейчас идёт через `glab`/`gh` и веб (там же
      CI-статусы и обсуждения), а `tuicr` закрывает только сам diff.
      Пересматривать при регулярных ревью MR из терминала: поддержка `jj`
      и отправка ревью одним нажатием — то, чего у `glab` нет.
- [ ] **quien** — whois и разведка по домену: вкладки WHOIS, DNS, MX/почта,
      TLS, HTTP-заголовки, SEO и определение стека; интерактивный TUI.
      *(1.3k ⭐, `linux_amd64.tar.gz`, apt-репозиторий `apt.quien.dev`, MIT)* —
      [retlehs/quien](https://github.com/retlehs/quien)
      — не добавлен: DNS закрывает `doggo`, распространение записей —
      `dnsglobe`, сертификаты — `showcert`. Пересматривать, когда понадобится
      именно `whois` (его в плейбуке нет вообще) или разбор «чужого» домена
      одним экраном вместо трёх утилит.
- [ ] **cloudflare-speed-cli** — замер скорости через Cloudflare в TUI:
      загрузка/отгрузка, latency под нагрузкой, потери и джиттер UDP,
      сравнение IPv4/IPv6, тайминги DNS и TLS, история и экспорт в JSON/CSV.
      *(1.1k ⭐, `x86_64-unknown-linux-musl.tar.xz`, GPL-3.0)* —
      [kavehtehrani/cloudflare-speed-cli](https://github.com/kavehtehrani/cloudflare-speed-cli)
      — не добавлен: утилиты для замера канала в плейбуке нет, но и задачи
      «померить скорость» он пока не решает. Первый кандидат на добавление,
      если такая задача появится: GPL-3.0 допустима (бинарник не линкуется),
      но имя
      ассета кривое — `cloudflare-speed-cli_-x86_64-...` с пустой версией,
      поэтому `url` в роли `binaries` придётся писать без `{version}`.
- [ ] **xleak** — просмотр Excel в терминале: TUI, полнотекстовый поиск,
      отображение формул, переход по листам. *(1.5k ⭐,
      `xleak-linux-x86_64.tar.gz`, MIT)* —
      [bgreenwell/xleak](https://github.com/bgreenwell/xleak)
      — не добавлен: CSV/Parquet разбирает `duckdb`, Excel на ноутбуке
      встречается редко. Пересматривать, если регулярно приходят `.xlsx`
      от заказчиков: конвертировать их в CSV ради `duckdb` — лишний шаг.
- [ ] **sql-tap** — прокси-демон плюс TUI: показывает SQL-трафик между
      приложением и PostgreSQL/MySQL/TiDB в реальном времени, разбирает
      транзакции и запускает EXPLAIN без правки кода приложения.
      *(1.6k ⭐, `sql-tap_0.0.1_linux_amd64.tar.gz`, MIT)* —
      [mickamy/sql-tap](https://github.com/mickamy/sql-tap)
      — не добавлен: версия `0.0.1`, последний пуш — июль 2026. Статистику
      по Postgres даёт `pgcenter`, сами запросы — `pgcli`/`lazysql`, но
      перехвата трафика нет ни у кого. Пересматривать после 0.1 и стабильного
      формата конфига.
- [ ] **cass** (coding-agent-search) — TUI и CLI для индексации и поиска
      истории сессий локальных coding-агентов (OpenCode, Claude Code, Codex,
      Gemini CLI, Cursor и ещё десятки) в одной временной шкале.
      *(1.2k ⭐, `cass-linux-amd64.tar.gz`, MIT + оговорки OpenAI/Anthropic)* —
      [Dicklesworthstone/coding_agent_session_search](https://github.com/Dicklesworthstone/coding_agent_session_search)
      — не добавлен: лицензия помечена как `NOASSERTION` — к MIT добавлены
      тексты-оговорки вендоров, и это надо читать перед установкой. Плюс
      `opencode` и `herdr` уже хранят сессии, а поиск по ним — редкая задача.
      Пересматривать, если сессий станет много и понадобится «где я это уже
      решал».

### Под условием или рано

- [ ] **vykar** — бэкапы на Rust: YAML-конфиг, дедупликация FastCDC,
      шифрование AES-256-GCM/ChaCha20, локальный диск, S3, SFTP, REST-сервер,
      встроенный демон вместо cron. Бинарник называется `vger`.
      *(714 ⭐, `vger-v0.2.0-x86_64-unknown-linux-gnu.tar.gz`, GPL-3.0)* —
      [borgbase/vykar](https://github.com/borgbase/vykar)
      — не добавлен: автор прямо пишет «не для продакшена, но протестируйте
      рядом с другими бэкапами», а YAML-бэкапы уже делает `nxs-backup`
      (без дедупликации). Пересматривать к 1.0, если понадобится
      дедуплицированный репозиторий на S3.
- [ ] **mcpc** — универсальный CLI-клиент MCP: вызывает tools/resources/
      prompts любого сервера, умеет собирать их в cli. *(1.1k ⭐, npm,
      Apache-2.0)* — [apify/mcpc](https://github.com/apify/mcpc)
      — не добавлен: ассетов нет, ставится через npm (Node.js в плейбуке есть
      из роли `runtimes`), а MCP-серверы пока используются только изнутри
      агентов (`kubernetes-mcp-server`, `kubectl-mcp-server`, `radar`).
      Пересматривать, когда понадобится дёргать MCP руками или проверять
      сервер без агента.
- [ ] **deja** — предиктивные inline-подсказки для zsh «как в fish».
      *(914 ⭐, `deja_0.1.0_linux_amd64.tar.gz`, MIT)* —
      [Giammarco-Ferranti/deja](https://github.com/Giammarco-Ferranti/deja)
      — не добавлен: историей и подсказками уже занимаются `atuin`, `mcfly`
      и `zoxide`. Пересматривать только если убрать `mcfly`: два механизма
      подсказок в одной строке конфликтуют.
- [ ] **weathr** — погода в терминале с ASCII-анимацией.
      *(3.1k ⭐, одиночный `weathr-linux-amd64`, GPL-3.0)* —
      [Veirt/weathr](https://github.com/Veirt/weathr)
      — не добавлен: полезной нагрузки для DevOps-ноутбука нет, только
      эстетика. Держать в уме как «одна команда, один бинарник», если
      захочется.
- [ ] **Surge** — TUI-менеджер загрузок на Go: многопоточные загрузки,
      докачка, очередь. *(3.6k ⭐, `*_linux_amd64.tar.gz`, MIT)* —
      [SurgeDM/Surge](https://github.com/SurgeDM/Surge)
      — не добавлен: релиз помечен `0.1.0-SNAPSHOT`, а скачивание закрывают
      `curl` и `wget` из роли `base` — очередей и докачки они не умеют, но и
      потребности в них пока нет. Пересматривать, если понадобится качать
      крупные образы с докачкой после обрыва.
- [ ] **eilmeldung** — TUI RSS-ридер на библиотеке news-flash.
      *(1.0k ⭐, `x86_64-unknown-linux-gnu.tar.gz`, GPL-3.0)* —
      [christo-auer/eilmeldung](https://github.com/christo-auer/eilmeldung)
      — не добавлен: чтение лент не входит в сценарии ноутбука.
      Пересматривать, если появится привычка читать релиз-ноуты через RSS.

### Не приложение или не для Linux

- [ ] **polyglot** — транспайлер SQL на 30+ диалектов. *(973 ⭐, MIT)* —
      [tobilg/polyglot](https://github.com/tobilg/polyglot)
      — ⛔ в релизах только FFI-библиотеки (`polyglot-sql-ffi-*.tar.gz`),
      CLI-бинарника нет: это библиотека для встраивания, а не утилита.
- [ ] **ghui** — TUI-клиент GitHub. *(1.1k ⭐, MIT)* —
      [kitlangton/ghui](https://github.com/kitlangton/ghui)
      — ⛔ релизных ассетов нет (сборка через bun/npm), а функционально
      дублирует `gh` и `ghgrab`.
- [ ] **look** — кросс-платформенный лаунчер в духе Spotlight/Raycast.
      *(1.2k ⭐, GPL-3.0)* — [kunkka19xx/look](https://github.com/kunkka19xx/look)
      — ⛔ в релизе только `Look-0.1.0-macOS.zip`: Linux-сборки нет, несмотря
      на заявленную кросс-платформенность.
- [ ] **Kaku** — терминал для macOS. *(6.1k ⭐, MIT)* —
      [tw93/Kaku](https://github.com/tw93/Kaku)
      — ⛔ единственный ассет — `Kaku.dmg`.
- [ ] **muxy** — терминальный мультиплексор на Rust. *(2.3k ⭐, MIT)* —
      [muxy-app/muxy](https://github.com/muxy-app/muxy)
      — ⛔ Linux-сборки нет (в релизе `.dmg`), да и мультиплексор в плейбуке —
      `zellij`.
- [ ] **tcpfit** — подбор TCP-параметров по замеру BDP на конкретной машине.
      *(909 ⭐, MIT)* — [Kylin010/tcpfit](https://github.com/Kylin010/tcpfit)
      — ⛔ это shell-скрипт без бинарника и с пояснениями на китайском;
      менять `sysctl` на ноутбуке ради тюнинга смысла нет.
- [ ] **plexo** — ускорение загрузок через несколько соединений.
      *(4.9k ⭐, MIT)* — [anmolkapil/plexo](https://github.com/anmolkapil/plexo)
      — ⛔ GUI (`.dmg`/`.exe`/AppImage, deb только arm64), CLI-режима нет.
- [ ] **fakecloud** — локальный эмулятор AWS, 107 сервисов, альтернатива
      LocalStack. *(768 ⭐, `fakecloud-v0.1.1-linux-amd64.tar.gz`, AGPL-3.0)* —
      [faiscadev/fakecloud](https://github.com/faiscadev/fakecloud)
      — ⛔ конкурирует с `Floci`, который уже описан в
      [`TODO-server-apps.md`](TODO-server-apps.md); держать как замену, если
      `Floci` не покроет нужный сервис. AGPL-3.0 для локального сервиса
      допустима, но лишняя.

### Дубли уже стоящего в плейбуке

- [ ] **ttl** — traceroute с TUI: статистика по хопам, ASN/geo, ECMP, MPLS.
      *(1.5k ⭐, `x86_64-unknown-linux-gnu.tar.gz`, Apache-2.0)* —
      [lance0/ttl](https://github.com/lance0/ttl)
      — ⛔ базовый сценарий закрывает `trippy`; ASN/geo и ECMP — приятные
      добавки, но второй traceroute ради них держать незачем.
- [ ] **elio** — терминальный файловый менеджер «всё в комплекте».
      *(902 ⭐, `x86_64-unknown-linux-gnu.tar.gz`, MIT)* —
      [elio-fm/elio](https://github.com/elio-fm/elio)
      — ⛔ дублирует `yazi` и `lf`.
- [ ] **keifu** — TUI для навигации по графу коммитов. *(811 ⭐, Rust)* —
      [trasta298/keifu](https://github.com/trasta298/keifu)
      — ⛔ дублирует `lazygit`/`gitui`: граф коммитов там есть.
- [ ] **syswatch** — диагностика одного хоста в TUI.
      *(916 ⭐, `syswatch-linux-x86_64.tar.gz`, MIT)* —
      [matthart1983/syswatch](https://github.com/matthart1983/syswatch)
      — ⛔ дублирует `btm`/`bottom` и `procs`.
- [ ] **droast** (dockerfile-roast) — линтер Dockerfile «с мнением».
      *(1.1k ⭐, `droast-linux-x86_64`, MIT)* —
      [immanuwell/dockerfile-roast](https://github.com/immanuwell/dockerfile-roast)
      — ⛔ дублирует `hadolint`.
- [ ] **sqlit** — TUI к SQL Server, MySQL, Postgres, SQLite и Turso.
      *(4.9k ⭐, Python, MIT)* — [Maxteabag/sqlit](https://github.com/Maxteabag/sqlit)
      — ⛔ дублирует `lazysql` и `harlequin`.
- [ ] **vex-tui** — редактор Excel/CSV в терминале. *(943 ⭐, MIT)* —
      [CodeOne45/vex-tui](https://github.com/CodeOne45/vex-tui)
      — ⛔ альтернатива `xleak` из списка выше; выбран `xleak` (свежие
      коммиты, режим только для чтения), `vex-tui` — если понадобится
      именно правка таблиц.
- [ ] **leaf** — просмотр Markdown в терминале «как в GUI».
      *(2.2k ⭐, Rust)* — [RivoLink/leaf](https://github.com/RivoLink/leaf)
      — ⛔ дублирует `glow`.
- [ ] **git-wt** — подкоманда `git worktree` без боли. *(563 ⭐, Go)* —
      [k1LoW/git-wt](https://github.com/k1LoW/git-wt)
      — ⛔ дублирует `wt` (worktrunk).
- [ ] **Netcatty**, **Voltius**, **RustConn**, **uniterm**, **navop**,
      **nyaterm**, **KKTerm**, **oxideterm**, **sshdesk** — SSH-клиенты и
      «рабочие пространства» с SFTP, RDP, VNC и вкладками (6.8k, 783, 632,
      906 ⭐ и т. д.) — ⛔ класс уже закрыт `assh` для конфигов и `sshx` для
      совместных сессий; GUI-менеджер подключений осознанно отклонён
      (`omnyssh` в этом же файле). Пересматривать не нужно.
- [ ] **rune** — GPU-ускоренный IDE-мультиплексор. *(1.3k ⭐,
      `rune-linux-amd64.tar.gz`, GPL-3.0)* —
      [unstablebuild/rune](https://github.com/unstablebuild/rune)
      — ⛔ дублирует `zellij` и редактор, которым пользуется владелец.
- [ ] **spotatui**, **gloomberb**, **bookokrat**, **late-sh**, **tuxedo**,
      **micasa** — Spotify-клиент, финансовый терминал, читалка EPUB,
      «клуб» по SSH, todo.txt и учёт домашних дел в терминале
      — ⛔ полезной нагрузки для DevOps-ноутбука нет; перечислены, чтобы
      не разбирать их повторно.
