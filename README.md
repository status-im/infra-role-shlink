# Description

This role configures [Shlink](https://shlink.io/) - an Open Source URL shortener.

# Configuration

A basic setup should look like:
```yaml
shlink_domain: 'link.example.org'
shlink_app_cont_port: 8080
shlink_db_name: 'shlink'
shlink_db_user: 'shlink'
shlink_db_pass: 'changeIfYouCare'
```
The admin interface is available under a separate domain via OAuth:
```
shlink_admin_domain: 'admin-link.example.org'
shlink_oauth_google_domain: 'example.org'
shlink_oauth_cookie_secret: '123qwe'
shlink_oauth_client_id: 'my-oauth-app-id'
shlink_oauth_client_secret: 'super-secret-password'
```

# Details

The service is deployed using the [shlinkio/shlink](https://hub.docker.com/r/shlinkio/shlink) Docker image using a Docker Compose config with a PostgreSQL database.

It uses the [shlinkio/shlink-web-client](https://hub.docker.com/r/shlinkio/shlink-web-client) Docker image to provide a management Web UI.
