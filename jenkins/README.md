kubectl apply -f jenkins/service-account.yaml
kubectl apply -f jenkins/volume.yaml
kubectl apply -f jenkins/deployment.yaml

Procurar por initialAdminPassword no pod logs pelo jenkins admin (primeira tela), pega o token
senha aplicada: allanserrav / 123456
porta: 60598