## Task
In k3d cluster using deployment run 4 pods with a container from nginxdemos/hello image. Provide access to the nginx web pages from outside of cluster using service and show load balancing.

## How to run

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/kubernetes/04-deployment-with-4-pods/manifests
```
- Create cluster with registry
```
k3d cluster create demo-cluster --registry-create demo-registry:12345
```
- Create namespace
```
kubectl create namespace demo
```
- Choose previously created namespace
```
kubectl ns *choose demo*
```

When working with Kubernetes, it is generally recommended to apply the Service before the Deployment. The reason for this is that a Service is responsible for providing a stable network endpoint for accessing the deployed application, and the Deployment manages the lifecycle of the application's Pods.

Firstly, apply the service
```
kubectl apply -f ./service-4-pods.yaml
```
Then, apply the deployment

```
kubectl apply -f ./deployment-with-4-pods.yaml
```

Wait until each pod will be up in OpenLens. Then execute: 

```
k3d cluster edit demo-cluster --port-add 4568:4567@loadbalancer
```

```
curl http://127.0.0.1:4568
```

After each task please execute [clean up]().
```
kubectl delete namespace demo
k3d cluster delete demo-cluster
```
