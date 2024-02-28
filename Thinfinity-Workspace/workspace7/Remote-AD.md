# How-to use Thinfinity Remote Active Directory Services

In this tutorial we will be using Remote Active Directory Services of Thinfinity Workplace v7. 

The main idea with this feature is give the possibility to have access to a remote Active Directory in our environment. 

The use case could be described as we have an idependent domain and our enviroment is already deployed in a domain without contact to the other or in a workgroup
                
In this example, we want to give access to the users from the other domain.

For our How-to we will have a Domain called cybele.local and a Thinfinity environment created in standalone servers, in a workgroup configuration. No domain at all.



Deploy Thinfinity Remote Active Directory Services 
=
1. To deploy the Remote AD Services, we will be using a server already joined to the domain. In our example, we will be using cybele.local (NETBIOS name is CYBELE)

2. Start the setup of the application and give all the defaults. 

3. Once the application were installed, run the Thinfinity Remote Active Directory Manager

4. In the General tab, you need to copy the Network ID from the Broker and ADD your gateway server to the Gateway list (in the following format http://<server>:9443)

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
    - Under Service, in the Filter expression: press the dots and complete your DOMAIN and NETBIOS .. for example cybele.local and cybele.
    - Default Expression and URL, will be populated for you
    - API Authentication: in those fields complete with the user that you define in the Remote Active Directory Services server. Use for example the administrator in the Username and complete the password

5. Now you need to have only your connection checked in the "Directory Services" tab. Uncheck Thinfinity Identity Provider and Local Windows Users to not allow local logins

6. Apply all the changes

Create a connection and test the domain
=

1. Now we could create an Access Profile with the information that you want

2. We could define who could access to this profile. For that select Permissions tab and uncheck Allow Annonymous access.

3. Then press ADD and type a user (administrator or any do you have created)

4. Press the magnifying glass next to the field where you type the username or the group

5. You will be presented the user or group that you search (for example CYBELE\alex)
