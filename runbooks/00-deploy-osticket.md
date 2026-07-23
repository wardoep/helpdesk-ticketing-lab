# 00 — Deploy osTicket in Docker

**Goal:** osTicket running against a MariaDB backend, through the web installer, with the admin/agent panels reachable.

## Prereqs

- Docker + Compose on the host (see [docker-selfhosted-lab](https://github.com/wardoep/docker-selfhosted-lab) runbook 00).
- Ports free: `8080` for the web UI.

## Steps

1. **Configure secrets:**
   ```bash
   cd compose
   cp .env.example .env
   # edit .env — set strong MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD
   ```

2. **Bring up the stack:**
   ```bash
   docker compose up -d
   docker compose ps
   ```
   Expected: `osticket-db` healthy, `osticket-web` running, port `8080` published. The web container waits for the DB healthcheck before starting (`depends_on: condition: service_healthy`).

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

## What I learned

*Filled in after I complete this milestone.*

<!-- Prompts to answer once done:
     - Why is the DB hostname "db" and not localhost inside the compose network?
     - What's the difference between the DB user creds and the osTicket admin login?
     - What did leaving the setup dir/writable config actually expose? -->
