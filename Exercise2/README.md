# Service

The smallest deployable entity in Kubernetes is a pod. Container(s) run in a pod.

In this exercise we will see how to spin up multiple pods. We will also see how the Kubernetes controller reacts when we kill one of the pods. In this exercise you will also set up a service and expose it.

## Start some containers

Let's start a bunch of pods that uses an pre-composed docker image of elasticsearch

```bash
kubectl run elastic --image=elasticsearch:2 --replicas=7
```

Watch the magic
```bash
kubectl get pods -w
```
You should see pods in 'Initialising' state and then will transition to 'Running' 

## Delete that pod
Grab one of the pod names and delete the pod

```bash
kubectl delete pod <PodName>

kubectl get pods -w
```

You should see that one of the old pods is deleted and Kubernetes has spun up a new pod

# Kubernetes service

A service in k8s land is a stable address for a pod or a cluster of pods. Service is created using:
```bash
kubectl expose
```
This allows us to connect to our pods. Let's expose elasticsearch HTTP api port:

```bash
kubectl expose deploy/elastic --port 9200 --type=LoadBalancer
```

This command will show you the internal IP address that was allocated to it
```bash
kubectl get svc
```

Get ip address from above and you can curl
```bash
curl http://<ipaddress>:9200/
```

You can find more detail about the service using:
```bash
kubectl describe service elastic
```
It includes information about the service e.g. the 7 endpoints for the 7 pods that we've spun up.

You can also scale up/down the replicas using the following commands.

To get the deployment name:
```bash
kubectl get deployments
```

Get the deployment name and scale down the number of pods running

```bash
kubectl scale deploy/<DeploymentName> --replicas=2
```
