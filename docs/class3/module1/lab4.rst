Lab 4 - Multi-vector Demo
=========================

In this simple demo you will launch a small number of network attacks
and show the configuration, logging and reporting capabilities of the
Hybrid Defender. The point of this demo is to provide context for a UI
walkthrough with some live data.

Task 1 - Access DoS Quick Configuration and display the **ServerNet** protected object
--------------------------------------------------------------------------------------

This protected object is defending all ports/protocols for 10.1.20.0/24,
which is the network behind the Hybrid Defender. Attacks will be
launched at 10.1.20.12, which is an interface on the LAMP server. Verify
that the following vectors are configured:

-  Add the TCP vectors for DDoS.

|image35|

- Click **Update** when finished.

You will now launch the attacks and show the behavior

- Open the following tabs in the DHD UI:

- **DoS Protection->Quick Configuration->ServerNet**

- **Security->DoS Protection->DoS Overview** (leave the filter at
  default: ’DoS Attack’)

- **Statistics->DoS Visibility**

- Access the Attacker System CLI and run the commands/attack

  .. code-block:: console

     # cd ~/scripts
     # sudo bash
     # ./multivector.sh

  - Click **Refresh** on the DoS Overview page. You will see some attacks
    mitigated by **Device Configuration** and some mitigated by the more
    specific settings on the **ServerNet Protected Object**.

  |image36|

Navigate to **Security->Event Logs->DoS->Network->Events**.

- Click on “custom search…” link.

- Drag one of the values from the “Attack Type” column into the custom
  search builder. From the Action column, drag Drop into the search
  builder. Click “Search”.

  |image37|

- Further explore the DoS Event logs as needed for your demo. For
  example, clear the search and identify the “Stop” and “Start” times
  for an attack, etc.

- In the Hybrid Defender WebUI, access the DoS Visibility reporting
  tool at **Statistics->DoS Visibility.**

.. NOTE:: DoS Visibility is a reporting tool, not a real-time
   monitoring tool. Events are displayed, much like other AVR-based
   reporting, in 5 minute windows. Do not expect events to be shown here
   immediately after running an attack. Quicker/real-time monitoring of on-going
   DoS attacks is best accomplished in the DoS Event Logs and DoS Overview areas
   of the WebUI.

- You should see the attacks in the timeline and a variety of details in
  the windows. Use the slider to shorten the timeframe if needed, and
  click the Network filter, to focus on L4 activities.

  |image38|

. NOTE:: You can select events from the timeline and see details about the attacks

  |image39|

  - Type **Ctrl + C** to stop the attack.

.. |image35| image:: /_static/image37.png
   :width: 6.41389in
   :height: 4.36042in
.. |image36| image:: /_static/image38.png
   :width: 7.29722in
   :height: 1.87424in
.. |image37| image:: /_static/image39.png
   :width: 7.35069in
   :height: 2.26358in
.. |image38| image:: /_static/image40.png
   :width: 7.40417in
   :height: 1.06667in
.. |image39| image:: /_static/image41.png
   :width: 7.28750in
   :height: 3.65347in
