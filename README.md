# ArgoCD-Integration
Walking through the process to install ArgoCD with EKS and deploy a sample application

## Architecture:

![1_cM-oD_QWXea-rYVEbtfFkA](https://github.com/user-attachments/assets/86f0887d-77fe-4b52-a2c0-363811e094c9)



## Pre-requisites:

1. AWS EKS Cluster
2. Kubectl client with updated kube-config


## Installation Instructions:

### 1. Run the latest version of ArgoCD from https://github.com/argoproj/argo-cd/releases/ (highly available version)
- kubectl create namespace argocd
- kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v3.0.12/manifests/ha/install.yaml


<img width="773" height="194" alt="image" src="https://github.com/user-attachments/assets/467eacf7-91f3-4366-993b-481e7033313e" />

### 2. Modify the argocd-server service and change it to a service of type "loadbalancer". This is so we can access the server over the internet:

- kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

<img width="1538" height="56" alt="image" src="https://github.com/user-attachments/assets/a2cf4929-9890-4739-8acd-809669c718dc" />


### 3. Classic Load Balancer will be created after a few minutes.


<img width="705" height="262" alt="image" src="https://github.com/user-attachments/assets/2073389d-8a5f-45b2-97d7-7a56a23cd29f" />


### 4. Retrieve the password for logging into your ArgoCD server. Username is Admin
- kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d


### 5. With your username and password, navigate to ArgoCD in browser using the load balancer DNS name:
- http://afa26b4c6f44f41c89f2d27b34cbdc0f-2066168060.us-east-1.elb.amazonaws.com

### 6. Sync the Github Manifest repo with ArgoCD
