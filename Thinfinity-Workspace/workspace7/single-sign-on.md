# Using Azure for Single Sign on 

In this tutorial we will be reviewing how to use Azure AD with Thinfinity Workplace. 

We define the following scenario
    - We have a Workspace already running 
    - We have a Azure subscription

Create Thinfinity as Application on Azure
=

1. From the Azure portal select Microsoft Entra ID (ex Azure Active Directory)

2. Select "App Registration" and complete the form with the following (as example)
    - Select "Accounts in this organizational directory only"
    - Complete Redirect URI, select Web and type the following  https://thinfinity.servehttp.com/azure

3. Once is completed, select Certificates and Secrets and create "New client secret"

4. Copy the "value" field

5. Go to the Thinfinity Configuration and create a connection.

6. Go to the "Authentication" tab and select Methods, then "Add", "OAuth 2.0" and Azure. Complete the following
    - General tab
        - Client ID: use the Application (client) ID from the last step
        - Client Secret: use the "value" that we obtain in the last step
    - Server tab
        - Authorization URL: modify the text and replace [DirectoryID] with the Directory (tenant) ID from the Azure Portal
        - Token Validation Server URL: do the same




8.   
