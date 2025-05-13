# ansible-role-caddy

Install and configure Caddy reverse proxy programatically. Tested on Ubuntu 24.04.

## Variables
TLS providers
```yaml
caddy_tls_providers:
  - provider: cloudflare
    challenge_type: dns
    provider_api_token: "1234567890abcdefg"
    resolver_ip: 1.1.1.1
```

Endpoints
```yaml
caddy_endpoints:
  - friendly_name: app1
    fqdn: app1.exaple.com
    upstream: 
        - to: "localhost:8081"
          tls_insecure: false
    client_ips:
      - "100.64.0.0/10"
      - "192.168.0.0/24"
    tls_provider: cloudflare
        
  - friendly_name: app2
    fqdn: app2.exaple.com
    headers:
      - field: "Strict-Transport-Security"
        value: "max-age=31536000"
    redirs:
      - matcher: "/.well-known/dev"
        to: "/app2.php/dav/"
        code: "301"
    upstream: 
      - to: "localhost:8082"
        tls_insecure: false
      - to: "/notifi localhost:8083"
        tls_insecure: false
    tls_provider: cloudflare

  - friendly_name: Wildcard *.local.example.com
    fqdn: '*.local.exaple.com'
    tls_provider: cloudflare
    wildcard_endpoints:
      - friendly_name: app3
        fqdn: app2.local.example.com
        upstream: 
          - to: "localhost:8084"
            tls_insecure: false
        client_ips:
          - "100.64.0.0/10"
          - "192.168.87.0/24"
        tls_insecure: false
```

