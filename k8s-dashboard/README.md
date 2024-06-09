# K8s Dashboard

- 1. Add kubernetes-dashboard repository
`helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/`

- 2. Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart

```
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

- 3. Configura o Dashboard

- 3.1. Aplica a config de admin user

`kubectl apply -f k8s-dashboard/dashboard-adminuser.yaml`

- 3.2. Obtem o token para colocar na tela inicial

`kubectl -n kubernetes-dashboard create token admin-user`

- 3.3. Faz o bind da porta para acessar localment: https:localhost:8443

`kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443`