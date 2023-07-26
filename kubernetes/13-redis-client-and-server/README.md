## Task

Run redis server & client in k3d. Check if the database file is accessible to the host user after the container stops.

## Prerequisites

Follow prerequisites steps 1-3 from [README.md](../../README.md)

## How to run

- Create a `data`folder for persisting data

```
mkdir $HOME/training-devops-solutions/kubernetes/13-redis-client-and-server/data
```

- Create new cluster with volume

```
k3d cluster create demo-cluster \
--registry-create demo-registry:12345 \
--volume $HOME/training-devops-solutions/kubernetes/13-redis-client-and-server/data:/data
```

- Create namespace called `demo`

```
kubectl create namespace demo
```

- Switch to it

```
kubectl config set-context --current --namespace 'demo'
```

- Navigate to the necessary folder

```
$HOME/training-devops-solutions/kubernetes/13-redis-client-and-server/src
```

- Build, tag and push the image

```
docker build -t 13-redis:0.1 --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) -f Dockerfile .
```

```
docker tag 13-redis:0.1 localhost:12345/13-redis:0.1
```

```
docker push localhost:12345/13-redis:0.1
```

- Navigate to the necessary folder and apply manifest

```
$HOME/training-devops-solutions/kubernetes/13-redis-client-and-server/manifests
```

```
kubectl apply -f ./manifest.yaml
```

- In OpenLens enter in redis-client container, enter the shell and execute

```
redis-cli
```

```
set key "hello redis"
```

```
get key
```

```
save
```

- On your host navigate to `data` folder

```
$HOME/training-devops-solutions/kubernetes/13-redis-client-and-server/data
```

- Verify if the file is present and has expected user and group owner

```
ls -lha
```

- Please execute the cleanup:

```
k3d cluster delete demo-cluster
```

```
docker rmi -f $(docker images -q)
```
