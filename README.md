# Spin Up Kubernetes Cluster in AKS

Follow these steps to spin up a Kubernetes Cluster in AKS with 3 nodes

You can use Shell in Azure Portal or [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

## Create ResourceGroup

```bash
az group create --name progNetK8s --location westeurope
```
If the response includes the following then the ResourceGroup has been created successfully

```json
"properties": {
    "provisioningState": "Succeeded"
  },
```

## Create a 3-node cluster

az aks create --resource-group progNetK8s --name myAKSCluster --node-count 3 --enable-addons monitoring --generate-ssh-keys

Leave this running, might take a few minutes (10-15 mins) to create the cluster. When running you should see this:

```bash
 - Running ..
```

## Connect to cluster

If you are using Azure Cloud Shell, you are good to go

If you are using Azure CLI then install kubectl:

```bash
az aks install-cli
```

Connect to cluster using:
```bash
az aks get-credentials --resource-group progNetK8s --name myAKSCluster
```

You should get a response like this:
```bash
Merged "myAKSCluster" as current context in /home/salman/.kube/config
```

Run the following command to get nodes:

```bash
kubectl get nodes
```

You should see the nodes
```bash
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-35631116-0   Ready     agent     23m       v1.9.9
aks-nodepool1-35631116-1   Ready     agent     23m       v1.9.9
aks-nodepool1-35631116-2   Ready     agent     23m       v1.9.9
```

