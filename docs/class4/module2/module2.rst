Module 2: Manual Mitigations
----------------------------

Lab 2.1 – Device Level Protection for Mitigating Attacks.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-1.sh``

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

-  Let’s mitigate this attack using Device Level mitigation.

Log into the DHD https://10.1.1.245 accept the SSL warning and proceed
to connect with credentials provided.

-  In the Configuration Utility, go to **DoS Protection>>Quick
   Configuration.**

-  In the **Device Protection** section click **Device Configuration.**

-  In the **Flood** row click the + icon, and then click **ICMPv4**
   flood.

-  On the right-side of the page select the drop-down to **"Mitigate"**

+-------------------------------+----------------+
| Parameter                     | Value          |
+===============================+================+
| Mitigation                    | Fully Manual   |
+-------------------------------+----------------+
| Detection Threshold EPS       | 100            |
+-------------------------------+----------------+
| Detection Threshold Percent   | 500            |
+-------------------------------+----------------+
| Rate/Leak Limit               | 500            |
+-------------------------------+----------------+

-  On the Hybrid Defender you will now see the attack is being mitigated
   (Where will you check this? Tip: It’s the same places that you are
   looking on the Passive device). You have successfully mitigated a
   network flood single vector attack. Use CTRL+C in the attacker window
   to stop the attack.

Lab 2.2 – Device Level Protections for Mitigating Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-2.sh``

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

Mitigate this attack using Device Level mitigation steps like those that
you did in Lab 2.1 above.

Lab 2.3 – Device Level Protections for Mitigating Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-3.sh``

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved? Look closely and you will
   notice that there is a range of destination IPs that are being
   targeted and a lot of SYN, Retransmit, Out of Sequence, RST packets.
   This looks like someone is trying to run a scan against your network.
   How will you mitigate against this? They are “Sweep”ing your network.

-  In the Configuration Utility, in the **Device Protection** section
   click **Device Configuration.**

-  In the **Single Endpoint** row click the + icon, and then click
   **Single Endpoint Sweep**.

-  On the right-side of the page select the drop-down to **"Mitigate"**

+---------------------------+------------+
| Parameter                 |  Value     |
+===========================+============+
| Detection Threshold EPS   | 100        |
+---------------------------+------------+
| Rate/Leak Limit           | 500        |
+---------------------------+------------+
| Packet Types (Selected)   | All IPv4   |
+---------------------------+------------+

-  On the Hybrid Defender you will now see the attack is being
   mitigated. This attack is short lived so make sure you launch it
   again if it has stopped to see the mitigation. You have successfully
   mitigated a sweep flood attack. Use CTRL+C in the attacker window to
   stop the attack.

Lab 2.4 – Device Level Protections for Mitigating Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-4.sh``

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

-  Use the manual mitigations steps you learned in previous tasks to
   mitigate against all the attack vectors that you have identified.

-  Use CTRL+C in the attacker window to stop the attack.

Lab 2.5 – Device Level Protections for Mitigating Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You received a call that a lot of users are intermittently getting a
page cannot be displayed for various applications. Your Network
Operations Center has stated that none of their monitoring systems for
those applications are reporting any outages. The NOC tools monitor
application health using the application URLs like
http://10.1.20.12/index.php and so on. Your users are using the
application using the FQDNs. You suspect that there is an ongoing DDoS
attack and you need to identify it and mitigate against it.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-5.sh``

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  Let’s look at an alternate way to see which vector is being triggered
   so that you can identify the attack. If in your environment you had
   no tools like the Wireshark or the Passive DHD device, you can still
   identify the attack. While the event logs, DoS Overview screens are
   populated only when an attack is detected based on the threshold
   values set, if the attack doesn’t trigger the detection threshold you
   will not see it in the Overview and Event Logs.

-  In the Configuration Utility of the Hybrid Defender, go to **DoS
   Protection>>Quick Configuration.**

-  In the **Device Protection** section click **Device Configuration.**

-  In the **DNS** row click the **+** icon, and then view the Current Device
   Statistics Section. You can see that we are triggering a vector and
   registering the packets for that vector even though we have the
   default detection/mitigation configured for it.

-  Alternately there is a CLI command also available to view the attack
   vector that is being triggered. Open a putty shell to the Hybrid
   Defender (use shortcut on desktop), login with the credentials:
   root/f5DEMOs4u and then :

``# cd f5agility``

``# ./show_attackvector_stats.sh``

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved? Hint: (Wireshark) Destination
   IP, Targeted Port and Protocol used.

-  Use the manual mitigations steps you learned in previous tasks to
   mitigate against the attack vector that you have identified.

-  Use CTRL+C in the attacker window to stop the attack.

Lab 2.6 – Protected Object Level Protections for Mitigating Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You mitigated a DNS vector attack above at device level. You have again
received a call that a lot of users are intermittently getting a page
cannot be displayed for various applications. Your Network Operations
Center has stated that none of their monitoring systems for those
applications are reporting any outages. The NOC tools monitor
application health using the application URLs like
http://10.1.20.12/index.php and so on. Your users are using the
application using the FQDNs. You suspect that there is an ongoing DDoS
attack and you need to identify it and mitigate against it. You don’t
want to implement a mitigation for a vector device wide and want to
specifically mitigate the suspected victim server.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-6.sh``

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

-  In the BIG-IP Configuration Utility, open the **DoS Protection > Quick Configuration** page.

-  In the **Protected Objects** section click **Create**.

-  Configure a protected object using the following information, and
   then click **Create.**

+------------------------+--------------------+
| Parameter              |     Value          |
+========================+====================+
| Name                   | DNSServer          |
+------------------------+--------------------+
| IP Address             | 10.1.20.14         |
+------------------------+--------------------+
| Port                   | 53                 |
+------------------------+--------------------+
| Protocol               | UDP                |
+------------------------+--------------------+
| Protection Settings:   | Log and Mitigate   |
| Action                 |                    |
+------------------------+--------------------+
| Protection Settings:   | DNS                |
| DDoS Settings          |                    |
+------------------------+--------------------+

-  In the **DNS** row click the **+** icon, and then click **DNS A
   Query**.

-  On the right-side of the page configure using the following
   information, and then click **Create**.

+-------------------------------+----------------+
| Parameter                     |     Value      |
+===============================+================+
| Detection Threshold EPS       | Specify: 10    |
+-------------------------------+----------------+
| Detection Threshold Percent   | Specify: 500   |
+-------------------------------+----------------+
| Mitigation Threshold EPS      | Specify: 100   |
+-------------------------------+----------------+

-  On the Hybrid Defender you will now see the attack is being
   detected/mitigated. You have successfully mitigated a DNS A Query
   flood. Use CTRL+C in the attacker window to stop the attack.

Lab 2.7 – Protected Object Level Protections for Mitigating Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There has been a high-profile DDoS attack and you must provide Law
Enforcement some details on the offending IP addresses. In your
environment at any given time you have a few hundred thousands of IP
addresses observed on your network. You want to identify a few offending
IP addresses and blacklist them so that you can provide the details to
Law Enforcement.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-7.sh``

-  On the WireShark start a capture and identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved? Make a note of the protocol of
   attack and the destination IP (target).

-  We will build a protected object and use Bad Actor Detection and
   Black Listing.

-  In the BIG-IP Configuration Utility, open the **DoS Protection > Quick Configuration** page

-  In the **Protected Objects** section click **Create**.

-  Configure a protected object using the following information, and
   then click **Create.**

+------------------------+--------------------+
| Parameter              |      Value         |
+========================+====================+
| Name                   | BadActorServer     |
+------------------------+--------------------+
| IP Address             | 10.1.20.12         |
+------------------------+--------------------+
| Port                   | \*                 |
+------------------------+--------------------+
| Protocol               | All                |
+------------------------+--------------------+
| Protection Settings:   | Log and Mitigate   |
| Action                 |                    |
+------------------------+--------------------+
| Protection Settings:   | UDP                |
| DDoS Settings          |                    |
+------------------------+--------------------+

-  In the **UDP** row click the **+** icon, and then click **UDP
   Flood**.

-  On the right-side of the page configure using the following
   information, and then click **Create**.

+--------------------------------------+----------------+
| Parameter                            |  Value         |
+======================================+================+
| Detection Threshold PPS              | Specify: 100   |
+--------------------------------------+----------------+
| Detection Threshold Percent          | Specify: 500   |
+--------------------------------------+----------------+
| Mitigation Threshold EPS             | Specify: 200   |
+--------------------------------------+----------------+
| Bad Actor Detection                  | Checked        |
+--------------------------------------+----------------+
| Per Source IP Detection Threshold    | 100            |
+--------------------------------------+----------------+
| Per Source IP Mitigation Threshold   | 30             |
+--------------------------------------+----------------+
| Blacklist Attacking Address          | Checked        |
+--------------------------------------+----------------+
| Sustained Attack Detection Time      | 15             |
+--------------------------------------+----------------+
| Category Duration Time               | 120            |
+--------------------------------------+----------------+

-  On the Hybrid Defender you will now see the attack is being
   detected/mitigated.

-  View the offending IP addresses at **Security>>Event Logs>>Network>>IP Intelligence**

-  View the Shun list / Blacklist at **Security>>Event Logs>>Network>>Shun**

-  You have successfully identified the Bad Actors and put them in a
   Blacklist. Use CTRL+C in the attacker window to stop the attack.

Lab 2.8 – Whitelisting
~~~~~~~~~~~~~~~~~~~~~~

You get a call from your QA team that is running load runner scripts
against your application server 10.1.20.12 that they are seeing packets
being dropped. You ask them what's the source IP address of the server
they are running the load runner script from and they provide you with
10.1.17.225.

-  Why do you think their packets are being dropped? Hint: Check the
   blacklist (**Event Logs>>Network>>Shun**). They have been added to
   that list. You will now need to maintain the mitigations in place and
   only allow 10.1.17.225 to not be enforced with any DDoS mitigations
   going to 10.1.20.12.

-  Go to the protected object 10.1.20.12 and add the IP to the
   whitelist.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab2-7.sh``

-  View the offending IP addresses at **Security>>Event Logs>>Network>>IP Intelligence** and **Security>>Event Logs>>Network>>Shun** 
   and confirm that 10.1.17.225 is not being added
   to the list.

-  You have successfully whitelisted an IP to bypass DDoS mitigations.
   Use CTRL+C in the attacker window to stop the attack.

Lab 2.9 – BOT Defense for Application Attacks.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

HTTP DoS attacks are very popular. Some can be in form of HTTP Floods
and some can be low and slow attacks (slow loris, slow post, slow read).
They have been used by BOTS to bring down a site. Sometimes even though
the BOTS don’t bring the site down they demand for you to stand up
additional infrastructure to support the traffic they are generating
costing your organization a significant spend when it can be mitigated
and avoided. Your organization just published a brand-new web
application. As soon as it was available to public you started getting
calls that the site is sometimes unavailable and slow to respond. Based
on the predicted traffic patterns one server was enough to handle the
valid user load. The application team viewed the web server logs and
noticed that there is 30% additional traffic then predicted from what
seems like automated tools. Your IT management has asked you to provide
a solution on what’s driving up the traffic to the server and
potentially mitigate it. You will now learn how to manually mitigate BOT
traffic.

-  Open a PuTTY shell to the WebServer (use the shortcut on the
   desktop). Login with credentials: root/default. You will use the
   webservers log to monitor the requests coming to the server. Once
   logged into the WebServer shell:

``# cd /usr/local/apache/logs``

``# tail -f access_log``

-  Hit the Enter key a few times so that you can see incoming requests
   clearly in the blank space.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack to simulate BOT traffic:

``# sudo bash``

``# cd f5agility``

``# ./lab2-9.sh``

-  We are just simulating 25 requests so that it’s a controlled
   environment and you can view the requests/logs.

-  View the WebServer shell where you have the tail -f access\_log
   running. Do you see the requests come in? What’s the source IP
   address of the requests?

-  As you can see the site is available to everyone including BOTS. You
   have not set this up on the DHD and hence no BOT protection is
   applied.

-  You will now publish the website through the DHD with needed
   protections.

-  In the BIG-IP Configuration Utility, open the **DoS Protection >  Quick Configuration** page and in the Protected Objects section click
   **Create**.

-  Configure a protected object using the following information, and
   then click **Create**.

+------------------------+---------------------------------+
| Parameter              |           Value                 |
+========================+=================================+
| Name                   | WebServer                       |
+------------------------+---------------------------------+
| IP Address             | 10.1.20.101                     |
+------------------------+---------------------------------+
| Port                   | 80                              |
+------------------------+---------------------------------+
| VLAN (Selected)        | **defaultVLAN (uncheck ANY)**   |
+------------------------+---------------------------------+
| Protection Settings:   | Log and Mitigate                |
| Action                 |                                 |
+------------------------+---------------------------------+
| Protection Settings:   | IPv4, TCP, HTTP                 |
| DDoS Settings          |                                 |
+------------------------+---------------------------------+

-  By simply creating the Protected Object and applying HTTP protections
   the BOT protections are automatically turned on. Everyone will now
   access the web application through the DHD with mitigations enforced.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack to simulate BOT traffic:

``# sudo bash``

``# cd f5agility``

``# ./lab2-9.sh``

-  View the WebServer log (tail -f access\_log) in the shell. You will
   not see requests come through this time from the attacker.

-  View the mitigation in **Security>>Event Logs>>Bot Defense>>Requests.** All the requests from the BOT are blocked.

-  Open a firefox browser on the Jumpbox and go to http://10.1.20.101.
   This request will open your web application and its not blocked as
   it’s not a BOT. You will also see the request in the WebServer log
   shell.

-  View the valid request from your browser in the DHD in
   **Security>>Event Logs>>Bot Defense>>Requests.** You will notice that
   valid requests are being challenged and allowed only after a valid
   response. Note: There is a default grace period of 300s when the
   mitigation is implemented so some requests are allowed as grace. This
   is Proactive BOT defense in action.

-  View the BOT Defense in **Security>>Reporting>>DoS>>Analysis** and
   look at the graph under HTTP -> Transaction Outcomes. **Please be
   patient as these graphs are usually populated with a delay.**

   You have successfully mitigated BOT traffic to your application.
   CTRL+C in all shell windows and close them all.

