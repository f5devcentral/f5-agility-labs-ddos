Module 1: Environment Review
----------------------------

Lab 1.1 – Review Tools and Environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You are the security engineer for Acme corporation. Your organization
has recently seen a lot of outages in your network and applications.
Some of these have been due to DDoS attacks and the outages have caused
a significant loss of revenue as well as reputational impact. You have
made the wise decision to invest in a world class leading edge DDoS
mitigation solution and have the F5 DHD installed in your environment.
It’s been configured in the Layer 2 inline mode and is now available to
you to enforce DDoS mitigations.

*Tools:*

#. In our lab we have an additional DHD available to you in a passive
mode. It’s basically setup on SPAN ports (out of band deployment) to
provide you visibility.

#. The Win-ToolsServer is also installed to listen on SPAN port and has
Wireshark available for visibility.

Let’s get familiar on how to use these tools.

Note: Not all attacks will be visible in both tools. So, use the tools
accordingly. This is done purposefully so that you get into the habit of
troubleshooting/fighting attacks in the real world.

Use a web browser (Chrome in incognito mode) to log into the WebUI of
the Passive DHD at https://10.1.1.246 or use the bookmarked shortcut.
Accept the SSL warning and proceed to connect.

Username: `admin`

Password: `f5DEMOs4u`

-  Click **Security>>Event Logs>>DoS>>Network>>Events**

-  Click **Security>>DoS Protection>>DoS Overview** (\ Tip: Right Click
   and open link in new tab/window)

-  You will use the above two screens on the Passive DHD for visibility
   of traffic/attacks.

-  On the Win-Tools Server launch Wireshark by using the shortcut link
   on desktop and then click on the blue shark fin on top left corner to
   start capturing data. (\ Tip: Use the Red Square button to stop
   captures when needed)

Lab 1.2 – Launch an attack and view traffic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

``# sudo bash``

``# cd f5agility``

``# ./lab1-2.sh``

-  View Wireshark and notice the ongoing captures.

-  What type of traffic do you notice? As you can see these are all ICMP
   requests/responses and a lot of them. What are the IP addresses
   involved? Can you identify the attacking IP? (\ Tip: Did you
   review the lab network diagram?)

|image2|

In the Passive DHD Windows what do you notice? (\ Tip: You may need
to click Search button/Refresh button or set Auto Refresh)

|image3|

|image4|

**As you can see the visibility is better in terms of the Attack Vector
and number of packets in/sec on the passive DHD.**

It’s up to you on which tool you may want to use for the remaining labs.
If you are comfortable with WireShark then use that or use the Passive
DHD or both. As noted previously you will have to visit both tools to
see where you can gather some visibility to fight a real-world DDoS
attack.

Use CTRL+C in the attacker shell to stop the attack.

.. |image2| image:: /_static/class4/image5.png
   :width: 5.30972in
   :height: 2.39444in
.. |image3| image:: /_static/class4/image6.png
   :width: 5.26736in
   :height: 0.69235in
.. |image4| image:: /_static/class4/image7.png
   :width: 5.30972in
   :height: 1.70139in
