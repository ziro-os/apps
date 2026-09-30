# Contributing an app

Open a pull request that adds `apps/<name>/app.json`. CI builds the catalog, and the build refuses any definition
that breaks the schema or the rules below. A maintainer then reviews it.

## Rules

1. **Official or verified images, pinned by digest** (`docker.io/library/postgres:18.6@sha256:...`). Bump the digest
   for patch releases. Add a new version key for a new major line.
2. **Generated secrets only.** Declare them in `secrets` (`hex:N`, `base64:N`, `alnum:N`) and pass them to
   containers through `components[].secrets`, which become env files. Never put them in `env`, `args` or a command
   line. Wrapping the command in a shell is fine if you feed secrets over stdin or a file.
3. **Not root.** Use the image's own privilege drop. If you wrap the command in `sh -c`, drop privileges yourself
   (`setpriv`, `gosu`).
4. **Persistent data** goes in `data` paths. On a cluster they are node-local, so apps with several replicas must
   replicate themselves.
5. **Replicas.** Several replicas need `"cluster": true`. Peers find each other through `ZIRO_PEERS` and
   `ZIRO_REPLICA` (`<index>.<app>.cluster.ziro`).
6. **Outputs** tell users how to connect (`url`, `user`, `password`, `host`, `port`).
7. **Test it** on a host (`ziroctl apps deploy`, redeploy, `rm`, then deploy again with the same data) and, for
   cluster apps, on a cluster: kill a replica and check that it recovers.
