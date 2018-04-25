
* Manage AKS from Portal: https://aka.ms/aksmanagepreview
* Create new AKS Cluster from Portal: https://aka.ms/createakspreview


```
export SPN_PW=
export SPN_CLIENT_ID=4f8dc533-caad-41c1-9ac2-00dcadd39c7c

export AKS_SUB=dd323474-a5cb-40c9-9360-3dbc04a7cf90
export AKS_RG=gabrtv-build-demo
export AKS_NAME=build-demo-2
```

## Create 1.9.6 AKS Cluster

```
az group deployment create -n 05-fix-gabe -g ${AKS_RG} --template-file 05-fix-gabe.json \
        --parameters \
        resourceName=${AKS_NAME} \
        dnsPrefix=${AKS_NAME} \
        servicePrincipalClientId=${SPN_CLIENT_ID} \
        servicePrincipalClientSecret=${SPN_PW} \
        enableHttpApplicationRouting=true \
        enableOmsAgent=true
```

## Create 1.9.6 AKS Cluster

```
az group deployment create -n 06-onboard-container-health -g ${AKS_RG} --template-file 06-onboard-container-health \
        --parameters \
        aksResourceId=/subscriptions/dd323474-a5cb-40c9-9360-3dbc04a7cf90/resourcegroups/gabrtv-build-demo/providers/Microsoft.ContainerService/managedClusters/build-demo-2 \
        workspaceId=/subscriptions/dd323474-a5cb-40c9-9360-3dbc04a7cf90/resourceGroups/gabrtv-build-demo/providers/Microsoft.OperationalInsights/workspaces/build-demo-2 \
        aksResourceLocation="Canada East" \
        workspaceRegion="Canada Central"
```

## GET AKS Cluster

```
az resource show --api-version 2018-03-31 \
        --id /subscriptions/${AKS_SUB}/resourceGroups/${AKS_RG}/providers/Microsoft.ContainerService/managedClusters/${AKS_NAME}
```
