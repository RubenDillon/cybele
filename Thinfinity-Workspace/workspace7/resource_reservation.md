# How-to use Resource Reservation

In this tutorial we will be using the feature Resource Reservation of Thinfinity Workplace v7. 

The main idea with this feature is give the possibility to manage when and who have accesss to different resources. Those resources could be Access to RDP sessions, applications or whatever you define in a profile. 
By default you have always access to the resource (if you have permissions), but you could determine the way on how the resource could be reserved.

For this How-to we will be creating two kind of permissions group.
  - Organizer: who could create, consume and allow to use the resource
  - Attendee: who could consume the resource

In this example, we want to create a Label where the resources that need reservation will be. Inside that label we will define different resources that by default no one will have access. The only way to access is by a Reservation with a approval process defined. When the  time comes to use the resource, it will be available for consume.

Generate Administrator permissions
=
1. Go to Configuration Manager

2. Go to Permissions and verify that your user (for example administrator) have the follwowing Permissions
    - Web Manager permission,
    - Administer Users and Groups


Create a Label and configure Permissions Group
=

1. Create a Label from the Configuration Manager of from the UI 

2. Go to Permissions and create the Permissions Group as follows. Select Permissions and select the gear icon next to "Permissions" in the table or the Permissions Group button in the UI of the Configuration Manager

3.  A Permission Group windows will be opened and depending in the UI.. you will have a button "+ Add" or the "+" to create a Group

4.  Create a Group called Organizer with the following information
    - Can Authorize
    - Can Self Book
    - Can Book Others
    - Allow Recurrency

6.  Create a Group called Atendee with the following information
    - Requires Authorization
    - Can Self Book
    - Can Get Booked
   
7. On Permissions Add for example Administrator as Admin and Organizer permissions

8. Then add a user already created (for example one of we created for RDP sessions) and select Atendee permission

Create Resources
=

1. Create any resource that you want to test

2. Assign it only to the label created recently (uncheck "\")

3. Review in Permissions that we have the following configuration
    - Enabled the option "Inherit label access permissions"
    - Unchecked "Allow anonymous access"

4. Resource Reservation
    - Modify the Booking limit
    - Modify Buffer

5. 
6. }df





Links used to create this documentation
=

- https://kb.cybelesoft.com/portal/en/kb/articles/setting-up-resource-reservation#Resource_Reservation_Permissions_Overview
- https://kb.cybelesoft.com/portal/en/kb/articles/individual-re
- https://thinfinity-remote-workspace-v7-docs.cybelesoft.com/resource-reservation/how-to-enable-resource-reservation
