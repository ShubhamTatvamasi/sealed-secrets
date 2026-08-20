# rustfs


```bash
kubectl create secret generic rustfs-credentials \
  --from-literal=RUSTFS_ACCESS_KEY=$(openssl rand -base64 32 | tr -dc 'A-Z0-9' | head -c 20) \
  --from-literal=RUSTFS_SECRET_KEY=$(openssl rand -base64 64 | tr -dc 'A-Za-z0-9' | head -c 40) \
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

```bash
kubectl get secret rustfs-credentials -n rustfs -o yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```
