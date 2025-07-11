## Common Issues & Fixes

### 1. DNS Resolution Errors

- **Error:** `ERR_NAME_NOT_RESOLVED`**Cause:** DNS record typo or propagation delay.**Fix:** `bash<br> dig +short finance.tyshub.xyz @1.1.1.1<br>` Ensure it returns your public IP. If not, correct the record and wait TTL.
    

---

### 2. Connection Refused / Timeout

- **Error:** `ERR_CONNECTION_REFUSED` or `ERR_CONNECTION_TIMED_OUT`**Cause:** Port forwarding missing, firewall blocking, or Caddy not listening.**Fix:** 1. Verify router forwards ports 80 & 443: - External Port: 80 → LAN IP: 192.168.x.x, Port: 80 - External Port: 443 → LAN IP: 192.168.x.x, Port: 443 2. Confirm UFW rules: `bash<br> sudo ufw allow 80,443/tcp<br> sudo ufw status verbose<br>` 3. Check Caddy listeners: `bash<br> ss -tulpn \| grep -E ':80|:443'<br>`
    

---

### 3. Caddy Fails to Start

- **Error:** `bind: address already in use`**Cause:** Another process (e.g., Python test server) occupies port 80/443.**Fix:** `bash<br> sudo lsof -i :80 -sTCP:LISTEN<br> kill <PID> # stop the conflicting process<br> sudo systemctl restart caddy<br>`
    

---

### 4. Certificate Issues

- **Error:** Renewal failures or missing OCSP stapling warnings**Cause:** Port 80 closed, DNS not pointing correctly, or HTTP->HTTPS redirect conflicts.**Fix:** 1. Ensure port 80 is open and forwarded. 2. Do not override Caddy’s default listeners (allow it to bind both 80/443). 3. Validate config: `bash<br> sudo caddy validate --config /etc/caddy/Caddyfile<br>`
    

---

## Alternatives to Caddy

|Tool|Pros|Cons|
|---|---|---|
|**NGINX**|Highly configurable, widely used|Manual TLS management, verbose|
|**Apache**|Full-featured, .htaccess support|Heavyweight, complex to setup|
|**Traefik**|Docker-native, auto-detects services|Learning curve, YAML configs|
|**Cloudflare Tunnel**|No port forwarding required, secure tunnels|External dependency, possible latency|
|**Tailscale Funnel**|Secure P2P access without exposing ports|Only supports HTTP(S), Tailscale needed|

---

## When to Use These Alternatives

- **NGINX/Apache**: if you need advanced rewrites, legacy support, or granular control.
    
- **Traefik**: if your environment is heavily Docker/Kubernetes-based.
    
- **Tunnels (Cloudflare/Tailscale)**: when port forwarding is not possible or IP changes frequently.
    

---

## Additional Resources

- Caddy Docs: https://caddyserver.com/docs/
    
- Let's Encrypt Docs: https://letsencrypt.org/docs/
    
- DNS Checker: https://dnschecker.org/
    
- Port Check: https://www.yougetsignal.com/tools/open-ports/