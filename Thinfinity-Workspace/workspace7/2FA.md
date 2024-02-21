# Deploying Thinfinity Workspace 

In this tutorial we will be deploying Thinfinity Workspace v7 as an example of a typical Proof of Concept. 

We define the following scenario
- We have a principal datacenter with the main infrastructure 
- We have a secondary site where will have to access some applications

In this PoC we start to deploy the simplest configuration, everything in one virtual machine because usually we start very limited in resources. In that virtual machine we will simulate the Web Server of the intranet, a file sharing and will provide administrative access to administrators (RDP and SSH). Everything without the need to open any port to internet.

Then we simulate a secondary site and with that, the needs to use a Secondary Broker to connect both sites. In that virtual machine we simulate a Remote Desktop Server where we will be hosting applications that will be accessed from outside. 

Requirements
============

1. A virtual Machine
