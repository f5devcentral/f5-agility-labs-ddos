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

  -  Set Behavioral to Standard Protection.

  - Make sure you check "Request Signature Detection"

  - Set Proactive Bot Defense to "Disabled"

  - Set DOS tool to "Report"

  |image96|

- When finished click **Create**

- From the Good Client CLI, issue the following command.

  .. code-block:: console

    ~/scripts/generate_clean_traffic.sh

.. NOTE::  This will need to run for approximately 10 minutes.

-  From the DHD CLI issue the following commands:

.. code-block:: console

  #/root/scripts/l7bdos-reset.sh
  #/root/scripts/l7-mon.sh

- Monitor the window.  When you see the following number go to 100, you will move on.

  |image91|

-  The health of the Protected Object will be shown. In general, a healthy system will show a value around .45. If the value is .5 consistently, then for some reason no learning is occurring and you should check your configuration and verify that baselining traffic is hitting the protected object in  question.

-  If the system has detected and is mitigating and attack, or not. This will show in the output of ‘info.attack’ signal. The two numbers in brackets indicate if there is an attack (1 = yes, 0 = no) and if the system is mitigating that attack (1 = yes, 0 = no).

-  The output will also include the ‘info.learning’ signal, which includes 4 comma-separated values that show the status of the admd behavioral dos learning:

   |image99|

   -  signal values: [baseline_learning_confidence, learned_bins_count , good_table_size , good_table_confidence]

   -  baseline learning_confidence in % - How confident the system is in the baseline learning.

      - This should be between 80% - 90%

   -  learned_bins_count - number of learned bins

      - This should be > 0

   -  good_table_size - number of learned requests

      - This should be > 4000

   -  good_table_confidence - how confident, as a percentage, the system is in the good table.

      - It must be 100% for behavioral signatures.

-  From the Attacker CLI issue the following command:

   .. code-block:: console

      ~/scripts/http_flood.sh

   |image92|

-  Choose option **1**, "Attack Auction"

-  You will see the attack start in the DHD SSH window:

   |image93|

-  In addition you will see the good client start returning a status of 000 as it is unresponsive. It no longer returns a Status 200. Until the DHD starts mitigation.

   |image97|

-  Once the DHD has enough data a Stable Signature is detected.

   |image98|

-  Let this run for 2 minutes.  Stop the attack by pressing "Enter"" a couple of times in the **Attacker** window the choosing option "3" to stop the "Attack"

   .. NOTE:: The DHD does not record the end of the attack right away, it is very conservative, therefore you may have to wait 5 minutes to see the results.

   |image94|

-  You can see in the top-left that a Behavioral Signature was created.

-  Click on this link, then click on the Signature to see it.

   |image95|

-  This concludes the DHD Hands on Labs.

.. |image91| image:: /_static/image57.png
   :width: 6.50000in
   :height: 1.87068in
.. |image92| image:: /_static/image58.png
   :width: 4.590033in
   :height: 1.17006in
.. |image93| image:: /_static/image66.png
   :width: 6.50000in
   :height: 2.11000in
.. |image94| image:: /_static/image60.png
   :width: 6.50000in
   :height: 4.58068in
.. |image95| image:: /_static/image61.png
   :width: 6.50000in
   :height: 3.72068in
.. |image96| image:: /_static/image67.jpg
   :width: 6.37000in
   :height: 4.32068in
.. |image97| image:: /_static/image68.png
   :width: 6.37000in
   :height: 4.32068in
.. |image98| image:: /_static/image69.png
   :width: 6.37000in
   :height: 4.32068in
.. |image99| image:: /_static/image63.png
   :width: 6.54000in
   :height: 0.68068in
