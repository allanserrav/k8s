kubectl apply -f namespace.yaml
kubectl apply  -k .\front\overlays\dev --validate=false

# Set cluster
ls $PWD/clusters/local.desktop.yaml 
export KUBECONFIG='dir retornado acima'


## Criar namespace
kubectl create namespace mssql-database

## Setar o namespace default
```
kubectl config set-context --current --namespace=mssql-database
```

## Criando uma secret
```
kubectl create secret generic mssql --from-literal=SA_PASSWORD=”123456lA@”
```

## Aplicar arquivo yaml no cluster
```
kubectl apply --namespace allanserrav-dev -f sqldeployment.yaml
```