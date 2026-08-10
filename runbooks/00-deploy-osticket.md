# 00 — Deploy osTicket in Docker

**Goal:** osTicket running against a MariaDB backend, through the web installer, with the admin/agent panels reachable.

## Prereqs

- Docker + Compose on the host (see [docker-selfhosted-lab](https://github.com/wardoep/docker-selfhosted-lab) runbook 00).
- Ports free: `8080` for the web UI.

## Why we build our own image

The community osTicket images on Docker Hub (`osticket/osticket`, `campbellsoftwaresolutions/osticket`) were last built in **2020 on PHP 7.2/7.3 — both long past end-of-life**. The "official" one is worse: it *git-clones osTicket's `master` at container start*, so a 2020 PHP runtime ends up trying to run 2026 osTicket code and dies with a PHP parse error, crash-looping forever.

Running EOL PHP inside a helpdesk is exactly the anti-pattern this portfolio exists to show I *wouldn't* ship. So [`image/Dockerfile`](../image/Dockerfile) builds a small image on the supported **`php:8.2-apache`** base (pinned to the Debian 12 variant, which still ships the IMAP library), installs the extensions osTicket needs, and lays down a **pinned osTicket 1.18.4** release. Compose builds it automatically.

## Steps

1. **Configure secrets:**
   ```bash
   cd compose
   cp .env.example .env
   # edit .env — set strong MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD
   ```

2. **Build the image and bring up the stack:**
   ```bash
   docker compose up -d --build      # first run builds the osTicket 8.2 image (~1 min)
   docker compose ps
   ```
   Expected: `osticket-db` healthy, `osticket-web` running *and* `healthy`, port `8080` published. The web container waits for the DB healthcheck before starting (`depends_on: condition: service_healthy`), and has its own healthcheck (PHP + `mysqli` loaded, app files present).

3. **Run the web installer.** Browse to `http://<host-ip>:8080`. The osTicket installer asks for:
   - **Helpdesk name** and the **default system email** (the address tickets appear to come from).
   - **Admin user** — name, a strong password (this is *your* staff login, distinct from the DB creds).
   - **Database settings** — host `db`, name/user/password exactly as in `.env` (the container name `db` is the DB hostname on the shared network).

4. **Lock down the installer.** osTicket warns that the config file is writable and the setup dir still exists. After install:
   ```bash
   docker compose exec osticket sh -c 'chmod 0644 /var/www/html/include/ost-config.php; rm -rf /var/www/html/setup'
   ```

5. **Find the two panels:**
   - **Staff/agent panel:** `http://<host-ip>:8080/scp`
   - **User portal:** `http://<host-ip>:8080/`

## Verify

```bash
docker compose ps                                   # both services up, db healthy
curl -sI http://localhost:8080/scp | head -n1       # 200/302 — staff panel responds
```
Log into `/scp` with the admin account and confirm the dashboard loads.

## If it breaks

- **Web container restarts / "can't connect to DB".** The DB creds in the installer didn't match `.env`, or you used `localhost` instead of `db` as the DB host. Use `db`.
- **Installer says config not writable.** First-run needs `ost-config.php` writable; lock it down *after* install (step 4), not before.
- **Port 8080 in use.** Change the left side of the `8080:80` mapping in the compose file.
