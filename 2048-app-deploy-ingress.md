# 2048 App

## 1. Create the namespace

```powershell
kubectl create namespace game-2048
```

## 2. Deploy the 2048 application

Create a file named:

```text
2048-app.yaml
```

Add:

```yaml
apiVersion: apps/v1

kind: Deployment
metadata:
  name: deployment-2048
  namespace: game-2048
spec:
  replicas: 5
  selector:
    matchLabels:
      app: app-2048
  template:
    metadata:
      labels:
        app: app-2048
    spec:
      containers:
        - name: app-2048
          image: public.ecr.aws/l6m2t8p7/docker-2048:latest
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: service-2048
  namespace: game-2048
spec:
  type: NodePort
  selector:
    app: app-2048
  ports:
    - port: 80
      targetPort: 80
```

Apply it:

```powershell
kubectl apply -f .\2048-app.yaml
```

## 3. Verify the application

```powershell
kubectl get pods -n game-2048
```

All 5 pods should be `Running`.

Check the service:

```powershell
kubectl get svc -n game-2048
```

The service should show a NodePort.

## 4. Create the OCI Ingress

Create:

```text
2048-ingress.yaml
```

Add:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: khaleel-2048-ingress
  namespace: game-2048
spec:
  ingressClassName: oci-native-ingress
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: service-2048
                port:
                  number: 80
```

Apply it:

```powershell
kubectl apply -f .\2048-ingress.yaml
```

## 5. Verify the Ingress

```powershell
kubectl get ingress -n game-2048
```

Wait until the `ADDRESS` column contains a public IP address.

Example:

```text
NAME                    CLASS                HOSTS   ADDRESS          PORTS
khaleel-2048-ingress   oci-native-ingress   *       140.245.244.32   80
```

## 6. Open the application

Copy the public IP shown in the `ADDRESS` column and open it in a browser:

```text
http://<INGRESS-PUBLIC-IP>
```

The 2048 game should be displayed.

## 7. Final verification

```powershell
kubectl get pods -n game-2048
kubectl get svc -n game-2048
kubectl get ingress -n game-2048
```

The 2048 application is now deployed on OKE and exposed through the OCI Native Ingress Controller.
