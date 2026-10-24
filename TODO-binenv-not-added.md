# TODO: что есть в binenv, но нет в плейбуке

Сверка каталога плейбука со списком программ
[binenv — DISTRIBUTIONS.md](https://github.com/devops-works/binenv/blob/develop/DISTRIBUTIONS.md)
(ветка `develop`, снято 2026-10-24: 295 записей).

«Отсутствует» = имя не встречается ни в `README.md`, ни в `not-install.md`, ни в
`TODO*.md`, ни в `roles/**` (defaults, tasks, templates). То есть утилита не
ставится и нигде в проекте не обсуждалась как кандидат.

Из 177 отсутствующих записей сюда попали **93** — активные и не дублирующие
уже установленное. Остальные разложены по своим трекерам, чтобы не дублировать
один кандидат в нескольких файлах:

- **48 заброшенных** (архив или два года без пушей) —
  [`TODO-abandoned.md`](TODO-abandoned.md), раздел «Из списка binenv»;
- **11 серверных и кластерных компонентов** (Loki, Promtail, Traefik, Velero,
  Consul, Nomad, Rancher, Athens, Parca, Logcli, Pomerium) —
  [`TODO-server-apps.md`](TODO-server-apps.md);
- **25 позиций — дубли, варианты уже имеющегося и отказы** (например, `rg` = `ripgrep`,
  `kubectl-krew` = `krew`, `kubeseal` при SOPS/bank-vaults) —
  [`not-install.md`](not-install.md).

Описания в таблицах — перевод апстрим-описаний из списка binenv на русский;
ссылки — оттуда же.

Ограничение сверки: наличие в binenv не гарантирует актуальный linux-amd64
бинарник — часть записей старая. Перед добавлением в плейбук проверять ассеты
последнего релиза (как в
[`TODO-no-prebuilt-binary.md`](TODO-no-prebuilt-binary.md)) и совместимость с
Ubuntu 26.04. Живость апстрима здесь уже проверена через GitHub API
(`archived`, `pushed_at`, 2026-10-24), для GitLab-проектов — через GitLab API.

---

### Kubernetes и GitOps

| Утилита | Что это |
| --- | --- |
| [cmctl](https://cert-manager.io/docs/usage/cmctl/) | CLI для управления ресурсами cert-manager в кластере |
| [havener](https://github.com/homeport/havener/) | швейцарский нож для задач Kubernetes |
| [k8stail](https://github.com/dtan4/k8stail/) | «tail -f» для подов Kubernetes |
| [kconnect](https://github.com/fidelity/kconnect) | CLI для управления подключениями к кластерам Kubernetes |
| [kdigger](https://github.com/quarkslab/kdigger/) | инструмент разведки контекста для пентеста Kubernetes |
| [kube-beacon](https://github.com/chen-keinan/kube-beacon/) | runtime-сканер кластера k8s с проверками безопасности по CIS Kubernetes Benchmark |
| [kubeeye](https://github.com/kubesphere/kubeeye/) | ищет проблемы в Kubernetes: ошибки конфигурации приложений, нездоровые компоненты кластера, проблемы нод |
| [kubestr](https://github.com/kastenhq/kubestr/) | обнаружение, проверка и оценка вариантов хранения в Kubernetes |
| [kubeswitch](https://github.com/danielfoehrKn/kubeswitch/) | «kubectx для операторов» |
| [ksd](https://github.com/mfuentesg/ksd/) | декодер секретов Kubernetes |
| [sloop](https://github.com/salesforce/sloop/) | визуализация истории изменений Kubernetes |
| [sonobuoy](https://github.com/vmware-tanzu/sonobuoy) | диагностика состояния кластера: прогон тестов на соответствие Kubernetes и плагинов, без разрушительных действий |

### IaC и облачные CLI

| Утилита | Что это |
| --- | --- |
| [exo](https://github.com/exoscale/cli/) | CLI для всего в Exoscale: вычисления, хранилище, DNS |
| [terraform-inventory](https://github.com/adammck/terraform-inventory/) | создание ansible-инвентаря из terraform |
| [tofu](https://github.com/opentofu/opentofu) | декларативное управление облачной инфраструктурой |

### Разработка (Go, JS, сборка)

| Утилита | Что это |
| --- | --- |
| [air](https://github.com/cosmtrek/air/) | live reload для Go-приложений |
| [asciigraph](https://github.com/guptarohit/asciigraph/) | Go-пакет для лёгких ASCII-графиков в консольных приложениях без зависимостей |
| [comet](https://github.com/liamg/comet/) | CLI для сообщений в стиле conventional commit |
| [gocloc](https://github.com/hhatto/gocloc/) | быстрый подсчёт строк кода (cloc) |
| [goreleaser](https://github.com/goreleaser/goreleaser/) | быстрая и простая доставка Go-бинарников |
| [gosec](https://github.com/securego/gosec/) | проверка безопасности Go-кода |
| [gowrap](https://github.com/hexdigest/gowrap/) | генерация декораторов для Go-интерфейсов |
| [mage](https://github.com/magefile/mage/) | dev-инструмент в духе Make/rake на Go |
| [pnpm](https://github.com/pnpm/pnpm) | быстрый и экономный по месту пакетный менеджер для nodejs |
| [swag](https://github.com/swaggo/swag/) | автоматическая генерация документации RESTful API по Swagger 2.0 для Go |
| [swagger](https://github.com/go-swagger/go-swagger/) | реализация Swagger 2.0 для Go |
| [tailwindcss](https://github.com/tailwindlabs/tailwindcss/) | utility-first CSS-фреймворк для быстрой разработки интерфейсов |
| [task](https://github.com/go-task/task/) | раннер задач, более простой аналог Make на Go |
| [taskctl](https://github.com/taskctl/taskctl/) | простая современная альтернатива GNU Make |
| [templ](https://github.com/a-h/templ/) | язык для написания HTML-интерфейсов на Go |

### Безопасность

| Утилита | Что это |
| --- | --- |
| [gitjacker](https://github.com/liamg/gitjacker/) | выгрузка git-репозиториев с неправильно настроенных сайтов |
| [hetty](https://hetty.xyz/) | HTTP-инструментарий для security-исследований |
| [lego](https://github.com/go-acme/lego/) | клиент и библиотека Let's Encrypt/ACME на Go |
| [osv-scanner](https://github.com/google/osv-scanner) | сканер уязвимостей на Go, использующий данные https://osv.dev |
| [subfinder](https://github.com/projectdiscovery/subfinder/) | поиск поддоменов; пассивный фреймворк для bug bounty и безопасного пентеста |
| [sx](https://github.com/v-byte-cpu/sx/) | быстрый современный сетевой сканер |
| [tfsec](https://github.com/tfsec/tfsec/) | статический анализ terraform-шаблонов на потенциальные проблемы безопасности |
| [tlsexp](https://gitlab.com/devopsworks/tools/tlsexp/) | проверка срока действия TLS-сертификата |

### Сеть, туннели и нагрузка

| Утилита | Что это |
| --- | --- |
| [ali](https://github.com/nakabonne/ali/) | генерация HTTP-нагрузки с построением результатов в реальном времени |
| [checkip](https://github.com/jreisinger/checkip/) | проверка IP-адреса через разные публичные сервисы |
| [ddosify](https://github.com/ddosify/ddosify) | высокопроизводительный инструмент нагрузочного тестирования на Go |
| [plow](https://github.com/six-ddc/plow/) | высокопроизводительный HTTP-бенчмарк с веб-UI и выводом в терминал в реальном времени |
| [pomerium-cli](https://github.com/pomerium/cli/) | CLI identity-aware прокси Pomerium |
| [shell2http](https://github.com/msoap/shell2http/) | выполнение shell-команд через HTTP-сервер |
| [sshproxy](https://github.com/cea-hpc/sshproxy/) | проксирование SSH-соединений на шлюзе |
| [yeshttp](https://gitlab.com/devopsworks/tools/yeshttp/) | HTTP-сервер, который всегда отвечает «да» |

### Мониторинг, логи, метрики

| Утилита | Что это |
| --- | --- |
| [egress-auditor](https://github.com/devops-works/egress-auditor) | аудит исходящих соединений и заполнение цепочки OUTPUT |
| [grafana-kiosk](https://github.com/grafana/grafana-kiosk/) | kiosk-режим для Grafana |
| [influxevent](https://gitlab.com/devopsworks/tools/influxevent/) | обёртка команд с отправкой результата, таймингов и метрик производительности в InfluxDB |
| [scan-exporter](https://github.com/devops-works/) | сканирование целей с выдачей результатов в Prometheus |

### Базы данных и Kafka

| Утилита | Что это |
| --- | --- |
| [kaf](https://github.com/birdayz/kaf) | современный CLI для Apache Kafka на Go |
| [kafkactl](https://github.com/deviceinsight/kafkactl/) | CLI для управления Apache Kafka |
| [pgmetrics](https://pgmetrics.io/) | сбор и показ информации и статистики работающего PostgreSQL |
| [pscale](https://github.com/planetscale/cli/) | CLI для базы PlanetScale |
| [table-trends](https://gitlab.kheops.ch/infrastructure/tools/table-trends) | тренды размера данных таблиц БД по метрикам InfluxDB |

### Shell, TUI и работа с текстом

| Утилита | Что это |
| --- | --- |
| [broot](https://github.com/Canop/broot/) | новый способ смотреть и перемещаться по дереву каталогов |
| [chezmoi](https://github.com/twpayne/chezmoi/) | управление dotfiles |
| [choose](https://github.com/theryangeary/choose/) | дружелюбная и быстрая замена cut (иногда awk) |
| [curlie](https://github.com/rs/curlie/) | сила curl с удобством httpie |
| [dstask](https://github.com/naggie/dstask/) | одномашинный терминальный todo-менеджер с синхронизацией через git и markdown-заметками к задачам |
| [fselect](https://github.com/jhspetersson/fselect/) | поиск файлов SQL-подобными запросами |
| [gotop](https://github.com/xxxserxxx/gotop/) | ещё один графический монитор активности в терминале |
| [gron](https://github.com/tomnomnom/gron/) | делает JSON удобным для grep |
| [himalaya](https://github.com/soywod/himalaya/) | CLI-клиент электронной почты |
| [jless](https://github.com/PaulJuliusMartinez/jless) | пейджер для JSON в командной строке |
| [lsd](https://github.com/Peltoche/lsd/) | ls нового поколения |
| [micro](https://github.com/zyedidia/micro/) | современный и интуитивный текстовый редактор для терминала |
| [miller](https://github.com/johnkerl/miller/) | awk, sed, cut, join и sort для данных с именованными полями вроде CSV и TSV |
| [onefetch](https://github.com/o2sh/onefetch/) | сводка по git-репозиторию в терминале |
| [pandoc-plot](https://laurentrdc.github.io/pandoc-plot/) | рендер графиков и вставка рисунков в документы Pandoc |
| [peco](https://github.com/peco/peco/) | простой интерактивный фильтр |
| [pet](https://github.com/knqyf263/pet) | простой менеджер сниппетов командной строки на Go |
| [shdotenv](https://github.com/ko1nksm/shdotenv/) | поддержка dotenv для shell и POSIX-спецификации .env |
| [slides](https://maaslalani.com/slides/) | презентации в терминале |
| [wtf](https://github.com/wtfutil/wtf/) | персональный информационный дашборд в терминале |
| [wuzz](https://github.com/asciimoo/wuzz/) | интерактивный CLI для инспекции HTTP |

### Файлы, хранилища и Docker

| Утилита | Что это |
| --- | --- |
| [crabz](https://github.com/sstadick/crabz) | аналог pigz на Rust |
| [docker-slim](https://github.com/docker-slim/docker-slim/) | уменьшает образ Docker до 30 раз без изменения содержимого (для компилируемых языков — сильнее), попутно повышая безопасность |
| [dockle](https://github.com/goodwithtech/dockle/) | линтер образов контейнеров по безопасности и best practices |
| [ffsend](https://github.com/timvisee/ffsend/) | безопасный обмен файлами из командной строки, полноценный клиент Firefox Send |
| [geesefs](https://github.com/yandex-cloud/) | реализация FUSE-ФС поверх S3 |
| [git-bug](https://github.com/MichaelMure/git-bug/) | распределённый офлайн-баг-трекер внутри git с мостами к внешним системам |
| [gorss](https://github.com/Lallassu/gorss/) | RSS-ридер для терминала на Go |
| [hugo](https://github.com/gohugoio/hugo/) | самый быстрый фреймворк для сборки сайтов |
| [imgpkg](https://github.com/vmware-tanzu/carvel-imgpkg/) | хранение и распространение наборов файлов (например, конфигурации приложения) как образов в Docker (OCI) registry |
| [jira-cli](https://github.com/ankitpokhrel/jira-cli) | функциональный интерактивный CLI для Jira |
| [juicefs](https://github.com/juicedata/juicefs/) | распределённая POSIX-ФС поверх Redis и S3 |
| [lux](https://github.com/iawia002/lux/) | быстрый, простой и аккуратный загрузчик видео |
| [toji](https://github.com/leucos/toji/) | мост Toggle → Jira: синхронизация записей Toggl в worklog задач Jira |
| [upx](https://github.com/upx/upx/) | упаковщик исполняемых файлов |
| [yj](https://github.com/sclevine/yj/) | конвертация между YAML, TOML, JSON и HCL |
| [yor](https://github.com/bridgecrewio/yor/) | расширяемый автотеггер IaC-файлов для связи сущностей в облаке с создавшим их кодом |
| [clx](https://github.com/bensadeh/circumflex) | Hacker News в терминале (circumflex) |
