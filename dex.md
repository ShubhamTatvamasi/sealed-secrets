# Dex


```bash
kubectl create secret generic dex-client-secrets \
  --from-literal=flux-web-ui=$(openssl rand -hex 32) \
  --dry-run=client -o yaml > /tmp/dex-client-secrets.yaml
```
