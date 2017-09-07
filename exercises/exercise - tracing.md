# Tracing

Let's run a complex system. To define your system you can use `docmock` and a delay pod.


# How to define services

Docmock has the ability to call a series of dependencies defined as envvars. The following snippet creates a pod that calls 3 kubernetes services: web, srv01, srv02 and then it resturns a json document.

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    run: docmock
  name: docmock
spec:
  replicas: 1
  selector:
    matchLabels:
      run: docmock
  template:
    metadata:
      labels:
        run: docmock
    spec:
      containers:
      - image: ipedrazas/docmock
        name: docmock
        ports:
        - containerPort: 5000
        env:
        - name: ENDPOINT
          value: docmock
        - name: BJSON
          value: ewogICJuYW1lIjogIkl2YW4iLAogICJBZ2UiOiA0Mgp9Cg==
        - name: DEPENDENCIES
          value: "web,srv01,srv02"
        - name: DEBUG
          value: "true"
```

This will allow us to quickly define and model a system as complex as we need to. Ideally you want to create between 5 and 10 services, create dependecy calls between them and verify the tracing using zipkin.

1.- Define 5 services, make at least 3 dependency jumps (this is a service that calls a service that calls a service). Verify the traces of that system.

2.- Define a circular dependency and verify the traces.

3.- Define 20 services with complex dependency tree. Investigate the traces.

4.- Add route-rules that create faults. Investigate the traces.



