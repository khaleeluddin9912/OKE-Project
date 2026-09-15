# Installing OKE

This project uses Oracle Kubernetes Engine (OKE) in the `ap-hyderabad-1` region.

## 1. Set the OCI values

```powershell
$REGION="ap-hyderabad-1"

$COMPARTMENT_OCID="ocid1.compartment.oc1..aa4zuibvhkesqldhnhozgszvomh3hxpwg7a6wf5ca"

$VCN_OCID="ocid1.vcn.oc1.ap-hyderabad-1.amaplnqlpyaboa3fybotqk4xoznorg7x3scy24fmstz3a"

$WORKER_SUBNET_OCID="ocid1.subnet.oc1.ap-hyderab.aaaatriwb35rbctmzptdslvijhdbs6vetbsqspgq"
```

## 2. Create the OKE cluster

```powershell
oci ce cluster create `
  --name test-cluster `
  --compartment-id $COMPARTMENT_OCID `
  --kubernetes-version v1.36.1 `
  --vcn-id $VCN_OCID `
  --region $REGION
```

Wait until the cluster becomes `ACTIVE`.

Verify:

```powershell
oci ce cluster get `
  --cluster-id ocid1.cluster.oc1.ap-hyderabad-1.a5dr7ps3nwdxx5xnmdg3cmfcbm37ocpr3a `
  --query "data.lifecycle-state"
```

Expected:

```text
"ACTIVE"
```

## 3. Create the node pool

The worker node pool used in this project:

* Node pool: `test-node-pool`
* Shape: `VM.Standard.E5.Flex`
* OCPUs: `2`
* Memory: `16 GB`
* Nodes: `1`
* Kubernetes: `v1.36.1`

```powershell
oci ce node-pool create `
  --cluster-id ocid1.cluster.oc1.ap-hyderabad-1.dr7ps3nwdxx5xnmdg3cmfcbm37ocpr3a `
  --compartment-id $COMPARTMENT_OCID `
  --name test-node-pool `
  --node-shape VM.Standard.E5.Flex `
  --kubernetes-version v1.36.1 `
  --subnet-ids "[`"$WORKER_SUBNET_OCID`"]" `
  --size 1 `
  --node-shape-config '{"memoryInGBs":16,"ocpus":2}' `
  --region $REGION
```

Wait until the node pool becomes `ACTIVE`.

## 4. Verify the node pool

```powershell
oci ce node-pool list `
  --compartment-id $COMPARTMENT_OCID `
  --cluster-id ocid1.cluster.oc1.ap-hyderabad-1.pcd2dkaql5dr7ps3nwdxx5xnmdg3cmfcbm37ocpr3a `
  --region $REGION
```

## 5. Create the Kubernetes kubeconfig

For this project, the working kubeconfig command uses the legacy Kubernetes endpoint.

```powershell
oci ce cluster create-kubeconfig `
  --cluster-id ocid1.cluster.oc1.ap-hyderabad-1.lpcd2dkaql5dr7ps3nwdxx5xnmdg3cmfcbm37ocpr3a `
  --file "$HOME\.kube\config" `
  --region ap-hyderabad-1 `
  --token-version 2.0.0 `
  --kube-endpoint LEGACY_KUBERNETES
```

## 6. Verify Kubernetes access

```powershell
kubectl get nodes
```

Expected result:

```text
NAME       STATUS   ROLES    AGE   VERSION
<node>     Ready    <none>   ...   v1.36.1
```

## 7. Verify the cluster

```powershell
kubectl get nodes -o wide
```

The OKE cluster and worker node are now ready for installing the OCI Native Ingress Controller and deploying the 2048 application.
