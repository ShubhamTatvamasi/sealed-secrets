# Dex


```bash
kubectl create secret generic dex-client-secrets \
  --from-literal=flux-web-ui=$(openssl rand -hex 32) \
  --dry-run=client -o yaml > /tmp/dex-client-secrets.yaml
```

```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/dex-client-secrets.yaml > /tmp/dex-client-secrets-sealedsecret.yaml
```

