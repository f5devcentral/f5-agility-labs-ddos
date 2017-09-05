Getting Started
---------------

Lab Topology
~~~~~~~~~~~~

|image0|

Access and Credential Summary
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You will using the Win7 jumpbox to access other systems for all labs.
You will use Putty that has been preconfigured with appropriate keys in
order to access the Good Client and the Attacker systems. In order to
run scripts, you will need to have root access, requiring you to ‘sudo
bash’ before running attacks, baselines, etc..

.. NOTE:: Ignore the “sudo: unable to resolve host xjumpbox” when you issue
   the sudo bash command throughout the labs.

Common Errors and Workarounds
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Since the environment and all its components are running in a
virtualized environment with limited shared resources you may encounter
a few known issues.

Error using the Quick Configuration and working with various settings

|image1|

This eventually goes away. You maybe prompted to accept External
Changes. Once you accept that and navigate to a different screen your
settings are saved and take effect.

.. WARNING:: Time skew when viewing DoS Visibility Reports/Analysis

|image2|

As you will proceed through accessing the lab environment task below we
are adjusting the time of the Windows PC that you are using to access
all components of the labs. The Windows PC cannot keep time and you may
notice the warning when the Windows PC clock doesn’t match the BIG-IP
time. You can ignore this warning or readjust the clock of the Windows
PC if the reporting time skew is too much to display anything.

Lab Components
^^^^^^^^^^^^^^
+------------------------------------+-------------------------------+-----------------------+
|     **System**                     |     **Username**              |  **Password**         |
+====================================+===============================+=======================+
| Ravello                            |     Given at site             |     Given at site     |
+------------------------------------+-------------------------------+-----------------------+
| Win7 Jumpbox                       |     external\_user            |     f5DEMOs4u         |
+------------------------------------+-------------------------------+-----------------------+
| Hybrid Defender - WebUI            |     admin                     |     f5DEMOs4u         |
+------------------------------------+-------------------------------+-----------------------+
| Hybrid Defender - CLI              |     root                      |     f5DEMOs4u         |
+------------------------------------+-------------------------------+-----------------------+
| Good Client                        |     ubuntu                    |     Use key           |
+------------------------------------+-------------------------------+-----------------------+
| Attacker                           |     ubuntu                    |     Use key           |
+------------------------------------+-------------------------------+-----------------------+
| Auction CLI                        |     root                      |     default           |
+------------------------------------+-------------------------------+-----------------------+
| Lamp CLI                           |     root                      |     default           |
+------------------------------------+-------------------------------+-----------------------+
| Lamp X-Server Shell                |     xubuntu                   |     <no password>     |
+------------------------------------+-------------------------------+-----------------------+

Accessing the Lab Environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Task 1 – Open your RDP client and connect to your Windows Jumpbox
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

IP address provided by your Instructor or IP you obtained from the
Training Portal.

Note: Use the show options to provide

-  User name: external\_user. Password: f5DEMOs4u

|image3|

-  Click YES at the warning

|image4|

.. NOTE:: Please validate the Windows System clock and adjust it to the
   correct time using Change Date and Time.

All Exercises/Tasks are to be completed from the Windows Jumpbox. There
are various shortcuts -- Chrome Incognito, Putty shortcuts, Licensing
Folders on the jumpbox that you will use through the exercises

|image5|

.. |image0| image:: /_static/image2.png
   :width: 5.30694in
   :height: 5.22014in
.. |image1| image:: /_static/image3.png
   :width: 5.30694in
   :height: 0.86667in
.. |image2| image:: /_static/image4.png
   :width: 5.30694in
   :height: 1.72708in
.. |image3| image:: /_static/image5.png
   :width: 2.98681in
   :height: 3.46042in
.. |image4| image:: /_static/image6.png
   :width: 2.92708in
   :height: 2.92708in
.. |image5| image:: /_static/image7.png
   :width: 5.30694in
   :height: 2.98681in
