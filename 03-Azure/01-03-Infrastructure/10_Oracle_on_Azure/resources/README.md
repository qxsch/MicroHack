# 🚀 Oracle Database @ Azure (ODAA) - Deployment Scripts

## ⚙️ Install the Microhack Environment

The following resources needs to be created before the workshop start.

- 📦 Azure Resource Group
- ⚓ Azure Kubernetes Service (AKS)
- 🌐 Install Ingress Controller NGINX on AKS

> ⚠️ **IMPORTANT:** Make sure the CIDR of the created VNet is added to the Oracle NSG.

### 📋 Prerequisites

- 🔧 install Azure CLI
- ⚓ install kubectl
- 📦 install helm
- 🔍 install jq
- 💻 Scripts need to run on bash (Linux, MacOS, WSL2 on Windows)

### 🔐 Login to Azure and set the right subscription

~~~powershell
az login --use-device-code
az account show
az account set -s "<your-subscription-id>"
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
az provider register --namespace Microsoft.OperationalInsights
az provider register --namespace Microsoft.Compute 
~~~

### 🌍 Define some environment variables

~~~powershell
$prefix="odaa"
$postfix="1"
$location="germanywestcentral"
~~~

### 🏗️ Create Azure Resources

> ℹ️ **NOTE:** Currently you will need to redo this steps for each Team environment. Make sure to change the postfix.

~~~bash
az deployment sub create -n "$prefix$postfix" -l $location -f ./resources/infra/bicep/aks/main.bicep -p location=$location prefix=$prefix postfix=$postfix aksVmSize="Standard_D8ads_v6" cidr="10.11.0.0"
# Verify the created resources, list all resource inside the resource group
az resource list -g "$prefix$postfix" -o table --query "[].{Name:name, Type:type}"
~~~

~~~text
Name    Type
------  ------------------------------------------
odaa1   Microsoft.Network/virtualNetworks
odaa1   Microsoft.OperationalInsights/workspaces
odaa1   Microsoft.ContainerService/managedClusters
~~~

### ⚓ Connect to AKS

~~~powershell
# login to aks
az aks get-credentials -g "$prefix$postfix" -n "$prefix$postfix" --overwrite-existing
# list namespaces
kubectl get namespaces # should show default, kube-system, kube-public
~~~

~~~text
NAME                STATUS   AGE
default             Active   10m
gatekeeper-system   Active   9m37s
kube-node-lease     Active   10m
kube-public         Active   10m
kube-system         Active   10m
~~~

### 🌐 Install Ingress Controller

🌟 An ingress controller on Azure Kubernetes Service (AKS) manages external access to services running inside your cluster. It acts as a gateway, routing HTTP and HTTPS traffic from outside the cluster to the appropriate internal services based on rules you define. This enables features like SSL termination, load balancing, and path-based routing, making it easier to securely expose and manage multiple applications within AKS.

~~~powershell
# Change directory to the scripts
# cd scripts/k8s_install/
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
kubectl create namespace ingress-nginx
helm install nginx-quick ingress-nginx/ingress-nginx -n ingress-nginx
# patch health probe:
kubectl patch service nginx-quick-ingress-nginx-controller -n ingress-nginx -p '{\"metadata\":{\"annotations\":{\"service.beta.kubernetes.io/azure-load-balancer-health-probe-request-path\":\"/healthz\"}}}'
# verify if annotation is added
kubectl get service nginx-quick-ingress-nginx-controller -n ingress-nginx -o jsonpath='{.metadata.annotations}' | jq
kubectl get service --namespace ingress-nginx nginx-quick-ingress-nginx-controller --output wide
# get external IP of nginx controller, you maybe need to wait a few minutes until the IP is assigned
kubectl get service -n ingress-nginx -o jsonpath='{.items[*].status.loadBalancer.ingress[*].ip}'
~~~

## Tips and Tricks

### VNet Peering between two subscriptions

In case your odaa does run in a different tenant / subscription, you need to create a VNet Peering between the two VNet.

~~~powershell
$postfixODAA = "2"
$postfixAKS = "1"
$subODAAName = "ODAA"
$subAKSName = "sub-1"

az login -t "<ODAA-tenant-id>"
az account set -s $subODAAName

# Peering AKS VNet to ODAA VNet
# We need to retrieve the subscription IDs first of the ODAA Vnet
az account set -s $subODAAName;
$subODAAId = az account show --query id -o tsv
# Now we need to login into the subscription where AKS is deployed
az login -t "<AKS-tenant-id>"
az account set -s $subAKSName;
$subAKSId = az account show --query id -o tsv
az network vnet peering create --name AKS-to-ODAA -g "$prefix$postfixAKS" --vnet-name "$prefix$postfixAKS" --remote-vnet /subscriptions/$subODAAId/resourceGroups/"$prefix$postfixODAA"/providers/Microsoft.Network/virtualNetworks/"$prefix$postfixODAA" --allow-vnet-access
# Peering ODAA VNet to AKS VNet
az account set -s $subODAAName;
az network vnet peering create -n ODAA-to-AKS -g "$prefix$postfixODAA" --vnet-name "$prefix$postfixODAA" --remote-vnet /subscriptions/$subAKSId/resourceGroups/"$prefix$postfixAKS"/providers/Microsoft.Network/virtualNetworks/"$prefix$postfixAKS" --allow-vnet-access


# Verify peering on sububscription sub-cptdx-01
az network vnet peering list -g "$prefix$postfixODAA" --vnet-name "$prefix$postfixODAA" -o table
az account set -s $subAKSName
az network vnet peering list -g "$prefix$postfixAKS" --vnet-name "$prefix$postfixAKS" -o table
~~~

### Validate Ingress controller in AKS

~~~powershell
kubectl get service --namespace ingress-nginx nginx-quick-ingress-nginx-controller
# validate health probe
kubectl get service nginx-quick-ingress-nginx-controller -n ingress-nginx -o jsonpath='{.metadata.annotations}'

~~~