# Defining Local Sites with a CF Proxy

Our NGINX virtual host definitions are fairly typical for a reverse proxy, along with a couple of tweaks for Docker.

```
server {
    ################### SERVER NAME AND PORT #####################
    server_name cfswarm-simple.localtest.me;
	listen  80;
    # listen 443 ssl http2;        
    index index.cfm index.cfml index.htm index.html;
    resolver 127.0.0.11 valid=30s; # docker DNS daemon
    set $cfml_host cfswarm-cfml;   # this causes nginx to be OK if the host isn't up
    root "/var/www/app-one";
    
    include inc_lockdown.conf;

    ################### LOCATION: ROOT #####################
    include inc_locationRewrites.conf;
    
    location ~* \.(jpg|jpeg|png|gif|ico|txt|woff|woff2|ttf)$ {
        expires 30d;
    }
    
    location ~* \.(css|js)$ {
        expires 7d;
    }
    ################### CFML HANDLER #####################
    location ~ (\.cfm|\.cfml|\.cfc|\.jsp|\.cfr|flex2gateway|messagebroker|flashservices|openamf)(.*)$ {
        proxy_pass  http://$cfml_host:8080;
        set $lucee_context $server_name;
        include     inc_lucee-proxy.conf;
    }
}
```

NGINX will serve static assets directly and proxy all other requests to the CF container. 

### resolver

**resolver 127.0.0.11** tells NGINX to use the Docker daemon's DNS resolution rather than the container's. This will allow successful DNS lookups of containers that exist but aren't running; otherwise, NGINX can fail if an upstream proxy destination isn't running.

### 