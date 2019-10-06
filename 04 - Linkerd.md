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


### Official doc website :
https://linkerd.io/