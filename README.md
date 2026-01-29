# Description

This role configures [Shlink](https://shlink.io/) - an Open Source URL shortener.

# Configuration

A basic setup should look like:
```yaml
shlink_domain: 'link.example.org'
shlink_admin_domain: 'admin-link.example.org'
shlink_fallback_url: 'https://example.org/'
shlink_db_pass: 'super-secret-password'
```

Optional GeoLite2 for visit geolocation:
```yaml
shlink_geolite_license_key: 'your-license-key'
```

The admin interface is available under a separate domain, protected by the `infra-role-oauth-proxy` role.

# Multi-Domain Setup

Shlink supports multiple domains from a single instance. Domains are automatically detected from the HTTP Host header when creating short URLs.

To add a new domain (e.g. `link.example.com`):

1. **Terraform** - Add Cloudflare DNS record pointing to the server
2. **Nginx** - Add site config for the new domain with SSL certificate
3. **Vault** - Ensure origin certificate exists for the domain

No Shlink configuration needed - the domain is automatically registered when the first short URL is created for it.

# Ports

| Port | Service | Description |
|------|---------|-------------|
| `{{ shlink_app_cont_port }}` (8080) | Shlink API | Main shortener service |
| `{{ shlink_web_cont_port }}` (8090) | Web Client | Admin UI (behind OAuth) |
| `{{ shlink_db_cont_port }}` (5432) | PostgreSQL | Database |

# Details

The service is deployed using:
- [shlinkio/shlink](https://hub.docker.com/r/shlinkio/shlink) - URL shortener
- [shlinkio/shlink-web-client](https://hub.docker.com/r/shlinkio/shlink-web-client) - Admin Web UI
- [postgres](https://hub.docker.com/_/postgres) (16-alpine) - Database

# Related Roles

- `infra-role-oauth-proxy` - Protects admin UI with Keycloak authentication
- `infra-role-nginx` - Reverse proxy / SSL termination
- `infra-role-restic-backups` - Database backups
- `infra-role-systemd-timer` - Scheduled DB dumps
- `infra-role-consul-service` - Service discovery