
# Deploying microservice applications in AKS using Azure DevOps

    Part1:   Manual Deployment (AzCLI + Docker Desktop + kubectl)  
    GitHub:  https://github.com/santosh-gh/k8s-01
    YouTube: https://youtu.be/zoJ7MMPVqFY

    Part2:   Automated Deployment (AzCLI + Docker + kubect + Azure Pipeline)
    GitHub:  https://github.com/santosh-gh/k8s-02
    YouTube: https://youtu.be/nnomaZVHg9I

    Part3:   Automated Infra Deployment (Bicep + Azure Pipeline)
    GitHub:  https://github.com/santosh-gh/k8s-03
    YouTube: https://www.youtube.com/watch?v=5PAdDPHn8F8

    Part4:   Manual Deployment (AzCLI + Docker Desktop + Helm charts + kubectl) 
    GitHub:  https://github.com/santosh-gh/k8s-04
    YouTube: https://www.youtube.com/watch?v=VAiR3sNavh0

    Part5:   Automated Deployment (AzCLI + Docker + Helm charts + kubectl + Azure Pipeline) 
    GitHub:  https://github.com/santosh-gh/k8s-04
    YouTube: https://www.youtube.com/watch?v=MnWe2KGRrxg&t=883s

    Part6:   Automated Deployment (AzCLI + Docker + Helm charts + kubectl + Azure Pipeline) 
             Dynamically update the image tag in values.yaml
    GitHub:  https://github.com/santosh-gh/k8s-06
    YouTube: https://www.youtube.com/watch?v=Nx0defm8T6g&t=11s

    Part7:   Automated Deployment (AzCLI + Docker + Helm charts + kubectl + Azure Pipeline)
             Store the helm chart in ACR
             Dynamically update the image tag in values.yaml
             Dynamically update the Chart version in Chart.yaml

    GitHub:  https://github.com/santosh-gh/k8s-07
    YouTube: https://www.youtube.com/watch?v=Y3RaxSZNTaU&t=1s

    Part8:   Automated Deployment (AzCLI + Docker + Helm charts + kubectl + Azure Pipeline)
             Store the helm chart in ACR
             Dynamically update the image tag in values.yaml
             Dynamically update the Chart version in Chart.yaml
             Deploy into multiple environments (dev, test, prod) with approval gates

    GitHub:  https://github.com/santosh-gh/k8s-08
    YouTube: https://www.youtube.com/watch?v=oNysAAGijGk&t=43s

    Part9:   Manual Deployment (AzCLI + Docker + kustomize + kubectl)          
             Deploy into multiple environments (dev, test, prod) through command line

    GitHub:  https://github.com/santosh-gh/k8s-09
    YouTube: https://www.youtube.com/watch?v=Jtz1KldOPAA&t=1s

    Part10:  Automated Deployment (AzCLI + Docker + kustomize + kubectl + Azure Pipeline)          
             Deploy into multiple environments (dev, test, prod) through automated pipeline

    GitHub:  https://github.com/santosh-gh/k8s-10
    YouTube: https://www.youtube.com/watch?v=VAiR3sNavh0

# Architesture

![Store Architesture](aks-store-architecture.png)

    # Store front: Web application for customers to view products and place orders.
    # Product service: Shows product information.
    # Order service: Places orders.
    # RabbitMQ: Message queue for an order queue.

# Directory Structure

![Directory Structure](image.png) 

# Steps

    1. Set up Azure DevOps
    2. Infra deployment: Azure Pipeline (AzureCLI@2)
    3. Build and push images to ACR: Azure Build and Push Pipeline (docker@2)
    4. App deployment: Azure Release (AzureCLI@2)
    5. Validate and Access the application
    6. Clean the Azure resources

# Set up Azure DevOps

    Create Organization, Project and Permissions

    Create Service Principal (app reg) for Azure DevOps to authenticate with Azure

    Create DevOps Service Connection to Azure (ARM) for Infrastructure Pipelines

# Setup and run Infra Pipeline

    infra-pipeline.yml

    Connect to cluster

        az login
        az account set --subscription=<subscriptionId>
        az account show

        RESOURCE_GROUP="rg-onlinestore-dev-uksouth-001"
        AKS_NAME="aks-onlinestore-dev-uksouth-001"

        az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_NAME

    Short name for kubectl

        alias k=kubectl

    Show all existing objects

        k get all

# Build and push images to ACR

    Create Azure ACR service connection
    Build and push images to ACR: Azure Pipeline (Docker@2)

    order-pipeline.yml
    product-pipeline.yml
    store-front-pipeline.yml

# Azure Release Pipeline

    Configure and run the Order Build and deployment Pipeline

        order-pipeline.yaml

    Configure and run the Product Build and deployment Pipeline

        product-pipeline.yaml

    Configure and run the store front Build and deployment Pipeline
    
        store-front-pipeline.yaml

    Configure and run the rabbitmq deployment Pipeline

        rabbitmq-pipeline.yaml

    Configure and run the configmap deployment Pipeline

        configmap-pipeline.yaml

# Write Kubernetes YAML Manifests for the applicatons

    configmap.yml
    order-deployment.yml
    order-service.yml
    product-deployment.yml
    product-service.yml
    store-front-deployment.yml
    store-front-service.yml
    rabbitmq-deployment.yml
    rabbitmq-service.yml

# Verify the Deployment

    k get pods
    k get services
    curl <LoadBalancer public IP>:80
    Browse the app using http://<LoadBalancer public IP>:80

# Clean the Azure resources

    az group delete --name rg-onlinestore-dev-uksouth-001 --yes --no-wait 