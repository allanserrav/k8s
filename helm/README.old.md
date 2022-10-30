# SEQ
```
helm install --set persistence.existingClaim=PVC_NAME datalust/seq
helm install -f seq.config.yaml my-seq datalust/seq
```
## linode
helm install --namespace dev -f .\helm\seq\seq.config.yaml my-seq datalust/seq

# ingress-nginx
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```
## linode
helm install --namespace allanserrav-dev dev-ingress-nginx ingress-nginx/ingress-nginx


# traefik
```
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install --namespace default -f .\1-helm\traefik\traefik.config.yaml traefik traefik/traefik
```
## Dashboard
```
kubectl port-forward $(kubectl get pods --namespace default --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
```
http://127.0.0.1:9000/dashboard/


# keycloak
## https://hustakin.github.io/bestpractice/keycloak-traefik-k8s/
```
helm repo add codecentric https://codecentric.github.io/helm-charts
helm repo update
helm install --namespace default -f .\1-helm\traefik\traefik.config.yaml keycloak codecentric/keycloak
# helm list
# helm delete <name> (delete the chart)
```

# Kong
https://docs.konghq.com/gateway/3.0.x/install/kubernetes/helm-quickstart/

curl -o ./dragon.yaml -L https://bit.ly/KongGatewayHelmValuesAIO
export BASE_DOMAIN="allanserrav.com"
sed -i '' "s/127-0-0-1\.nip\.io/$BASE_DOMAIN/g" ./dragon.yaml

kubectl create secret generic kong-config-secret -n kong --from-literal=portal_session_conf='{"storage":"kong","secret":"super_secret_salt_string","cookie_name":"portal_session","cookie_samesite":"off","cookie_secure":false}' --from-literal=admin_gui_session_conf='{"storage":"kong","secret":"super_secret_salt_string","cookie_name":"admin_session","cookie_samesite":"off","cookie_secure":false}' --from-literal=pg_host="enterprise-postgresql.kong.svc.cluster.local" --from-literal=kong_admin_password=kong --from-literal=password=kong 

helm repo add kong https://charts.konghq.com ; helm repo update
helm install dragon kong/kong --namespace kong --values 0/helm/kong/dragon.yaml

To connect to Kong, please execute the following commands:

HOST=$(kubectl get svc --namespace kong dragon-kong-proxy -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
PORT=$(kubectl get svc --namespace kong dragon-kong-proxy -o jsonpath='{.spec.ports[0].port}')
export PROXY_IP=${HOST}:${PORT}
curl $PROXY_IP

Once installed, please follow along the getting started guide to start using
Kong: https://docs.konghq.com/kubernetes-ingress-controller/latest/guides/getting-started/

