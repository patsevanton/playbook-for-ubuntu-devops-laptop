# TODO-nora: NORA (artifact registry) и локальный TLS-сертификат

Отдельный трекер под **NORA** — self-hosted мультиформатный artifact registry
(Docker, npm, PyPI, Cargo, Go, Maven, Terraform, Ansible Galaxy, RPM, DEB и др.):
- [getnora-io/nora](https://github.com/getnora-io/nora)
- доки: [getnora.dev](https://getnora.dev) (TLS —
  [configuration/tls](https://getnora.dev/configuration/tls/), прод —
  [deployment/production](https://getnora.dev/deployment/production/))

Задача: поставить NORA на ноутбук, выписать для неё **локальный сертификат**
и **автоматически** добавить этот серт в доверенные у всех клиентов
(docker, containerd, npm, pip, cargo, go, apt, maven).

Проект, который делает выписку + автоматическое добавление серта в trust store —
**mkcert**: [FiloSottile/mkcert](https://github.com/FiloSottile/mkcert).
`mkcert -install` сам создаёт локальный CA и ставит его в системный корневой
store, дальше `mkcert <домены>` выписывает уже доверенные серты. Ноль
конфигурации, ноль ручного openssl.

Как читать:

- `- [ ]` — задача ещё не сделана.
- Команды — как есть для Ubuntu 26.04 / x86_64.
- В скобках — способ установки: `deb` (готовый пакет в GitHub-релизе),
  `apt` (уже есть в репозитории Ubuntu), `release` (готовый бинарник в релизе,
  роль `binaries` кладёт его в `{{ bin_dir }}`), `cargo` (из исходников).

---

## 1. Установка NORA

- [ ] **nora** — бинарник артефакт-реестра. *(`deb`)* —
      [getnora-io/nora](https://github.com/getnora-io/nora)

  ```
  # .deb из релиза (v1.3.6): nora-amd64.deb — с systemd-юнитом
  curl -fsSLO https://github.com/getnora-io/nora/releases/latest/download/nora-amd64.deb
  sudo apt install ./nora-amd64.deb
  ```

  Конфиг — `/etc/nora/nora.env`, данные — `/var/lib/nora`.
  Альтернативы: `docker run -d -p 4000:4000 -v nora-data:/data getnora/nora:latest`,
  готовый бинарник `nora-linux-amd64` из релиза, `cargo install nora-registry`.

- [ ] Выставить адрес и публичный URL: `NORA_HOST=0.0.0.0`,
      `NORA_PUBLIC_URL=https://registry.nora.lan` (с TLS из раздела 3),
      иначе ссылки на скачивание в UI будут на `localhost`.

- [ ] Включить авторизацию: `NORA_AUTH_ENABLED=true`,
      htpasswd-файл / API-токены (см.
      [configuration/authentication](https://getnora.dev/configuration/authentication/)).

- [ ] Smoke-проверка: `curl http://localhost:4000/health` → `{"status":"healthy"}`,
      UI — `http://localhost:4000/ui/`.

- [ ] Про запас: `nora backup` / `nora restore`, `nora mirror` (офлайн-синк),
      `nora import` (миграция из Artifactory/Nexus).

## 2. Локальный сертификат — mkcert

Проект: [FiloSottile/mkcert](https://github.com/FiloSottile/mkcert) (v1.4.4).
Почему он: выписывает локальные серты **и сам** ставит свой CA в системный
trust store — как раз «сертификат + автоматическое добавление» одним инструментом.

- [ ] Установить mkcert. *(`apt` — `mkcert 1.4.4-1ubuntu2` в universe; или
      `release` — `mkcert-v1.4.4-linux-amd64` через роль `binaries`)*

      ```
      sudo apt install mkcert libnss3-tools   # libnss3-tools — для Firefox/Chrome
      ```

- [ ] `mkcert -install` — CA создаётся и **автоматически** добавляется в
      системный trust store (`/etc/ssl/certs` для curl/git/apt/go)
      и в NSS-базы браузеров. CA-материал — в `mkcert -CAROOT`
      (`~/.local/share/mkcert`), ключ не раздаём и не коммитим.

- [ ] Выписать серт под домен реестра (`.local` не брать — конфликтует с mDNS,
      лучше `registry.nora.lan` / `nora.home`):

      ```
      mkcert -cert-file /etc/nora/tls/nora.crt -key-file /etc/nora/tls/nora.key \
        registry.nora.lan localhost 127.0.0.1 ::1
      ```

      Опционально wildcard: `mkcert '*.nora.lan'`.

- [ ] Права и хранение: ключ `/etc/nora/tls/nora.key` — `600 root:root`,
      серт — `644`. В репозитории плейбука серты/ключи не держим.

- [ ] Авторотация: mkcert сам серты не продлевает. Либо cron/systemd-timer с
      `mkcert` + перечитывание конфига (Caddy/SIGHUP), либо ставим
      `step-ca` (раздел 6) с автообновлением.

## 3. TLS-терминатор (у NORA нет встроенного TLS — by design)

- [ ] **caddy** — reverse proxy с TLS, рекомендован NORA. *(`apt`)* —
      [caddyserver/caddy](https://github.com/caddyserver/caddy)

      ```
      # /etc/caddy/Caddyfile
      registry.nora.lan:443 {
          tls /etc/nora/tls/nora.crt /etc/nora/tls/nora.key
          reverse_proxy localhost:4000 {
              header_up Host {host}
              header_up X-Real-IP {remote}
          }
      }
      ```

- [ ] Альтернатива — nginx (тем же сертом от mkcert), из коробки потребует
      `client_max_body_size 0` для больших образов.

- [ ] Проверить связку: `curl https://registry.nora.lan/health` без `-k` —
      серт должен считаться доверенным.

## 4. Автоматически добавляем серт клиентам

`mkcert -install` покрывает систему и браузеры; контейнерные рантаймы и
языковые клиенты доверяют своим store — их надо прописать отдельно
(доки NORA: [configuration/tls → Custom CA](https://getnora.dev/configuration/tls/)).

- [ ] Системный store Ubuntu/Debian (curl, git, apt, go, rustls):

      ```
      sudo cp "$(mkcert -CAROOT)/rootCA.pem" /usr/local/share/ca-certificates/nora-ca.crt
      sudo update-ca-certificates
      ```

- [ ] Docker daemon:

      ```
      sudo mkdir -p /etc/docker/certs.d/registry.nora.lan/
      sudo cp "$(mkcert -CAROOT)/rootCA.pem" /etc/docker/certs.d/registry.nora.lan/ca.crt
      ```

      (перезапуск Docker не нужен; для нестандартного порта каталог —
      `registry.nora.lan:5443/`). Проверка: `docker pull registry.nora.lan/myapp:1`.

- [ ] containerd (Kubernetes-ноды): тот же CA в системный store +
      `sudo systemctl restart containerd`, либо секция
      `[plugins...registry.configs.<host>.tls]` в `/etc/containerd/config.toml`.

- [ ] npm: `npm config set cafile "$(mkcert -CAROOT)/rootCA.pem"`
      (и `npm config set registry https://registry.nora.lan/npm/`,
      `//registry.nora.lan/npm/:_authToken=<token>`).

- [ ] pip/pipx: `pip install --cert "$(mkcert -CAROOT)/rootCA.pem" ...`
      или `PIP_CERT=...`; для «одного раз и навсегда» — пакет
      [sethmlarson/truststore](https://github.com/sethmlarson/truststore)
      (pip ходит через системный store). Также `PIP_INDEX_URL=https://registry.nora.lan/pypi/simple/`.

- [ ] go: `GOPROXY=https://registry.nora.lan/go`, `GONOSUMCHECK`/`GONOSUMDB`
      по необходимости; на Linux go читает системный store, иначе `SSL_CERT_FILE`.

- [ ] cargo: `CARGO_HTTP_CAINFO="$(mkcert -CAROOT)/rootCA.pem"` (если системного
      store недостаточно), `source.crates-io.replace-with` на
      `sparse+https://registry.nora.lan/cargo/index/`.

- [ ] Maven/Java — свой keystore: `keytool -importcert -cacerts -file rootCA.pem -alias nora`.

- [ ] Terraform/OpenTofu: системный store + `~/.terraformrc`
      (`provider_installation { network_mirror { ... } }`, см.
      [registries/terraform](https://getnora.dev/registries/terraform/)).

- [ ] apt/DEB-репозиторий NORA: `deb [signed-by=/etc/apt/keyrings/nora.gpg] ...`
      (NORA умеет подписывать индексы, см.
      [registries/deb](https://getnora.dev/registries/deb/)).

## 5. Проверка после настройки

- [ ] `curl -fsS https://registry.nora.lan/health` — без ошибок TLS.
- [ ] `docker login / pull / push registry.nora.lan/...`.
- [ ] `npm publish` в `https://registry.nora.lan/npm/`.
- [ ] `pip install -i https://registry.nora.lan/pypi/simple/ <пакет>`.
- [ ] `GOPROXY=https://registry.nora.lan/go go mod download`.
- [ ] `openssl s_client -connect registry.nora.lan:443 -showcerts` —
      цепочка ведётся на rootCA из `mkcert -CAROOT`.

## 6. Что сделать в этом плейбуке

- [ ] Новая роль `registry` (или `nora`): установка `nora-amd64.deb`
      (или бинарник через роль `binaries`), `nora.env` из шаблона,
      systemd-юнит (пакет его уже кладёт), `NORA_AUTH_ENABLED`.

- [ ] mkcert в роль: `apt: mkcert libnss3-tools` (предпочтительно, есть в
      universe Ubuntu 26.04) или бинарник `FiloSottile/mkcert` через
      `binaries` (поле `url` на `mkcert-{version}-linux-amd64`).

- [ ] Задача выписки серта (идемпотентность через `creates`/статус серта,
      SAN — `registry.nora.lan`, `localhost`, `127.0.0.1`).

- [ ] Задача установки CA в доверенные: системный store
      (`update-ca-certificates`), `/etc/docker/certs.d/<host>/ca.crt`,
      npm `cafile`, pip/truststore; хендлеры на перечитывание
      Docker/containerd.

- [ ] Caddy как TLS-терминатор: шаблон `Caddyfile` с путями на серт из mkcert.

- [ ] `group_vars`: `nora_hostname`, `nora_public_url`, `nora_tls_dir`,
      флаг `nora_tls_enabled`. Проверка `ansible-lint` (`.ansible-lint` в корне).

## 7. Альтернативы mkcert (если понадобится настоящий PKI)

- [ ] **step-ca** — свой локальный CA с ACME и **автообновлением** сертификатов.
      *(`deb` / `release`)* — [smallstep/certificates](https://github.com/smallstep/certificates)
- [ ] **certstrap** — генерация CA/сертов из Go, простая утилита. *(`go`)* —
      [square/certstrap](https://github.com/square/certstrap)
- [ ] **minica** — минимальный CA + листовые серты. *(`go`)* —
      [jsha/minica](https://github.com/jsha/minica)
- [ ] **cfssl** — полноценный PKI-тулкит от Cloudflare. *(`go`)* —
      [cloudflare/cfssl](https://github.com/cloudflare/cfssl)
- [ ] Let's Encrypt + Caddy/Traefik — если есть публичный домен (NORA так и
      советует для прода; тогда локальный CA не нужен).

---

Связанное: [`TODO-awesome-github.md`](TODO-awesome-github.md) — там же
кандидат **mkcert** (раздел «Сеть, DNS, TLS»), **showcert** для разбора
выданных сертов и **ansible-collection-smallstep** для `step-ca`.
