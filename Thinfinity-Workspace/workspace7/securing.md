# Securing the deployment of Thinfinity Workspace 

In this tutorial we will be securing the deployment of Thinfinity Workspace v7 as an example of a typical Proof of Concept. 

In a typical PoC after we deploy the infrastructure, we secure the deployment as follows.

Requirements
============

1. The Thinfinity Workspace environment working.
2. A certificate (pfx file and passphrase) or we could use the win-acme call to Let's Encrypt



IIS Crypto configuration
=
1. Access to Nartac Software site to download the IIS Crypto tool to secure Windows Server (the gateway server) https://www.nartac.com/Products/IISCrypto   

2. Download the GUI tool of cli (whatever you find confortable with)

3. Open for example the GUI Tool and select Templates. From the list select PCI 4.0

4. Go to SCHANNEL, review the configurations, mark Reboot checkbox and press the APPLY button. 

            
Use of Digital Certificate
=
1. If we have a certificate (PFX file) and the passphrase we could add this to the Gateway
   
2. Go to the Gateway server and select Thinfinity Gateway application
   
3. Go to the Bindings and select ADD
	- On Protocol select HTTPS
	- On Bind to IP, you could select the default or you could use the IP who have internet connectivity
	- On Port select 443
	- On SSL select NEW button
 	- Select Import certificate and follow the wizard
  	- Select on the Certicate field, the certificate that you have imported
   	- Press the OK button and then Apply 

4. Now try to access the Thinfinity Workspace using https://serverFQDN  instead of HTTP://serverFQDN:9443
