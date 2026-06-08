# oauth2-proxy

Create oauth2-proxy secret for traefik:
```bash
kubectl create secret generic oauth2-proxy \
  --from-literal=client-id=traefik-dashboard \
  --from-literal=client-secret=traefik-dex-client-secret \
  --from-literal=cookie-secret="$(openssl rand -base64 32 | tr -d '\n')" \
  --dry-run=client -o yaml > /tmp/oauth2-proxy.yaml
```
