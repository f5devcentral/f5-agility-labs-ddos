Lab 4 – Configuring L7 Attack Protection
========================================

In this exercise we will use a protected object and enforce mitigation for low and slow/encrypted layer 7 attacks.

.. NOTE:: We will first launch attacks with no protection to see the results.  Then enable protection and compare the results.

Task 1 – Use Firefox to access Website and use Attacker to bring it down.
-------------------------------------------------------------------------

- Open the following in separate tabs in the Hybrid Defender Web UI:

- **DoS Configuration >> Dos Overview**

- **Visibility >> Event Logs >> DoS >> Application Events**

- From a the **Firefox browser** on the jumphost go to https://10.1.20.11. Ignore SSL warning and Add Exception.

.. NOTE:: This bypasses the Hybrid Defender and accesses the server directly, showing the availability and/or performance of the site directly.

Click around a few links. This is the site we will launch an attack against and mitigate.

- Verify that the configuration is providing no L7 protections by taking the server offline with a slowloris attack.

.. NOTE:: Apache will try to clean up the slow flows, but they will do so inefficiently and the server is impacted (which will show as an outage, missing objects and/or slower responsiveness).

- Run the slowloris attack from the Attacker CLI:

.. code-block:: console

  # cd ~/scripts
  # ./slowloris.sh

- The tool will rapidly show the site offline (10-15 seconds, with trivial traffic load)

- Refresh https://10.1.20.11 to show the effects of the attack. Click links on the page.

.. NOTE:: Since we are running locally from the Win7 system in a virtualized environment, you may be able to access the site, however it will be slower and often the GIFs will not load. An Internet user would not be able to “fight through” the attack to get to the server as often as a system on the local LAN.

- Stop the slowloris attack by using CTRL+C.

Start a more effective Slow Read attack.

.. NOTE:: This attack is harder for DoS mitigation tools to mitigate and can be very effective even with a tiny number of concurrent connections trickling in very slowly to the server to fly below the radar of network detections.
 In our example we will open 10 connections per second and read the response data at 1 byte / sec. The attack would be effective even at 1 cps, it would just take a bit longer to build up the connections.

- From the **Attacker** CLI/shell start the slowread attack:

.. code-block:: console

  # cd ~/scripts
  # ./slowread.sh

As soon as the site is down (service available: NO) in the Attacker CLI, refresh https://10.1.20.11 to show that it is down/slow/intermittent.

- In the |dhd| GUI access the tabs you opened previously and notice no attacks were detected.

- Stop the slowread attack by using CTRL+C.

Task 2 – Create Protection Profile for Dos https Object
-------------------------------------------------------

- In the BIG-IP Configuration Utility, open the **DoS Configuration >> Protection Profiles** page and click the **Create** button.

- Name the profile dos_HTTPS and **select** the HTTP Families Vectors.
  Change the settings depicted in the image below.

- Hover in the HTTP box and **Click** in the ""White Space""
- Click "Per Source IP requests"
- Click the HTTP Group Configuration Link. On the Right Side.
- Under Behavioral and Stress Based Attributes, Set the Operation Mode to **Blocking**
- Leave Threshold Mode in Manual.
- Under Behavioral Based, Set the Mitigation to **Standard Mitigation**
- Ensure Signature Detection is Selected.
- Under Mitigation select Request Blocking "Rate Limit"
- **Commit Changes to System**

|image402|

Task 3 – Modify Default Eviction Policy
---------------------------------------

.. IMPORTANT:: When making a Slow-Read attack, a client establishes a connection to the Server and sends an appropriate HTTP request, However, the client reads
 the response at a very slow speed. Some Slow-Read attack clients don’t read the response at all for long time and then starts reading data
 one byte at a time just before the idle connection timeout. The clients sends a Zero window to the server which makes the Server to assume that the client is busy reading the data. As a result, the server to keeps the connection opened for long period of time. Such multiple connections to the Server will consume the resources of the server and can make the server unresponsive to the new and genuine requests.

In order to mitigate such an attack we need to make adjustments to the default-eviction-policy.

- Navigate to Dos Configuration >> Eviction Policy and **Click** on the default-eviction-policy.

- Under "Slow Flow Monitoring" choose "enable" and change the value to 1024.
- Under the "Grace Period" change the default value to 5 Seconds.
- Under "Slow Flow Throttling" change the value to "absolute" and 50 connections as the value.
- Click **Update** when finished.

|image403|

What we are doing here is setting up the policy to recognize and then evict slow flows through the |dhd|.



Task 3 – Create Protected Object
--------------------------------

- In the BIG-IP Configuration Utility, open the **DoS Configuration >> Protected Objects** page and in the **Protected Objects** section click the **Create** dropdown and select **Protected Object**.

|image401|

- Configure a protected object using the following information, and then click **Save**.

+------------------------+-----------------------------+
| Name:                  | Server_HTTPS                |
+------------------------+-----------------------------+
| Destination Address:   | 10.1.20.11                  |
+------------------------+-----------------------------+
| Service Port:          | 443                         |
+------------------------+-----------------------------+
| Protocol:              | TCP                         |
+------------------------+-----------------------------+
| Service Profile:       | http                        |
+------------------------+-----------------------------+
| Protection Profile:    | dos_HTTPS                   |
+------------------------+-----------------------------+
|  Eviction Policy:      | default-eviction-policy     |
+------------------------+-----------------------------+
| VLAN(s):               | default_VLAN                |
+------------------------+-----------------------------+
| Logging Profile(s):    | local-dos                   |
+------------------------+-----------------------------+




Task 4 – Configure Protection/Mitigation
----------------------------------------

Next we need to modify the VS we created to pass traffic.

- At the bottom of the Menu **Click** the "Show Advanced Menu"" >> Local Traffic >> Virtual Servers >> Virtual Server List >> Select the Server_HTTPS VS.

- Under ""Configuration"" Select **Advanced**
- Ensure the following are Set:
- SSL Profile (Client) to **clientssl**
- SSL Profile (Server) to **serverssl**
- Source Address translation to **none**
- Uncheck Address translation
- Uncheck Port translation
- Set Transparent Next Hop to the Internal Interface Bridge Member of the VLAN. If you have followed along, it will be the interface associated with 1.2
- To figure out interface type "tmsh list net vlan" You want the next hop to be the internal interface.

- Click **Update**

Next we need to modify the Virtual Server Address List Address

- At the bottom of the Menu **Click** the "Show Advanced Menu"" >> Local Traffic >> Virtual Servers >> Virtual Address List >> Select the address 10.1.20.11

- Under **Configuration** disable/ uncheck ARP.

- Click **Update**


Task 5 – Attack Website notice Mitigation/Protection
----------------------------------------------------
- From the **Attacker** CLI/shell start the slowread attack:

.. code-block:: console

  # cd ~/scripts
  # ./slowread.sh

- From Firefox access the website and click around.  You will notice although the website is being DoS'd via slow read, the website remains available.

- If you look in the command window of the Attacker the tool even reports the site off-line, although the site remains available.

- On the DHD CLI run the following command.

.. code-block:: console

   #tmctl -w 200 virtual_server_stat -s name,clientside.cur_conns,clientside.slow_conns,clientside.slow_killed,serverside.cur_conns,serverside.slow_conns,serverside.slow_killed

- Notice as the slow connections increase, the |dhd| will start killing them.

-  **Clean-up**: On the Attacker CLI, if the attack is still running be certain to end it with Ctrl-C.

-  **Clean-up**: After stopping the attack, delete the Server Protected Object.

.. |image401| image:: /_static/class5/protectedobject.png
   :width: 1641px
   :height: 366px
.. |image402| image:: /_static/class5/dos_http6.png
   :width: 1321px
   :height: 693px
.. |image403| image:: /_static/class5/slowflow.png
   :width: 1326px
   :height: 553px
