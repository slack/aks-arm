```
#export SPN_PW=
export SPN_CLIENT_ID=398d56f3-49de-44ee-aae6-b07a9f0237b4

export AKS_SUB=8ecadfc9-d1a3-4ea4-b844-0d9f87e4d7c8

export AKS_RG=jahanse-canadaeast
export AKS_NAME=aks-vnet01

export AKS_VNET_RG=jahanse-canadaeast-vnet
export AKS_VNET_NAME=Apriori
export AKS_VNET_RANGE=172.15.0.0/16
export AKS_SUBNET1_NAME=VNET-Local
export AKS_SUBNET1_RANGE=172.15.0.0/22
export AKS_SUBNET2_NAME=AKS-Nodes
export AKS_SUBNET2_RANGE=172.15.4.0/22

export AKS_SVC_RANGE=172.15.8.0/22
export AKS_SUBNET=/subscriptions/${AKS_SUB}/resourceGroups/${AKS_VNET_RG}/providers/Microsoft.Network/virtualNetworks/${AKS_VNET_NAME}/subnets/${AKS_SUBNET2_NAME}
export AKS_BRIDGE_IP=172.16.0.1/24
export AKS_DNS_IP=172.15.8.2
```

## Create VNET Resources

```
az group deployment create -n 01-aks-vnet -g ${AKS_VNET_RG} --template-file 01-aks-vnet.json \
    --parameters \
    vnetName=${AKS_VNET_NAME} \
    vnetAddressPrefix=${AKS_VNET_RANGE} \
    subnet1Name=${AKS_SUBNET1_NAME} \
    subnet1Prefix=${AKS_SUBNET1_RANGE} \
    subnet2Name=${AKS_SUBNET2_NAME} \
    subnet2Prefix=${AKS_SUBNET2_RANGE}
```

## Create AKS Cluster

```
az group deployment create -n 02-aks-custom-vnet -g ${AKS_RG} --template-file 02-aks-custom-vnet.json \
    --parameters \
    resourceName=${AKS_NAME} \
    dnsPrefix=${AKS_NAME} \
    servicePrincipalClientId=${SPN_CLIENT_ID} \
    servicePrincipalClientSecret=${SPN_PW} \
    networkPlugin=azure \
    serviceCidr=${AKS_SVC_RANGE} \
    dnsServiceIP=${AKS_DNS_IP} \
    dockerBridgeCidr=${AKS_BRIDGE_IP} \
    vnetSubnetID=/subscriptions/${AKS_SUB}/resourceGroups/${AKS_VNET_RG}/providers/Microsoft.Network/virtualNetworks/${AKS_VNET_NAME}/subnets/${AKS_SUBNET2_NAME} \
    enableHttpApplicationRouting=false \
    enableOmsAgent=false
```

## GET AKS Cluster

```
az resource show --api-version 2018-03-31 --id /subscriptions/${AKS_SUB}/resourceGroups/${AKS_RG}/providers/Microsoft.ContainerService/managedClusters/${AKS_NAME}
```
