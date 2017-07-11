Lab 1 – DDoS Hybrid Defender Setup
=======================================

Task 1 – BIG-IP Herculon Hybrid Defender Licensing and Provisioning
-------------------------------------------------------------------

→NOTE: When you first power up a F5 DHD device you would go through the
steps of Licensing and Provisioning. We have assigned the management IP,
hostname, NTP and DNS servers. You will be re-activating the license
using a new license key.

→NOTE: For Silverline device registration to function properly there
must be some specific considerations. The BIG-IP system must have a
unique device ID, which is comprised of attributes like Base MAC and
registration key. Hence we are re-licensing the device as all student
instances are spun up using the same license.

Use a web browser (Chrome in incognito mode) to log into the WebUI of
your DHD at https://10.1.1.245 . or use the bookmarked shortcut. Accept
the SSL warning and proceed to connect.

- Username : admin
- Password : f5DEMOs4u

-  Click **System>>License** and Click Re-activate

   |image6|

-  Click **Edit** button, replace the existing key by entering your
   student license key. Select the "Manual" radio button and Click Next.

   |image7|

-  Select all in the Dossier frame and copy. Click on "Click here to
   access F5 Licensing Server"

   |image8|

-  You will be taken to the F5 Activation Site. Enter your Dossier that
   you copied in the step above and click next. Accept User Legal
   Agreement - Check box to agree to terms of license and click next.

   |image9|

-  Select Everything the License frame and copy it.

   |image10|

-  Go back to your F5 DHD management and paste the contents copied from
   above into Step 3: License and Click Next.

   |image11|

-  The bigip will restart daemons and a window will pop up indicating
   system configuration has changed. Please wait for it to reconnect and
   click Continue. Your device is now licensed. Click Next.

   |image12|

-  On the Resource Provisioning page validate that Management and DDOS
   Protection are provisioned.

-  Click Submit once.

   |image13|

.. NOTE:: The above task ensures that you are using a purpose built DDoS
   Hybrid Defender. If you are familiar with other F5 Modules/Technology
   that you have used in the past, you will notice that we have none of
   those provisioned.

Task 2 – BIG-IP Herculon Hybrid Defender Initial Setup
------------------------------------------------------

-  Click **System>>Platform **

-  Change the hostname to
   **<yourfirstinitiallastname>.hybriddefender.f5agility.com**. For
   example, John Smith would register as
   **jsmith.hybriddefender.f5agility.com**. This is needed so that we
   can register your DHD to Silverline and uniquely identify it. Click
   Update.

   |image14|

-  Click **Device Management>>Devices** select the device and then click
   “Change Device Name…”. Update the device name to match the hostname
   you have chosen and click Update

   |image15|

-  Use Putty Shortcut to ssh to the F5 DHD and login as: root password:
   f5DEMOs4u

   |image16|

-  From the Hybrid Defender shell, restart services with:

# bigstart restart

.. NOTE:: Be patient as services are restarting. The DHD will change state to
   INOPERATIVE and then to Active. You can check in the ssh window when the
   prompt changes.

-  Click **System>>Configuration>>Device>>NTP** and review that NTP
   server is configured

-  Click **System>>Configuration>>Device>>DNS** and review that DNS
   server lookup is configured

DDoS Hybrid Defender Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the BIG-IP Configuration Utility, open the **DoS Protection>>Quick
   Configuration** page. Click Install. This installs the onboard
   package for quick configuration of DDoS Hybrid Defense

   |image17|

-  Once the installation is completed. Open the **About** page.

-  This page displays the current version of DDoS Hybrid Defender (DHD).
   You use this page to install and update the iApp LX version for DHD.

   |image18|

**The System is installed with the latest version of the iApp LX. The
below steps are for future reference on how to obtain the latest iApp LX
and use the above step to install. Do not download and install during
the Agility labs.**

-  Newer versions of iApp LX packages are made available on the **F5
   downloads** site under Security>>DDoS Hybrid Defender.

   |image19|

   |image20|

-  Open the **Network Configuration** page

   |image21|

-  In the **Default Network** section click **defaultVLAN**.

-  Configure the VLANs using following information, and then click Done
   Editing. Make sure to Click "Add"

   +---------------------+--------------------------+
   | Internal:           | 20                       |
   | VLAN Tag            |                          |
   +=====================+==========================+
   | Internal:           | 1.2 (Untagged checked)   |
   | Interfaces          |                          |
   |                     | (**Click Add)**          |
   +---------------------+--------------------------+
   | Internal:           | 10.1.20.240/21           |
   | IP Address / Mask   |                          |
   +---------------------+--------------------------+
   | External:           | 10                       |
   | VLAN Tag            |                          |
   +---------------------+--------------------------+
   | External:           | 1.1 (Untagged checked)   |
   | Interfaces          |                          |
   |                     | **(Click Add)**          |
   +---------------------+--------------------------+

   |image22|

   |image23|

-  Click **UPDATE**.

-  Open the **Network>>VLANs>>VLAN Groups** page and click
   **defaultVLAN**.

A transparent L2 configuration consistent with recommended practices for
most deployments was automatically created.

-  Open the **Network >> DNS Resolvers >> DNS Resolver** list page and
   click Create.

-  Enter **default\_DNS\_resolver** for the name and then click
   Finished.

A DNS resolver is required by bot signatures to allow for proper
detection of benign search engines such as Google and Bing. This is a
workaround and its setup is planned to be added to the Quick
Configuration, it’s not included in the version accompanying the
installed release for the labs.

-  In the BIG-IP putty ssh window verify DNS by typing (or copying and
   pasting) the following:

   ``nslookup api.f5silverline.com``

   |image24|

-  Type the following to verify the correct date setting:

   ``date``

-  Do this step only if the BIG-IP system date is not accurate, correct
   it using the following commands:

   .. code-block:: console

      bigstart stop ntpd
      ntpdate pool.ntp.org
      bigstart start ntpd

Configure Silverline Signaling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Use a Firefox web browser and access
   **https://portal.f5silverline.com**.

-  Log in as **dhd2017us@f5agility.com / HybridDefense!Wins!**

-  In the BIG-IP Configuration Utility, open the **DoS Protection
   >>Quick Configuration** page.

-  Open the **Silverline** page in Dos Protection>>Quick Configuration

   |image25|

-  Configure using following information, and then click Update. **Make
   sure to use all lowercase for username.**

   +-------------------+--------------------------------+
   | Username          | dhd2017us@f5agility.com        |
   +===================+================================+
   | Password          | HybridDefense!Wins!            |
   +-------------------+--------------------------------+
   | Service Address   | https://api.f5silverline.com   |
   +-------------------+--------------------------------+

-  In the Silverline portal browser page, open the **Config>>Hybrid
   Configuration>>Hybrid Device Management** page.

   |image26|

-  Locate your DHD device
   (<yourfirstinitiallastname>.hybriddefender.f5agility.com) and click
   Approve for ALL instances of YOUR device

   |image27|

Configure DHD Device Bandwidth Thresholds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  On the DHD WebUI go to **DoS Protection>>Quick Configuration**. In
   the Configuration Utility, open the **Protected Objects** page.

-  In the **Network Protection** section click Create.

-  Configure using following information, and then click **Save**.

   +------------------------------+-----------------+
   | Maximum Bandwidth: Specify   | 500             |
   +==============================+=================+
   | Scrubbing Threshold: Type    | Percentage      |
   +------------------------------+-----------------+
   | Scrubbing Threshold: Value   | 75              |
   +------------------------------+-----------------+
   | Advertisement Method         | Silverline      |
   +------------------------------+-----------------+
   | Scrubber Details: Type       | Advertise All   |
   +------------------------------+-----------------+

   |image28|

This completes the initial setup for BIG-IP DDoS Hybrid Defender
including registration with Silverline.

.. |image6| image:: /_static/class2/image8.png
   :width: 3.74625in
   :height: 2.79145in
.. |image7| image:: /_static/class2/image9.png
   :width: 3.36979in
   :height: 2.14664in
.. |image8| image:: /_static/class2/image10.png
   :width: 5.30972in
   :height: 1.91796in
.. |image9| image:: /_static/class2/image11.png
   :width: 3.60824in
   :height: 3.96232in
.. |image10| image:: /_static/class2/image12.png
   :width: 3.91667in
   :height: 5.15286in
.. |image11| image:: /_static/class2/image13.png
   :width: 5.30972in
   :height: 1.95868in
.. |image12| image:: /_static/class2/image14.png
   :width: 2.97253in
   :height: 2.56771in
.. |image13| image:: /_static/class2/image15.png
   :width: 5.30972in
   :height: 4.28328in
.. |image14| image:: /_static/class2/image16.png
   :width: 3.98781in
   :height: 3.99961in
.. |image15| image:: /_static/class2/image17.png
   :width: 4.55208in
   :height: 1.68995in
.. |image16| image:: /_static/class2/image18.png
   :width: 0.29687in
   :height: 0.29687in
.. |image17| image:: /_static/class2/image19.png
   :width: 2.50000in
   :height: 0.91619in
.. |image18| image:: /_static/class2/image20.png
   :width: 5.30972in
   :height: 1.67913in
.. |image19| image:: /_static/class2/image21.png
   :width: 5.30972in
   :height: 0.90955in
.. |image20| image:: /_static/class2/image22.png
   :width: 5.30972in
   :height: 2.50991in
.. |image21| image:: /_static/class2/image23.png
   :width: 5.20878in
   :height: 0.73340in
.. |image22| image:: /_static/class2/image24.png
   :width: 5.30972in
   :height: 1.20180in
.. |image23| image:: /_static/class2/image25.png
   :width: 5.30972in
   :height: 0.46555in
.. |image24| image:: /_static/class2/image26.png
   :width: 4.94271in
   :height: 1.19823in
.. |image25| image:: /_static/class2/image27.png
   :width: 3.88367in
   :height: 0.70006in
.. |image26| image:: /_static/class2/image28.png
   :width: 2.21239in
   :height: 2.00521in
.. |image27| image:: /_static/class2/image29.png
   :width: 5.30972in
   :height: 0.32270in
.. |image28| image:: /_static/class2/image30.png
   :width: 5.30972in
   :height: 0.70319in
