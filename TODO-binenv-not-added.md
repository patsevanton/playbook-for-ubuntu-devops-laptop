# TODO: что есть в binenv, но нет в плейбуке

Сверка каталога плейбука со списком программ
[binenv — DISTRIBUTIONS.md](https://github.com/devops-works/binenv/blob/develop/DISTRIBUTIONS.md)
(ветка `develop`, снято 2026-10-24: 295 записей).

«Отсутствует» = имя не встречается ни в `README.md`, ни в `not-install.md`, ни в
`TODO*.md`, ни в `roles/**` (defaults, tasks, templates). То есть утилита не
ставится и нигде в проекте не обсуждалась как кандидат.

Из 185 отсутствующих записей сюда попали **113** — активные и не дублирующие
уже установленное. Остальные разложены по своим трекерам, чтобы не дублировать
один кандидат в нескольких файлах:

- **48 заброшенных** (архив или два года без пушей) —
  [`TODO-abandoned.md`](TODO-abandoned.md), раздел «Из списка binenv»;
- **11 серверных и кластерных компонентов** (Loki, Promtail, Traefik, Velero,
  Consul, Nomad, Rancher, Athens, Parca, Logcli, Pomerium) —
  [`TODO-server-apps.md`](TODO-server-apps.md);
- **13 дублей и вариантов уже имеющегося** (например, `rg` = `ripgrep`,
  `kubectl-krew` = `krew`, `kubeseal` при SOPS/bank-vaults) —
  [`not-install.md`](not-install.md).

Описания в таблицах — цитаты из апстрим-списка binenv (на английском, чтобы не
терять формулировки), ссылки — оттуда же.

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
| [cmctl](https://cert-manager.io/docs/usage/cmctl/) | CLI tool that can help you to manage cert-manager resources inside your cluster |
| [eksctl](https://github.com/weaveworks/eksctl/) | eksctl is a simple CLI tool for creating clusters on AWS EKS |
| [havener](https://github.com/homeport/havener/) | Think of it as a swiss army knife for Kubernetes tasks |
| [k8stail](https://github.com/dtan4/k8stail/) | tail -f experience for Kubernetes Pods |
| [kconnect](https://github.com/fidelity/kconnect) | A CLI for managing Kubernetes cluster connections |
| [kdigger](https://github.com/quarkslab/kdigger/) | kdigger is a context discovery tool for Kubernetes penetration testing |
| [kube-beacon](https://github.com/chen-keinan/kube-beacon/) | Open Source runtime scanner for k8s cluster and perform security audit checks based on CIS Kubernetes Benchmark specification |
| [kubeeye](https://github.com/kubesphere/kubeeye/) | KubeEye aims to find various problems on Kubernetes, such as application misconfiguration, unhealthy cluster components and node problems |
| [kubens](https://github.com/ahmetb/kubectx/) | Faster way to switch between namespaces in kubectl |
| [kubestr](https://github.com/kastenhq/kubestr/) | A tool to discover, validate and evaluate Kubernetes storage options |
| [kubeswitch](https://github.com/danielfoehrKn/kubeswitch/) | The kubectx for operators |
| [ksd](https://github.com/mfuentesg/ksd/) | Kubernetes secret decoder |
| [kyverno](https://github.com/kyverno/kyverno/) | Kyverno (Greek for “govern”) is a policy engine designed specifically for Kubernetes |
| [operator-sdk](https://github.com/operator-framework/operator-sdk/) | SDK for building Kubernetes applications. Provides high level APIs, useful abstractions, and project scaffolding |
| [sloop](https://github.com/salesforce/sloop/) | Kubernetes History Visualization |
| [sonobuoy](https://github.com/vmware-tanzu/sonobuoy) | Sonobuoy is a diagnostic tool that makes it easier to understand the state of a Kubernetes cluster by running a set of Kubernetes conformance tests and other plugins in an accessible and non-destructive manner |

### IaC и облачные CLI

| Утилита | Что это |
| --- | --- |
| [aws-iam-authenticator](https://github.com/kubernetes-sigs/) | A tool to use AWS IAM credentials to authenticate to a Kubernetes cluster |
| [aws-vault](https://github.com/99designs/aws-vault/) | A vault for securely storing and accessing AWS credentials in development environments |
| [awstaghelper](https://github.com/mpostument/awstaghelper/) | AWS bulk tagging tool |
| [cli53](https://github.com/barnybug/cli53/) | Command line tool for Amazon Route 53 |
| [doctl](https://github.com/digitalocean/doctl/) | The official digital ocean command line interface |
| [exo](https://github.com/exoscale/cli/) | Command-line tool for everything at Exoscale: compute, storage, dns |
| [hcloud](https://github.com/hetznercloud/cli/) | a tool for controlling hetzner cloud resources |
| [helm-docs](https://github.com/norwoodj/helm-docs/) | a tool for controlling hetzner cloud resources |
| [iamlive](https://github.com/iann0036/iamlive/) | Generate an IAM policy from AWS calls using client-side monitoring (CSM) or embedded proxy |
| [saml2aws](https://github.com/Versent/saml2aws/) | CLI tool which enables you to login and retrieve AWS temporary credentials using a SAML IDP |
| [ssosync](https://github.com/awslabs/ssosync/) | Populate AWS SSO directly with your G Suite users and groups using either a CLI or AWS Lambda |
| [terraform-inventory](https://github.com/adammck/terraform-inventory/) | terraform-inventory creates an ansible inventory from terraform |
| [tofu](https://github.com/opentofu/opentofu) | OpenTofu lets you declaratively manage your cloud infrastructure |

### Разработка (Go, JS, сборка)

| Утилита | Что это |
| --- | --- |
| [air](https://github.com/cosmtrek/air/) | ☁️ Live reload for Go apps |
| [asciigraph](https://github.com/guptarohit/asciigraph/) | Go package to make lightweight ASCII line graph ╭┈╯ in command line apps with no other dependencies |
| [comet](https://github.com/liamg/comet/) | Command line tool to help you use conventional commit messages |
| [gocloc](https://github.com/hhatto/gocloc/) | A little fast cloc(Count Lines Of Code) |
| [golangci-lint](https://github.com/golangci/golangci-lint/) | Fast linters Runner for Go |
| [goreleaser](https://github.com/goreleaser/goreleaser/) | Deliver Go binaries as fast and easily as possible |
| [gosec](https://github.com/securego/gosec/) | Golang security checker |
| [gowrap](https://github.com/hexdigest/gowrap/) | GoWrap is a command line tool for generating decorators for Go interfaces |
| [mage](https://github.com/magefile/mage/) | a Make/rake-like dev tool using Go |
| [pnpm](https://github.com/pnpm/pnpm) | Fast, disk space efficient package manager for nodejs |
| [swag](https://github.com/swaggo/swag/) | Automatically generate RESTful API documentation with Swagger 2.0 for Go |
| [swagger](https://github.com/go-swagger/go-swagger/) | Swagger 2.0 implementation for go |
| [tailwindcss](https://github.com/tailwindlabs/tailwindcss/) | A utility-first CSS framework for rapid UI development |
| [task](https://github.com/go-task/task/) | A task runner / simpler Make alternative written in Go |
| [taskctl](https://github.com/taskctl/taskctl/) | simple modern alternative to GNU Make |
| [templ](https://github.com/a-h/templ/) | A language for writing HTML user interfaces in Go |

### Безопасность

| Утилита | Что это |
| --- | --- |
| [gitjacker](https://github.com/liamg/gitjacker/) | Leak git repositories from misconfigured websites |
| [hetty](https://hetty.xyz/) | Hetty is an HTTP toolkit for security research |
| [lego](https://github.com/go-acme/lego/) | Let's Encrypt/ACME client and library written in Go |
| [osv-scanner](https://github.com/google/osv-scanner) | Vulnerability scanner written in Go which uses the data provided by https://osv.dev |
| [subfinder](https://github.com/projectdiscovery/subfinder/) | Subfinder is a subdomain discovery tool that discovers valid subdomains for websites. Designed as a passive framework to be useful for bug bounties and safe for penetration testing |
| [sx](https://github.com/v-byte-cpu/sx/) | Fast, modern, easy-to-use network scanner |
| [tfsec](https://github.com/tfsec/tfsec/) | TFsec uses static analysis of your terraform templates to spot potential security issues |
| [tlsexp](https://gitlab.com/devopsworks/tools/tlsexp/) | TLS certificate expiration checker |

### Сеть, туннели и нагрузка

| Утилита | Что это |
| --- | --- |
| [ali](https://github.com/nakabonne/ali/) | Generate HTTP load and plot the results in real-time |
| [checkip](https://github.com/jreisinger/checkip/) | Check an IP address using various public services |
| [chisel](https://github.com/jpillora/chisel/) | A fast TCP/UDP tunnel over HTTP |
| [ddosify](https://github.com/ddosify/ddosify) | High-performance load testing tool, written in Golang |
| [plow](https://github.com/six-ddc/plow/) | A high-performance HTTP benchmarking tool with real-time web UI and terminal displaying |
| [pomerium-cli](https://github.com/pomerium/cli/) | Pomerium is an identity-aware access proxy (cli) |
| [shell2http](https://github.com/msoap/shell2http/) | Executing shell commands via HTTP server |
| [sshproxy](https://github.com/cea-hpc/sshproxy/) | Proxy SSH connections on a gateway |
| [yeshttp](https://gitlab.com/devopsworks/tools/yeshttp/) | A HTTP server that always says yes |

### Мониторинг, логи, метрики

| Утилита | Что это |
| --- | --- |
| [egress-auditor](https://github.com/devops-works/egress-auditor) | Audit your egress connections and finally populate this OUTPUT chain ! |
| [grafana-kiosk](https://github.com/grafana/grafana-kiosk/) | Kiosk utility for Grafana |
| [influxevent](https://gitlab.com/devopsworks/tools/influxevent/) | Influxevent wraps commands and sends command result, timing & performance samples to an influxDB server |
| [pint](https://github.com/cloudflare/pint/) | Prometheus rule linter |
| [scan-exporter](https://github.com/devops-works/) | Scans targets and exposes results to Prometheus |

### Базы данных и Kafka

| Утилита | Что это |
| --- | --- |
| [kaf](https://github.com/birdayz/kaf) | Modern CLI for Apache Kafka, written in Go |
| [kafkactl](https://github.com/deviceinsight/kafkactl/) | Command Line Tool for managing Apache Kafka |
| [pgmetrics](https://pgmetrics.io/) | Collect and display information and stats from a running PostgreSQL server |
| [pscale](https://github.com/planetscale/cli/) | The CLI for PlanetScale Database |
| [table-trends](https://gitlab.kheops.ch/infrastructure/tools/table-trends) | This tool let's you check database table data size trends from influxdb metrics |

### Shell, TUI и работа с текстом

| Утилита | Что это |
| --- | --- |
| [broot](https://github.com/Canop/broot/) | A new way to see and navigate directory trees |
| [chezmoi](https://github.com/twpayne/chezmoi/) | Dotfile management tool |
| [choose](https://github.com/theryangeary/choose/) | A human-friendly and fast alternative to cut and (sometimes) awk |
| [curlie](https://github.com/rs/curlie/) | The power of curl, the ease of use of httpie |
| [dstask](https://github.com/naggie/dstask/) | Single binary terminal-based todo manager with git-based sync + markdown notes per task |
| [fselect](https://github.com/jhspetersson/fselect/) | Find files with SQL-like queries |
| [gotop](https://github.com/xxxserxxx/gotop/) | Just another terminal based graphical activity monitor |
| [gron](https://github.com/tomnomnom/gron/) | Make JSON greppable! |
| [himalaya](https://github.com/soywod/himalaya/) | 📫 CLI email client |
| [jless](https://github.com/PaulJuliusMartinez/jless) | A command-line pager for JSON data |
| [lsd](https://github.com/Peltoche/lsd/) | The next gen ls command |
| [micro](https://github.com/zyedidia/micro/) | A modern and intuitive terminal-based text editor |
| [miller](https://github.com/johnkerl/miller/) | Miller is like awk, sed, cut, join, and sort for name-indexed data such as CSV, TSV |
| [onefetch](https://github.com/o2sh/onefetch/) | Git repository summary on your terminal |
| [pandoc-plot](https://laurentrdc.github.io/pandoc-plot/) | Render and include figures in Pandoc documents using your plotting toolkit of choice |
| [peco](https://github.com/peco/peco/) | Simplistic interactive filtering tool |
| [pet](https://github.com/knqyf263/pet) | Simple command-line snippet manager, written in Go |
| [rga](https://github.com/phiresky/ripgrep-all/) | ripgrep, but also search in PDFs, E-Books, Office documents, zip, tar.gz |
| [shdotenv](https://github.com/ko1nksm/shdotenv/) | dotenv support for shell and POSIX-compliant .env syntax specification |
| [slides](https://maaslalani.com/slides/) | Terminal based presentation tool |
| [wtf](https://github.com/wtfutil/wtf/) | The personal information dashboard for your terminal |
| [wuzz](https://github.com/asciimoo/wuzz/) | Interactive cli tool for HTTP inspection |

### Файлы, хранилища и Docker

| Утилита | Что это |
| --- | --- |
| [buildx](https://github.com/docker/buildx/) | Docker CLI plugin for extended build capabilities with BuildKit |
| [crabz](https://github.com/sstadick/crabz) | Like pigz, but rust |
| [docker-slim](https://github.com/docker-slim/docker-slim/) | DockerSlim (docker-slim): Don't change anything in your Docker\ncontainer image and minify it by up to 30x (and for compiled languages even\nmore) making it secure too! (free and open source) |
| [dockle](https://github.com/goodwithtech/dockle/) | Container Image Linter for Security, Helping build the Best-Practice Docker Image, Easy to start |
| [ffsend](https://github.com/timvisee/ffsend/) | Easily and securely share files from the command line. A fully featured Firefox Send client |
| [geesefs](https://github.com/yandex-cloud/) | Finally, a good FUSE FS implementation over S3 |
| [git-bug](https://github.com/MichaelMure/git-bug/) | Distributed, offline-first bug tracker embedded in git, with bridges |
| [gorss](https://github.com/Lallassu/gorss/) | Go Terminal Feed Reader |
| [hugo](https://github.com/gohugoio/hugo/) | The world’s fastest framework for building websites |
| [imgpkg](https://github.com/vmware-tanzu/carvel-imgpkg/) | Allows you to store and distribute sets of files (e.g. application configuration) as images in Docker (OCI) registries |
| [jira-cli](https://github.com/ankitpokhrel/jira-cli) | 🔥 Feature-rich interactive Jira command line |
| [juicefs](https://github.com/juicedata/juicefs/) | JuiceFS is a distributed POSIX file system built on top of Redis and S3 |
| [kopia](https://github.com/kopia/kopia/) | Cross-platform backup tool for Windows, macOS & Linux with fast, incremental backups, client-side end-to-end encryption, compression and data deduplication. CLI and GUI included |
| [lux](https://github.com/iawia002/lux/) | Fast, simple and clean video downloader |
| [toji](https://github.com/leucos/toji/) | Toji is a Toggle ➡ Jira bridge: sync your Toggl entries directly into Jira issues worklog |
| [upx](https://github.com/upx/upx/) | UPX - the Ultimate Packer for eXecutables |
| [yj](https://github.com/sclevine/yj/) | Convert between YAML, TOML, JSON, and HCL |
| [yor](https://github.com/bridgecrewio/yor/) | Extensible auto-tagger for your IaC files. The ultimate way to link entities in the cloud back to the codified resource which created it |
| [clx](https://github.com/bensadeh/circumflex) | 🌿 It's Hacker News in your terminal (circumflex) |

