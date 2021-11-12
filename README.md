## Build
- required xcaddy along with its requirements

```bash
# replace v2.4.6 with your target version
# you can use newcaddy binary in working directory

xcaddy build v2.4.6 \
  --with github.com/kesuskim/caddy-samplemodule \
  --output newcaddy
```

## Caddyfile example
```Caddyfile
{ # this is global option
  email kesuskim@gmail.com
  http_port 80
  https_port 443
  admin localhost:2019
  storage file_system {
    root /var/lib/caddy
  }
  grace_period 5s
  on_demand_tls {
    interval 10s
    burst 10
  }
  key_type rsa2048

  ## NOTE: to use module globally, order directive must be placed here
  ## Reference: https://caddyserver.com/docs/caddyfile/options
  ## below is example
  order visitor_ip before respond
}

site1.localhost {
  visitor_ip  stdout    # must work pair with above global order directive
  reverse_proxy http://127.0.0.1:8080
}

site2.localhost {
  route / {
    visitor_ip  stdout    # can work without above global order directive within route block
    reverse_proxy http://127.0.0.1:8081
  }
}
```

## Reference

- https://caddyserver.com/docs/extending-caddy
- https://github.com/caddyserver/caddy/issues/3438