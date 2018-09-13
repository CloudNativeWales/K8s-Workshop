# Scaling k8s and logs 

In this exercise, we will run a pod and in that pod we will run a single container. The container will run a simple ping command. We can watch the logs to see what happens in the pod. We can scale the pod and see what happens.

## Let's get pinging

Make sure your terminal is connected to the cluster, run this command if it isn't

```bash
az aks get-credentials --resource-group progNetK8s --name myAKSCluster
```

We will use 'kubectl run'. This command creates and run a particular image. The pod will create and run an alpine image that will ping Cloudflare's public DNS resolver 

```bash
kubectl run pingpong --image alpine ping 1.1.1.1
``` 
'pingpong' is the label for this deployment

You can see all the resources that have been created by the command above.

```bash
kubectl get all
```

## Logs

Let's view the logs for the pingpong deployment

```bash
kubectl logs deploy/pingpong
```
We have one pod and you should see a single ping get logged

You can stream the logs too:

```bash
kubectl logs deploy/pingpong --tail 1 --follow
```

--follow: streams logs in real-time
--tail: how many lines to be seen from the end

If you wan to view the logs of a specific pod use:

```bash
kubectl logs -f POD-NAME
```

## Scale the deployment
```bash
kubectl scale deploy/pingpong --replicas 8
```

You can watch the pods being created
```bash
kubectl get pods -w
```

You can delete a pod and Kubernetes will spin up a pod to ensure that there are 8 pods running at all time.

```bash
kubectl delete pod pingpong-xxxxxxxxxx-yyyyy
```

You can watch the pods again, did you notice what is different this time around?

## View multiple logs

Using the 'pingpong' label that we defined for this deployment, we can view logs from all the pods that have a deployment with label = pingpong

```bash
kubectl logs -l run=pingpong --tail 1
```

You can see that our scaled deployment has pinged 1.1.1.1 8 times already. Unfortunately, you cannot --follow logs from multiple pods.

## Deleting Deployments and Services

You can delete deployments and services using:

```bash
kubectl delete deployment <deploymentname>
```