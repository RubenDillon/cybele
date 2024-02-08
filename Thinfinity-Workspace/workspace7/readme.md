# Deploying Thinfinity Workspace 
### all by default 

In this tutorial we will be Thinfinity Worspace v7 with the default configuration. We will focus on the in the simplest installation, which involves deploying all components on a single server.

Requirements
============

1. A virtual Machine with Windows Server 

Environment
==========

At the end of this step by step we will be using the following infrastructure
- A .....


Configure the Windows virtual Machine
=====================================
1. For this environment we will be using a Windows Server 2022
    
2. Download the latest application from https://downloads.cybelesoft.com/thinfinity_remote_workspace_setup_x64.exe
    
3. Run the installer of the application and select the following configuration
	All the components
	The default directory
	
4. Once the deployment completes, we need to request the Community Serial Number. For that we need to run the
Thinfinity Remote Desktop Configuration Manager

5. 
				

Create the environment
======================

1. Deploy azure client or upgrade it (in my case, Im using a Mac)
```
brew update && brew install azure-cli
```
    
2. Log to Azure 
```
az login
```
    
3. Define your subscription
```
az account set --subscription dddddxxxx-xx-xxxxx-xxxxxxxx
```
    
4. Create the resource group where AKS will be deployed
```
az group create --name LATAM-TAP-RG --location eastus
```
    
5. Verify the kubernetes versions available for AKS
```
az aks get-versions --location eastus
```
    
6. Create the AKS cluster

	to test.............. Standard_D4ds_v4 creates nodes with 4 vCPU and 16 GB RAM

	to have a real use... Standard_D8ds_v5 creates nodes with 8 vCPU and 32 GB RAM

```
az aks create -g LATAM-TAP-RG -n latam-tap-azure --enable-managed-identity --node-count 6 --enable-addons monitoring --enable-msi-auth-for-monitoring --generate-ssh-keys --node-vm-size Standard_D8ds_v5 --kubernetes-version 1.24.10
```

7. Deploy kubectl
```
sudo su
az aks install-cli
```
    
8. Download the kubectl 
```
az aks get-credentials --name latam-tap-azure --resource-group LATAM-TAP-RG
```
    
9. Test the connection
```
kubectl get nodes
```

10. In TMC create a Cluster Group (I create "TAP" Cluster group)

11. Review that you are on the correct cluster
```
kubectl config get-contexts
```
    
12. Attach the AKS Cluster to TMC under TAP cluster group 
	
13. Accept the EULA and deploy the Tanzu cli. Follow this instructions https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.5/tap/install-tanzu-cli.html
    
14. Deploy Cluster Essentials. Follow this instructions https://docs.vmware.com/en/Cluster-Essentials-for-VMware-Tanzu/1.5/cluster-essentials/deploy.html
    	
15. Deploy Cert-Manager
```
tanzu package available list -A
tanzu package available list cert-manager.tanzu.vmware.com -A	
kubectl create ns cert-manager
tanzu package install cert-manager --package cert-manager.tanzu.vmware.com --namespace cert-manager --version 1.10.2+vmware.1-tkg.1
```
NOTE: at this moment we have available for example 1.10.2+vmware.1-tkg.1   
  
16. Deploy Contour 
```    		
tanzu package available list contour.tanzu.vmware.com -A			
kubectl create ns contour
		
tanzu package install contour \
--package contour.tanzu.vmware.com \
--version 1.23.5+vmware.1-tkg.1 \
--values-file contour-values.yaml \
--namespace contour
```
NOTE: at this time we have available for example 1.23.5+vmware.1-tkg.1

17. Obtain the Public IP Address where Envoy is running
```    
kubectl get svc -A | grep envoy
```
      
18. Create the namespace tanzu-system-registry
```       
kubectl create ns tanzu-system-registry
```
        
19. Create a Cluster issuer using the following command
```
kubectl apply -f - <<'EOF'
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-contour-cluster-issuer
  namespace: cert-manager
spec:
  acme:
    email: "rdillon@vmware.com"
    privateKeySecretRef:
      name: acme-account-key
    server: https://acme-v02.api.letsencrypt.org/directory
    solvers:
    - http01:
        ingress:
          class: contour
EOF
```
    
20. Request the certificate as follow
    
        kubectl apply -f - <<'EOF'
        apiVersion: cert-manager.io/v1
        kind: Certificate
        metadata:
          name: harbor-cert
          namespace: tanzu-system-registry
        spec:
          secretName: harbor-cert-tls
          duration: 2160h
          renewBefore: 360h
          subject:
            organizations:
            - vmware
          commonName: harbor.latamteam.name
          isCA: false
          privateKey:
            size: 2048
            algorithm: RSA
            encoding: PKCS1
          dnsNames:
          - harbor.latamteam.name
          - notary.harbor.latamteam.name
          issuerRef:
            name: letsencrypt-contour-cluster-issuer
            kind: ClusterIssuer
        EOF
    
21. Now we will expect to wait more or less 10 minutes until we could have the certificate. To verify it use the following command
```
kubectl get certificates -n tanzu-system-registry harbor-cert
```
    
22. When you have TRUE in ready column, you will continue to the next step that is obtain the certificates
```        
kubectl get secret harbor-cert-tls -n tanzu-system-registry -o=jsonpath={.data."tls\.crt"} | base64 --decode > tls-crt.txt            
kubectl get secret harbor-cert-tls -n tanzu-system-registry -o=jsonpath={.data."tls\.key"} | base64 --decode > tls-key.txt
```       
 
23. Deploy Harbor using the Tanzu packages (follow the steps from Tanzu documentation)
```
kubectl create ns harbor
tanzu package install harbor --package harbor.tanzu.vmware.com --version 2.6.3+vmware.1-tkg.1 --values-file harbor-values.yaml --namespace harbor
```                 
    
24. Connect to harbor and create "tap" project as public.
        

Configure the cluster
=
1. We connect to the cluster
            
2. Create the namespace "tap-install"
    
3. Create a internal registry secret
```    
tanzu secret registry add registry-credentials \
--server   harbor.latamteam.name \
--username admin \
--password "PASSw0rd2019202020212022" \
--namespace tap-install \
--export-to-all-namespaces \
--yes
```
    
4. Wait a couple of minutes and review if registry-credentials is in default namespace. 
   If it not, create it for the default namespace.


create the Tanzu Net secret using TMC
=
```
Secret name: tap-registry
Image registry URL: registry.tanzu.vmware.com
Username: <your-tanzu-net-username>
Password: <your-tanzu-net-password>
namespace: tap-install
export to all namespaces
```


Based on the following documentation
=
https://kb.cybelesoft.com/portal/en/kb/articles/guide-workspace-7-installation-license-registration#2_Download_and_Install
