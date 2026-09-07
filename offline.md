# Offline

### Encrypt

Download certificate from sealed-secrets controller:
```bash
kubeseal \
  --controller-name sealed-secrets \
  --controller-namespace sealed-secrets \
  --fetch-cert > /tmp/sealed-secrets.pem
```

Verify details:
```bash
openssl x509 -in /tmp/sealed-secrets.pem -text -noout
```

Create a sealed secret offline:
```bash
kubeseal \
  --cert /tmp/sealed-secrets.pem \
  --scope cluster-wide \
  --format yaml \
  < /tmp/shubhamtatvamasi-tls.yaml > /tmp/shubhamtatvamasi-tls-sealedsecret.yaml
```

---

### Decrypt


Download the `sealed-secrets-key` from cluster:
```bash
kubectl -n sealed-secrets get secret -l sealedsecrets.bitnami.com/sealed-secrets-key \
  -o yaml > /tmp/sealed-secrets-key.yaml
```

Decrypt the sealed secret: 
```bash
kubeseal --recovery-unseal \
  --recovery-private-key /tmp/sealed-secrets-key.yaml \
  < shubhamtatvamasi-tls-sealedsecret.yaml \
  > shubhamtatvamasi-tls.yaml
```

