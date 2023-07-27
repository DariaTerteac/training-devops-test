## Task
Run optimized version of HelloWorld.java in k3d.
## How to run

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/kubernetes/01-hello-world-java-in-k3d/src
```

```
k3d cluster create demo-cluster --registry-create demo-registry:12345
```

```
kubectl create namespace demo
```

```
kubectl ns *choose demo*
```
[Build](https://docs.docker.com/engine/reference/commandline/build/) the image
```
docker build -t 01-hello-world-java-in-k3d:0.1 -f ./Dockerfile .
```

```
docker tag 01-hello-world-java-in-k3d:0.1 localhost:12345/01-hello-world-java-in-k3d:0.1
```

```
docker push localhost:12345/01-hello-world-java-in-k3d:0.1
```

```
cd ../manifests
```

```
kubectl apply -f ./hello-world-java.yaml
```

1. Connect to the `demo-cluster` in OpenLens.
2. Go to the `Pods` list and select `demo` namespace.
3. Watch container's logs, the line `Hello world!` should be printed.

Please execute clean up:

```
kubectl delete -f ./hello-world-java.yaml
k3d cluster delete demo-cluster
docker rmi -f 01-hello-world-java-in-k3d:0.1
docker rmi -f localhost:12345/01-hello-world-java-in-k3d:0.1
docker system prune
```

## References
1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Kubernetes](https://kubernetes.io/)
4. [Kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
5. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
6. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)