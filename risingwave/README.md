

kubectl create -f https://github.com/jetstack/cert-manager/releases/download/v1.8.2/cert-manager.yaml
kubectl apply --server-side -f ./risingwave-operator.yaml

helm upgrade --install operator -f etcd-values.yaml ../charts/etcd --namespace risingwave --create-namespace --debug
k apply -f risingwave.yaml

kubectl apply -f ./psql-console.yaml
kubectl -n risingwave exec -it psql-console -- bash

psql -h risingwave-frontend -p 4567 -d dev -U root

#https://docs.risingwave.com/docs/current/sink-to-iceberg/