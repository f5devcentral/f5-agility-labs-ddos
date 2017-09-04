Lab 1 – DDoS Hybrid Defender Setup
==================================

Task 1 – Initial Set-up
-----------------------

- Open a web browser and access supplied link.(Given at Location)

- Login to the BIG-IP Configuration Utility via your preferred browser. You
  will land on the welcome page.

.. NOTE:: When you first power up a F5 DHD device you would go through the
     steps of Licensing and Provisioning.  We have assigned the management
     IP, hostname, NTP and DNS servers.  We have already licenesed the device
     for you.

- Review and Verify the following: \ **System -> Configuration ->
  Device -> NTP** page. This should be already populated with **pool.ntp.org**

- Review and Verify the following: **System -> Configuration -> Device
  ->DNS** page.  This should be already populated with 8.8.8.8

- Click **System** and explore **Resource Provisioning** page.

  |image23|

.. NOTE:: The above task ensures that you are using a purpose built
   DDoS Hybrid Defender.  If you are familiar with other
   F5 Modules/Technology that you have used in the past, you will
   notice that we have none of those provisioned.  We have a new section DDOS Protection only.

Task 2 – DDoS Hybrid Defender iApp and Base Configuration
---------------------------------------------------------

- In the BIG-IP Configuration Utility, open **DoS Protection > Quick
  Configuration** page.

- If not already installed, Select Install RPM method of Onboard.

- Click **Install**.

  |image7|

- After the RPM is installed you will see the following:

- Open the About page.

  |image8|

- This page displays the current version of DDoS Hybrid Defender (DHD).
  You use this page to install and update the iApp LX version for DHD
  when newer versions are released.

  |image9|

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

.. NOTE:: A Bridged (VLAN Group) L2 configuration consistent with recommended
  practices for most deployments was automatically created. Also called "Bump in the Wire".  Can support Routed mode, SPAN and Netflow.

- Open the **Network > DNS Resolvers > DNS Resolver** list page and
  click **Create**.

- Enter default\_DNS\_resolver and then click **Finished**.

- A DNS resolver is required by bot signatures to allow for proper
  detection of benign search engines such as Google and Bing.

- On the Jumpbox desktop, SSH to the BIG-IP, it will log you in automatically
  as user ``root``, using the shortcut.

- Verify DNS by typing the following:

  ``nslookup api.f5silverline.com``

- Verify the Date by typing the following:

  ``date``

- If the BIG-IP system date is not accurate, correct it using the
  following commands:

  .. code-block:: console

     bigstart stop ntpd
     ntpdate 10.1.1.254
     bigstart start ntpd

Task 3 – Explore DHD Device Bandwidth Thresholds
--------------------------------------------------

- In the **DoS Protection > Quick Configuration** \page, open the
  **Protected Objects** page.

- In the **Network Protection** section click **Create**.

- This page is where you would supply values to protect your bandwidth and
  integrate with Silverline or use BGP to change your routing to go through a
  scrubbing center.

- CLick **Cancel** when done exploring the available settings.

  |image22|

- That completes the  initial setup for BIG-IP DDoS Hybrid Defender.

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
.. |image23| image:: /_static/image62.png
      :width: 7.29722in
      :height: 1.87424in
