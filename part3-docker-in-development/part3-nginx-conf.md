# nginx.conf: Reverse Proxy to a CF Engine

Our NGINX virtual host definitions are fairly typical for a reverse proxy, along with a couple of tweaks for Docker.

```text
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

NGINX will serve static assets directly and proxy all other requests to the CF container. The last section \(under **CFML Handler**\) is the most important, but let's look at all of them:

## location ~ / proxy\_pass

This line directs NGINX to proxy any request whose request matches the specified extensions to the hostname matching the value of the $cfml\_host variable. Because it is the last `location` directive, it will also proxy any request that doesn't match any other location directive so our SES-friendly requests will land here as well.

## resolver

**resolver 127.0.0.11** tells NGINX to use the Docker daemon's DNS resolution rather than the container's.

## Upstream proxy variable \(set $cmfl\_host\)

Whether we're using NGINX's **upstream** directive \(as we would with a swarm configuration, as you'll see later in the Production setup section\) or a simple **proxy\_pass** directly to a single host, making the destination -- the container to which we're proxying the request -- a variable rather than a proper hostname will allow successful DNS lookups of containers that exist but aren't running; otherwise, NGINX can fail if an upstream proxy destination isn't running.

## Common includes \(inc\_lockdown.conf, inc\_locationRewrites.conf\)

Some configuration directives will be common to all our CF containers -- denying \(or allowing only specific hosts\) to **/CFIDE** \(for Adobe Coldfusion\) or **/lucee**, for example. These should be factored out of site-specific definitions and into include files that can be shared by several hosts. You can look at these files in the sample app; one handles lockdowns and the other enables URL rewriting on the NGINX-side. These are both outside the scope of this guide and neither is necessary for the sample app to function.

## location \(CSS, JPG, Other Static Assets\)

While the "meat" of this section is the **proxy\_pass** directive, ideally we want NGINX to serve static assets like CSS and images; there's no reason for Coldfusion to serve those assets. If the NGINX container can see all the static assets in your app, this is a nice way of handling them, but it's not necessary; Commandbox particularly can serve just about anything very quickly, so if you must proxy everything to your CF container you certainly can.

