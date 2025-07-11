A quick reference to the essential commands for managing your Homelab hosting environment.

---

## Service Control

```bash
sudo systemctl status caddy --no-pager
```

Show the current status of the Caddy service without paging.

```bash
sudo systemctl enable --now caddy
```

Enable Caddy at boot and start it immediately.

```bash
sudo systemctl restart caddy
```

Restart Caddy fully (use after deep configuration or binary updates).

```bash
sudo systemctl reload caddy
```

Reload Caddy’s configuration without downtime (use after editing Caddyfile).

```bash
sudo systemctl stop caddy
```

Stop the Caddy service.

---

## Caddy Config Management

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
```

Check Caddyfile syntax and configuration validity.

```bash
sudo caddy fmt --overwrite /etc/caddy/Caddyfile
```

Auto-format and clean up the Caddyfile for consistent style.

---

## Port & Process Inspection

```bash
ss -tulpn | grep -E ':80|:443'
```

List processes listening on ports 80 and 443 to verify Caddy binding.

```bash
sudo lsof -i :80 -sTCP:LISTEN
```

Identify which process is listening on port 80 (e.g., Python test server).

```bash
sudo ss -tulpn | grep caddy
```

Confirm Caddy’s sockets and listening addresses.

---

## DNS & Network Checks

```bash
curl -4 https://icanhazip.com
```

Show your current public IPv4 (used for DNS A records).

```bash
dig +short finance.tyshub.xyz @1.1.1.1
```

Verify that the A record for a domain resolves correctly via Cloudflare’s DNS.

```bash
dig +short ai.tyshub.xyz @ns1.porkbun.com
```

Query Porkbun’s authoritative nameserver to confirm DNS record at the source.

---

## HTTP/HTTPS Testing

```bash
curl -IL http://finance.tyshub.xyz
```

Perform a HEAD request over HTTP to check redirects or status.

```bash
curl -IL https://finance.tyshub.xyz
```

Perform a HEAD request over HTTPS to inspect TLS handshake and status code.

```bash
curl -k https://ai.tyshub.xyz
```

Bypass certificate verification for quick local testing of HTTPS.

---

## Logs & Monitoring

```bash
journalctl -u caddy -f
```

Tail live logs for the Caddy service (press Ctrl+C to exit).

```bash
journalctl -u caddy --no-pager -n 50
```

Show the last 50 log entries for Caddy without paging.

---

## Firewall Management

```bash
sudo ufw allow 80/tcp
```

Allow HTTP through Ubuntu’s UFW firewall.

```bash
sudo ufw allow 443/tcp
```

Allow HTTPS through Ubuntu’s UFW firewall.

```bash
sudo ufw status verbose
```

View detailed UFW status and rules.