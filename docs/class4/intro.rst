Introduction to DDoS Hybrid Defender
------------------------------------

F5 DDoS Hybrid Defender (DHD) protects your organization against a
wide range of DDoS attacks using a multi-pronged approach. By combining
on-premises and cloud technologies, analytics, and advanced methods,
DDoS Hybrid Defender is a hybrid solution that detects network and
application layer attacks and is easy to deploy and manage.

DDoS Hybrid Defender mitigates against the full spectrum of DDoS attacks
including:

- Network capacity attacks

- DNS and SIP protocol volumetric attacks

- HTTP and HTTPS volumetric attacks

- HTTP and HTTPS CPU-based (heavy URL) attacks

You can specify which objects to protect on the network, assigning the
appropriate protections to network devices and application servers, and
prevent attackers from exhausting network resources and impacting
application availability.

**Deployments:**

The deployment you use for DDoS Hybrid Defender depends on the needs of
your organization. For maximum DDoS protection, it is recommended that
you deploy DDoS Hybrid Defender inline. However, it can also be deployed
out of band, or in locations where symmetric data flows are not
guaranteed.

Typical locations for the placement of DDoS Hybrid Defender are at the
edge of the network or at the edge of the data center

**Inline deployment**

DDoS Hybrid Defender provides maximum protection when deployed inline in
one of two ways:

- Bridged mode with VLAN groups (This is default and we will use in our
  labs)

- Routed mode

**Out of band deployment**

You can deploy DDoS Hybrid Defender out of band in two ways:

- Set up a Layer 2 switch with span ports so that it mirrors traffic
  onto DDoS Hybrid Defender. (Our passive device is setup this way in our
  labs)

- Configure network devices so that they send NetFlow data to DDoS
  Hybrid Defender.

