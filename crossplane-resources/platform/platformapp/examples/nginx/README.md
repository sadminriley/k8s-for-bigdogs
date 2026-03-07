This creates an ingress url and a cert-manager certificate for url https://demo-nginx.test:8443/

Make sure forward the ssl servce as so
```
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8443:443
```

This is now a ssl managed resource with an ingress rule + svc.
