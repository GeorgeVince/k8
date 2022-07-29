Helpful K8s notes

use kind for local cluster
use https://kind.sigs.k8s.io/docs/user/local-registry/ for local docker registry

Build image

```
docker build -t "localhost:5001/hello-world:1.0" .
docker push "localhost:5001/hello-world:1.0" 
```

Create deployment
```
kubectl create -f example.yaml 
```

Expose deployment via a Service
```
kubectl expose deployment hello-world --type=LoadBalancer --port=80
```

View service created
```
kubectl get services
```

Since we're working locally (Not using a cloud ALB) we need to portfoward to see this

```
kubectl port-forward service/hello-world 9000:80
```