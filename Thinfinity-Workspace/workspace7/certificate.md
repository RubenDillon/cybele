# Securing the Thinfinity Workspace connection 

In this tutorial we will be reviewing how to secure the connection using https. 

We define the following scenario
- We have a Workspace already running 
- We will be using a free DNS domain
- We will be using a free public certificate

Require a DNS hostname
=

1. We will be using a DNS hostname from No-IP. We could use any provider, but in this step by step we will be using NO-IP domain.

2. Open www.noip.com and select Free Hostname. We will be creating thinfinity.servehttp.com as hostname. Complete the steps to obtain the hostname appropiated for your environment.

3. Test the connectivity using the fqdn


Require the certificate
=

1. We will be using a certificate from Let´s Encrypt. To facilitate the steps, we will be using win-acme from www.win-acme.com

2. Complete the following steps in the Thinfinity primary broker
              - Downlad the application and start it
              - Select to create a full certificate (option M from the menu)
              - Select Manual input (option 2)
              - Serve verification files from memory (option 2)



3. xxcx
4. 
