# Using a Developer workstation with Thinfinity Virtual UI 

In this tutorial we will be deploying and configuring Thinfinity Virtual UI in a developer workstation. 

We define the following scenario
- We have a Windows workstation used by a developer with Visual Studio 2022 Community Edition 
- We have the Thinfinity Workplace environment already deployed 

Download and configure the virtual UI
=

1. Ask to your Account Manager for the VirtualUI trial key and download a trial from the following link https://www.cybelesoft.com/es/thinfinity/virtualui/

2. Run the VirtualUI to deploy the application but only select "Development environment" using the defaults (C:\Program Files\Thinfinity\VirtualUI)


Test an example
=

1. IF you don´t have .Net Framework 4.8 Developer Pack already deployed, install it because in this example we need an old .NET framework to show a real life situation... how to provide a quick modern interface to an old application.

2. Download the Visual C# .NET example from https://www.cybelesoft.com/thinfinity/virtualui/tutorials/

3. Unzip the file

4. Using "Developer Command Prompt for VS 2022" move up to the directory where you place the VB.NET example

5. Type "code ." and Visual Studio 2022 Community Edition will be open with the VB.NET solution

6. If you want to modify something of the application, you could do it and then Build it


Deploy and access the example  
= 

1. First we will run the demo application to show how it is in a "normal" situation.. for that we need to go to <your folder>\vbnet\WindowsApp2\WindowsApp2\bin\Debug and run the WindowsApp2.exe

2. Now we move this application to the Thinfinity Workplace environment

3. We need to copy this file to a folder of the server where we want to run this application

4. Then we need to open the Thinfinity Configuration manager and create a new profile selecting "Add VirtualUI" app

5. Complete the form with the following information
    - Name : C# example
    - Check the option "Open in new tab" to start the app in a separated tab
    - Program file name: where you copy the exe file
    - Start in: the folder where the exe were copied
    - Accept the defaults
  
6. Apply the changes in the Thinfinity Configuration Manager

7. Connect to the environment and test the new connection

Create your own application
=

1. Open Visual Studio and select "Create a new project..."

2. For this example we will be using the following data
    - C#
    - Windows
    - Desktop
  
3. Select "Windows Forms App (.Net Framework)" option

4.  Complete the following
    - Project Name: HelloWorld
    - Location: your default location
    - Solution name: HelloWorld

5. Select .NET 4.8 

6. The project will be created and you have the Designer for the Form1.vb open

7. Complete what you want into the form using the Toolbox, for example something easy like add a checkbox, a checked list box, a calendar, a Progress Bar and so on..

8. Save Form1

9. Run and test the application

10. Close Visual Studio

Modify your own application to use VirtualUI
=

1. Copy the file Thinfinity.VirtualUI.cs from the folder c:\Program Files\Thinfinity\VirtualUI\Dev\dotNet\ in the project folder.

2. Open Visual Studio with your project

3. (Optional) Right-click on the project name in the 'Solution Explorer' panel and then select 'Add' - 'Existing Item'. Look for the Thinfinity.VirtualUI.cs file, which is typically located in c:\Program Files\Thinfinity\VirtualUI\Dev\dotNet\.

4. Right click the file Thinfinity.VirtualUI.cs and select "Include in the project"

5. Open Form1.cs file. The file need to looks like the following
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using Cybele;
using Cybele.Thinfinity;

namespace HelloWorld
{
    public partial class Form1 : Form
    {

        public VirtualUI vui;
        public Form1()
        {
            vui = new VirtualUI();
            vui.Start();
            vui.StdDialogs = true;

            InitializeComponent();
        }
    }
}

```

6. Save the file and run the application

7. You will receive a message saying that we need to run the windows application and a web mode, accept that and you will see the application running in windows form (like you already watched) and in a web browser.
   

Publishing your own application using VirtualUI in Thinfinity Workplace
=

1. Copy the HelloWorld.exe file from the folder <your folder>\HelloWorld\HelloWorld\bin\Debug in a folder in the Thinfinity server.

2.Then we need to open the Thinfinity Configuration manager and create a new profile selecting "Add VirtualUI" app

3. Complete the form with the following information
    - Name : Hello World C# 
    - Check the option "Open in new tab" to start the app in a separated tab
    - Program file name: where you copy the exe file
    - Start in: the folder where the exe were copied
    - Accept the defaults
  
6. Apply the changes in the Thinfinity Configuration Manager

7. Connect to the environment and test the new connection
