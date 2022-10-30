# Elastic
https://www.elastic.co/guide/en/cloud-on-k8s/master/k8s-deploy-eck.html
```
helm install --set persistence.existingClaim=PVC_NAME datalust/seq
helm install -f seq.config.yaml my-seq datalust/seq
```
