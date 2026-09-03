# Backup Private Key

Only backup 1 key:
```bash
SEALED_SECRET_PRIVATE_KEY_SECRET=$(kubectl get secrets \
  -n sealed-secrets \
  -l sealedsecrets.bitnami.com/sealed-secrets-key=active \
  -o jsonpath='{.items[0].metadata.name}')
```

```bash
kubectl -n sealed-secrets \
  get secret $SEALED_SECRET_PRIVATE_KEY_SECRET \
  -o yaml > /tmp/sealed-secrets-key-backup.yaml
```

Backup multiple keys:
```bash
kubectl get secret \
  -n sealed-secrets \
  -l sealedsecrets.bitnami.com/sealed-secrets-key \
  -o yaml > /tmp/sealed-secrets-backup.yaml
```
