# Learn To Implement GitOps On Kubernetes Using Flux In Just 15 Minutes
## Introduction
### What is GitOps ?
### What is Flux ?
### Sidecar Pattern

## System Requirements
### Setup development cluster using KIND (Kubernetes in docker)
   - ```
        wget https://github.com/kubernetes-sigs/kind/releases/download/v0.10.0/kind-darwin-amd64
        mv kind-darwin-amd64 kind
        chmod +x kind
        mv kind /usr/local/bin/
        kind version
        kind create cluster
        kubectl get ns
     ```
      ![](.README/224c32de.png)

### Install & Configure Flux in Kubernetes Cluster  
   - ```
        wget https://github.com/fluxcd/flux2/releases/download/v0.13.3/flux_0.13.3_darwin_amd64.tar.gz
        tar -xvf flux_0.13.3_darwin_amd64.tar.gz
        mv flux /usr/local/bin/
        flux --version
        flux check --pre
        flux install
     ```
      ![](.README/94dd5562.png)

## GitOps In Action
### Setup Git repository "app-flux-infra"
   - ![](.README/d95b4d71.png)
   - ![](.README/a8a9d248.png)
   - ![](.README/6265978c.png)
### Apply kubernetes deployments
#### Bootstrap Git repository in flux
   - ```flux bootstrap git app-flux-infra --url=https://github.com/rajat965ng/app-flux-infra.git -u <GIT_USERNAME> -p <GIT_PAT> --token-auth=true --path=./cluster/dev/```
   
     ![](.README/7ac12368.png)
      
#### Take a git pull and view the cluster hierarchy 
   - ![](.README/a54e6b15.png)  
#### Create a Nginx deployment under cluster/dev
   - ![](.README/46957eb1.png)
#### Push Nginx deployment in Git repo
   - ![](.README/68378e6f.png)
#### Observe the deployments rolling
   - ![](.README/1dae1f2a.png)
#### Query Flux to view the current deployed revision
   - ```flux get all```
       
     ![](.README/68838edb.png)
     
#### Trace applied revision to match with Git SHA
   - ![](.README/d0b65799.png)    

### Apply Helm Chart
#### Create a Helm Chart "ms-template"
   - ![](.README/537bb8a6.png)
#### Package Helm Chart
   - To avoid bot crawling on my repository, add the following robots.txt file:
     ```
     echo -e “User-Agent: *\nDisallow: /” > robots.txt
     ```
   - Lint the helm chart
     ```
     helm lint helm-chart/*
     ``` 
   - Package helm chart
     ```
     cd helm-chart/ && helm package ms-template/
     ```   
   - Create index.yaml for ms-template
     ```
     helm repo index --url=https://rajat965ng.github.io/app-flux-infra/helm-chart/ .
     ```  
#### Push Helm Chart in Git
   - ![](.README/624aaaa5.png) 
#### How to convert Github in Helm repository ?
   - Click on Git repository "Settings"
   - Scroll down options to choose "Pages"
   - Select "Branch" -> "main" from dropDown and click "save"
   
     ![](.README/34097a07.png)
     
#### Configure Helm Repository in Flux
#### Update the Helm Chart
#### Observe the helm rolling

## Benefits of using Flux
## Who else is using Flux in production ?
## References