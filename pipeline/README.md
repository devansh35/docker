## Run with Docker Compose

This Compose file uses host networking because containers on the bridge
network could resolve `pgdatabase` but timed out when connecting to PostgreSQL.
Host networking is supported by Docker on Linux, including Codespaces. Make
sure ports `5432` and `8085` are available.

Run these commands from the `pipeline` directory. If standalone containers
named `pgadmin` or `pgdatabase` from the earlier commands still exist, stop and
remove them first; this does not remove their named volumes:

```bash
docker stop pgadmin pgdatabase
docker rm pgadmin pgdatabase
```

Then recreate the Compose services with host networking:

```bash
docker compose down
docker compose up -d
```

Forward port `8085` in the VS Code **Ports** tab and open the forwarded URL to
sign in to pgAdmin. Register the PostgreSQL server with host `pgdatabase`, port
`5432`, database `taxi`, and the configured credentials (defaults: `root` /
`root`). With host networking, use `127.0.0.1` as the database host.

The Compose file currently starts only PostgreSQL and pgAdmin; it does not run
the ingestion job. To stop the services without deleting their data:

```bash
docker compose down
```

The named volumes preserve PostgreSQL and pgAdmin data. Do not add `--volumes`
to `down` if you want to keep that data. PostgreSQL and pgAdmin credentials can
be overridden with `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`,
`PGADMIN_DEFAULT_EMAIL`, and `PGADMIN_DEFAULT_PASSWORD`.