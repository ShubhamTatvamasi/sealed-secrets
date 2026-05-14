# Dex


```bash
kubectl create secret generic dex-client-secrets \
  --from-literal=FLUX_WEB_UI=$(openssl rand -hex 32) \
  --dry-run=client -o yaml > /tmp/dex-client-secrets.yaml
```

```
yq '.data.FLUX_WEB_UI' /tmp/dex-client-secrets.yaml | base64 -d ; echo
```

```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/dex-client-secrets.yaml > /tmp/dex-client-secrets-sealedsecret.yaml
```

