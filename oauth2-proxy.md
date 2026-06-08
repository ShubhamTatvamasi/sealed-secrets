# oauth2-proxy

Create oauth2-proxy secret for traefik:
```bash
kubectl create secret generic oauth2-proxy \
  --from-literal=client-id=traefik-dashboard \
  --from-literal=client-secret=traefik-dex-client-secret \
  --from-literal=cookie-secret=$(openssl rand -hex 32) \
  --dry-run=client -o yaml > /tmp/oauth2-proxy.yaml
```

```bash
cat /tmp/oauth2-proxy.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```
