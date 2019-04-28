# Spin Up Kubernetes Cluster in AKS

If you need to create a trial Azure account sign up [here](https://azure.microsoft.com/free/)

Follow these steps to spin up a Kubernetes Cluster in AKS with 2 nodes.

You can use Shell in Azure Portal or [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

## Create ResourceGroup

```bash
az group create --name k8sWorkshop --location westeurope
```
If the response includes the following then the ResourceGroup has been created successfully

```json
"properties": {
    "provisioningState": "Succeeded"
  },
```

## Create a 2-node cluster

```bash
az aks create --resource-group k8sWorkshop --name myAKSCluster --node-count 2 --enable-addons monitoring --generate-ssh-keys
```

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
az aks get-credentials --resource-group k8sWorkshop --name myAKSCluster
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
aks-nodepool1-35631116-0   Ready     agent     23m       v1.12.7
aks-nodepool1-35631116-1   Ready     agent     23m       v1.12.7
```

## Deploy a sample app

We will deploy a simple [voting app](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough) with the following command

```bash
kubectl apply -f https://raw.githubusercontent.com/CloudNativeWales/K8s-Workshop/master/azure-vote.yaml
```

You should get a message that says a front end and a back end service has been created

Website is currently deploying and is not yet ready to be accessed externally yet.

Run the following command and you will see that EXTERNAL-IP is in 'pending' state

```bash
kubectl get service azure-vote-front --watch
```
It might take some time for the application to get externally available. Once it is, you will be able to navigate to website.

If the azure-vote-front takes more than 5 minutes to be assigned an external ip, delete the deployment and try again.

```bash
kubectl delete -f https://raw.githubusercontent.com/CloudNativeWales/K8s-Workshop/master/azure-vote.yaml
``` 

## Kubernetes Dashboard

If you have Azure CLI installed you can access your cluster's k8s dashboard. Gives you a host of information and functionality. Run the following commands:

ONLY FOR AZURE CLI
```bash
az aks get-credentials --resource-group k8sWorkshop --name myAKSCluster

az aks browse --resource-group k8sWorkshop --name myAKSCluster
```

If you are getting errors when accessing dashboard, use the following:

```bash
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Try some of the features from the dashboard. Can you scale one of your deployments? 

Be careful with the dashboard though, you don't want to end up like [Tesla](https://arstechnica.com/information-technology/2018/02/tesla-cloud-resources-are-hacked-to-run-cryptocurrency-mining-malware/)

## Issues
If you have issue regarding duplicate names that exist in aks, delete the kubeconfig and start again

```bash
rm -r ~/.kube
```