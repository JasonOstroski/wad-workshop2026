# Workshop Shop

A small, editable ecommerce application for workshops. It has a browser frontend, shop API, PostgreSQL database, PostgREST data gateway, and separate mock payment service. The application source is mounted into prebuilt Node containers, so there are no application image builds.

## Run locally

Run the complete local stack from this directory:

```bash
docker compose up
```

### GitHub Codespaces

This repository runs in Codespaces without building application images. Open
the repository in a Codespace, then run:

```bash
make up
```

When the command finishes, open the forwarded port `8088` from the Ports panel.
The shop source is bind-mounted into the Node containers, so edits to the
frontend are visible after a browser refresh. Restart the stack after editing
`server.js` or `payment.js`:

```bash
make down && make up
```

Start generated traffic in another terminal with:

```bash
make load
```

The load run lasts 60 seconds. It uses only public prebuilt images; the first
startup downloads images but does not compile the workshop application.

Open http://localhost:8088. The stack has four services: shop, payment, PostgreSQL, and PostgREST.

Run the generated workload in a second terminal:

```bash
docker compose --profile loadgen up load-generator
```

The load generator uses the prebuilt `grafana/k6` image and runs catalog,
multi-item cart, and checkout traffic for 60 seconds. With Podman Compose, use
the equivalent `--profile loadgen` command and the Podman image path configured
by your local setup.

Edit `frontend/index.html`, `frontend/app.js`, or `frontend/styles.css`, then refresh the browser. Edit `server.js` or `payment.js`, then restart only the relevant Node container.

PostgreSQL is initialized from `db/init.sql`. Reset it with `docker compose down -v`. The payment service approves every non-zero charge and is intentionally for teaching only.
