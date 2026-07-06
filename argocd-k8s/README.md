# Commands
```bash
kubectl create ns argocd

kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get pods -n argocd -w

kubectl port-forward svc/argocd-server -n argocd 8080:443

# Default username is admin, password get with the following command
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# Create cert for HTTPS to argocd
openssl req -x509 -nodes -newkey rsa:2048 \
  -keyout argocd.key -out argocd.crt \
  -days 365 \
  -subj "/CN=argocd.internal.com" \
  -addext "subjectAltName=DNS:argocd.internal.com"

# Import into ACM
aws acm import-certificate \
  --certificate fileb://argocd.crt \
  --private-key fileb://argocd.key \
  --region <your-region>
```
