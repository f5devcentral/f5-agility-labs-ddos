Lab 9 – Configuring L7  Behavioral Attack Protection
========================================

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
   | Port / Protocol        | 80  TCP                    |
   +------------------------+-----------------------------+
   | VLAN (Selected)        | defaultVLAN (uncheck ANY)   |
   +------------------------+-----------------------------+
   | Protection Settings:   | Log and Mitigate            |
   | Action                 |                             |
   +------------------------+-----------------------------+
   | Protection Settings:   | HTTP                        |
   | DDoS Settings          |                             |
   +------------------------+-----------------------------+

- Under the HTTP section make the following adjustments;
    Set Proactive Bot Defense to “Disabled”
    Set DOS tool to “None”
    Select “ Detection By source IP” and set the settings to:
      Detect: 15
      Attack Threshold: 25
    Select the Rate Limit check box
    Unselect the Sever Health box

- When finished click **Create*

-  From the Attacker CLI issude the following command:

.. code-block:: console
 curl auction.f5demo.com

- You should see a webpage returned


   .. code-block:: console

      # sudo bash
      # cd ~/scripts
      # ./slowloris.sh





Task 2 – Configure Protection/Mitigation, launch attack and view reports
------------------------------------------------------------------------

-  In the Hybrid Defender WebUI, access the **Auction** Protected
   Object.
