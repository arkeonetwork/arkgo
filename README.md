# Arkeo ArgoCD

This repository contains Argo CD ApplicationSets to manage different daemons as a single unit. It also includes a Helm chart values file for installing Argo CD.  
The **arkeo-applicationset.yaml** file deploys the daemons in this [repo](https://github.com/arkeonetwork/helm-charts), and 
the **daemons-applicationset.yaml** file deploys the daemons in this [repo](https://gitlab.com/thorchain/devops/node-launcher/-/tree/master?ref_type=heads)  

## Prerequisites 
- A Kubernetes cluster
- Helm installed

## ArgoCD Installation
**Using Helm**  
1. Add the Helm repository for Argo CD:  
   ``` bash
   helm repo add argo https://argoproj.github.io/argo-helm
   ```
2. Install ArgoCD:
   ```bash
   helm install arkeo-argo-cd argo/argo-cd --version 5.51.2 -f argo-values.yaml --namespace argocd --create-namespace
   ```
3. To access the ArgoCD UI:
   ```bash
   kubectl port-forward svc/arkeo-argo-cd-argocd-server 8080:443 -n argocd
   ```  
**Heads up:** Set your ArgoCD password in the `argocdServerAdminPassword` field using a bcrypt hashed password. You can also set up a discord webhook url in the `discord-url` field to receive ArgoCD alerts  

## Apply the ApplicationSet resources  
```bash
kubectl apply -f arkeo-applicationset.yaml
kubectl apply -f daemons-applicationset.yaml
```
This will create two sets of Argo CD Applications, one for the daemons in this [repo](https://github.com/arkeonetwork/helm-charts) and another for the other daemons deployed in this [repo](https://gitlab.com/thorchain/devops/node-launcher/-/tree/master?ref_type=heads)   

## Additional Notes

* The `syncPolicy` field can be set to `automated` or `manual`. When set to `automated`, the ApplicationSet controller will automatically sync the applications with the contents of the repository. When set to `manual`, the applications must be synced manually using the Argo CD UI.
