## Task

Parametrize previously created [helm chart](../15-helm-chart) with springboot application, postgresql and pgadmin. 
Demonstrate that the helm chart deletion and re-installation does not cause data loss.

A Helm chart must contain the following components:

- Chart.yaml: This file contains metadata about the chart, such as the name, version, description, etc.

- Values.yaml: This file allows users to customize the deployment by providing configurable parameters.

- Templates: The templates directory contains YAML or text templates that define the Kubernetes resources required to deploy the application. These templates can include deployment configurations, services, config maps, secrets, and other Kubernetes objects.

## How to run

1. Follow prerequisites steps 1-3 from [README.md](../../README.md)

2. Create a cluster with registry
```
k3d cluster create demo-cluster --registry-create demo-registry:12345
```

3. Create `src` folder in `$HOME/training-devops-solutions/kubernetes/16-parametrized-helm-chart` and copy necessary submodule

```
cp -r $HOME/training-devops-solutions/submodules/springboot-api $HOME/training-devops-solutions/kubernetes/16-parametrized-helm-chart/src
```

4. Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/kubernetes/16-parametrized-helm-chart/src/springboot-api/api
```

5. Build, tag and push the image

```
docker build -t 16-springboot-api:0.1 -f ./docker/Dockerfile.prod .
docker tag 16-springboot-api:0.1 localhost:12345/16-springboot-api:0.1
docker push localhost:12345/16-springboot-api:0.1
```

6. Navigate to the necessary folder
```
cd $HOME/training-devops-solutions/kubernetes/16-parametrized-helm-chart
```

7. Install helm chart demo-chart
```
helm upgrade --install --cleanup-on-fail demo-chart ./demo-chart --namespace demo-chart --create-namespace --set service.type=LoadBalancer --wait
```

8. Forward the host ports to the ports, which are listening by the services
```
k3d cluster edit demo-cluster --port-add 18081:8081@loadbalancer
k3d cluster edit demo-cluster --port-add 18080:8080@loadbalancer
```

9. In browser open pgadmin and springboot app
```
http://localhost:18081
http://localhost:18080
```
10. On [springboot app page](http://localhost:18080) click on *Sign up* and enter any test values.

11. On [pgadmin page](http://localhost:18081) enter values from [values.yaml](./demo-chart/values.yaml) and log in.

12. After log in click on *Add New Server* give a `demo` name and fill in *Connection* tab with values from [secret](./demo-chart/templates/secret.yaml) and [database-service](./demo-chart/templates/db-service.yaml)

13. To check newly created user in database choose Servers -> demo -> Databases -> demodb -> Schemas -> Tables -> user(right click) -> View/Edit Data -> All rows

14. To demonstrate that the helm chart deletion and re-installation does not cause data loss execute
```
helm uninstall demo-chart -n demo-chart  

helm upgrade --install --cleanup-on-fail demo-chart ./demo-chart --namespace demo-chart --create-namespace --set service.type=LoadBalancer --wait 
```

15. Wait until each component will be up, refresh the pgadmin page, log in again and execute step 13. 

16. Please clean up
```
helm uninstall demo-chart -n demo-chart 
k3d cluster delete demo-cluster
docker rmi 16-springboot-api:0.1
docker rmi localhost:12345/16-springboot-api:0.1
docker system prune
```

## References
1. [Lens](https://k8slens.dev/)
2. [Lens Desktop Core ("OpenLens")](https://github.com/lensapp/lens)
3. [Kubernetes](https://kubernetes.io/)
4. [Kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
5. [Command line tool (kubectl)](https://kubernetes.io/docs/reference/kubectl/)
6. [Useful k3d and kubectl commands](https://ramigs.dev/blog/useful-k3d-and-kubectl-commands/)
7. [Charts](https://helm.sh/docs/topics/charts/)
8. [What is a Helm Chart? A Tutorial for Kubernetes Beginners](https://www.freecodecamp.org/news/what-is-a-helm-chart-tutorial-for-kubernetes-beginners/)
9. [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)
10. [Values](https://helm.sh/docs/chart_template_guide/values_files/)
