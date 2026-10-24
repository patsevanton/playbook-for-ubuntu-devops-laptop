# TODO: утилиты из статьи Habr «Полезные консольные Linux утилиты», которых нет в плейбуке

Сверка плейбука со статьёй
[«Полезные консольные Linux утилиты»](https://habr.com/ru/articles/553000/)
(Habr, 18.04.2021; в статье около 130 утилит, часть — из подборки
`skx/sysadmin-util` и пакета `moreutils`).

«Отсутствует» = имя не встречается ни в `README.md`, ни в `not-install.md`, ни в
`TODO*.md`, ни в `roles/**` (defaults, tasks, templates, `playbook.yml`). То есть
утилита не ставится и нигде в проекте не обсуждалась как кандидат.

Итого по статье:

| Статус | Сколько | Где описано |
| --- | --- | --- |
| уже ставится или упоминается в проекте | 51 | `README.md`, `not-install.md`, `roles/**` |
| отсутствует — живой кандидат | 31 | этот файл |
| отсутствует — заброшено | 34 | [`TODO-abandoned.md`](TODO-abandoned.md) |
| отсутствует — нет linux-бинарника | 4 | [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md) |
| отсутствует — сознательный отказ | 12 | [`not-install.md`](not-install.md) |
| отсутствует — сервис, не на ноутбук | 1 | [`TODO-server-apps.md`](TODO-server-apps.md) |

Дедупликация: каждый кандидат описан ровно в одном файле-трекере, как принято в
[`TODO.md`](TODO.md). Заброшенные проекты из статьи (архив GitHub или последний
пуш в 2023 году и раньше) перенесены в
[`TODO-abandoned.md`](TODO-abandoned.md), кандидаты без linux-бинарника — в
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md), сознательные отказы,
включая «только скрипт», — в [`not-install.md`](not-install.md), сервисы не для
ноутбука — в [`TODO-server-apps.md`](TODO-server-apps.md).

Способ установки сверен 2026-10-24: пакеты — `apt-cache policy` на Ubuntu,
релизы — последний GitHub-релиз (`gh api .../releases/latest`). Наличие пакета в
Ubuntu не значит, что плейбук его ставит — это отдельное решение. Перед
добавлением проверять актуальность релизных ассетов, как в
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md).

---

## Сеть, DNS, туннели

| Утилита | Что это | Как ставится и что проверить |
| --- | --- | --- |
| [mtr](https://github.com/traviscross/mtr) | traceroute + ping в одном, живая статистика по хопам | пакет Ubuntu `mtr` (0.95) |
| [dnspeep](https://github.com/jvns/dnspeep) | смотрит DNS-запросы процессов (Rust, нужен libpcap) | GitHub: `dnspeep-linux.tar.gz` |
| [pingtop](https://github.com/laixintao/pingtop) | TUI, пингует несколько хостов одновременно | PyPI → роль `pipx_apps`, релизов на GitHub нет |
| [iprange](https://github.com/firehol/iprange) | арифметика подсетей и IP-диапазонов (для ipset/фаервола) | пакет Ubuntu `iprange`; есть и GitHub-релиз |
| [frp](https://github.com/fatedier/frp) | обратный прокси: доступ к локальному серверу за NAT | GitHub: `frp_*_linux_amd64.tar.gz` |

## SSH и удалённые сессии

| Утилита | Что это | Как ставится и что проверить |
| --- | --- | --- |
| [mosh](https://mosh.org/) | мобильный shell: устойчив к обрывам и смене IP | пакет Ubuntu `mosh` (1.4.0) |
| [tmate](https://tmate.io/) | мгновенный доступ к терминалу через tmux-сервер | пакет Ubuntu `tmate` (2.4.0) |

## HTTP, JSON и анализ логов

| Утилита | Что это | Как ставится и что проверить |
| --- | --- | --- |
| [rebound](https://github.com/shobrook/rebound) | при исключении сразу ищет вопрос на Stack Overflow | пакет Ubuntu `rebound` (тот же проект, 2.0.0) |
| [HTTP Prompt](https://github.com/httpie/http-prompt) | интерактивный HTTPie с автодополнением и OpenAPI | PyPI → роль `pipx_apps` |
| [httping](https://github.com/flok99/httping) | «ping» по HTTP: доступность и задержка сайта | пакет Ubuntu `httping` (4.4.0) |
| [red](https://github.com/antonmedv/red) | статистика по JSON-логам из пайпа | GitHub: `red_*_Linux_x86_64.tar.gz` |
| [jql](https://github.com/cube2222/jql) | альтернатива jq с более простым синтаксисом | GitHub: `jql_*_linux_amd64.tar.gz` |
| [topngx](https://github.com/gsquire/topngx) | `top` по access-логам nginx (замена ngxtop) | GitHub: `topngx-*_x86_64-unknown-linux-musl.tar.gz` |
| [GoAccess](https://goaccess.io/) | анализатор веб-логов, TUI и HTML-отчёты | пакет Ubuntu `goaccess` (1.9.4) |
| [lnav](https://github.com/tstack/lnav) | продвинутый просмотрщик логов с SQL по ним | пакет Ubuntu `lnav` (0.13.2) |

## Система, файлы, диагностика

| Утилита | Что это | Как ставится и что проверить |
| --- | --- | --- |
| [sponge](https://joeyh.name/code/moreutils/) | «губка» для stdin: читает всё, потом пишет в файл | пакет Ubuntu `moreutils` |
| [watchman](https://github.com/facebook/watchman) | следит за файлами и триггерит действия при изменениях | пакет Ubuntu `watchman` (4.9.0) |
| [tlog](https://github.com/Scribery/tlog) | запись и воспроизведение терминальных сессий | пакет Ubuntu `tlog` (14) |
| [whatfiles](https://github.com/spieglt/whatfiles) | какие файлы открыл процесс (упрощённый strace) | GitHub: `whatfiles-v*_linux-x86_64` |
| [fatrace](https://github.com/martinpitt/fatrace) | file access trace без strace/blktrace | пакет Ubuntu `fatrace` (0.19) |
| [sysdig](https://github.com/draios/sysdig) | трассировка системных вызовов с фильтрами | пакет Ubuntu `sysdig` (0.40) |
| [ioping](https://github.com/koct9i/ioping) | проверка отзывчивости дисковой подсистемы | пакет Ubuntu `ioping` (1.3) |
| [sosreport](https://github.com/sosreport/sos) | сбор диагностики системы одним архивом | пакет Ubuntu `sosreport` (4.11) |
| [hstr](https://github.com/dvorka/hstr) | TUI-поиск по истории команд | пакет Ubuntu `hstr` (3.1) |
| [pastebinit](https://github.com/pastebinit/pastebinit) | отправка текста на pastebin из CLI | пакет Ubuntu `pastebinit` (1.8) |
| [datanymizer](https://github.com/datanymizer/datanymizer) | анонимизация дампа БД по правилам | GitHub: `pg_datanymizer-linux-x86_64.tar.gz` |

## Безопасность и TLS

| Утилита | Что это | Как ставится и что проверить |
| --- | --- | --- |
| [testssl.sh](https://github.com/drwetter/testssl.sh) | проверка TLS/SSL сервиса по всем параметрам | пакет Ubuntu `testssl.sh`; есть GitHub-релиз |
| [ssl-cert-check](https://github.com/Matty9191/ssl-cert-check) | проверка срока действия сертификатов + письмо | пакет Ubuntu `ssl-cert-check` (4.14) |

## Разработка, shell, шпаргалки

| Утилита | Что это | Как ставится и что проверить |
| --- | --- | --- |
| [clog-cli](https://github.com/clog-tool/clog-cli) | CHANGELOG из истории git-коммитов | GitHub: `clog-*_x86_64-unknown-linux-gnu.tar.gz` |
| [GNU parallel](https://www.gnu.org/software/parallel/) | параллельное выполнение команд вместо `xargs`/циклов | пакет Ubuntu `parallel` (20240222) |
| [cheat.sh](https://github.com/chubin/cheat.sh) | шпаргалки по командам из терминала | пакет Ubuntu `cht.sh` или установочный скрипт с cheat.sh |

---

## Куда ушли остальные

- **Заброшенные** (34): dnsmeter, ssh-tools, Nginx-builder, GeoLogonalyzer,
  container-diff, asciicast2gif, runnel, ssh-config, SSH-Attack-Stats, tunnelto,
  localtls, tfquery, kubesql, logtop, frontail, critic.sh, pd.sh,
  linux-exploit-suggester-2 и 16 скриптов из `skx/sysadmin-util` (ago, cronic,
  cidr2ip, collapse, dupes, empty-dir, expand-ipv6, multi-ping, pyhttpd, randpass,
  since, ssl-expiry-date, until-error / until-success, when-down / when-up,
  mysql-slave-check, which-shell) — в
  [`TODO-abandoned.md`](TODO-abandoned.md).
- **Нет готового linux-бинарника** (4): Eternal Terminal (в 7.0.0 только
  `arm64.deb`), httpstat, desed, bashdb — в
  [`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md).
- **Сознательный отказ** (12): `watch`, `timeout`, `gitlab`, graphpath,
  teleconsole, SSHHeatmap, fpaste, mkchain, domain-check-2,
  linux-exploit-suggester, Prochunter, DNSBL-скрипт — в
  [`not-install.md`](not-install.md).
- **Сервис, не на ноутбук** (1): https-portal — в
  [`TODO-server-apps.md`](TODO-server-apps.md).

## Что из статьи в проекте уже есть

Ставится или обсуждается (51): binenv, bat, exa, fd, procs, sd, dust, starship,
ripgrep, ripgrep-all, grex, fzf, jq, peco, HTTPie, xh, shell2http, reachable,
lazydocker, gotty, ngrok, lazygit, bottom, bandwhich, delta, gdu, dog, gitleaks,
fx, dive, termshark, sysinfo, dry, gh, assh, txeh, asn, choose, jl, wondershaper,
mkcert, netutils-linux, ctop, ShellCheck, kubectx, kubens, fselect, k3d, arkade,
helmfile, 3mux, localstack.
