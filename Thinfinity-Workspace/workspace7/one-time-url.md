# How-to use One Time URL
In this tutorial we will be using the feature One Time URL of Thinfinity Workplace v7. 

The main idea with this feature is give the possibility to access a resource one time and for a specific period of time without define user and things like that.

In this example, we want to give access to a particular resource to an user who is not part of our company (for example a vendor)

Create the target resource
=
1. Go to Configuration Manager and create a profile

2. for example we could give access to a virtual app, web or whatever we want to test


Obtain the APIKey
=
1. To obtain the API Key of the server, go to the Primary Broker

2. Open the setting.ini from C:\ProgramData\Cybele Software\Thinfinity\Workspace\DB

3. Search for the API paragraph. It looks like the following
```
[API]
Key=E534332-A834-4P34-67FB-3FFA8A2C45A2
```

4. Copy the information 


Obtain the Profile/Model or the Profile Virtual Path from the resource
=

1. To obtain the Profile/Model or the Profile Virtual Path open the Configuration Manager

2. Under Access Profile select the profile that you want to share

3. From the resource copy any of the following information
    - Virtual Path
    - Access Key

Create the link
=

1. You could create the parameters on your own or you could use a form. To access the form, access to the following link http://server:9443/oturltest.html

2. In the form you need to complete the following information
      - Server: you need to complete with the information of your server (http://server:9443)
      - APIKEY: complete the API Key of the server that you obtain recently
      - Profile/Model Access Key or Profile Virtual Path: complete this information from the last step using Access Key / Virtual Path

3. Click GENERATE button

4. Then use CONNECT button

5. If everything goes well, copy and distribute the link
