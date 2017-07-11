Lab 4 – Using Auto Thresholding
===============================

This exercise will simulate a newly configured Protected Object where
the security administrator is unsure what values to assign to a few
common vectors. Note that auto-thresholding is useful at both the Device
and Protected Object levels

**Note: This demo may place significant stress on the demo environment.
This may make the DHD UI less responsive. This is unavoidable since for
auto-thresholding to block, the attack must be damaging enough to cause
stress, which will push the CPU on the Virtual Environment very high.
Remember that this is a virtual environment with minimal resources for
lab under high stress and that the Hybrid Defender appliances mitigate
these attacks in dedicated hardware.**

Task 1 – Configure Auto Thresholding
------------------------------------

-  On the Good Client, if you have not already done so, start the
   network baselining. This step is needed if you didn’t start the good
   traffic generation in Exercise 2 or accidently stopped it.

   .. code-block:: console

      # sudo bash
      # cd ~/scripts
      # ./baseline_l4.sh

-  In the Hybrid Defender UI, in Quick Configuration, select the Server5
   Protected Object and verify that the IP and TCP vectors are all at
   default thresholds with auto-threshold disabled:

   +----------------------------+-------------+
   | Setting                    | Value       |
   +============================+=============+
   | All Detection Thresholds   | 30000 pps   |
   +----------------------------+-------------+
   | All Rate Limits            | Infinite    |
   +----------------------------+-------------+
   | Auto Thresholding          | Disabled    |
   +----------------------------+-------------+

   |image51|

-  In the Hybrid Defender CLI (BIGIP ssh window), restart
   auto-thresholding:

   .. code-block:: console

      # tmsh run security dos device-config auto-threshold-relearn
      # tmsh run security dos virtual name Server5 auto-threshold-relearn

In the Hybrid Defender WebUI, in the **Server5** Protected Object
configuration, enable auto-thresholding for the following vectors:
**ICMPv4 Flood, TCP SYN Flood, TCP Push Flood, TCP RST Flood, TCP SYN
ACK Flood** by selecting each vector and **clicking the Auto-Threshold
Configuration radio button**. When all vectors are configured, click
**Update** at the bottom of the screen.

-  In the Hybrid Defender WebUI, view the Auto Threshold event log by
   navigation to **Security>>Event Logs>>DoS>>Network>>Auto Threshold**.

   |image52|

The system is updating the detection thresholds. With auto-thresholding,
the system adjust the detection thresholds based on observed traffic
patterns. However, mitigation rate limits are always dynamic based on
detected system or protected object stress. If anomalous levels of
traffic are running, but there is no stress, the Hybrid Defender will
generate alerts but will not block traffic. Under stress, the rate
limits are automatically created and adjusted dynamically.

Task 2 – Create Stress to trigger Auto Thresholding and view Reports.
---------------------------------------------------------------------

-  Let’s create some stress with a Flood attack. In the Attacker CLI
   start the auto-threshold flood:

   .. code-block:: console

      # sudo bash
      # cd ~/scripts
      # ./autot_flood.sh

   This is a long duration attack. You can terminate it with Ctrl+C when
   finished.

-  In the Hybrid Defender WebUI, review the Auto Threshold event log.
   You will see that Rate limits are being automatically set and
   adjusted to mitigate the flood attack.

   |image53|

-  In the Hybrid Defender WebUI, view the DoS Overview. Note that the
   ICMP Flood attack is being mitigated and the rate limit thresholds
   for each of the auto-threshold vectors have been adjusted based on
   stress, including vectors that are not detecting or blocking an
   attack.

   |image54|

   |image55|

-  Select the filter type to **Virtual Server (DoS protected)** and
   **Server5** and view how various Thresholds are dynamically adjusted
   based on the stress

   |image56|

-  Terminate the attack in the Attacker CLI with Ctrl+C.

-  After the attack has ended, in the Hybrid Defender WebUI, navigate to
   the DoS Visibility page. Under Vectors, select ICMPv4 Flood. View
   various details.

   |image57|

-  **Clean-up**: On the Attacker CLI, if the attack is still running
   be certain to end it with Ctrl-C.

-  **Clean-up**: For repeatability, it is necessary to disable the
   auto-thresholding for the **ICMPv4 Flood, TCP RST Flood, TCP Push
   Flood, TCP SYN ACK Flood** and **TCP SYN Flood** vectors on the
   **Server5** protected object. **Switch them back to Manual
   Configuration.**

   |image58|

-  **Clean-up**: After disabling auto-thresholding, clear the learning
   on the Hybrid Defender CLI with:

   .. code-block:: console

      # tmsh run security dos device-config auto-threshold-relearn
      # tmsh run security dos virtual name Server5 auto-threshold-relearn

-  **Clean-up**: Stop the baseline traffic generation from the
   **good-client** if still running using CTRL+C

.. |image51| image:: /_static/class2/image52.png
   :width: 5.30972in
   :height: 3.63532in
.. |image52| image:: /_static/class2/image53.png
   :width: 5.30972in
   :height: 1.23126in
.. |image53| image:: /_static/class2/image54.png
   :width: 5.30972in
   :height: 2.24436in
.. |image54| image:: /_static/class2/image55.png
   :width: 5.30972in
   :height: 1.32482in
.. |image55| image:: /_static/class2/image56.png
   :width: 5.30972in
   :height: 1.30599in
.. |image56| image:: /_static/class2/image57.png
   :width: 5.30972in
   :height: 2.71126in
.. |image57| image:: /_static/class2/image58.png
   :width: 5.30972in
   :height: 2.48122in
.. |image58| image:: /_static/class2/image59.png
   :width: 2.31293in
   :height: 2.81771in

