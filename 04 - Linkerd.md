# Playbook Part 4 : Linkerd

Linkerd is a service mesh solution for Kubernetes, which is also a  Cloud Native Computing Foundation (CNCF) project, running across an entire cluster to provide platform-wide telemetry,security, and reliability.It is designed to give service owners automatic observability, reliability, and runtime diagnostics for their service without requiring configuration or code changes. 

## Play 0 : Set up Linkerd on your local machine 
In this guide, we’ll walk you through how to install Linkerd into your Kubernetes cluster. Then we’ll deploy a sample application to show off what Linkerd can do.



Install the CLI by using the following : 
 
     curl -sL https://run.linkerd.io/install | sh

Next, add linkerd to your path with by using the following : 

     export PATH=$PATH:$HOME/.linkerd2/bin

Verify the CLI is installed and running correctly with:

     linkerd version


## Play 1 : Set up Azure Kubernetes Cluster

Create Azure Kubernetes 

     az group create --name melonk8s-linkerd-rg --location northeurope

Get the available latest K8s version in current region ( use northeurope as example ) : 

```shell
version=$(az aks get-versions -l northeurope --query 'orchestrators[-1].orchestratorVersion' -o tsv)
```

Attention: Make sure your Azure CLI is the latest version, try to double check with Azure documentation or compare to Cloud Shell by using az --version command. Here we're using Kubernetes 1.15.3

Create AKS cluster Gen2 using cluster autoscaler :

```shell
 az aks create --resource-group melonk8s-linkerd-rg \
    --name melonk8s \
    --location northeurope \
    --kubernetes-version $version \     ## version : 1.15.3
    --generate-ssh-keys \
    --enable-vmss \
    --enable-cluster-autoscaler \
    --min-count 1 \
    --max-count 3
```
Connect to the cluster using the following : 

   az aks get-credentials --resource-group melonk8s-linkerd-rg --name melonk8s

You can check you have output like the following : 
<img src="screenshots/Get credentials.PNG" alt="package management" width="600px"/>   

To check how many nodes in your cluster, you can use the following command : 

    kubectl get nodes


## Play 3 : Set up Linkerd to AKS

To check that your cluster is configured correctly and ready to install the control plane, you can run:

     linkerd check --pre

You'll get an output similar to the following : 

<img src="screenshots/Linkerd Prep check.PNG" alt="package management" width="600px"/>


To run Linerd onto AKS by using : 

     linkerd install | kubectl apply -f -

This command generates a Kubernetes manifest with all the necessary control plane resources. You can use k get all -n linkerd command to check all the resources created by linkerd.

You'll get an output similar to the following : 

<img src="screenshots/Linkerd Resources.PNG" alt="package management" width="600px"/>

Using the following to check if everything's ok :

    linkerd check

You'll get an output similar to the following : 

<img src="screenshots/Linkerd Install check.PNG" alt="package management" width="300px"/>   

To get the details of what kind of components you have installed with Linkerd, you can use the following command :

     kubectl -n linkerd get deploy

You'll have a clear view through the output of this command : 

<img src="screenshots/Linkerd Component.PNG" alt="package management" width="600px"/>   


## Play 4 : Up and running 

After checking the control plane installed and running, you can now view the Linkerd dashboard by running:

     linkerd dashboard &

You'll have a clear view through the output of this command : 

<img src="screenshots/Linkerd Dashboard.PNG" alt="package management" width="600px"/>   


Open your browser at http://127.0.0.1:50750/, you can explore the dashobard of Linkerd : 

<img src="screenshots/Linkerd Dashboard UI.PNG" alt="package management" width="800px"/>   


You can expect to see Graphana Dashboard at http://127.0.0.1:50750/grafana as well :

<img src="screenshots/Grafana Dashboard.PNG" alt="package management" width="800px"/>   

### Official doc website :

You can expect more details about Linkerd by visiting the following website : https://linkerd.io/
