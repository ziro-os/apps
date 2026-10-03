# Ziro OS apps

The official app catalog for [Ziro OS](https://github.com/ziro-os/ziro-os). With it, `ziroctl apps deploy <app>`
gives you a running, hardened app with generated credentials and persistent data, in one command, on a single
host or across a cluster. CI checks every definition, signs the index with the key whose public half is compiled
into `ziroctl` (`catalog.pub`), and publishes it to <https://ziro-os.github.io/apps>.

```sh
ziroctl apps search
ziroctl apps deploy postgres            # PostgreSQL 18 on 127.0.0.1:5432
ziroctl apps credentials postgres       # URL, user, password
ziroctl apps deploy mysql-cluster       # on a cluster master: 3-member Group Replication
```

| App | Versions | What you get |
|---|---|---|
| [`postgres`](apps/postgres/app.json) | 16, 17, 18 | PostgreSQL with SCRAM authentication and data checksums, plus a database and owner. |
| [`mysql`](apps/mysql/app.json) | 8.4 | MySQL LTS with a database and user; the root and app passwords are generated. |
| [`mysql-cluster`](apps/mysql-cluster/app.json) | 8.4 | InnoDB Group Replication: 3 members, single primary, automatic failover, TLS between members. |
| [`valkey`](apps/valkey/app.json) | 8, 9 | Valkey (Redis-compatible, BSD): password auth, append-only persistence, runs as `valkey`. |
| [`mariadb`](apps/mariadb/app.json) | 11.8, 11.4 | MariaDB LTS with a database and user; the root and app passwords are generated. |
| [`mongodb`](apps/mongodb/app.json) | 8.0, 7.0 | MongoDB with a generated root password; the URL authenticates against `admin`. |
| [`wordpress`](apps/wordpress/app.json) | 7 | WordPress (Apache, PHP 8.4) with persistent `wp-content`. Use a stack for the database. |
| [`ghost`](apps/ghost/app.json) | 6 | Ghost publishing with persistent content, on MySQL 8. |
| [`n8n`](apps/n8n/app.json) | 2 | n8n workflow automation with a generated encryption key; SQLite alone, PostgreSQL in its stack. |
| [`umami`](apps/umami/app.json) | 3 | Umami privacy-friendly web analytics on PostgreSQL. |
| [`openclaw`](apps/openclaw/app.json) | 2026.9 | OpenClaw AI assistant gateway with a generated gateway token; your model key is an input secret. |

## Stacks

A stack deploys several apps together, wired by links (the database password never leaves the host's secret files).

| Stack | Apps | Published on |
|---|---|---|
| [`wordpress`](stacks/wordpress/stack.json) | wordpress + mysql 8.4 | 127.0.0.1:8080 |
| [`wordpress-mariadb`](stacks/wordpress-mariadb/stack.json) | wordpress + mariadb 11.8 | 127.0.0.1:8080 |
| [`ghost`](stacks/ghost/stack.json) | ghost + mysql 8.4 | 127.0.0.1:2368 |
| [`n8n`](stacks/n8n/stack.json) | n8n + postgres 18 | 127.0.0.1:5678 |
| [`umami`](stacks/umami/stack.json) | umami + postgres 18 | 127.0.0.1:3000 |

```sh
ziroctl stack search
ziroctl stack up wordpress                          # deploy as published
ziroctl stack init wordpress -o blog                # or edit it first (hostname, versions, resources)
ziroctl stack up -f blog/stack.yaml
ziroctl gateway expose wordpress-web --host blog.example.com
ziroctl apps deploy openclaw --secret ANTHROPIC_API_KEY=@anthropic.key
```

A stack's database gets no host port; only the stack's apps reach it. Coolify is not offered: it needs the host's
Docker socket (root on the host). `ziroctl deploy` covers deploying your own code from git.

Every image is pinned by digest. Patch updates reach you as catalog updates (`ziroctl plugin update`, then
redeploy).

Usage, the schema and the security rules are in the
[apps guide](https://github.com/ziro-os/ziro-os/blob/main/docs/apps.md). To add an app, see
[CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Each app runs software under its own license.
