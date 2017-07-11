Lab 6 – Configuring L7 Attack Protection
========================================

In this exercise we will use a protected object and enforce mitigation
for low and slow/encrypted layer 7 attacks.

Task 1 – Create Protected Object and Launch Attack
--------------------------------------------------

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page and in the Protected Objects section click
   **Create**.

-  Configure a protected object using the following information, and
   then click **Create**.

   +------------------------+-----------------------------+
   | Name                   | Server1                     |
   +========================+=============================+
   | IP Address             | 10.1.20.11                  |
   +------------------------+-----------------------------+
   | Port                   | 443                         |
   +------------------------+-----------------------------+
   | VLAN (Selected)        | defaultVLAN (uncheck ANY)   |
   +------------------------+-----------------------------+
   | Protection Settings:   | Log and Mitigate            |
   | Action                 |                             |
   +------------------------+-----------------------------+
   | Protection Settings:   | Yes (selected)              |
   | Silverline             |                             |
   +------------------------+-----------------------------+
   | Protection Settings:   | IPv4, TCP                   |
   | DDoS Settings          |                             |
   +------------------------+-----------------------------+

   |image72|

-  Launch attacks without any layer 7 protection configured

-  Open the following in separate tabs in the Hybrid Defender WebUI:

-  **DoS Protection>>Quick Configuration**

-  **Security>>Reporting>>DoS>>Analysis**

-  From a **firefox browser** go to https://10.1.20.11. Ignore SSL
   warning and Add Exception. Note that this bypasses the Hybrid
   Defender and accesses the server directly, showing the availability
   and/or performance of the site directly. Click around a few links.
   This is the site we will launch an attack against and mitigate.

-  Verify that the configuration is providing no L7 protections by
   taking the server offline with a slowloris attack. Note that apache
   will try to clean up the slow flows, but they will do so
   inefficiently and the server is impacted (which will show as an
   outage, missing objects and/or slower responsiveness). Run the
   slowloris attack from the Attacker CLI:

   .. code-block:: console

      # sudo bash
      # cd ~/scripts
      # ./slowloris.sh

   The tool will rapidly show the site offline (10-15 seconds, with trivial
   traffic load):

   |image73|

-  Refresh https://10.1.20.11 to show the effects of the attack. [Note
   that since we are running locally from the Win7 system in a
   virtualized environment, you may be able to access the site, however
   it will be slower and often the GIFs will not load. An internet user
   would not be able to “fight through” the attack to get to the server
   as often as a system on the local LAN.]

-  Stop the slowloris attack by using CTRL+C.

-  Start a more effective Slow Read attack.

   This attack is harder for DoS mitigation tools to mitigate and can be
   very effective even with a tiny number of concurrent connections
   trickling in very slowly to the server to fly below the radar of network
   detections. In our example we will open 10 connections per second and
   read the response data at 1 byte / sec. The attack would be effective
   even at 1 cps, it would just take a bit longer to build up the
   connections.

-  From the **Attacker** CLI/shell start the slowread attack:

   .. code-block:: console

      # sudo bash
      # cd ~/scripts
      # ./slowread.sh

   |image74|

As soon as the site is down (service available: NO), refresh
https://10.1.20.11 to show that it is down/slow/intermittent.

Task 2 – Configure Protection/Mitigation, launch attack and view reports
------------------------------------------------------------------------

-  In the Hybrid Defender WebUI, access the **Server1** Protected
   Object.

-  Enable SSL.

-  Select the default certificate and key. In your environment you would
   select a valid/cert key for your application.

-  Enable ‘\ **Encrypt Session to Server**\ ’ to avoid any server
   reconfiguration.

-  Enable the **HTTPS** mitigation family.

-  Click **Update**.

   |image75|

-  View the Attacker CLI/shell. The slow read attack is now no longer
   showing the site as down (service available: YES) because Proactive
   Bot Detection has mitigated the attack.

   |image76|

-  Refresh https://10.1.20.11 to see that the site behavior has returned
   to normal.

-  You were able to mitigate an encrypted layer 7 attack quickly and
   with only a few simple steps.

-  In the Hybrid Defender WebUI, view various reports in the
   **Security>>Reporting>>DoS>>Analysis**

-  **HTTP Report (Scroll towards the bottom) shows Proactive
   Mitigation**.

   |image77|

-  Stop the Slow Read attack by using CTRL+C.

**This concludes your hands on labs. In this class you learned how to
mitigated various DDoS attacks using F5 BIGIP Hybrid Defender (DHD). **

.. |image72| image:: /_static/class2/image73.png
   :width: 5.30972in
   :height: 4.87068in
.. |image73| image:: /_static/class2/image74.png
   :width: 3.76233in
   :height: 3.28646in
.. |image74| image:: /_static/class2/image75.png
   :width: 5.30972in
   :height: 4.10714in
.. |image75| image:: /_static/class2/image76.png
   :width: 5.30972in
   :height: 3.07640in
.. |image76| image:: /_static/class2/image77.png
   :width: 4.94792in
   :height: 4.12023in
.. |image77| image:: /_static/class2/image78.png
   :width: 5.30972in
   :height: 1.25578in
