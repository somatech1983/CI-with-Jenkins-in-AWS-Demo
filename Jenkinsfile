---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gcpssmyapp
spec:
  replicas: 2
  selector:
   matchLabels:
    app: gcpssmyapp
  template:
    metadata:
      labels:
        app: gcpssmyapp
    spec:
      containers:
      - name: gcpssmyapp
        image: somaam/firstimage:tagversion
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: gcpssmyapp
  labels:
    app: gcpssmyapp
spec:
  type: LoadBalancer
  selector:
    app: gcpssmyapp
  ports:
  - protocol: TCP
    port: 6655
    targetPort: 8080
