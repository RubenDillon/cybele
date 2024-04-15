# Using Delphi with Thinfinity Virtual UI 

In this tutorial we will be deploying and configuring Thinfinity Virtual UI in a developer workstation that uses Delphi language. 

We define the following scenario
- We have a Windows workstation used by a developer with Embarcadero Delphi 11 Community Edition 
- We have the Thinfinity Workplace environment already deployed 

Download and configure the virtual UI
=

1. Ask to your Account Manager for the VirtualUI trial key and download a trial from the following link https://www.cybelesoft.com/es/thinfinity/virtualui/

2. Run the VirtualUI to deploy the application but only select "Development environment" using the defaults (C:\Program Files\Thinfinity\VirtualUI)


Test an example ( in Delphi )
=

1. Download the Delphi example from https://www.cybelesoft.com/thinfinity/virtualui/tutorials/

2. Unzip the file

3. Using "Embarcadero Delpi 11 Community Edition" open the  Delphi example

4. If you want to modify something of the application, you could do it and then Build it

5. Navigate for the application and experiment it with this demo 


Create your own application (in Delphi )
=

1. Open Embarcadero Delphi 11 Community Edition and under "Create New..." select Wndows VCL Application - Delphi

2. The project will be created and you will have the Designer for the default form open

3. Complete what you want into the form using the Toolbox, for example something easy like add a checkbox, a checked list box, a calendar, a Progress Bar and so on..

4. Run and test the application

Modify your own application to use VirtualUI
=

1. Copy the files VirtualUI*.* from the delphi example to the folder where you created your application.

2. Open Delphi 11 Community Edition with your project

3. Open Unit2.pas file. Change the "uses" part of the file and include VirtualUI_AutoRun
```

uses
  Winapi.Windows, VirtualUI_AutoRun, Winapi.Messages, System.SysUtils, System.Variants, System.Classes, Vcl.Graphics,
  Vcl.Controls, Vcl.Forms, Vcl.Dialogs, Vcl.StdCtrls;

```

4. Save the file and run the application (press F9)

5. You will receive a message saying that we need to run the windows application and a web mode, accept that and you will see the application running in windows form (like you already watched) and in a web browser.
   

Publishing your own application using VirtualUI in Thinfinity Workplace
=

1. Copy the Project exe file from the folder \Win32\Debug in a folder in the Thinfinity server.

2.Then we need to open the Thinfinity Configuration manager and create a new profile selecting "Add VirtualUI" app

3. Complete the form with the following information
    - Name : Delphi Demo  
    - Program file name: where you copy the exe file
    - Start in: the folder where the exe were copied
    - Accept the defaults
  
4. Apply the changes in the Thinfinity Configuration Manager

5. Connect to the environment and test the new connection
