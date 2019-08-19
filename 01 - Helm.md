# Playbook Part 1 : Helm 2

## Basics of package management

Every operating sytem out there has some forms of package manager, such as Yum for Red Hat, Apt for Debian, there's Home brew for OS X and Chocolatey for Windows. The basics of package management are like the following : 

<img src="screenshots/Package management.PNG" alt="package management" width="600px"/>

## Basics of Helm

Helm in Kubernetes like the following diagram : 

<img src="screenshots/Helm architecture.PNG" alt="package management" width="600px"/>

Basically, Helm has two parts, the helm client and tiller server, they're not necessarily to be in the same cluster, the client part can be in a remote server  :

- The **Helm Client** interacts with the tiller server, create and manages charts, packages charts into archives, interact with chart repositories, installs and uninstalls charts, manage chart release cycle.

- The **Tiller Server** listens for requests from the helm client, interacts with the Kubernetes API, manage releases, combien a chart and a config into a release, installs charts and track the release, upgrades and uninstalls charts. You can check the tiller in Kubernetes by using the following : 

   kubectl get pods --all-namespaces | grep tiller

## Install Helm 

The best teacher is at Github ( like always ) : https://github.com/helm/helm/blob/master/docs/install.md

## In action :

Check the helm home space : 

    helm home 

The output should be something like this : 

    /home/cloud_user

Check the version of Helm:

    helm version

If you're using Helm 2, the output will cover the version information for both client and server. 

First start to use Helm, you have to initialise using the following command :

    helm init 

  Upgrade if Tiller is already installed, you can use the following : 
  
   helm init --upgrade  

To query the helm charts has been deployed : 

    helm ls

To query helm chart are available : 

    helm list

To install the Helm charts 

    helm install stable/melonchart

You can also deploy a specific version : 

    helm install stable/melonchart --version 0.0.7

After entering this command, the output of the command will show how it deploy in Kubernetes :

Usually in **default** namespace, related resource including : Secret / ConfigMap / PVC / Service / Deployment / StatefulSet ( MariaDB )

Check the depedencies by using the following : 

     helm dep list

Update the dependencies :

     helm dep update

Dependencies are in requirements.yaml

Delete a helm chart has been deployed using the following : 

    helm delete melonchart