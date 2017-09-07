# Routing

Deploy two versions of the `simple` microservice, create a service that acces both versions.

```
mkdir chart
helm create chart/simple 
helm upgrade --install v1 ./chart/simple --set image.repository=ipedrazas/simple,image.tag=v1,service.externalPort=5000,service.internalPort=5000
helm upgrade --install v2 ./chart/simple --set image.repository=ipedrazas/simple,image.tag=v2,service.externalPort=5000,service.internalPort=5000

```

Explain what the following route-rule does:

```
type: route-rule
name: version1
spec:
  destination: simple.default.svc.cluster.local
  precedence: 1
  route:
  - tags:
      release: v1
```

Modify the tag to release v2, verify that all the requests go to the right pod.

Create the following rule:

```
type: route-rule
name: version2
spec:
  destination: simple.default.svc.cluster.local
  match:
    httpHeaders:
      simple:
        exact: very
  precedence: 2
  route:
  - tags:
      release: v1
```

Test it. Does it work? Should it work? flip your table and grab a beer if not. Buy a beer to your instructor: the night is dark and full of horrors.


Create the following resources:

```
kubectl apply -f <(istioctl kube-inject -f samples/apps/httpbin/httpbin.yaml)
```

And now, create an ingress rule:

```
cat <<EOF | kubectl create -f -
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: simple-ingress
  annotations:
    kubernetes.io/ingress.class: istio
spec:
  rules:
  - http:
      paths:
      - path: /headers
        backend:
          serviceName: httpbin
          servicePort: 8000
      - path: /delay/.*
        backend:
          serviceName: httpbin
          servicePort: 8000
EOF
```

Test the headers endpoint. Verify that you can trace the request ID:

* Check the logs in the ingress controller. Find the `X-Request-Id` in the logs of the pod. 

Now, let's test the delay endpoint:

```
time curl -o /dev/null -s -w "%{http_code}\n" http://$INGRESS_URL/delay/5
```

Click here, the result will surprise you! or not, anyway, let's do it a bit more interesting. let's add a timeout. Folks say that life is better with {chocolate | peanut butter | all the above}, but that's not true. Life is better with proper timeouts! For example, timeouts in the supermarket queue... Mention at leats 3 timeouts that do not exist but they should be mandatory. Start an online campaign to make it happen. DO IT!!

Then, create the following route-rule:

```
cat <<EOF | istioctl create
type: route-rule
name: httpbin-3s-rule
spec:
  destination: httpbin.default.svc.cluster.local
  http_req_timeout:
    simple_timeout:
      timeout: 3s
EOF
```

Repeat the same command than earlier:

```
time curl -o /dev/null -s -w "%{http_code}\n" http://$INGRESS_URL/delay/5
```

Talk with the person near you about the awesomic of this result.

Ok, let's test it with the bookinfo app (sigh)

* https://istio.io/docs/tasks/request-routing.html
* https://istio.io/docs/tasks/rate-limiting.html
