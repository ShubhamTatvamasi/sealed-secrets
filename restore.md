# Restore Private Key

Delete new secret:
```bash
kubectl delete secret -n sealed-secrets \
  -l sealedsecrets.bitnami.com/sealed-secrets-key
```

Apply old secret:
```bash
kubectl apply -f /tmp/sealed-secrets-key-backup.yaml
```

Restart sealed-secrets controller:
```bash
kubectl rollout restart deployment sealed-secrets -n sealed-secrets
```
