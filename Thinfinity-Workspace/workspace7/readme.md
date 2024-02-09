# Deploying Thinfinity Workspace 
### All the configurations using defaults 

In this tutorial we will be deploying Thinfinity Workspace v7 with the default configuration. We will focus on the simplest installation, which involves deploying all components on a single server.

Requirements
============

1. A virtual Machine with Windows Server to deploy all the Thinfinity components
2. A second virtual machine to be used as Remote Desktop Session Host and Thinfinity Workplace Secondary Broker

![alt text](https://github.com/RubenDillon/cybele/blob/main/Thinfinity-Workspace/workspace7/layout.png?raw=true)

Environment
==========

We will be using the following configuration
- The first server will be used as Thinfinity Workspace server 
	- 2 vCPU
 	- 4 GB RAM
  	- Windows Server 2022  
- The second server will be used as Secondary Broker Server and Microsoft RDS 
	- 2 vCPU
 	- 4 GB RAM
  	- Windows Server 2022 

Create the Thinfinity Environment
=====================================
1. For the deployment of the Thinfinity workspace environment we will be using a Windows Server 2022
    
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
        

Configure and test a connection to the local intranet
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


Configure and test a RDP connection to the local server
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


Configure and test a Web Folder sharing 
=

1. On the server create a folder in the root called "Prueba"
   
2. Open Thinfinity Configuration Manager, select Profiles and Add Web Folder connection
    
3. On the form complete the following
	- name: Web Folder
	- Click "Visible" and "Open in new tab"
	- Select Local Server
	- Root Path: C:\prueba
	- Select Ask for new credentials
	- Accept the configuration

4. Apply the configuration in the Configuration Manager

5. Open the browser and select the "Web Folder" icon. A new tab will be open with the folder connection.

6. Upload a file, edit and download if you want it.

7. Review the folder in your server.

Configure and test a SSH to Windows Server 
=

1. On the server verify is openSSH is already deployed using the following Powershell command
```
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

2. If it not, use the following to deploy openSSH server
```
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

3. Then use the following script to start and configure the service
```
# Start the sshd service
Start-Service sshd

# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'

# Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```
   
5. Open Thinfinity Configuration Manager, select Profiles and Add Web Folder connection
    
6. On the form complete the following
	- name: Web Folder
	- Click "Visible" and "Open in new tab"
	- Select Local Server
	- Root Path: C:\prueba
	- Select Ask for new credentials
	- Accept the configuration

7. Apply the configuration in the Configuration Manager

8. Open the browser and select the "Web Folder" icon. A new tab will be open with the folder connection.

9. Upload a file, edit and download if you want it.

10. Review the folder in your server.


Improving security 
=

1. On the Configuration Manager go to Authentication and deselect the "Allow anonymous access"

2. On the protection tab, select "Enable brute force detection"

3. If you want to some profile / connection will be available only for a specific Windows group, edit that profile and select Permissions and add that group to that profile.

4. Logout the Thinfinity Workspace and log with a user who is not in that group. You will not see that profile in the Portal.

5. Logout the Thinfinity Workspace and log with a user who is part in that group. You will see that profile in the Portal.
   

Create your own connections
=

1. On the Thinfinity Workspace portal login with any user, select the "+ New Profile" button from the upper right corner

2. Select the following options
	- Desktop
 	- RDP
  	- This PC
   	- Credentials : select use the authenticated credentials
   	- Define the name of the connection
  
3. Select the profile and open RDP connection with the server 	


Deploying a Secondary Broker (in the second machine)
=

1. For this deployment, we will be using the second machine as a Remote Desktop Session Host and Secondary Broker

2. Deploy RDS as Standard Deployment in the secondary server from the Server Manager. Configure RDS using Role Services and select the following
   	- Remote Desktop Session Host
   	- Remote Desktop Licensing

3.  Activate RDS. As we are using a RDS without a domain, we need to do some steps manually
	- open the registry editor
 	- modify the following registry key
  		- Computer Configuration\ Administrative Templates\ Windows Components\ Remote Desktop Services\ Remote Desktop Session Host\ Licensing\
    		- Use the specified Remote Desktop licensing servers     Enabled and Localhost as server
      		- Set the Remote Desktop licensing mode     Enabled and select Per Device

5. Using the Remote Desktop Services License Manager, use the Install Licenses option to define Per Device CAL licenses (use all by default)

6. Create a user and asign it to the "Remote Desktop User". Test the connection to the server using this user.

5. Download de Thinfinity installer https://downloads.cybelesoft.com/thinfinity_remote_workspace_setup_x64.exe

6. Follow the Wizard and select "Broker and HTML5 Services" option.

7. Access the Registry Editor and navigate to the following key
	- Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Cybele Software\Thinfinity\Wolrkspace

8. Modify "BrokerRole" to secondary

9. Open the Configuration Manager of the first server, and copy the "Network ID" from the General Tab

10. Now go to the "Broker" tab and select Secondary Brokers and click ADD button
	- Define the name of the pool
 	- Define the amount of users

11. In the second server, open the Configuration Manager and in "General" tab complete the following information
	- Pool, the name defined in the previous step
 	- Network ID, complete the information with the info obtained from the previous step
  	- Gateway list, complete with the address of the first server (http://server:9443)   

12. Review the log using the "Review Log" button. You will notice something like the following
	Connecting to http://server:9443
	Registerd on http://server:9443


Connect a RDP session to the RDS Host using the Secondary Broker 
=

1. Open Thinfinity Configuration Manager from the first server, select Profiles and Add RDP connection
    
2. On the form complete the following
	- name: RDP session in RDS Host
	- Click "open in new tab"
	- Select RDP
	- Computer: complete the second IP address
	- Select Use the authenticated credentials
	- Accept the configuration

3. Apply the configuration in the Configuration Manager

4. Open the browser and select the "RDS session in RDS Host". A new tab will be open with the RDP connection.


This Step by Step is based on the following documentation
=
- https://kb.cybelesoft.com/portal/en/kb/articles/guide-workspace-7-installation-license-registration#2_Download_and_Install
- https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=powershell
- https://kb.cybelesoft.com/portal/en/kb/articles/how-to-install-a-secondary-broker#Introduction
