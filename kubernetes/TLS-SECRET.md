# Kubernetes TLS Secret

The TLS private key must not be committed to Git. Create the Secret locally from the certificate and key files:

```powershell
kubectl create secret tls nginx-tls-secret `
  --cert=kubernetes/tls.crt `
  --key=kubernetes/tls.key `
  --dry-run=client -o yaml | kubectl apply -f -
```

Deploy the workload:

```powershell
kubectl apply -f kubernetes/webserver-config.yaml
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
kubectl apply -f kubernetes/ingress-tls.yaml
kubectl apply -f kubernetes/hpa.yaml
```

For a new local certificate, generate one with:

```powershell
openssl req -x509 -nodes -days 365 -newkey rsa:2048 `
  -keyout kubernetes/tls.key `
  -out kubernetes/tls.crt `
  -subj "/CN=vps-interview.local"
```