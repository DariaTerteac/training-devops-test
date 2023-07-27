## Task
Run in k3d cluster pod with 2 containers: Docker server and Docker client. From Docker client launch nginxdemos/hello on Docker Server. Provide access to nginxdemos/hello web page from outside of the cluster using service. 
Please use images:
docker:23.0.1-dind (Docker server)
docker:23.0.1-cli-alpine3.17 (Docker client)

## How to run

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder
```
cd $HOME/training-devops-solutions/kubernetes/07-dind-client-and-server/manifests
```
- Create cluster with registry
```
k3d cluster create demo-cluster --registry-create demo-registry:12345
```
- Create namespace and switch to it
```
kubectl create namespace demo
kubectl ns *choose demo*
```

When working with Kubernetes, it is generally [recommended](https://kubernetes.io/docs/concepts/configuration/overview) to apply the Service before the Deployment. The reason for this is that a Service is responsible for providing a stable network endpoint for accessing the deployed application, and the Deployment manages the lifecycle of the application's Pods.

- Apply the service
```
kubectl apply -f ./dind-service.yaml
```

- Apply the deployment
```
kubectl apply -f ./dind-client-and-server.yaml
```

1. Connect to the demo-cluster in OpenLens.
2. Go to the Pods list and select demo namespace.
3. Wait until both containers will be running and then execute:

```
k3d cluster edit demo-cluster --port-add 8081:8080@loadbalancer
curl http://127.0.0.1:8081 
```
As the result nginx web page will be displayed

After each task please execute cleanup.
```
kubectl delete -f ./dind-service.yaml
kubectl delete -f ./dind-client-and-server.yaml
k3d cluster delete demo-cluster
```

## References
1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)
4. [Kubernetes](https://kubernetes.io/)
5. [Kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
6. [Kubernetes service](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)
7. [Services](https://kubernetes.io/docs/concepts/configuration/overview/#services)
8. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
9. [K3d cluster edit](https://k3d.io/v5.2.1/usage/commands/k3d_cluster_edit/)
10. [Load Balancing](https://www.nginx.com/resources/glossary/load-balancing/)
