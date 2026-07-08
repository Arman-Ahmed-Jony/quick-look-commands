# Logrotate — Daily Story

## The problem

**Arman's** chat API has run on a single VPS for six months. Nginx access logs, app error logs, and journal exports all write to disk around the clock. Nobody set up rotation.

One Tuesday at 3 AM:

- Disk hits **100%**
- Postgres can't write its WAL files → database errors
- Nginx can't write `access.log` → every request returns 500
- Deploys fail with "no space left on device"

The 3 AM fix is manual: `rm` giant log files, hope nothing important was deleted, restart services. Two hours of downtime that a five-line config would have prevented.

```mermaid
flowchart LR
    APP[App + nginx logging 24/7] --> LOG[access.log grows forever]
    LOG --> DISK[Disk 100%]
    DISK --> DOWN[Services crash]
    style DISK fill:#f99
    style DOWN fill:#f99
```

---

## How logrotate fixes it

**Logrotate** renames, compresses, and deletes old logs on a schedule — triggered by **cron**, usually daily. The app keeps writing to a fresh file while old versions age out as `.1`, `.2.gz`, and so on.

```
BEFORE:  /var/log/nginx/access.log       (50 GB)

AFTER:   /var/log/nginx/access.log        (0 bytes, new)
         /var/log/nginx/access.log.1      (yesterday)
         /var/log/nginx/access.log.2.gz   (compressed, older)
         /var/log/nginx/access.log.3.gz   (oldest kept)
```

| Problem | Without logrotate | With logrotate |
|---------|-------------------|----------------|
| Disk full | One file eats everything | Old logs compressed / deleted |
| Slow debugging | `grep` on 50 GB | Search smaller dated files |
| App stops logging | Some apps halt on full disk | Fresh file after rotation |
| No retention policy | Logs forever or nothing | Keep exactly N days |

---

## Industry norms & conventions

### 1. cron-triggered, not a daemon

Logrotate does **not** run continuously. The standard setup is a daily cron job:

```mermaid
flowchart TD
    CRON[/etc/cron.daily/logrotate] --> CONF[Read /etc/logrotate.conf]
    CONF --> D[Read /etc/logrotate.d/*]
    D --> DUE{Log due for rotation?}
    DUE -->|yes| ROT[Rotate + compress + prune]
    DUE -->|no| SKIP[Skip]
```

### 2. One config file per app in `/etc/logrotate.d/`

The convention: the main policy lives in `/etc/logrotate.conf`, and each app **drops its own file** into `/etc/logrotate.d/`. Packages (nginx, mysql) do this automatically; you do the same for your app.

```
/etc/logrotate.conf          # global defaults
/etc/logrotate.d/nginx       # shipped by the nginx package
/etc/logrotate.d/chat-api    # you add this for your app
```

### 3. A standard app stanza

```
/var/log/chat-api/*.log {
    daily                # rotate once a day
    rotate 14            # keep 14 generations
    compress             # gzip old logs
    delaycompress        # keep the most recent rotation uncompressed
    missingok            # don't error if the log is absent
    notifempty           # skip rotation if empty
    copytruncate         # copy then truncate (app needs no reload)
}
```

| Directive | Convention |
|-----------|-----------|
| `daily` / `weekly` / `monthly` | Time-based trigger |
| `size 100M` | Size-based trigger (combine with time for busy logs) |
| `rotate N` | Retention count — set by policy/compliance |
| `compress` | Standard for anything kept more than a day |
| `copytruncate` | For apps that hold the file open and can't reopen |
| `postrotate ... endscript` | Signal the app to reopen its log (nginx/Apache) |

### 4. copytruncate vs postrotate

Two standard ways to handle an app that has the log file open:

| Approach | How | Use when |
|----------|-----|----------|
| **postrotate** | Rename file, then signal app to reopen (`nginx -s reopen`) | App supports log reopening (nginx, Apache, rsyslog) |
| **copytruncate** | Copy the file, then truncate the original in place | App can't reopen; small risk of losing lines mid-copy |

### 5. Retention driven by policy, not guesswork

`rotate N` should reflect a real **retention policy** — often compliance-driven (e.g. keep 30 or 90 days). Balance:

- **Longer retention** → better audits/debugging, more disk.
- **Compression** (`compress`) → keep more history in less space.

### 6. Test before trusting

The norm is to dry-run config changes before they run unattended at 3 AM:

```bash
sudo logrotate -d /etc/logrotate.conf          # debug/dry-run — shows what would happen
sudo logrotate -f /etc/logrotate.d/chat-api    # force a real rotation once, to verify
cat /var/lib/logrotate/status                  # rotation history/state
```

> **Container note:** for Docker hosts, the equivalent norm is limiting the `json-file` log driver (`max-size`, `max-file`) — the [Docker story](docker.md) covers logging to stdout so the platform handles it.

---

**Next:** [Linux — where everything lives](linux.md) · **Deep dive:** [logrotate/README.md](../logrotate/README.md)
