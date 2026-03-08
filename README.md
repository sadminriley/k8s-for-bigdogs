# k8s-for-bigdogs-examples
## How to create your own IDP(internal developer platform) locally.
<img width="600" height="800" alt="chart_diagram" src="https://github.com/user-attachments/assets/fd2bb158-ce39-4b82-bc30-3fc4725805be" />

This is what I run for local machine for testing reasons, interview practice,whatever it might be. There is not enough tutorials on using Crossplane locally, in my opinion. These are some of the things I use; and a couple of dead simple demos for now.

An example of using a fully featured Argo, Helm, Kubernetes stack on minikube via crossplane. Woof woof. 🐕




Apply steps -

- `kubectl apply -f ops/`
- `kubectl apply -f crossplane-resources/`

This creates 26 pods in total on my local, feel free to check though:
 `kubectl get pods -A --no-headers | awk '{print $1}' | sort | uniq -c`
   * 7 argocd
   * 5 crossplane-system
   * 1 demo-nginx
   * 7 kube-system
   * 6 observability


## Launch argocd
`kubectl port-forward -n argocd svc/argocd-server 8080:80`

Get the password for the admin user and login
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`

## Launch demo-nginx nginx platformapp example
`kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8443:443` from the ingress rule + svc to access it from
cert-manager port.

Add an entry locally in your hosts file for demo-nginx.local pointing to 120.0.01.

Visit -

https://demo-nginx.test:8443/

You should see an nginx welcome page.

You should now be able to access the nginx instance at https://demo-nginx.test:8443/ and argocd at http://localhost:8080.

For now, this is short short demo of how to use crossplane locally with minikube to create your own IDP(internal developer platform) and use argocd to deploy applications to it via platformapps and their manifests from git; and Crossplane composing them into your running app.

## Minikube Dashboard
`minikube dashboard`


[Example Crossplane Apps Repo](https://github.com/sadminriley/k8s-for-bigdogs-apps)


## What argo looks like when everything from the repo is synced as apps and runnning -
![Argo](https://github.com/user-attachments/assets/79af4b68-2ab7-4d48-8714-b850138f62f8)



## All port-forward commands until I make them into ingress rules/svc -
Prometheus - `kubectl port-forward -n observability svc/kube-prometheus-stack-prometheus 9090:9090`
Argo - `kubectl port-forward -n argocd svc/argocd-server 8081:80`
Ingress for demo - `kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8082:80`

### minikube cmds
`minikube tunnel`
`minikube dashboard`
