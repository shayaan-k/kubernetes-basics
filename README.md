# kubernetes-basics
Learning the basics of Kubernetes

## Starting up a minikube VM
minikube is an enviroment that acts as both the master and worker node. Useful for testing deployments.
To create a minikube session run: ```minikube start --vm-driver=hyperkit``` (or any other vm provisioner)

Kubectl is the command line tool used for K8. Comes as a dependency for minikube. Docker is also pre installed.
Running ```kubectl get nodes``` will show active nodes such as the one I just started.

Minikube for startup
kubectl for configuration.

Useful kubctl commands: 

kubectl get nodes -> Shows nodes in a cluster
kubectl get pods -> Running pods in the cluster
kubectl get services -> running services in the cluster

## Creating a deployment
In K8 we dont create pods, we create deployments. An abstration later over pods which communicated with master node API server.

```kubectl create deployment nginx-depl --image=nginx``` created a deployment called nginx-depl which uses the publicly available nginx image from docker.
```kubectl get deployment``` will list the deployments running in the cluster
```kubectl get pod``` shows the pod and the deployment it is running
```kubectl get replicaset``` shows the replicasets being run. ReplicaSets are the replicas of deployments that can be copied over when scaling up clusters.

```kubectl edit deployment nginx-depl``` allows me to edit the configuration file (which is automaticaly generated) that the deployment is built off of. config yaml file.
By editing the configuration, K8 will automatically end the old deployment and create a new one with the correct configuration.

```kubectl create deployment mongo-depl --image=mongo``` created deployment with mongo image
```kubectl get pod``` will now show both pods running.

```kubectl delete deployment mongo-depl``` deletes deployments.
deleting terminates all pods and deletes replicasets. Everything is handled on deployment level.

## Debugging deployments
```kubectl describe pod PODNAME``` will give me additional information about the pod
```kubectl logs PODNAME``` will show the current logs of that pod. Useful for debugging.
```kubectl get pod -o wide`` for additional pod info like IP
```kubectl exec -it PODNAME -- bin/bash``` will open an interactive terminal in that pod

## Creating Deployment file
Once we have our configuration file made (check nginx-deployment.yaml) we can run the following command to create it.
```kubectl apply -f nginx-deployment.yaml```
```kubectl get pod``` to show pod running
```kubectl get deployment``` to show deployment created

I can change something in my local configuration file and apply it again to recreate the deployment

I can delete deployments generated from a config file with
```kubectl delete -f nginx-deployment.yaml```

### Congifuration file syntax
Metadata; name, specification=configuration
> deployments have their own specs and services have their own
> status is another thing: K8 checks if the actual state = desited state and changes to ensure it is equal. This is the crux of the terminating a recreating of pods

status info comes from etcd (the cluster brain)

Template is a configuration in a configuration file.

Labels are key value pairs used to connect pods together.