# Kubectl Utils Commands

#### 1. Listing Pods status

``` 
kubectl get pods -n airflow
```

#### 2. Extract logs from a Pod

```
kubectl logs --namespace airflow -f airflow-postgresql-0
```