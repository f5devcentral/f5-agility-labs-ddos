.. _module4:

Request Signatures
==============================================================
In this module you will be initiating a L7 DDoS attack on the hackazon virtual server, from eth1, using 10.1.10.53 as the source IP address. This source IP will match XFF\_mixed\_Attacker\_Good\_iRule, and an X-Forward-For header will be inserted in the HTTP request in the 132.173.99.0/24 IP address range.

Once the attack begins the F5 |awaf| will immediately switch into attack mode due to the server health deteriorating almost immediately. As the server gets totally overwhelmed, you may at first notice the good script dropping requests. As we discussed in module3, BaDoS will move through mitigation techniques from course to granular as it learns more about the malicious traffic.  BaDoS first mitigates with a global rate limit just to protect the server, and if you see the good traffic dropping upon an initial attack this is what you are witnessing.  In a short time, however, the good traffic will go back to all 200 OK responses. During this time Behavioral DoS identifies anamolous traffic and generates dynamic signatures matching only the malicious traffic. Once mitigation is in effect, the server health will rapidly improve and application performance will return to normal.

In |bip| version 14.1 a Behavioral DoS dashboard was added in the GUI.  This dashboard adds tremendous insight to the legitimate and attack traffic and the mitigations that the BaDoS features uses as it is mitigating an ongoing attack.  Before getting started with the attack exercises below we will review the BaDoS Dashboard.



1.  Using Chromium Browser on the |xj|, open tab to the GUI on bigip01 (https://10.1.10.245)

2.  Navigate to **Security >> Overview >> Dashboard** on the right side of the dashboard under **Protected Applications** select the radio button next to the **/Common/insecureApp1_vs. 

 |bados-dashboard|

 Explore the Behavioral DoS Dashboard and familiarize yourself with the various charts.  With only baseline traffic running you will be able to view the **Client HTTP Transactions** and the **Client HTTP Requests & Transactions** charts you should get a feel for HTTP RPS baselines for the normal or good traffic.  Also, viewing the **TLS Handshakes** chart you can see the normal TLS baselines (very low in this lab environment).  Finally, viewing the **Layer 3-4 & SSL Mitigations** and **HTTP Mitigations** chart you can see that no traffic is currently being mitigated by BaDoS.  That is about to change!

3.  Return to your original browser tab, Navigate to **Security ›› DoS Protection:Signatures** and click on the **Dynamic** box, then set the **Refresh** value to **20 secs**. This chart shows any active BaDoS signatures.  At this point, you shouldn't have any Dynamic Signatures.

4.  Open another tab/window in Chromium Browser, and go to **Security ››Reporting : DoS : Dashboard**. The dashboard is NOT real time in may take up to 10 minutes for traffic to display.  However, since our baseline traffic has been running since module1 you should have data in the DoS Dashboard.  Explore the reporting and filtering options available from this view.

5.  Revisit the Terminal window you opened earlier which is monitoring behavioral DoS learning signals.  Verify the first number (baseline\_learning\_confidence) is at or above 80%.  Normally, above 90% would be ideal, but for the purposes of this lab over 80% will suffice.

6.  Revisit the Terminal window you opened earlier which is still running the baseline traffic generation script.  Make note of the normal, pre-attack, response time for each request.

7. From |xj| open a NEW Terminal window or tab. From the ~/agility2020wafTools directoryenter:

   .. code-block:: console

      f5student@client01:~/agility2020wafTools$ ./AB_SSL_DOS.sh
        
      - Select **2** – Attack start - score


8.  Using Chromium Browser on the |xj|, open another tab to the GUI on bigip01, and navigate to **Security ›› Event Logs ››  DoS ›› Application Events**

9.  Almost immediately you should see an attack has started, and |awaf| has assigned an Attack ID to the event.  You will see something similar to the screenshot below:
   
   |event-log-bados-start|


10.  Review the **Dyanmic Signatures** UI page opened in step #2. It might take a few moments for a dynamic signature(s) to generate, but shortly after the attack has been detected a signature should be created.  Once a signature(s) is generated, if you click on the signature (NOT on the blue link, but somewhere on the signature bar), you will get the details about the signature in Wireshark format.  Also, you can examine the current status of the signature (mitigating or not), and statistics on recent attacks which used the signature.

   |dos-attack-sig-detail|

   - ** (#1) Signature ID**: Signature ID generated for this signature.  You can use the signature ID in DoS Analysis/Dashboard views (explored in module 6) to get more details on actions taken by this signature.

   - ** (#2) Deployment State**: current state of the signature.  Options include:
     
      * **Mitigate** - Collect stats, learn, alert, and mitigate.  All thresholds and threshold actions are applied, and rate limiting occurs if the device is under high stress.  
      * **Detect Only** - Collects stats, learn, and alert.  Develops dynamic signatures without enforcing any thresholds or limits.  
      * **Learn Only** - Collect stats and learn.  Develops dynamic signatures without enforcing any thresholds or limits
      * **Disabled** - No stat collection or mitigation, totally disables the signature.

   - ** (#3) Attack Status** - the state of the signature with respect to ongoing attacks.  Specifically, defines whether this particular signature is being used to mitigate an on-going attack.

   - ** (#4) Attack ID** - the attack ID for the attack that generated this signature.  Clicking the attack ID will take you to the DoS Analysis views filtered on this attack ID.

   - ** (#5) Predicates List** - the conditions for the request to be associated with this signature.  Includes one or more match ,expresssions, joined by logical operators, which the system uses to match traffic causing a DoS attack.

   - ** (#6) Attack History** - provides an account of all attacks in which this signature has been used to mitigate.  

   .. NOTE:: Dynamic Attack signatures generated will remain in the list up to the max number of signatures supported, and will be will re-used whenever an attack is detected, and traffic matches the conditions defined in the signature


11.  With the attack script still running, examine the output of the baseline script.  You should be getting HTTP 200 OK responses, and the response time should be inline with pre-attack response times.  Also, verify you can use browse to https://insecureapp1.f5.demo/WebGoat/login without issue.


12.  With an attack running return to the Behavioral DoS Dashboard.  Now, let's examine the mitigation charts in the dashboard to get a better feel for how the Behavioral DoS Engine is mitigation this attack.




12.  In the window where you are running the attack script, enter **CTRL-C**, then type **4** to kill the attack script cleanly.  

13.  Using Chromium Browser, navigate to **Security ›› DoS Protection:Signatures** and click on the **Dynamic** box.  Then click the check box next to the Name column to select all signatures, and click delete to remove all attack signatures created during this module.

14.  Leave **baseline_menu.sh** script running.

.. |event-log-bados-start| image:: _images/event-log-bados-start.png
   :width: 6.59740in
   :height: 1.33203in


.. |dos-attack-sig-detail| image:: _images/dos-attack-sig-detail.png
   :width: 8.59740in
   :height: 4.33203in

.. |bados-dashboard| image::_images/bados-dashboard.png
   :width: 8.59740in
   :height: 10.59740in
