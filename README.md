# Deploy-a-Full-Stack-Web-App-to-Azure-Kubernetes-Service-with-Docker

What we will cover:

Azure Kubernetes Service

NGINX Ingress Controller

Docker Container

Azure CLI

Kubectl, Helm

React frontend, Flask backend


Overview

We will create two Kubernetes deployments, one for the React frontend and the other for the Flask API. Two Kubernetes services will also be created for us to access the deployed application.

After having both frontend and backend running on Kubernetes cluster, we create an ingress resource to route traffic to each application. By using an ingress controller and ingress rules, a single IP address can be used to route traffic to multiple services in a Kubernetes cluster.

![image](https://user-images.githubusercontent.com/58148717/106949611-cb11eb00-66f2-11eb-8b05-521164b6cb38.png)

Create AKS Cluster

https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough#delete-the-cluster

We use kubectl to manage the Kubernetes cluster. Run the command below in Azure CLI 

az aks get-credentials --resource-group myGroup --name myCluster

Create an NGINX Ingress Controller

# Create a K8s namespace for the ingress resources

kubectl create namespace ingress-basic

# Add the ingress-nginx repository 

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx  

# Use Helm to deploy an NGINX ingress controller 

helm install nginx-ingress ingress-nginx/ingress-nginx \

     --namespace ingress-basic \
     
     --set controller.replicaCount=2 \
     
     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
     
     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
     
     
Run the Application

we create this file in Azure CLI by code App.yaml

Run the frontend and backend in the namespace we created using kubectl apply

kubectl apply -f Prod.yaml --namespace ingress-basic

Create an Ingress Route

Both the frontend and backend are now running on the Kubernetes Cluster. 

Now we create an ingress resource to configure the rules that route traffic to our website or API. 

This resource can be created with a similar method by code ingress.yaml

Create the ingress resource

kubectl apply -f ingress.yaml

The Kubernetes load balancer service is created for the NGINX ingress controller, we can access our app with the assigned dynamic public IP address.















