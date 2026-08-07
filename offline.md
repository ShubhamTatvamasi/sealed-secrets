# Offline

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
