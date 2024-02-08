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


Create the environment
=====================================
1. For this environment we will be using a Windows Server 2022
    
2. Download the latest application from https://downloads.cybelesoft.com/thinfinity_remote_workspace_setup_x64.exe
    
3. Run the installer of the application and select the following configuration
	- All the components
	- The default directory
	
4. Once the deployment completes, we need to request the Community or a Trial Serial Number. For that we need to run the
Thinfinity Remote Desktop Configuration Manager and select the appropiate option.

5. Complete the internet form and you will receive an email with the data we need to complete.
   
6. Select the offline or online validation and leave blank the options for the Primary and Second Licence server.

7. When the deployment finish, you will have the Configuration Manager application open.

8. You could connect to http://localhost:9443 and connects to the Thinfinity Remote Workspace portal.

9. If you have exposed this server to the internet, you could select the option "Enable External access in Windows firewall" from the Configuration Manager
				

Configure the environment
======================

1. The 
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

        

Configure a connection to the local intranet
=
1. Deploy IIS in the Windows server with all the defaults
	- Open Server Manager
	- Select Deploy Server Roles and Features on the local server
	- Select Web Server (IIS)
	- Accept all by default 
            
2. Open Thinfinity Configuration Manager, select Profiles and Add web link
    
3. On the form complete the following
	- name: intranet
	- click "open in a new tab"
	- link: localhost
	- Accept the configuration

4. Apply the configuration in the Configuration Manager

5. Open the browser and select the "intranet" icon. A new tab will be open with the IIS Welcome page.


Configure a RDP connection to the local server
=
            
1. Open Thinfinity Configuration Manager, select Profiles and Add RDP connection
    
2. On the form complete the following
	- name: Windows Server
	- Click "open in new tab"
	- Select RDP
	- Computer: complete your local IP address
	- Select Ask for new credentials
	- Accept the configuration

3. Apply the configuration in the Configuration Manager

4. Open the browser and select the "Windows Server" icon. A new tab will be open with the RDP connection.





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
