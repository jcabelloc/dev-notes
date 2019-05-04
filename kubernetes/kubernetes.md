# Kubernetes Udacity Course Notes

## Kubernetes

### Setting up Kubernetes for this course

* Use project directory
```
cd $GOPATH/src/github.com/udacity/ud615/kubernetes
```
Note: At any time you can clean up by running the cleanup.sh script

* Provision a Kubernetes Cluster with GKE using gcloud
To complete the work in this course you going to need some tools. Kubernetes can be configured with many options and add-ons, but can be time consuming to bootstrap from the ground up. In this section you will bootstrap Kubernetes using Google Container Engine (GKE).

GKE is a hosted Kubernetes by Google. GKE clusters can be customized and supports different machine types, number of nodes, and network settings.

Use the following command to create your cluster for use for the rest of this course.
```
# (Optional) gcloud config set compute/zone us-central1-c
gcloud container clusters create k0 
```


### Kubernetes Intro Demo

* Launch a single instance:
```
kubectl run nginx --image=nginx:1.10.0
```

* Get pods
```
kubectl get pods
```

* Expose nginx
```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

* List services
```
kubectl get services
```

* Kubernetes cheat sheet
We just went over a lot and we know you’re probably a little overwhelmed. Fear not! We’ll be going over each of these concepts, over the next two lessons. And you can always come back to this demo -- if you need to watch it again.

To help out, here’s a Kubernetes command cheat sheet. http://kubernetes.io/docs/user-guide/kubectl-cheatsheet/

### Creating pods

* Explore config file
```
cat pods/monolith.yaml
```

* Create the monolith pod
```
kubectl create -f pods/monolith.yaml
```

* Examine pods
```
kubectl get pods
```
It may take a few seconds before the monolith pod is up and running as the monolith container image needs to be pulled from the Docker Hub before we can run it.

Use the kubectl describe command to get more information about the monolith pod.
```
kubectl describe pods monolith
```

### Interacting with Pods

* Cloud shell 1: set up port-forwarding
```
kubectl port-forward monolith 10080:80
```

* Open new Cloud Shell session 2
```
curl http://127.0.0.1:10080
curl http://127.0.0.1:10080/secure
```

* Cloud shell 2 - log in (password: password)
```
curl -u user http://127.0.0.1:10080/login
curl -H "Authorization: Bearer <token>" http://127.0.0.1:10080/secure
```

* View logs
```
kubectl logs monolith
kubectl logs -f monolith
```

* In Cloud Shell 3
```
curl http://127.0.0.1:10080
```
* In Cloud Shell 2

    * Exit log watching (Ctrl-C)
    * You can use the kubectl exec command to run an interactive shell inside the monolith Pod. This can come in handy when you want to troubleshoot from within a container:
```
kubectl exec monolith --stdin --tty -c monolith /bin/sh
```

For example, once we have a shell into the monolith container we can test external connectivity using the ping command.
```
ping -c 3 google.com
```

When you’re done with the interactive shell be sure to logout.
```
exit
```


## Creating secrets

### Commands from video
```
ls tls
```
The cert.pem and key.pem files will be used to secure traffic on the monolith server and the ca.pem will be used by HTTP clients as the CA to trust. Since the certs being used by the monolith server where signed by the CA represented by ca.pem, HTTP clients that trust ca.pem will be able to validate the SSL connection to the monolith server.

### Use kubectl
to create the tls-certs secret from the TLS certificates stored under the tls directory:

```
kubectl create secret generic tls-certs --from-file=tls/
```

kubectl will create a key for each file in the tls directory under the tls-certs secret bucket. Use the kubectl describe command to verify that:

```
kubectl describe secrets tls-certs
```

Next we need to create a configmap entry for the proxy.conf nginx configuration file using the kubectl create configmap command:
```
kubectl create configmap nginx-proxy-conf --from-file=nginx/proxy.conf
```

Use the kubectl describe configmap command to get more details about the nginx-proxy-conf configmap entry:

```
kubectl describe configmap nginx-proxy-conf
```


## Accessing a Secure HTTPS Endpoint

### Commands from video
```
cat pods/secure-monolith.yaml
```

### Create the secure-monolith Pod using kubectl.
```
kubectl create -f pods/secure-monolith.yaml
kubectl get pods secure-monolith

kubectl port-forward secure-monolith 10443:443

curl --cacert tls/ca.pem https://127.0.0.1:10443

kubectl logs -c nginx secure-monolith

```