# How-to use Thinfinity Remote Active Directory Services

In this tutorial we will be using Remote Active Directory Services of Thinfinity Workplace v7. 

The main idea with this feature is give the possibility to have access to a remote Active Directory in our environment. The use case will be as follow
        - we have an independent domain
        - we have Thinfinity already deployed in a domain without contact to the other or in a workgroup

In this example, we want to give access to the users from the other domain.

Deploy Thinfinity Remote Active Directory Services 
=
1. To deploy the Remote AD Services, we will be using a server already joined to the domain. In our example, we will be using cybele.local (NETBIOS name is CYBELE)

2. Start the setup of the application and give all the defaults. 

3. Once the application were installed, run the Thinfinity Remote Active Directory Manager

4. In the General tab, you need to copy the Network ID from the Broker and ADD a your gateway to the Gateway list (in the following format http://<server>:9443)

5. In the Group tabs, no changes

6. In the API Users, select ADD and select for example administrator from the domain (in our case CYBELE) and select 0 in the Expiration seconds field of Token Expiration window

7. Finally select apply and press the Show Log button to validate the connectivity against the gateway


Broker configuration
=
1. We need to modify each Broker of our solution, for that we need to open the Configuration Manager

2. Go to Authentication and select "Enable Directory Services Extensions" and verify that "Allow anonymous access" is unchecked

3. Now you need to go to the Directory Services tab and select ADD

4. You will have a form with the following information
    - Name: Name of the service, for example CYBELE AD
    - Under Service, in the Filter expression: press the dots and select your DOMAIN and NETBIOS .. for example cybele.local and cybele
    - API Authentication: in those fields complete with the user that you define in the Remote Active Directory Services server. Use for example the administrator in the Username and complete the password

5. asf
