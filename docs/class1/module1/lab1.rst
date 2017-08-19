Lab 1 – DDoS Hybrid Defender Setup
==================================

Estimated completion time: 45 minutes

Task 1 – Initial Set-up
-----------------------

- Open a web browser and access supplied link.(Given at Location)

- Login to the BIG-IP Configuration Utility via your preferred browser?

 .. NOTE:: When you first power up a F5 DHD device you would go through the
  steps of Licensing and Provisioning.  We have assigned the management
  IP, hostname, NTP and DNS servers.  You will be re-activating the
  license using a new license key.

- On the **System > Platform** page configure the following, and then
  click **Update**.

  +----------------------------------------+--------------------------+
  | Host Name                              | <your name>.f5demo.com   |
  +========================================+==========================+
  | Root Account (Password and Confirm)    | f5DEMOs4u                |
  +----------------------------------------+--------------------------+
  | Admin Account (Password and Confirm)   | f5DEMOs4u                |
  +----------------------------------------+--------------------------+

- This will log you out. Log back in

- On **Device Management->Devices** select the device and then click
  “\ **Change Device Name**\ …”. Update the device name to match
  the hostname you have chosen. Retain Current Authority

- Click **Update** to save changes

- Review and Verify the following: \ **System -> Configuration ->
  Device -> NTP** page add **pool.ntp.org** to the Time Server
  List, and then click **Update**.

- Review and Verify the following: **System -> Configuration -> Device
  ->DNS** page add 8.8.8.8 to the DNS Lookup Server List, and then
  click **Update**.

- Open the **System > License** page and **re-activate** the BIG-IP
  system using the new development license key using Manual mode.
  Copy and Paste License file.

  |image6|

- Click **Next** and explore **Resource Provisioning** page

.. NOTE:: The above task ensures that you are using a purpose built
  DDoS Hybrid Defender.  If you are familiar with other
  F5 Modules/Technology that you have used in the past, you will
  notice that we have none of those provisioned.

- When done click **Submit**.

- Access the Jumbox via RDP. PuTTY into the Hybrid Defender. Login with
  ``root`` and restart services

  ``bigstart restart``

Take a break, ask questions, talk to your neighbor ..it will take
several minutes to restart

.. NOTE:: You MUST re-activate, even if the current license key
   hasn’t expired. For Silverline access each BIG-IP system must use a
   unique license key.

Task 2 – DDoS Hybrid Defender iApp and Base Configuration
---------------------------------------------------------

- In the BIG-IP Configuration Utility, open **DoS Protection > Quick
  Configuration** page.

- Select Install RPM method of Onboard

- Click **Install**

  |image7|

- Open the About page

  |image8|

- This page displays the current version of DDoS Hybrid Defender (DHD).
  You use this page to install and update the iApp LX version for DHD
  when newer versions are released.

  |image9|

- In the BIG-IP Configuration Utility, click **iApps**, **Templates**
  and **Import**, importing the two templates located on the jumpbox documents
  folder.

  |image10|

- Use the **Browse** and **Upload** buttons. (You will do this once for
  each template)

- In the BIG-IP Configuration Utility, open **iApps > Application
  Services** and select **Create**

  |image11|

- You will be creating two services based on the two Silverline
  Templates:

  - F5.silverline\_connector

  - F5.silverline\_dos\_monitor

  |image12|

- Use the default settings for the Silverline connector

- Use the Silverline username and password supplied

.. Note:: This is case sensitive – make sure email address is all lowercase

|image13|

|image14|

- Create the 2\ :sup:`nd` service for the Silverline DOS Monitor
  (f5.silverline\_dos\_monitor)

  |image15|

- Use the default settings for the dos\_connector except for Volumetric
  Attack Event Monitoring, switch the network object from interface to
  VLAN.

  |image16|

- Open the **DoS Protection > Quick Configuration** **Network
  Configuration** page.

  |image17|

- In the Default Network section click **default VLAN**.

- Configure the VLANs using following information, and then click
  **Done Editing**.

  +-----------------------+----------------------------------+
  | \ **Internal:         | 20                               |
  | VLAN Tag**            |                                  |
  +=======================+==================================+
  | **Internal:           | 1.2 Untagged                     |
  | Interfaces**          |                                  |
  +-----------------------+----------------------------------+
  | **Internal:           | 10.1.20.240/21 (Click **Add**)   |
  | IP Address / Mask**   |                                  |
  +-----------------------+----------------------------------+
  | **External:           | 10                               |
  | VLAN Tag**            |                                  |
  +-----------------------+----------------------------------+
  | **External:           | 1.1 Untagged (Click **Add**)     |
  | Interfaces**          |                                  |
  +-----------------------+----------------------------------+

  |image18|

- At the bottom of the page click **Update** to create the default
  network.

- Open the **Network > VLANs > VLAN Groups** page and click
  **defaultVLAN**.

- A Bridged (VLAN Group) L2 configuration consistent recommended
  practices for most deployments was automatically created

- Open the **Network > DNS Resolvers > DNS Resolver** list page and
  click **Create**.

- Enter default\_DNS\_resolver and then click **Finished**.

- A DNS resolver is required by bot signatures to allow for proper
  detection of benign search engines such as Google and Bing.

- On the Jumpbox desktop, PuTTY to the BIG-IP

- Login as ``root``

- Verify DNS by typing the following

  ``nslookup api.f5silverline.com``

- Type the following to verify the correct date setting:

  ``date``

- If the BIG-IP system date is not accurate, correct it using the
  following commands:

  .. code-block:: console

     bigstart stop ntpd
     ntpdate 10.1.1.254
     bigstart start ntpd

Task 3 – Configure Silverline Signaling
---------------------------------------

- In the BIG-IP Configuration Utility, open the **DoS Protection >
  Quick Configuration** page.

- Open the **Silverline** page.

  |image19|

- Configure using following information, and then click **Update**.

  +-------------------+--------------------------------+
  | Username          | dhd2017us@f5agility.com        |
  +===================+================================+
  | Password          | HybridDefense!Wins!            |
  +-------------------+--------------------------------+
  | Service Address   | https://api.f5silverline.com   |
  +-------------------+--------------------------------+

- Register the device with the Silverline iApp, to provide bandwidth
  utilization updates in **iApps->Application
  Services->Applications->silverline\_connector**. In the iApp, select
  **Reconfigure** and then click **Finished**. This will cause the iApp
  to register under the new device name.

- Use a web browser and access https://portal.f5silverline.com.

- Log in with the above credentials

- In the Silverline browser, open the **Config->Hybrid
  Configuration->Hybrid Device Management** page\ **.**

  |image20|

- Locate your DHD device by searching for (<your name
  prefix>.f5demo.com) .

- Click the **Approve** button to approve device registration.

  |image21|

.. NOTE:: For Silverline device registration to function properly there
   must be some specific considerations. The BIG-IP system must have a
   unique device ID, which is comprised of attributes like Base MAC and
   registration key. In Ravello and similar virtual environments the Hybrid
   Defender VE must be re-licensed uniquely each time.

Task 4 – Configure DHD Device Bandwidth Thresholds
--------------------------------------------------

- In the **DoS Protection > Quick Configuration** \page, open the
   **Protected Objects** page.

- In the **Network Protection** section click **Create**.

- Configure using following information, and then click **Save**.

  +--------------------------------------+-----------------+
  | **Maximum Bandwidth: Specify**       | 500             |
  +======================================+=================+
  | **Scrubbing Threshold: Type**        | Percentage      |
  +--------------------------------------+-----------------+
  | **1.20Scrubbing Threshold: Value**   | 75              |
  +--------------------------------------+-----------------+
  | **Advertisement Method**             | Silverline      |
  +--------------------------------------+-----------------+
  | **Scrubber Details: Type**           | Advertise All   |
  +--------------------------------------+-----------------+

  |image22|

- That completes the setup for BIG-IP DDoS Hybrid Defender with
  Silverline integration.

.. |image6| image:: /_static/image8.png
   :width: 6.64028in
   :height: 3.15377in
.. |image7| image:: /_static/image9.png
   :width: 6.64028in
   :height: 1.13399in
.. |image8| image:: /_static/image10.png
   :width: 6.44722in
   :height: 0.53333in
.. |image9| image:: /_static/image11.png
   :width: 6.64028in
   :height: 1.84583in
.. |image10| image:: /_static/image12.png
   :width: 6.64028in
   :height: 2.01931in
.. |image11| image:: /_static/image13.png
   :width: 6.64028in
   :height: 1.12569in
.. |image12| image:: /_static/image14.png
   :width: 4.83435in
   :height: 2.68715in
.. |image13| image:: /_static/image15.png
   :width: 6.51491in
   :height: 3.29901in
.. |image14| image:: /_static/image16.png
   :width: 6.51491in
   :height: 1.61067in
.. |image15| image:: /_static/image17.png
   :width: 5.82741in
   :height: 2.98196in
.. |image16| image:: /_static/image18.png
   :width: 6.64028in
   :height: 4.05694in
.. |image17| image:: /_static/image19.png
   :width: 5.20878in
   :height: 0.73340in
.. |image18| image:: /_static/image20.png
   :width: 6.14167in
   :height: 0.76803in
.. |image19| image:: /_static/image21.png
   :width: 3.88367in
   :height: 0.70006in
.. |image20| image:: /_static/image22.png
   :width: 3.57500in
   :height: 2.71750in
.. |image21| image:: /_static/image23.png
   :width: 6.64028in
   :height: 1.65186in
.. |image22| image:: /_static/image24.png
   :width: 6.64028in
   :height: 3.17847in
