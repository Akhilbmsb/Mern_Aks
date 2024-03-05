# Mern_Aks
##Clone the MERN application repository
```
git clone https://github.com/Akhilbmsb/SampleMERNwithMicroservices.git
```
## Build Docker images for each microservice:

# Navigate to each microservice directory (e.g., sample-mern-client, sample-mern-server, sample-mern-mongo)
```
docker build -t sample-mern-client .
docker build -t sample-mern-server .
docker build -t sample-mern-mongo .
```

# Login to your Azure Container Registry (ACR) using Azure CLI:
```
az acr login --name sample-mern-acr
```

# Tag Docker images for ACR:
```
docker tag sample-mern-client sample-mern-acr.azurecr.io/sample-mern-client:deployment
docker tag sample-mern-server sample-mern-acr.azurecr.io/sample-mern-server:deployment
docker tag sample-mern-mongo sample-mern-acr.azurecr.io/sample-mern-mongo:deployment
```

# Push Docker images to ACR:
```
docker push sample-mern-acr.azurecr.io/sample-mern-client:deployment
docker push sample-mern-acr.azurecr.io/sample-mern-server:deployment
docker push sample-mern-acr.azurecr.io/sample-mern-mong:deployment
```

# Create an AKS cluster:

## Create a resource group:
```
az group create --name mern_deployment --location centralindia
```

## Create AKS cluster:
```
az aks create --resource-group mern_deployment --name deployment_cluster --node-count 2 --enable-addons monitoring --generate-ssh-keys
```

# Configure kubectl to connect to the AKS cluster:
```
az aks get-credentials --resource-group mern_deployment --name deployment_cluster
```

# Deploy microservices to AKS:
## Create Kubernetes deployments and services for each microservice by applying the YAML files:
```
kubectl apply -f <path-to-kubernetes-yaml>
```

# Expose microservices using Ingress controller:
## Install Nginx Ingress Controller:

```
 kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

# Create an Ingress resource to route traffic to microservices:

#

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mern-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: <hostname>
    http:
      paths:
      - path: /sample-mern-client
        pathType: Prefix
        backend:
          service:
            name: sample-mern-client-service
            port:
              number: 3000
      - path: /sample-mern-server
        pathType: Prefix
        backend:
          service:
            name: sample-mern-server-service
            port:
              number: 5000
      - path: /sample-mern-mongo
        pathType: Prefix
        backend:
          service:
            name: sample-mern-mongo-service
            port:
              number: 27017
        #
    #
    Add an entry to the /etc/hosts file with the external IP and hostname configured in the Ingress resource.
    Access the MERN application using the configured hostname.
    #
    


![Screenshot (3091)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/3a9094ee-a185-435c-ae97-7566a4ecd93f)
![Screenshot (3090)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/f4543ecb-54fe-48e4-bb46-38a6ee420369)
![Screenshot (3089)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/e2c11479-2ffd-4c00-adee-5e4c68e319db)
![Screenshot (3088)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/9d029907-6b46-4017-a5fa-9e2f6130a220)
![Screenshot (3087)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/104119bc-300f-4d96-8895-b6039f990504)
![Screenshot (3086)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/844e3977-5a6d-43c4-9c6a-606037f009ae)
![Screenshot (3085)](https://github.com/Akhilbmsb/Mern_Aks/assets/54345937/91e48d3f-9f65-4112-89ac-b972843c9320)
