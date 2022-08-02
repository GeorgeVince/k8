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

Loadbalancers
First we need a load balancer
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install hello-world ingress-nginx/ingress-nginx 
```

We have an example of path bath + host based routing in `example-ingress.yaml`
```
 kubectl apply -f example-ingress.yaml   
```

Then we need to modify hosts
```
127.0.0.1  hello.com
127.0.0.1  hello.shield.com
127.0.0.1  hello.world.com
```

Lets try the load balancer!
```
17:59:43 ~/dev/test/k8 main $ curl hello.com:8082/shield
{"Hello":"shield"}%
17:59:58 ~/dev/test/k8 main $ curl hello.com:8082/default
{"Hello":"World"}%
18:00:07 ~/dev/test/k8 main $ curl hello.world.com:8082
{"Hello":"World"}%
18:00:32 ~/dev/test/k8 main $ curl hello.shield.com:8082
{"Hello":"shield"}%
```