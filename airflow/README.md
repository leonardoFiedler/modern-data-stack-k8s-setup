# Running Airflow on K8s

#### 1. Install Docker
#### 2. Install [KinD](https://kind.sigs.k8s.io/docs/user/quick-start#installation)

##### 2.1. give `kind` ability to execute and move to executable folder

``` 
chmod +x ./kind                               
sudo mv ./kind /usr/local/bin/kind
```

#### 3. Download [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client
```

#### 4. Install [Helm](https://helm.sh/docs/intro/install/#from-script)

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64

helm repo add apache-airflow https://airflow.apache.org
helm repo update
```

#### 4. Install Airflow through KinD/Helm

##### 4.0. Go to Airflow folder

```
cd airflow
```

##### 4.1. Cluster creation

```
kind create cluster --config kind-cluster.yaml
```

##### 4.2. In case of needing delete the cluster

```
kind delete cluster --name airflow-tutorial-cluster
```

##### 4.3. Creating the app through the airflow.yaml file

```
kubectl create namespace airflow --dry-run=client -o yaml > airflow.yaml
kubectl apply -f airflow.yaml
```

##### 4.4. Applicating persistent volumes to map logs and dags folder

```
kubectl apply -f kubernetes-persistent-volumes.yaml -n airflow

kubectl apply -f persistent-volume-claims.yaml
```

##### 4.5. Mapping Ingress Nginx

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
kubectl wait --namespace ingress-nginx \
--for=condition=ready pod \
--selector=app.kubernetes.io/component=controller \
--timeout=90s
```

##### 4.6. Getting default values from Helm and applying it

###### 4.6.1. Necessario rodar somente na primeira vez

```
helm show values apache-airflow/airflow > helm/values.yaml
```

###### 4.6.2. Applying and deploying

```
helm upgrade --install airflow apache-airflow/airflow -n airflow -f helm/values.yaml --debug
```