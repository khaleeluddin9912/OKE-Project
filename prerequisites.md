# Prerequisites

This project uses Oracle Kubernetes Engine (OKE) to deploy the Kubernetes 2048 application.

Before creating the OKE cluster, install and verify the required tools.

## 1. Install OCI CLI

Open **PowerShell as Administrator**.

Run:

```powershell
Set-ExecutionPolicy RemoteSigned
```

Download the OCI CLI installer:

```powershell
Invoke-WebRequest https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1 -OutFile install.ps1
```

Run the installer:

```powershell
.\install.ps1 -AcceptAllDefaults
```

After installation, close PowerShell and open a new PowerShell window.

Verify OCI CLI:

```powershell
oci --version
```

Expected output will show the installed OCI CLI version, for example:

```text
Oracle-PythonCLI/3.92.0 ...
```

---

## 2. Configure OCI CLI

Run:

```powershell
oci setup config
```

The setup asks for:

* User OCID
* Tenancy OCID
* Region
* API key information

Follow the prompts to create the OCI CLI configuration.

The default configuration file is:

```text
C:\Users\<USERNAME>\.oci\config
```

Verify the OCI configuration by running:

```powershell
oci iam region-subscription list
```

If the command returns your subscribed regions, the OCI CLI configuration is working.

---

## 3. Verify OCI CLI

Run:

```powershell
oci --version
```

The command must return the OCI CLI version.

---

## 4. Install kubectl

If `kubectl` is not already installed, install it using the Kubernetes installation method appropriate for your Windows system.

After installation, verify:

```powershell
kubectl --version
```

You can also verify the client version with:

```powershell
kubectl version --client
```

Expected output should show the installed `kubectl` client version.

---

## 5. Install Helm

Install Helm using Windows Package Manager:

```powershell
winget install Helm.Helm
```

After installation, close PowerShell and open a new PowerShell window.

Verify Helm:

```powershell
helm version
```

Expected output should show the installed Helm version.

---

## 6. Verify All Required Tools

Run these commands:

### OCI CLI

```powershell
oci --version
```

### kubectl

```powershell
kubectl --version
```

### Helm

```powershell
helm version
```

All three commands must work before continuing.

---

## 7. Verify OCI Authentication

Run:

```powershell
oci iam region-subscription list
```

If the command returns your OCI region subscription information, OCI CLI authentication is working.

For this project, the OCI region used is:

```text
ap-hyderabad-1
```

---

## 8. Verify the OCI Region

Run:

```powershell
oci iam region-subscription list --query "data[?regionName=='ap-hyderabad-1']"
```

The command should return the subscription information for:

```text
ap-hyderabad-1
```

---

## Prerequisites Completed

At this point the following tools should be available:

```text
OCI CLI    → Installed and authenticated
kubectl    → Installed
Helm       → Installed
Region     → ap-hyderabad-1
```

Continue to:

[`installing-oke.md`](./installing-oke.md)
