## Task
Run in k3d cluster pod with 2 containers from ubuntu image. The first container writes a string "content of test file" in test1.txt file located on the volume. The second container reads this file and prints file content in the console.
 
## How to run
- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/kubernetes/05-interconnect-containers-via-volume/manifests
```

- Create cluster with registry
```
k3d cluster create demo-cluster --registry-create demo-registry:12345
```
- Create namespace and choose it
```
kubectl create namespace demo
kubectl ns *choose demo*
```

- Apply manifest
```
kubectl apply -f ./interconnection-via-volume.yaml
```

1. Connect to the `demo-cluster` in OpenLens.
2. Go to the `Pods` list and select `demo` namespace.
3. Find `reader-container` and open it's logs. The line `content of test file` should be displayed.

Please execute [clean up]():
```
kubectl delete -f ./interconnection-via-volume.yaml
k3d cluster delete demo-cluster
```
## References
1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Kubernetes](https://kubernetes.io/)
4. [Kubernetes pod](https://kubernetes.io/docs/concepts/workloads/pods/)
5. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
6. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)
