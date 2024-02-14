# Deploying Thinfinity VirtualUI in Microsoft Access

In this tutorial we will be configuring Thinfinity Virtual UI as an example of a typical use in a Access application. 

We define the following scenario
- We have a Microsoft Access application already working or use the example of this git
- We have a machine where we could configure Virtual UI to use it as PoC

Download and configure the application
=

1. Download the example https://files.cybelesoft.com/support/demosvui/virtualui_access.zip or use your own Access application.
   
2. Unzip the example and select the properties from the virtualUI_Access.accdb (or the accdb file of your application)

3. Select "Unblock" and click Apply 

4. Open the file and review that you have a modVirtualUI module created. Double click them, and review the code.. is very simple
```
Option Compare Database
Option Explicit
Dim vui As Variant
Dim run As Integer

Sub StartVirtualUI()
    If run <> 1 Then
        run = 1
        Set vui = CreateObject("Thinfinity.VirtualUI")
        'vui.DevMode = True
        'vui.DevServer.Port = 6080
        'vui.DevServer.Enabled = True
        vui.Start (60)
    End If
End Sub
```

5. The main module of this application is "Form_Contact_List". In the main module ( Form_Load() )we need to add the call to this module and needs to looks like the folllowing
```
Private Sub Form_Load()
    StartVirtualUI
End Sub
```
6. Save the Access Database

7. Copy the file to the Server where you want to run this application. This server needs to have Access already deployed.


Configure the Server
=

1. From the Thinfinity Configuration Manager select VirtualUI tab.

2. Select Create users on demand and complete the following
     - "virtualui" as Username prefix

3. If you will be using the application in a pool, you need to go Broker tab}

4. Select the pool, double click and in the Secondary Broker Pool windows select Sessions tab

5. In the Sessions tab, select Create users on demand and use "virtualui" as prefix. 

6. Select Apply to confirm the configuration


Configure the Profile
=

1. From the Thinfinity Configuration Manager select to Add a "VirtualUI app" profile.

2. Complete with the following information in the General Tab
     - Name: Contact Access Database
     - Click "Visible" and "Open in new tab"
     - Program path and file name: C:\Program Files(x86)\Microsoft Office\root\Office16\MSACCESS.EXE
     - Arguments: C:\access\VirtualUI_Access.accdb (where you copy the application)  and "Allow Browser arguments"
     - Start in : complete the following C:\Program Files(x86)\Microsoft Office\root\Office16

3. From the Credentials use the defaults (Use server´s account)

4. From the Permissions use the defaults (Inherit and Allow anonymous)

5. Apply the configuration

6. Test the application  
