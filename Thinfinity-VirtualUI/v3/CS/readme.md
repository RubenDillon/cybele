# Using Thinfinity VirtualUI with C#

In this tutorial we will be configuring Thinfinity Virtual UI with a C# application. We will expose a Windows Form C# application to the internet using Thinfinity Workplace. 

We define the following scenario
- We have a WinForm C# application already working or we will be using the example of this git
- We already have deployed Thinfinity Workplace 

Download and configure the application
=

1. Download the example https://www.cybelesoft.com/es/thinfinity/virtualui/tutorials/ or use your own Access application.
   
2. Unzip the file and open it using for example Visual Studio 2022

3. Open the file frmMyOwnApp.cs and go to "private void Initialize()" 

4. You will find a lot of code there, because this is a very good example with a lot of features, but the more important in those lines are the code that you need to put your Windows Form application in a web

5. The lines that you need pay attention are basically
```
....
vui = new Cybele.Thinfinity.VirtualUI();
....
vui.Start();

```

6. The rest of lines are to improve and configure in a particular way the relationship between how Windows Form works and the Web based implementation do the job

7. Copy the file to the Server where you want to run this application. 


Configure the Server
=

1. From the Thinfinity Configuration Manager select VirtualUI tab. Review or configure it.

2. Select Create users on demand and complete the following
     - "virtualui" as Username prefix

3. If you will be using the application in a pool, you need to go Broker tab

4. Select the pool, double click and in the Secondary Broker Pool windows select Sessions tab

5. In the Sessions tab, select Create users on demand and use "virtualui" as prefix. 

6. Select Apply to confirm the configuration


Configure the Profile
=

1. From the Thinfinity Configuration Manager select to Add a "VirtualUI app" profile.

2. Complete with the following information in the General Tab
     - Name: C# demo
     - Click "Visible" and "Open in new tab"
     - Program path and file name: the folder and file where you copy the application
     - Start in : folder where you copy the file

3. From the Credentials use the defaults (Use server´s account)

4. From the Permissions use the defaults (Inherit and Allow anonymous)

5. Apply the configuration

6. Test the application  
