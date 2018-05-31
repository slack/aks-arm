
```
export SPN_PW=
export AKS_AAD_APP_SECRET=

export AKS_SUB=57ac26cf-a9f0-4908-b300-9a4e9a0fb205

export SPN_CLIENT_ID=23129df1-a721-4c06-8ca6-34f6f52a7163

export AKS_RG=jahanse-int-eastus
export AKS_NAME=aks-rbac01

export AKS_AAD_KUBECTL_ID=ec246917-6dd0-4120-8d93-7d71e0e3f797
export AKS_AAD_APP_ID=4b1dad15-cd83-4f48-9fd5-303b139ef885
export AKS_AAD_TENANT_ID=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## Create 1.9.6 AKS Cluster

```
az group deployment create -n 01-aks-create -g ${AKS_RG} --template-file 01-aks-create.json \
    --parameters resourceName=${AKS_NAME} \
    agentCount=1 \
    dnsPrefix=${AKS_NAME} \
    kubectlAppId=${AKS_AAD_KUBECTL_ID} \
    authAppId=${AKS_AAD_APP_ID} \
    authAppSecret=${AKS_AAD_APP_SECRET} \
    azureTenantId=${AKS_AAD_TENANT_ID} \
    servicePrincipalClientId=${SPN_CLIENT_ID} \
    servicePrincipalClientSecret=${SPN_PW}
```

## GET AKS Cluster

```
az resource show \
   --api-version 2018-03-31 
   --type Microsoft.ContainerService/managedClusters \
   -g ${AKS_RG} -n ${AKS_NAME}
```
