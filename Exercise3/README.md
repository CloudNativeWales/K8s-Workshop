# Persistent Volumes

PV is storage that has been provisioned in the cluster. These are treaded just like other resources in the cluster.

PV are not encouraged to be used in production yet. We will deploy a MySql instance and a Wordpress website.

From what you already know you should be able to deploy MySQL and Wordpress using the yaml files provided in this repo. You also know how to expose the external ip for the wordpress site.

In order set password for MySQL you'll have to run this command.

## Secrets 

```bash
kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD
```

Replace 'YOUR_PASSWORD'

You can confirm if secret have been set by running this command:

```bash
kubectl get secrets
```

If you can access a wordpress site, you have successfully deployed MySQL and wordpress.

Have fun.

The solution of this exercise can be found [here](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/) - Click if you are stuck