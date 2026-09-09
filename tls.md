# TLS

Create a TLS secret with CA:
```bash
kubectl create secret generic shubhamtatvamasi-tls \
  --from-file=tls.crt=fullchain.cer \
  --from-file=tls.key="*.k7s.shubhamtatvamasi.com.key" \
  --from-file=ca.crt=ca.cer \
  --dry-run=client -o yaml |
kubectl patch --local -f - \
  --type=merge \
  -p '{"type":"kubernetes.io/tls"}' \
  -o yaml \
  > /tmp/shubhamtatvamasi-tls.yaml
```

```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/shubhamtatvamasi-tls.yaml > /tmp/shubhamtatvamasi-tls-sealedsecret.yaml
```

---

### OLD


Create a secret:
```bash
kubectl create secret tls shubhamtatvamasi-tls \
  --cert=fullchain.cer \
  --key="*.k7s.shubhamtatvamasi.com.key" \
  --dry-run=client -o yaml > /tmp/shubhamtatvamasi-tls.yaml
```
