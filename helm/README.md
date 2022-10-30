# Postgres

## https://github.com/bitnami/charts/tree/main/bitnami/postgresql
kubectl create secret generic postgres-config-secret -n default --from-literal=postgres-password=123456 --from-literal=password=234567 --from-literal=replication-password=345678
helm repo add my-repo https://charts.bitnami.com/bitnami
helm install postgresql -f helm/postgres/postgres.config.yaml  my-repo/postgresql --namespace default
helm upgrade postgresql -f helm/postgres/postgres.config.yaml  my-repo/postgresql --namespace default

## initdbScripts
https://stackoverflow.com/questions/66333474/postgresql-helm-chart-with-initdbscripts

** Please be patient while the chart is being deployed **

PostgreSQL can be accessed via port 5432 on the following DNS names from within your cluster:

    postgresql.default.svc.cluster.local - Read/Write connection

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace default svc/postgresql 5432:5432 &
    PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432

# Kong
## Values
https://github.com/Kong/charts/tree/main/charts/kong/example-values
Kong ingress: https://docs.konghq.com/kubernetes-ingress-controller/latest/guides/getting-started/

Kong manager: https://docs.konghq.com/gateway/latest/kong-manager/workspaces/

## Fazer o yaml secret
## https://phoenixnap.com/kb/kubernetes-secrets
kubectl create secret generic kong-config-secret -n kong \
    --from-literal=portal_session_conf='{"storage":"kong","secret":"super_secret_salt_string","cookie_name":"portal_session","cookie_samesite":"off","cookie_secure":false}' \
    --from-literal=admin_gui_session_conf='{"storage":"kong","secret":"super_secret_salt_string","cookie_name":"admin_session","cookie_samesite":"off","cookie_secure":false}' \
    --from-literal=pg_host="postgresql.default.svc.cluster.local" \
    --from-literal=kong_admin_password=kong \
    --from-literal=password=123456 \
    --from-literal=pg_password=123456

kubectl create secret generic kong-enterprise-license --from-literal=license="'{}'" -n kong --dry-run=client -o yaml | kubectl apply -f -

helm repo add kong https://charts.konghq.com ; helm repo update
helm install api kong/kong --namespace kong --values helm/kong/kong.config-http.yaml
helm upgrade api kong/kong --namespace kong --values helm/kong/kong.config-http.yaml

# Konga
helm install konga ./ -n kong -f 0/helm/kong/konga.config.yaml