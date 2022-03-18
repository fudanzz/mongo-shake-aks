# Mongo-shake-AKS
### This project is used to deploy mongo-shake in aks with helm charts

step1: fetch the soure code

```
git clone  git@github.com:fudanzz/mongo-shake-aks.git
```

step2: update the value.yaml to match your mongo cluster configuration

step3: package as helm charts
```
cd ongo-shake-aks
helm package .
```

step4: push into ACR

before push, you should create the registry first and login, detailed steps can be found in https://docs.microsoft.com/en-us/azure/container-registry/container-registry-helm-repos

```
helm push mongo-shake-0.15.1.tgz oci://$ACR_NAME.azurecr.cn/helm
az acr repository show --name $ACR_NAME --repository helm/mongo-shake
```

step5: deploy into AKS
mongo-shake support HA MODE, so we deploy two mongo-shake
```
helm install main oci://$ACR_NAME.azurecr.cn/helm/mongo-shake

helm install backup  oci://$ACR_NAME.azurecr.cn/helm/mongo-shake

helm list
```

step6: you can use helm uninstall to delete the deployment
```
helm list
helm uninstall xxxx
```