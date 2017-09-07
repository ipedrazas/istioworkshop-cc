# Running nginx with Istio

Let's try to run an nginx server with Istio. The easiest way of doing this is to leverage the `kubectl run` command:

```
kubectl run web --image=nginx:alpine --port=80 --dry-run -oyaml
```

But we want to inject our envoy sidecar container in our deployment, so, let's do the following:

```
kubectl apply -f <(istioctl kube-inject -f <(k run web --image=nginx:alpine --port=80 --dry-run -oyaml))
```

Does it work? Why?

Envoy requires HTTP/1.1 or HTTP/2 traffic for upstream services. To use NGINX for serving traffic behind Envoy, you will need to set the `proxy_http_version` directive in your NGINX config to be "1.1", since the NGINX default is 1.0.

HTTP/2 is supported only over HTTPS.

```
upstream http_backend {
    server 127.0.0.1:8080;

    keepalive 16;
}

server {
    ...

    location /http/ {
        proxy_pass http://http_backend;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        ...
    }
}
```