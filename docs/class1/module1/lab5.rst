Lab 5 - Auto-threshold demo
===========================

This demo will simulate a newly configured Protected Object where the
security administrator is unsure what values to assign to a few common
vectors. Note that auto-thresholding is useful at both the Device and
Protected Object levels, and this should be pointed out to the audience.
In the interest of having a repeatable demo in an environment where many
different types of traffic are executed, we are focusing on the
per-VS/per-PO auto-thresholding

**→NOTE:** This demo may place significant stress on the demo
environment. Due to the virtual environment limitations, this may make
the DHD UI less responsive. This is unavailable since for
auto-thresholding to block, the attack must be damaging enough to cause
stress, which will push the CPU on the VE very high. Remind the audience
that this is a virtual environment under high stress and that the Hybrid
Defender appliances mitigate these attacks in dedicated hardware

- Open the following tabs in the Hybrid Defender WebUI:

- **DoS Protection->Quick Configuration**

- **Security->DoS Protection->DoS Overview** (set filter to Virtual
  Server->Server5)

- **Security->Event Logs->DoS->Network->Auto Threshold**

- **Statistics->DoS Visibility**

- On the Good Client, if you have not already done so, start the
  network baselining

  .. code-block:: console

     # cd ~/scripts
     # sudo bash
     # ./baseline\_l4.sh

- 3. In the Hybrid Defender UI, in Quick Configuration, select the
  Server5 Protected Object and verify that the IP and TCP vectors are
  all at default thresholds with auto-threshold disabled

  |image44|

- In the Hybrid Defender CLI, restart auto-thresholding

  .. code-block:: console

     # cd ~/scripts
     # ./autothreshold-reset.sh

- In the Hybrid Defender WebUI, in the Server5 Protected Object
  configuration, enable auto- thresholding for the following vectors:
  ICMPv4 Flood, TCP SYN Flood, TCP Push Flood, TCP RST Flood, TCP SYN
  ACK Flood by selecting each vector and clicking the Auto- Threshold
  Configuration radio button. When all vectors are configured, click
  **Update** at the bottom of the screen

  |image45|

- In the Hybrid Defender WebUI, show the Auto Threshold event log
  (**Security->Event Logs->Dos->Network->Auto Threshold).**

  |image46|

The system is updating the detection thresholds. With auto-thresholding,
the system adjusts the detection thresholds based on observed traffic
patterns. However, mitigation rate limits are always dynamic based on
detected system or protected object stress. If anomalous levels of
traffic are running, but there is no stress, the Hybrid Defender will
generate alerts but will not block traffic. Under stress, the rate
limits are automatically created and adjusted dynamically

- Let’s create some stress with a SYN Flood attack. In the Attacker CLI
  start the auto- threshold SYN flood

  .. code-block:: console

     # cd ~/scripts
     # sudo bash
     # ./autot\_flood.sh

This is a long duration attack. You can terminate it with ctrl-C when
finished.

- In the Hybrid Defender WebUI, show the Auto Threshold event log. Now
  you will see that Rate limits are being automatically set and
  adjusted to mitigate the flood attack

  |image47|

- In the Hybrid Defender WebUI, show the **Security > DoS >** **DoS
  Overview** page. Note that the SYN Flood attack is being mitigated
  and the rate limit thresholds for each of the auto-threshold vectors
  have been adjusted based on stress, including vectors that are not
  detecting or blocking an attack

  |image48|

- Terminate the attack in the Attacker CLI with ctrl-C

- After the attack has ended, in the Hybrid Defender WebUI, show the
  **DoS Visibility** page. Under Vectors, select TCP SYN Flood.
  Identify the Critical attack and show the details

  |image49|

- Clean-up. On the Attacker CLI, if the attack is still running be
  certain to end it with ctrl-C.

- Clean-up. For repeatability, it is necessary to disable the
  auto-thresholding for the ICMPv4 Flood, TCP RST Flood, TCP Push
  Flood, TCP SYN ACK Flood and TCP SYN Flood vectors on the Server5
  protected object

  |image50|

- Clean-up. After disabling auto-thresholding, clear the learning on
  the Hybrid Defender CLI with

  .. code-block:: console

     # cd ~/scripts
     # ./autothreshold-reset.sh

- Clean-up. After disabling auto-thresholding, clear the learning on
  the Hybrid Defender CLI with:

  .. code-block:: console

     # cd ~/scripts
     # ./autothreshold-reset.sh

Learn More
==========

***F5 DDoS Education***

Web based training and product information

-  Product Training https://university.f5.com/

-  `DDoS Protection Reference
   Architecture <https://hive.f5.com/docs/DOC-14753>`__

-  `DDoS Protection Recommended Best
   Practices <https://f5.com/solutions/architectures/ddos-protection/ddos-exclusive>`__

-  ***F5 DDoS Hybrid Defender overview and user guide***

***Silverline DDoS Education ***

Web based training and product information

-  Product Training https://university.f5.com/

`Onboarding Tech. Notes <https://support.f5.com/kb/en-us/products/silverline-waf.html>`__ on f5.

.. |image44| image:: /_static/image46.png
   :width: 5.60833in
   :height: 1.23949in
.. |image45| image:: /_static/image47.png
   :width: 6.60694in
   :height: 4.36736in
.. |image46| image:: /_static/image48.png
   :width: 6.58750in
   :height: 1.16667in
.. |image47| image:: /_static/image49.png
   :width: 6.63403in
   :height: 2.58056in
.. |image48| image:: /_static/image50.png
   :width: 6.49375in
   :height: 3.06042in
.. |image49| image:: /_static/image51.png
   :width: 6.60069in
   :height: 3.44722in
.. |image50| image:: /_static/image52.png
   :width: 2.02014in
   :height: 2.41389in

