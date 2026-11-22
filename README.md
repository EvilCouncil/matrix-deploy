# EvilCouncil Matrix Stack (`matrix-deploy`)

Secret-free Portainer GitOps Docker Compose stack deploying **Tuwunel** (`ghcr.io/matrix-construct/tuwunel`) on `control.evilcouncil.org` (`10.1.0.10`).

## Architecture

- **Homeserver:** Tuwunel (Rust + embedded RocksDB, native Simplified Sliding Sync for Element X)
- **Matrix `server_name`:** `matrix.evilcouncil.org` (immutable after first boot)
- **LAN Endpoint:** `https://matrix.evilcouncil.org` (via Traefik on `control`, external `proxy` network, Cloudflare DNS-01 TLS)
- **DMZ / Tailscale Endpoint:** `https://matrix-dmz.evilcouncil.org` (via Traefik on `jumpgate.evilcouncil.org` forwarding to `http://10.1.0.10:8008`)
- **Storage:** Live RocksDB database at `/var/lib/matrix/data` on `control`'s local disk (`/dev/sda1`). Do **not** mount live RocksDB on `/srv` (autofs `soft`-mounted NFSv4).

## Deployment

Managed via `EvilCouncil/sitebook` Ansible playbook (`homelab/ansible/playbooks/deploy_matrix.yml`) and Portainer GitOps (Endpoint ID `3` on `control.evilcouncil.org`).

Runtime secrets (such as `TUWUNEL_REGISTRATION_TOKEN` during initial account bootstrap) are injected exclusively via Portainer stack environment variables sourced from Infisical (`secrets.evilcouncil.org`). Never commit secrets to this repository.
