Monitor Windows Machines with Windows Management Instrumentation (WMI)
======================================================================

Introduction to WMI
-------------------

WMI is a set of specifications included in Microsoft Windows which
provides NEMS Linux with information about the status of Windows-based
hosts.

Disabled by default, WMI requires some configuration before NEMS Linux
can monitor the Windows host.

Setting up WMI
--------------

**Configure WMI on the Windows end**

In order for the agent to have access to query WMI to collect  and
database metrics, the agent must have permission to access both DCOM and
WMI.

Verify that the Remote Registry, Server, and the Windows Management
Instrumentation services are running.

-  Click **Start**.
-  Click **Run**.
-  Type *services.msc* and press “Enter”. This will open the Services
   dialogue.
-  Scroll down to the *Remote Registry service*. Verify that the service
   is started and is set to **Automatic**.

.. figure:: ../../../img/wmi_windows_01.png
  :width: 600
  :align: center
  :alt: Remote Registry


By default even if the Remote Registry service is started Windows 7 and
above systems will deny remote access to the registry.

-  Scroll down to the *Server* service. Verify that the service is
   started and set to **Automatic**.

.. figure:: ../../../img/wmi_windows_02.png
  :width: 500
  :align: center
  :alt: Server

-  Scroll down to the *Windows Management Instrumentation* service.
   Verify that it too is started and set to **Automatic**.

.. figure:: ../../../img/wmi_windows_03.png
  :width: 500
  :align: center
  :alt: Windows Management Instrumentation

.. note:: **The best practice is to use a Local account on the monitored host as the agent  user.**

**Where this is not possible, use these procedures to grant permissions
for a remote user.**

-  All windows workstations must have a user with the same local user
   name and password.
-  Local user account on the target computer must have explicit DCOM and
   WMI namespace access rights granted specifically for remote
   connections.
-  Local security policies must be enabled for “Classic - local users
   authenticate as themselves

**Grant minimal WMI permissions to the remote user**

This limits users other than those configured from remotely accessing
WMI.

.. note:: In the following example, replace "remoteuser" with the username of the user created on your Windows hosts.

On the monitored host machine, right-click on *My Computer*, and
navigate to Manage \| Services and Applications \| WMI Control.

.. figure:: ../../../img/wmi_windows_04.png
  :width: 400
  :align: center
  :alt: WMI Control

1. Right-click WMI Control and click Properties.
2. In the WMI Control Properties dialog box, click the Security tab.
3. Expand the Root node and select CIMV2, then click Security.

.. figure:: ../../../img/wmi_windows_05.png
  :width: 400
  :align: center
  :alt: CIMV2

Select the user in the *Group or user names* box. If not listed
select **Add**.

.. figure:: ../../../img/wmi_windows_06.png
  :width: 400
  :align: center
  :alt: Add User to CIMV2

Type in the user name and click **Check Names**.

.. figure:: ../../../img/wmi_windows_07.png
  :width: 400
  :align: center
  :alt: Check Names

Grant the required permissions to the remote user by enabling the
following check boxes in the Allow column:

1. Execute Methods
2. Enable Account
3. Remote Enable
4. Read Security

.. figure:: ../../../img/wmi_windows_08.png
  :width: 400
  :align: center
  :alt: Execute Methods and Enable Account

.. figure:: ../../../img/wmi_windows_09.png
  :width: 400
  :align: center
  :alt: Remote Enable and Read Security

**To grant DCOM permissions to a remote user**

This limits users other than those configured from remotely accessing
WMI.

1. On the monitored host machine, at the Windows Run prompt,
   type *DCOMCNFG* and press Enter.
2. In the Component Services dialog box that opens, navigate to
   Component Services \| Computers \| My Computer.
3. Right-click **My Computer** and click **Properties**.
4. Select the **Default Properties** tab.
5. To enable DCOM, select the *Enable Distributed COM on this
   computer* checkbox.
6. Click **Apply**.

.. figure:: ../../../img/wmi_windows_10.png
  :width: 400
  :align: center
  :alt: Enable Distributed COM

1. In the My Computer Properties dialog box, click the COM Security tab.
2. Under Access Permissions, click Edit Limits. Give your chosen user
   Remote Access permission. (If user is not in the list of names follow
   steps above to add the user).
3. In the Launch and Activation Permissions area, click Edit Limits.
4. NOTE: In some cases, you also need to click the Edit Default and
   perform the succeeding steps.
5. In the Launch Permission dialog box, add the user or group name
   necessary for the remote user.

.. figure:: ../../../img/wmi_windows_11.png
  :width: 500
  :align: center
  :alt: COM Security

Grant the remote user all the permissions available in the Permissions
for Administrators area by enabling all of the check boxes in the Allow
column.

.. figure:: ../../../img/wmi_windows_12.png
  :width: 400
  :align: center
  :alt: Permissions

Click **OK** and/or **Yes** to close the dialog boxes.

**Enable Classic Security policies for Windows Systems that are not part
of a domain.**

1. Open the Control panel, and go to *Administrative Tools* → *Local
   Security Policy*.
2. The Local Security Settings window appears.
3. Go to *Local Policies* → *Security Options*.
4. Change the value of *Network access: Sharing and security model for
   local accounts.* to **Classic**.

.. figure:: ../../../img/wmi_windows_13.png
  :width: 600
  :align: center
  :alt: Security Options

**Open the Windows firewall for WMI traffic**

Enter the following in an Administrator Command Prompt:

.. code-block:: console

   netsh advfirewall firewall set rule group=”windows management
   instrumentation (wmi)” new enable=yes

**Add Your Windows User to NEMS SST**

Enter the username and password of the user created on the Windows
devices who was granted access to the WMI data.

.. figure:: ../../../img/nems_sst_windows_domain_credentials.png
  :width: 500
  :align: center
  :alt: SST Domain Credentials

Basic Configuration of Windows Devices In NEMS Linux Using WMI Check Commands
-----------------------------------------------------------------------------

**Adding check_win_xxxx Commands in Advanced Services**

A) In NEMS NConf click the *Add* button next to *Advanced Services*.
Then click the drop-down arrow in the *check command* select list, and
scroll down to the check_win\_\ *xxx* commands to choose the command you
wish to add.

.. figure:: ../../../img/nconf_add_advanced_service.png
  :width: 500
  :align: center
  :alt: Add advanced service

B) Configure the required fields and be sure to assign the Advanced
Service to your Windows host. Then click *Submit*. You will see your new
command in the list of available Advanced Services.

Repeat Steps A and B above as needed to add any further
check_win\_\ *xxx* services you require.

When complete these commands will now be available in the *Advanced
Services* list.

.. figure:: ../../../img/nconf_advanced_services_check_wmi.png
  :width: 500
  :align: center
  :alt: Advanced services list

Configure these Advanced Services as required to meet your needs and
assign them to one or multiple Windows devices.

Special Thanks to Bill Marshall
-------------------------------

This documentation would not be possible were it not for the effort of
Bill, also known as UltimateBugHunter-NitPicker on our Discord server.
Bill setup a test environment, tested, documented, and screen captured
the entire setup process and submitted it for inclusion in the official
docs. Thanks Bill!
