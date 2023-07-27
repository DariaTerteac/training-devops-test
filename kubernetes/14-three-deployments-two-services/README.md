## Task
Create 3 deployments with nginxdemos/hello and expose them using 2 services.
1st service (accessible at port 8092) - to provide load balancing for the 1st and 2nd deployments, and second one (accessible at port 8093) - for the 2nd and 3rd deployments. 

## How to run

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/kubernetes/14-three-deployments-two-services/manifests
```

- Create a cluster

```
k3d cluster create demo-cluster
```

- Create and apply namespace

```
kubectl create namespace demo

kubectl ns
```

- Apply manifests in such order

```
kubectl apply -f ./service1.yaml

kubectl apply -f ./service2.yaml

kubectl apply -f ./manifest1.yaml

kubectl apply -f ./manifest2.yaml

kubectl apply -f ./manifest3.yaml
```

- In OpenLens wait until all pods will be up

- Forward the host port 8092 to the port 8092, which is listening by the service
```
k3d cluster edit demo-cluster --port-add 8092:8092@loadbalancer
```
- Forward the host port 8093 to the port 8093, which is listening by the service
```
k3d cluster edit demo-cluster --port-add 8093:8093@loadbalancer
```
- Check newly created services:
```
w3m http://127.0.0.1:8092
w3m http://127.0.0.1:8093
```

Output: the nginx html file is displayed. If you execute one of the commands above several times, you might notice that `Server address` and `names` are changing, proving load balancing.

Please clean up:

```
k3d cluster delete demo-cluster
```

## References
1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Kubernetes](https://kubernetes.io/)
4. [Kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
5. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
6. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)