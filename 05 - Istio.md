# Playbook Part 5 : Istio

Istio is an open-source service mesh that provides a key set of functionality across the microservices in a Kubernetes cluster. These features include traffic management, service identity and security, policy enforcement, and observability.

## Play 0 : Set up Azure Kubernetes Cluster

Create Azure Kubernetes 

     az group create --name melonk8s-istio-rg --location northeurope

Get the available latest K8s version in current region ( use northeurope as example ) : 

```shell
version=$(az aks get-versions -l northeurope --query 'orchestrators[-1].orchestratorVersion' -o tsv)
```

Attention: Make sure your Azure CLI is the latest version, try to double check with Azure documentation or compare to Cloud Shell by using az --version command. Here we're using Kubernetes 1.15.3

Create AKS cluster Gen2 using cluster autoscaler :

```shell
 az aks create --resource-group melonk8s-istio-rg \
    --name melonistiok8s \
    --location northeurope \
    --kubernetes-version $version \     ## version : 1.15.3
    --generate-ssh-keys \
    --enable-vmss \
    --enable-cluster-autoscaler \
    --min-count 1 \
    --max-count 3
```
Connect to the cluster using the following : 

   az aks get-credentials --resource-group melonk8s-istio-rg --name melonistiok8s

## Play 1 : Set up Istio in your local machine 

curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.3.1 sh -


export PATH="$PATH:/home/melony/istio-1.3.1/bin"


Begin the Istio pre-installation verification check by running:
      
      istioctl verify-install

<img src="screenshots/Istio Verify-Install.PNG" alt="package management" width="800px"/>   


## Play 3 : Set up Istio to your Kubernetes cluster

After download the binaries, you can use helm to install istio to your cluster, by using the following command : 

```shell
 helm repo add istio.io https://storage.googleapis.com/istio-release/releases/1.3.1/charts/

 kubectl create namespace istio-system

 helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -
```s
 

### Official doc website :

You can expect more details about Linkerd by visiting the following website : https://istio.io/docs/