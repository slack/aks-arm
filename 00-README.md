```
export AKS_SUB=8ecadfc9-d1a3-4ea4-b844-0d9f87e4d7c8

export SPN_CLIENT_ID=398d56f3-49de-44ee-aae6-b07a9f0237b4
export SPN_PW=

export AKS_RG=jahanse-canadaeast
export AKS_NAME=aks-basic01
```

```
az group deployment create -n 01-aks-create -g ${AKS_RG} --template-file 01-aks-create.json \
    --parameters resourceName=${AKS_NAME} \
    dnsPrefix=${AKS_NAME} \
    servicePrincipalClientId=${SPN_CLIENT_ID} \
    servicePrincipalClientSecret=${SPN_PW}
```

```
az group deployment create -n 02-enable-http-application-routing -g ${AKS_RG} --template-file 02-aks-enable-http-application-routing.json \
    --parameters resourceName=${AKS_NAME} \
    dnsPrefix=${AKS_NAME} \
    servicePrincipalClientId=${SPN_CLIENT_ID} \
    servicePrincipalClientSecret=${SPN_PW} \
    enableHttpApplicationRouting=true
```
