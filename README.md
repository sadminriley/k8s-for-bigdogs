# k8s-for-bigdogs-examples
## How to create your own IDP(internal developer platform) locally.
<img width="600" height="800" alt="chart_diagram" src="https://github.com/user-attachments/assets/fd2bb158-ce39-4b82-bc30-3fc4725805be" />

This is what I run for local machine for testing reasons, interview practice,whatever it might be. There is not enough tutorials on using Crossplane locally, in my opinion. These are some of the things I use; and a couple of dead simple demos for now.


This gives you a real, and very usable IDP where Argo deploys your platform resources from Github and Crossplane composes them into
your actual workloads.

Argo syncs everything from Git, and Crossplane builds the actual k8s resources.

An example of using a fully featured Argo, Helm, Kubernetes stack on minikube via crossplane. Woof woof. 🐕


[Example Crossplane Apps Repo](https://github.com/sadminriley/k8s-for-bigdogs-apps)



## Index

- [Components used](#components-used)
- [Setup](#setup)
- [Manual apply steps](#manual-apply-steps)
- [Login to ArgoCD](#login-to-argocd)
- [Minikube Dashboard](#minikube-dashboard)
- [What Argo looks like](#what-argo-looks-like-when-everything-from-the-repo-is-synced-as-apps-and-runnning)
- [Minikube commands](#minikube-cmds)
- [Start or Build the stack](#start-or-build-the-stack)
- [Local Host Entries](#local-host-entries)
- [Open URLs](#open-urls)
- [Local CA + TLS setup](#local-ca--tls-setup)


## Components used
- minikube
- Crossplane
- ArgoCD
- cert-manager
- nginx-ingress
- helm
- prometheus-kube-stack

## Setup

### Initial Manual Apply Steps

- `kubectl apply -f ops/`
- `kubectl apply -f crossplane-resources/`

This creates 26 pods in total on my local, feel free to check though:
 `kubectl get pods -A --no-headers | awk '{print $1}' | sort | uniq -c`
   * 7 argocd
   * 5 crossplane-system
   * 1 demo-nginx
   * 7 kube-system
   * 6 observability


## Login to ArgoCd

Get the password for the admin user and login
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`


## Minikube Dashboard
`minikube dashboard`

## What argo looks like when everything from the repo is synced as apps and runnning -
![Argo](https://github.com/user-attachments/assets/79af4b68-2ab7-4d48-8714-b850138f62f8)


## All port-forward commands as managed by argocd + crossplane for nginx-ingress
Do not use the minikube addon, for consistency in hostnames manage it in argo.

### minikube cmds
`minikube tunnel`
`minikube dashboard`


## Start or Build the stack(after you've done the initial manual apply step, you don't need to
 After you've done the initial manual apply step, you won't need to do it again.
`minikube start`
And sync argocd -
`argocd sync`


## Local Host Entries

**127.0.0.1 argocd.dev demo-nginx.dev hello.dev**


## Open URLs
https://argocd.dev
https://demo-nginx.dev
https://hello.dev



# Local CA + TLS setup
The CA gets installed into your macOS keychain so browsers can trust the cluster domains.
`
kubectl get secret local-dev-ca-secret \
  -n cert-manager \
  -o jsonpath='{.data.tls\.crt}' | base64 -d > local-dev-ca.crt
`

Then you can add the local-dev-ca.crt to your keychain, on a mac this works -
`sudo security add-trusted-cert \
  -d \
  -r trustRoot \
  -k /Library/Keychains/System.keychain \
  local-dev-ca.crt
`

Restart your browser and you should be able to see all the URLs without any port-forwards.
