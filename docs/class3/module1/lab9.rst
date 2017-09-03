Lab 9 – Configuring L7  Behavioral Attack Protection
====================================================

In this exercise we will use a protected object and show how behavioral DDoS works.

Task 1 – Create Protected Object and Launch Attack
--------------------------------------------------

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page and in the Protected Objects section click
   **Create**.

-  Configure a protected object using the following information, and
   then click **Create**.

   +------------------------+-----------------------------+
   | Name                   | Auction                     |
   +========================+=============================+
   | IP Address             | 10.1.20.101                 |
   +------------------------+-----------------------------+
   | Port / Protocol        | 80  TCP                     |
   +------------------------+-----------------------------+
   | VLAN (Selected)        | defaultVLAN (uncheck ANY)   |
   +------------------------+-----------------------------+
   | Protection Settings:   | Log and Mitigate            |
   | Action                 |                             |
   +------------------------+-----------------------------+
   | Protection Settings:   | HTTP                        |
   | DDoS Settings          |                             |
   +------------------------+-----------------------------+

- Make sure **Auction** is with a capital "A".

- Under the HTTP section make the following adjustments:

  - Set Proactive Bot Defense to "Disabled"

  - Set DOS tool to "None"

- When finished click **Create**

- From the Good Client CLI, issue the following command.

  .. code-block:: console

    ~/scripts/generate_clean_traffic.sh

.. NOTE::  This will need to run for approximately 10 minutes.

- From the DHD CLI issue the following commands:

  .. code-block:: console

  /root/scripts/l7bdos-reset.sh
  /root/scripts/l7-mon.sh

- Monitor the window.  When you see the following number go to 100, you will move on.

  |image91|

-  From the Attacker CLI issue the following command:

   .. code-block:: console

      ~/scripts/http_flood.sh

  |image92|

- Choose option **1**, "Attack Auction"

- You will see the attack start in the DHD SSH window:

  |image93|

- Let this run for 2 minutes.  Stop the attack by pressing "Enter"" a couple of times in the **Attacker**
  window the chhosing option "3" to stop the "Attack"

..NOTE:: The DHD doesnt record the end of the attack right away, it is very conservative, therefore you may have to wait 5
minutes to see the results.

  |image94|

- You can see in the top-left that a Behavioral Signature was creaated.

- Click on this link, then click on the Signature to see it.

  |image95|



.. |image91| image:: /_static/image57.png
   :width: 6.50000in
   :height: 1.87068in
.. |image92| image:: /_static/image58.png
   :width: 4.590033in
   :height: 1.17006in
.. |image93| image:: /_static/image59.png
   :width: 6.50000in
   :height: 1.87068in
.. |image94| image:: /_static/image60.png
   :width: 6.50000in
   :height: 4.58068in
.. |image95| image:: /_static/image61.png
   :width: 6.50000in
   :height: 3.72068in
