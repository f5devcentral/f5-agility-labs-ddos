Introduction
------------


**THE PROBLEM**

On-premises DDoS defenses can be very effective for blocking most DDoS 
attacks locally and, being Always-On, can block most attacks immediately. 
However, they are useless in the case of large volumetric attacks. 
On the other hand, while Cloud-based DDoS protection (On-Demand) works well 
for volumetric attacks, it struggles with much slower mitigation response, 
increased latency, higher operational complexity and the inability 
to handle HTTPS encrypted attacks due to its asymmetric nature.


**THE SOLUTION**

Thoroughly and effectively protect your critical web applications from all 
types of DDoS attacks with a combination of On-Premises and Cloud-Based 
DDoS services, leveraging multilayer protection techniques that are able 
to mitigate volumetric attacks, application-level attacks and HTTPS 
encrypted attacks while minimizing both application downtime 
and business impact. Intelligent application attacks, encrypted or not, 
can be handled on-premises with F5’s DDoS Hybrid Defender (DHD) which 
provides next-generation DDoS defense to ensure real-time, Always-ON, 
protection while large volumetric attacks are handled by F5’s Silverline 
DDoS Protection cloud service which, working On-Demand, detects and 
mitigates DDoS attacks in real time.


F5 Silverline
=============

F5 Silverline is a cloud-based, fully managed security service for WAF and DDoS 
protection. F5 Silverline provides Enterprise customers proven security technologies
coupled with world-class security professionals. F5’s security experts are an 
extension to the customer’s staff and allow them to defeat the largest and most 
complex attacks.

The primary customer benefits to F5 Silverline include:

• Minimize the risk of data breach and downtime  
• Enhance security visibility to their application state  
• Reduced operational expense and capital investment required for application security  
• Ensure timely detection and fast restoration of services in the event of an attack  



F5 DDoS Hybrid Defender
=======================

F5® DDoS Hybrid Defender™ (DHD) protects your organization against a
wide range of DDoS attacks using a multi-pronged approach. By combining
on-premises and cloud technologies, analytics, and advanced methods,
DDoS Hybrid Defender is a hybrid solution that detects network and
application layer attacks and is easy to deploy and manage.

DDoS Hybrid Defender mitigates against the full spectrum of DDoS attacks
including:

• Network capacity attacks  
• DNS and SIP protocol volumetric attacks  
• HTTP and HTTPS volumetric attacks  
• HTTP and HTTPS CPU-based (heavy URL) attacks  

You can specify which objects to protect on the network, assigning the
appropriate protections to network devices and application servers, and
prevent attackers from exhausting network resources and impacting
application availability.

**Deployments:**

The deployment you use for DDoS Hybrid Defender™ depends on the needs of
your organization. For maximum DDoS protection, it is recommended that
you deploy DDoS Hybrid Defender inline. However, it can also be deployed
out of band, or in locations where symmetric data flows are not
guaranteed.

Typical locations for the placement of DDoS Hybrid Defender are at the
edge of the network or at the edge of the data center

**Inline deployment**

DDoS Hybrid Defender provides maximum protection when deployed inline in
one of two ways:

• Bridged mode with VLAN groups (This is default and we will use in our labs)
• Routed mode

**Out of band deployment**

You can deploy DDoS Hybrid Defender out of band in two ways:

• Set up a Layer 2 switch with span ports so that it mirrors traffic onto DHD  
• Configure network devices so that they send NetFlow data to DDoS Hybrid Defender
