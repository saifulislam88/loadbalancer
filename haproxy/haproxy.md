```sh

# Global settings
global
    log /dev/log local0
    log /dev/log local1 notice
    daemon
    maxconn 2048
    user haproxy
    group haproxy

# Default settings for all sections
defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    timeout connect 5s
    timeout client  50s
    timeout server  50s
    retries 3

# Frontend configuration
frontend http_front
    bind *:80
    default_backend apache_backend
    option http-server-close
    option forwardfor

# Backend configuration
backend apache_backend
    balance roundrobin
    server web1 192.168.56.101:80 check
    server web2 192.168.56.102:80 check

# Optional: stats interface
listen stats
    bind *:8404
    mode http
    stats enable
    stats uri /stats
    stats refresh 10s
    stats auth admin:admin123
```

