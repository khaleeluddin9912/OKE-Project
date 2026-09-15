# OCI Native Ingress Controller

The OCI Native Ingress Controller creates and manages an OCI Load Balancer for Kubernetes `Ingress` resources.

## 1. Clone the OCI Native Ingress Controller repository

```powershell
git clone https://github.com/oracle/oci-native-ingress-controller
```

Move into the directory:

```powershell
cd oci-native-ingress-controller
```

## 2. Install cert-manager

```powershell
kubectl apply -f https://github.com/jetstack/cert-manager/releases/latest/download/cert-manager.yaml
```

Verify cert-manager:

```powershell
kubectl get pods -n cert-manager
```

Wait until the cert-manager pods are `Running`.

## 3. Configure the OCI Native Ingress Controller

Open:

```text
values.yaml
```

Set the OCI compartment ID used by the cluster:

```yaml
compartment_id: <YOUR_COMPARTMENT_OCID>
```

For this project, use the compartment where the OCI Load Balancer will be created.

## 4. Install the OCI Native Ingress Controller

From inside the `oci-native-ingress-controller` directory:

```powershell
helm install oci-native-ingress-controller helm/oci-native-ingress-controller
```

## 5. Verify the controller

```powershell
kubectl get pods -n native-ingress-controller-system --selector="app.kubernetes.io/name in (oci-native-ingress-controller)" -o wide
```

The controller pod should show:

```text
READY   STATUS
1/1     Running
```

## 6. Verify the namespace

```powershell
kubectl get pods -n native-ingress-controller-system
```

The OCI Native Ingress Controller is now installed and ready to manage Kubernetes Ingress resources.
