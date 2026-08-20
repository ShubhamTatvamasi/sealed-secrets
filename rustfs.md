# rustfs


```bash
kubectl create secret generic rustfs-credentials \
  --from-literal=RUSTFS_ACCESS_KEY=$(openssl rand -hex 10 | tr '[:lower:]' '[:upper:]') \
  --from-literal=RUSTFS_SECRET_KEY=$(openssl rand -base64 30) \
  --dry-run=client -o yaml > /tmp/rustfs-credentials.yaml
```

```bash
cat /tmp/rustfs-credentials.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```


```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/rustfs-credentials.yaml > /tmp/rustfs-credentials-sealedsecret.yaml
```

