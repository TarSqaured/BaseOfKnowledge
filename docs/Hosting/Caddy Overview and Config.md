## What Is Caddy?

Caddy is a modern web server and reverse proxy that:

- 🔒 Automatically obtains and renews TLS certificates via Let’s Encrypt
    
- ⚙️ Serves HTTP (port 80) and HTTPS (port 443)
    
- 🔄 Routes traffic to local applications or static files based on domain
    
- 📝 Uses a single **Caddyfile** for all configuration
    

---

## Installing Caddy on Debian/Ubuntu

```
sudo apt update
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https \
  curl gnupg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' \
  | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' \
  | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install -y caddy
```

- Installs Caddy server and systemd service
    
- Enables automatic updates from the Caddy repository
    

---

## Basic Caddyfile Structure

Location: `/etc/caddy/Caddyfile`

```
example.tyshub.xyz {
  respond "🚀 It works!" 200
}
```

- **Header**: domain name
    
- **Body**: site-specific directives (e.g., `respond`, `reverse_proxy`, `file_server`)
    

---

## Common Directives

|Directive|Purpose|
|---|---|
|`respond`|Return static text and HTTP status|
|`reverse_proxy`|Forward requests to a backend server|
|`file_server`|Serve static files from a directory|
|`encode`|Enable compression (gzip, zstd)|
|`tls`|Specify custom certificate paths|
|`log`|Configure access/error logging|

---

## Example: Reverse Proxy to Local App

```
finance.tyshub.xyz {
  encode gzip zstd
  reverse_proxy localhost:3000
}
```

- Compresses responses
    
- Forwards traffic to an app on port 3000
    

---

## Managing the Caddy Service

```
# Start and enable at boot
sudo systemctl enable --now caddy

# Reload config after editing Caddyfile
sudo systemctl reload caddy

# Restart (for core changes)
sudo systemctl restart caddy

# Check status
sudo systemctl status caddy --no-pager
```

---

## Validating & Formatting the Config

```
# Check syntax
sudo caddy validate --config /etc/caddy/Caddyfile

# Auto-format for readability
sudo caddy fmt --overwrite /etc/caddy/Caddyfile
```

---

## Logs and Debugging

```
# Tail live logs (Ctrl+C to exit)
journalctl -u caddy -f

# Show recent errors
journalctl -u caddy --no-pager -n 50
```

---

## Best Practices

- Keep the Caddyfile **DRY**: reuse global settings where possible
    
- Use **wildcard certificates** for subdomains if supported
    
- Backup your Caddyfile after major changes
    
- Limit HTTP (port 80) to ACME challenges and redirects only
    
- Monitor logs and set up alerts for certificate renewals
    

---

## Further Reading

- Official documentation: https://caddyserver.com/docs/
    
- ACME challenge details: https://letsencrypt.org/docs/
    
- Caddy GitHub: https://github.com/caddyserver/caddy