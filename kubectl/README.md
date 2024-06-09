# Comandos uteis do Kubectl


- 1. Listagem situação dos Pods

`kubectl get pods -n airflow`

- 2. Obter os logs de um Pod

`kubectl logs --namespace airflow -f airflow-postgresql-0`