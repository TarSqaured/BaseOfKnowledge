## What Is a Domain?

A domain name (e.g., `finance.tyshub.xyz`) is a human-readable label that maps to a server’s IP address (e.g., `79.235.252.101`). You configure it via DNS records at your registrar.

---

## Key DNS Record Types

- **A record**: maps a hostname to an IPv4 address
    
- **AAAA record**: maps a hostname to an IPv6 address
    
- **CNAME record**: aliases one hostname to another
    
- **TXT record**: holds arbitrary text (used for verification, ACME challenges)
    

---

## Basic DNS Setup Steps

1. **Log in** to your DNS provider (e.g., Porkbun).
    
2. **Add an A record** for your subdomain:
    
    ```text
    Type: A  
    Host: finance  
    Value: YOUR_PUBLIC_IP  # e.g., 79.235.252.101  
    TTL: 300
    ```
    
3. **(Optional) Wildcard record** if you want any subdomain to resolve:
    
    ```text
    Type: A  
    Host: *  
    Value: YOUR_PUBLIC_IP  
    TTL: 300
    ```
    
4. **Save** and wait ~30 seconds for propagation.
    

---

## Verifying DNS Propagation

Run from any terminal or use a DNS-checking website:

```bash
dig +short finance.tyshub.xyz @1.1.1.1
```

Expected output: your public IPv4 (e.g., `79.235.252.101`).

---

## Common DNS Troubleshooting

- **Nothing returns**: record not saved or typo in host/name
    
- **Old IP returns**: wait for TTL (300 s) or reduce TTL when editing
    
- **Different IP**: verify your current public IP with:
    
    ```bash
    curl -4 https://icanhazip.com
    ```
    

---

## Tips & Best Practices

- Use a **static or reserved DHCP lease** on your router so your server’s LAN IP never changes.
    
- Keep TTL low (300–600 s) during setup, then raise to 3600 s once stable.
    
- Maintain a **wildcard A record** (`*`) if you plan multiple subdomains.