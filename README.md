# demo-postgres

Helm chart (`k8s/`) for a single Postgres (`postgres:18-alpine`) used by the doublure demo.

It runs as the `postgres` catalog service in every preview, so each preview namespace gets its own database.

## Connecting

The chart creates a Secret named after the release, `postgres` in previews, with:

- `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`
- `DATABASE_URL`: `postgresql://app:preview-demo-only@postgres:5432/app?sslmode=disable`

Services load it with `envFrom: [{secretRef: {name: postgres}}]`.

## Notes

- **Ephemeral:** data lives on an `emptyDir`. It is reset when the pod restarts and removed with the preview namespace.
- **Demo password:** the password is a fixed value in `values.yaml`, because Argo CD keeps randomly generated values OutOfSync. It is reachable only inside the preview namespace.
- This is a demo chart, not a production database: no persistence, backups or TLS.
