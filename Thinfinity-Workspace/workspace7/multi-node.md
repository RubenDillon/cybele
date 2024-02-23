# Deploying Thinfinity Workspace in a multi node infrastructure

In this tutorial we will be deploying Thinfinity Workspace v7 as an example of a typical Proof of Concept but with multiple nodes. 

We define the following scenario
- We have a DMZ where we deploy the Gateway (in our example we will be using 198.71.59.129)
- We have an internal secure network where we will deploy the primary broker (in our example we will be using 198.71.59.151)
- We have a secondary broker where we will have a terminal server (RDS) (in our example we will be using 216.225.206.8)

Requirements
============

1. A virtual Machine with Windows Server to deploy the gateway.
2. A second virtual machine where to deploy the broker
3. A third virtual machine to be used as Remote Desktop Session Host where will deploy applications and Thinfinity Workplace Secondary Broker

![alt text](https://github.com/RubenDillon/cybele/blob/main/Thinfinity-Workspace/workspace7/multi-node.png?raw=true)

Create the Reverse Gateway
=
1. For the deployment of the gateway we will be using a Windows Server 2022 in a DMZ zone.
    
2. Download the latest application from https://downloads.cybelesoft.com/thinfinity_remote_workspace_setup_x64.exe
    
3. Run the installer of the application and select the following configuration
	- Reverse Gateway
	- The default directory
	
4. Once the deployment completes, we need to request the Trial Serial Number. For that we need to run the
Thinfinity Remote Desktop Configuration Manager and select the appropiate option.

5. Complete the internet form and you will receive an email with the data we need to complete.
   
6. Select the offline or online validation and leave blank the options for the Primary and Second Licence server.


Create the Primary Broker
=
1. For the deployment of the broker we will be using a Windows Server 2022 in the second virtual machine in the internal (Secure) network.
    
2. Download the latest application from https://downloads.cybelesoft.com/thinfinity_remote_workspace_setup_x64.exe
    
3. Run the installer of the application and select the following configuration
	- Broker
	- The default directory
	
4. Once the deployment completes, we need to request the Trial Serial Number. For that we need to run the
Thinfinity Remote Desktop Configuration Manager and select the appropiate option.

5. Complete the internet form and you will receive an email with the data we need to complete.
   
6. Select the offline or online validation and leave blank the options for the Primary and Second Licence server.

7. When the deployment finish, you will have the Configuration Manager application open.

8. Now you need to modify the Network ID (use the ID from the Gateway) and add the first node inside Gateway List using the following format http://198.71.59.129:9443

9. Now, we could test Thinfinity Workplace against the first node using http://198.71.59.129:9443
        

Create the Secondary Broker
=

1. For this deployment, we will be using the third machine as a Remote Desktop Session Host and Secondary Broker in the internal (Secure) network.

2. Deploy RDS as Standard Deployment in the secondary server from the Server Manager. Configure RDS using Role Services and select the following
   	- Remote Desktop Session Host
   	- Remote Desktop Licensing

3.  Activate RDS. As we are using a RDS without a domain, we need to do some steps manually
	- open the Group Policy Editor (gpedit.msc)
 	- modify the following registry key
  		- Computer Configuration\ Administrative Templates\ Windows Components\ Remote Desktop Services\ Remote Desktop Session Host\ Licensing\
    		- Use the specified Remote Desktop licensing servers     Enabled and Localhost as server
      		- Set the Remote Desktop licensing mode     Enabled and select Per Device

5. Using the Remote Desktop Services License Manager, use the Install Licenses option to
	- activate the server
 	- use academic type licencing and use for example 1234567 as licence key 
 	- define Per Device CAL licenses (use all by default)

6. Create a user and asign it to the "Remote Desktop User". Test the connection to the server using this user.

7. Download de Thinfinity installer https://downloads.cybelesoft.com/thinfinity_remote_workspace_setup_x64.exe

8. Follow the Wizard and select "Broker and HTML5 Services" option.

10. Access the Registry Editor and navigate to the following key
	- Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Cybele Software\Thinfinity\Wolrkspace

11. Modify "BrokerRole" to secondary

12. Open the Configuration Manager of the first server, and copy the "Network ID" from the General Tab

10. Now go to the "Broker" tab and select Secondary Brokers and click ADD button
	- Define the name of the pool (for example pool1)
 	- Define the amount of users (use the default)

11. In the second server (the Primary Broker), open the Configuration Manager and in "General" tab under Secondary Broker complete the following information
	- Pool, the name defined in the previous step (pool1)
 	- Network ID, complete the information with the info from the gateway server
  	- Gateway list, complete with the address of the gateway server (http://198.71.59.129:9443)   

12. Connect to the third server (Secondary Broker) and review the log using the "Review Log" button. You will notice something like the following
	Connecting to http://192.71.59.129:9443
	Registerd on http://192.71.59.129:9443



Configure and test the environment
=
            
1. Open Thinfinity Configuration Manager in the Primary Broker, select "Access Profiles" and Add RDP
    
2. On the form complete the following
	- name: Notepad
	- click "open in a new tab"
	- link: 198.71.59.129
 	- under Program select
  		- On Connection: RemoteApp
    		- On Program Path.. notepad.exe  
	- Accept the configuration

3. Apply the configuration in the Configuration Manager

4. Open the browser, connect to htt://http://198.71.59.129:9443 and select the "Notepad" icon. A new tab will be open with notepad.


Configure and test a RDP connection to the Secondary broker (the RDS Host Server)
=
            
1. Open Thinfinity Configuration Manager in the Primary Broker, select "Access Profiles" and Add RDP connection
    
2. On the form complete the following
	- name: RDP
	- Click "open in new tab"
	- Select RDP
	- Computer: 127.0.0.1
 	- Broker Pool: complete pool1
	- Select Ask for new credentials (because we need to use a local user who have RDS permissions)
	- Accept the configuration

3. Apply the configuration in the Configuration Manager

4. Open the browser, connect to htt://http://198.71.59.129:9443 and select the "RDS" icon. A new tab will be open asking for credentials and then you will have the RDP connection.


Configure and test a Web Folder sharing 
=

1. On the Primary Broker server create a folder in the root called "Prueba"
   
2. Open Thinfinity Configuration Manager in the Primary Broker server, select Profiles and Add Web Folder connection
    
3. On the form complete the following
	- name: Web Folder
	- Click "Visible" and "Open in new tab"
	- Select Local Server
	- Root Path: C:\prueba
	- Select Ask for new credentials
	- Accept the configuration

4. Apply the configuration in the Configuration Manager

5. Open the browser, connect to htt://http://198.71.59.129:9443 and select the "Web Folder" icon. A new tab will be open with the folder connection.

6. Upload a file, edit and download if you want it.

7. Review the folder in your server.


Configure and test a SSH to Primary Broker 
=

1. On the Primary Broker server verify is openSSH is already deployed using the following Powershell command
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
   
5. Open Thinfinity Configuration Manager in the Primary Broker, select Profiles and Add Web Folder connection
    
6. On the form complete the following
	- name: ssh
	- Click "Visible" and "Open in new tab"
	- In the Address put the IP of this server and 22 in Port
	- Select Enable Keep Alive and SSH
	- Select Ask for new credentials
	- Accept the configuration

7. Apply the configuration in the Configuration Manager

8. Open the browser, connect to htt://http://198.71.59.129:9443 and select the "ssh" icon. A new tab will be open with the ssh emulation.


Improving security 
=

1. On the Configuration Manager of the Gateway server go to the protection tab, select "Enable brute force detection"

2. On the Configuration Manager of the Primary Broker go to Authentication and deselect the "Allow anonymous access"

3. If you want to some profile / connection will be available only for a specific Windows group, edit that profile and select Permissions and add that group to that profile.

4. Open the browser, connect to htt://http://198.71.59.129:9443 and log with a user who is not in that group. You will not see that profile in the Portal.

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
  
3. Open the browser, connect to htt://http://198.71.59.129:9443 and select the profile and open RDP connection with the server 	

Configure and test a Remote application running in the RDS host
=

1. Deploy Microsoft office using the default configuration in the Secondary Broker Server where RDS is running. Remember to configure the Office License as for a Shared Computer, if you don't do it at the Office setup you have another opportunity. Use Registry Editor to add a String value (Reg_SZ) of SharedComputerLicensing with a setting of 1 under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\ClickToRun\Configuration.
  
2. In the Primary Broker Server open Thinfinity Configuration Manager, select Profiles and Add RDP connection
    
3. On the form complete the following
	- name: WinWord
	- Click "open in new tab"
	- Select RDP
	- Computer: complete the IP address of the secondary server or if you have more than a server in the pool you could use 127.0.0.1 to use any server asigned to it
	- Select Ask for new credentials (because we are using users from that Second Broker server who have RDS permissions)
 	- In the Program tab complete the following
  		- Program Path and file name: "C:\Program Files (x86)\Microsoft Office\root\Office16\WINWORD.EXE"
    		- Deselect "Show Windows Logon and Logout Screen"	 
	- Accept the configuration

4. Apply the configuration in the Configuration Manager

6. Open the browser, connect to htt://http://198.71.59.129:9443 and select the "WinWord" icon. A new tab will be open with Microsoft Word.


Another way to configure a Remote application running in the RDS host
=
  
1. Open the browser, connect to htt://http://198.71.59.129:9443 and select the "+ New Profile" button
    
2. On the Wizard select Application and then select Remote Application

3. Then select Another PC and complete in Address the FQDN or Address of the second server

4. In the Choose the Application complete for example "C:\Program Files (x86)\Microsoft Office\root\Office16\EXCEL.EXE"

5. In the Authentication select for example "Ask for new credentials"

6. In the Profile Name for example use Excel

7. When the profile appears in the UI, select Edit from the three points at upper left of the icon and modify the following.

	- Broker Pool: pool1
 	- Enable the option "Open in new tab"
  	- Disable the option "Show Windows Login and logout screen"
   	- Enable the option "Remote FX"    

9. Open the browser and select the "Excel" icon. A new tab will be open with Microsoft Excel.


USB Redirection
=

USB redirection allows you to use a local USB device in your endpoint. 

1. In the Second Broker machine navigate up to  C:\Program Files\Thinfinity\Workspace\bin64

2. Create a file called Thinfinity.Params.ini with the following
```
[USBRedirection]
Enabled=True
```

3. Navigate up to   C:\Program Files\Thinfinity\Workspace and review the file web.settings.js 

4. Edit the file and include the following
	- webUsb: true

5. The part where we need to modify the file should look like the following
```
var settings = {
	// Settings available for RDP connections
    RDP: {		
		synchronizeClipboard: true,	
                webUsb: true,	
```

6. Open the browser, connect to htt://http://198.71.59.129:9443 and open a RDP connection

7. At the top of the connection window, you have different options like Actions, File Transfer and Options. Inside Option you will see a new option in the menu called "Connect USB". There you will see the USB devices from your endpoint.


This Step by Step is based on the following documentation
=
- https://kb.cybelesoft.com/portal/en/kb/articles/guide-workspace-7-installation-license-registration#2_Download_and_Install
- https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=powershell
- https://kb.cybelesoft.com/portal/en/kb/articles/how-to-install-a-secondary-broker#Introduction
- https://aventistech.com/2018/12/27/deploy-windows-2019-rds-in-workgroup-without-ad/
- https://learn.microsoft.com/en-us/archive/msdn-technet-forums/c1e4c4e5-3933-442f-85a4-34684d09824c
- https://kb.cybelesoft.com/portal/en/kb/articles/how-to-create-a-remoteapp-pool-on-a-secondary-broker#Introduction
- https://thinfinity-remote-workspace-v7-docs.cybelesoft.com/advanced-settings/advanced-features/redirecting-devices/usb-redirection
