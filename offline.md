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

```bash
kubeseal \
  --cert /tmp/sealed-secrets.pem \
  --scope cluster-wide \
  --format yaml \
  < /tmp/shubhamtatvamasi-tls.yaml > /tmp/shubhamtatvamasi-tls-sealedsecret.yaml
```
