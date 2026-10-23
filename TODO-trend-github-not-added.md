# TODO: кандидаты из трендов GitHub, которые не попали в плейбук

Остаток разбора трендовых утилит GitHub: трекер `TODO-trend-github.md` разобран
полностью (последняя партия — 2026-10-23) и удалён, здесь только те кандидаты,
которые осознанно не добавлены — это не приложение либо утилита удалена после
добавления. Свежая партия на рассмотрение — в разделе
[«Тренды GitHub, 2026-10-23»](#тренды-github-2026-10-23). Добавленные
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
