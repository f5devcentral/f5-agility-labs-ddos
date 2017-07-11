Lab 2 – Start Baseline Traffic Generation
==============================================

Task 1 – Create Protected Objects that the baseline traffic will be targeting
-----------------------------------------------------------------------------

-  In the BIG-IP Configuration Utility, open the **DoS Protection>>Quick
   Configuration** page and in the **Protected Objects** section click
   **Create**.

-  Configure a protected object using the following information, and
   then click **Create**.

   +------------------------+--------------------+
   | Name                   | Server5            |
   +========================+====================+
   | IP Address             | 10.1.20.15         |
   +------------------------+--------------------+
   | Port                   | \*                 |
   +------------------------+--------------------+
   | Protocol               | All Protocols      |
   +------------------------+--------------------+
   | VLAN                   | Any                |
   +------------------------+--------------------+
   | Protection Settings:   | Log and Mitigate   |
   | Action                 |                    |
   +------------------------+--------------------+
   | Protection Settings:   | Yes (selected)     |
   | Silverline             |                    |
   +------------------------+--------------------+
   | Protection Settings:   | IPv4, TCP,         |
   | DDoS Settings          |                    |
   +------------------------+--------------------+

   |image29|

-  This protected object will be used for Auto-Threshold

   |image30|

Task 2 – Run Scripts to start L4 traffic generation – Good Traffic
------------------------------------------------------------------

-  Putty SSH (use the shortcut) to open a shell to the good client
   system.

-  Login as user : ubuntu. The session is preconfigured to authenticate
   with a certificate.

-  Start the auto-threshold baselining script with:

   .. code-block:: console

      # sudo bash
      # cd ~/scripts
      # ./baseline_l4.sh

.. NOTE:: Ignore the “sudo: unable to resolve host xjumpbox” when you
   issue the sudo bash command throughout the labs.

.. |image29| image:: /_static/class2/image31.png
   :width: 5.15178in
   :height: 4.97569in
.. |image30| image:: /_static/class2/image32.png
   :width: 5.30972in
   :height: 0.45031in

