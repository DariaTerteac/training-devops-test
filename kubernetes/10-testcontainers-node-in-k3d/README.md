## Task

Run [testcontainers-node](https://github.com/testcontainers/testcontainers-node) in k3d and make reports after running the tests accessible to the host machine user after the container is stopped.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Copy necessary submodule to this task

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-node $HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/src
```

- Navigate to the necessary folder:

```
$HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/src
```

## How to run

- Create new folder called `report` in `$HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/`

```
mkdir $HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/report
```

- Create cluster with volume

```
k3d cluster create demo-cluster \
--registry-create demo-registry:12345 \
--volume $HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/report:/jest-test-report
```

- Create namespace and switch to it

```
kubectl create namespace demo
```
```

kubectl config set-context --current --namespace 'demo'
```

- Build, tag and push the image

```
docker build -t 10-tc-node-in-k3d:0.1 \
--build-arg USER_ID=$(id -u) \
--build-arg GROUP_ID=$(id -g) -f Dockerfile .
```

```
docker tag 10-tc-node-in-k3d:0.1 localhost:12345/10-tc-node-in-k3d:0.1
```

```
docker push localhost:12345/10-tc-node-in-k3d:0.1
```

- Change directory to apply the manifest

```
$HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/manifests
```

- Apply the manifest

```
kubectl apply -f ./manifest.yaml
```

1. Connect to the `demo-cluster` in OpenLens.
2. Go to the Pods list and select `demo` namespace.
3. Watch `testcontainers-node` container logs to see if the build is successful.

- Change the directory to `$HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/report` folder on the host to check the report (index.html).

```
$HOME/training-devops-solutions/kubernetes/10-testcontainers-node-in-k3d/report
```

```
ls
```

```
w3m index.html
```

- Please execute the cleanup:  

```
k3d cluster delete demo-cluster
```

```
docker rmi 10-tc-node-in-k3d:0.1
```

```
docker rmi localhost:12345/10-tc-node-in-k3d:0.1
```

## References

1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Kubernetes](https://kubernetes.io/)
4. [Kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
5. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
6. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)
