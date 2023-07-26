## Task

Run [testcontainers-node](https://github.com/testcontainers/testcontainers-node), [testcontainers-java](https://github.com/testcontainers/testcontainers-java) and [testcontainers-python](https://github.com/testcontainers/testcontainers-python) in k3d using [dockerize](https://github.com/powerman/dockerize) and make reports after running all three tests accessible to the host machine user after the container is stopped.

## Prerequisites

- Follow steps 1-3 from [README.md](../../../README.md)

- Copy necessary submodules

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-node $HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/src
```

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-java $HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/src
```

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-python $HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/src
```

- Create folder `report` in `$HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows`

```
mkdir $HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/report
```

- Navigate to the necessary folder

```
$HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/src
```

## How to run

Create cluster with volume:

```
k3d cluster create demo-cluster \
--registry-create demo-registry:12345 \
--volume $HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/report:/report
```

- Build, tag and push `12-testcontainers-node-argo:0.1` image

```
docker build -t 12-testcontainers-node-argo:0.1 \
--build-arg USER_ID=$(id -u) \
--build-arg GROUP_ID=$(id -g) -f Dockerfile-node .
```

```
docker tag 12-testcontainers-node-argo:0.1 localhost:12345/12-testcontainers-node-argo:0.1
```

```
docker push localhost:12345/12-testcontainers-node-argo:0.1
```

- Build, tag and push `12-testcontainers-java-argo:0.1` image

```
docker build -t 12-testcontainers-java-argo:0.1 \
--build-arg USER_ID=$(id -u) \
--build-arg GROUP_ID=$(id -g) -f Dockerfile-java .
```

```
docker tag 12-testcontainers-java-argo:0.1 localhost:12345/12-testcontainers-java-argo:0.1
```

```
docker push localhost:12345/12-testcontainers-java-argo:0.1
```

- Build, tag and push `12-testcontainers-python-argo:0.1` image

```
docker build -t 12-testcontainers-python-argo:0.1 \
--build-arg USER_ID=$(id -u) \
--build-arg GROUP_ID=$(id -g) -f Dockerfile-python .
```

```
docker tag 12-testcontainers-python-argo:0.1 localhost:12345/12-testcontainers-python-argo:0.1
```

```
docker push localhost:12345/12-testcontainers-python-argo:0.1
```

- Create namespace, switch to it and apply argo workflows

```
kubectl create namespace argo
```

```
kubectl config set-context --current --namespace 'argo'
```

```
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.4.6/install.yaml
```

- Change the directory to apply the manifest

```
$HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/manifests
```

```
kubectl apply -f ./manifest.yaml
```

- Change directory to `$HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/report` folder on the host to check the reports.

```
$HOME/training-devops-solutions/kubernetes/12-three-tests-in-argo-workflows/report
```

```
ls
```

- Please execute the cleanup: 

```
k3d cluster delete demo-cluster
```

```
docker rmi -f $(docker images -q)
```
