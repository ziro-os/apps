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

Every image is pinned by digest. Patch updates reach you as catalog updates (`ziroctl plugin update`, then
redeploy).

Usage, the schema and the security rules are in the
[apps guide](https://github.com/ziro-os/ziro-os/blob/main/docs/apps.md). To add an app, see
[CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Each app runs software under its own license.
