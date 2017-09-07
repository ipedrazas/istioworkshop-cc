# Exercise 01 - Deploying a REST api

In this exercise we're going to deploy a simple REST API that return a json document.

Deploy the following resource:

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
        - name: DEBUG
          value: "true"
```

Now create an internal service and an Ingress rule to access this application.