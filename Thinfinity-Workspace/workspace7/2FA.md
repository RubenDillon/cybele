# Integrate 2FA security to Thinfinity Workplace 

In this tutorial we will be deploying second factor of authentication to Thinfinity Workplace v7. 

We define the following scenario
- We have already deployed Thinfinity Workspace
- We will be using Duo and OneLogin 2FA Security Login

 

DUO and Thinfinity Workspace configuration
=

1. Complete the steps to create a DUO account following the steps from the following link https://duo.com/docs/onelogin

2. Download and configure Duo Mobile in your smartphone

3. Access the Duo Dashboard

4. Go to Applications and then to "Protect an Application"

5. Search for  "Web SDK" and select Protect

6. Open the Thinfinity Configuration Manager, go to Authentication, select 2FA, press button ADD and then "DUO" 

7.  Complete the form with the following mapping
- Client ID to Integration Key
- Client secret to Secret Key
- API Hostname to API Hostname

8.  Apply the changes

9. Go to Methods and select Windows Logon

10. Modify the configuration where Displays 2FA Method. Select DUO

11. Apply the changes

12. Test the access to Thinfinity Dashboard. Now you will ask to authenticate with DUO
