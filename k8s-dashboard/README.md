
# Add kubernetes-dashboard repository
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

# Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard

kubectl apply -f k8s-dashboard/dashboard-adminuser.yaml

kubectl -n kubernetes-dashboard create token admin-user

kubectl get secret admin-user -n kubernetes-dashboard -o jsonpath={".data.token"} | base64 -d