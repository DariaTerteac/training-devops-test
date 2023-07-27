## Task
How to control the sequence of containers running (writer-reader example, 4 ways to make it)

## How to run
- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/kubernetes/06-control-the-sequence-of-running-containers/src
```
- Create cluster with registry

```
k3d cluster create demo-cluster --registry-create demo-registry:12345
```
- Create namespace and switch to it
- 
```
kubectl create namespace demo
kubectl ns *choose demo*
```

- [Build](https://docs.docker.com/engine/reference/commandline/build/), tag and push the image

```
docker build -t 06-writer-reader-with-dockerize:0.1 -f ./Dockerfile .

docker tag 06-writer-reader-with-dockerize:0.1 localhost:12345/06-writer-reader-with-dockerize:0.1

docker push localhost:12345/06-writer-reader-with-dockerize:0.1
```

- Navigate to the necessary folder to apply manifests

```
cd $HOME/training-devops-solutions/kubernetes/06-control-the-sequence-of-running-containers/manifests
```

- Apply manifests in a row

```
kubectl apply -f ./01-initcontainers.yaml
kubectl apply -f ./02-sleep.yaml
kubectl apply -f ./03-job.yaml
kubectl apply -f ./04-dockerize.yaml
```

1. Connect to the `demo-cluster` in OpenLens.
2. Go to the `Pods` list and select `demo` namespace.
3. Reader containers should display the line `content of test file`.

After each task please execute [clean up]().
```
docker rmi localhost:12345/06-writer-reader-with-dockerize:0.1
docker rmi 06-writer-reader-with-dockerize:0.1
kubectl delete namespace demo
k3d cluster delete demo-cluster
```
## References
1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Kubernetes](https://kubernetes.io/)
4. [Kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
5. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
6. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)
