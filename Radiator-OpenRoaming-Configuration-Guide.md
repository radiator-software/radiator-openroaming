# Radiator OpenRoaming Configuration Guide

Date: 2023-05-22

Most recent version is always available here: https://github.com/radiator-software/radiator-openroaming/

## Introduction

This is a configuration guide for Radiator AAA (Authentication, Authorisation, Accounting) software and OpenRoaming roaming federation.  The Radiator OpenRoaming Configuration Guide contains both instructions such as this guide and example configurations ready to be adapted and used to join to the Wireless Broadband Alliance coordinated OpenRoaming Wi-Fi roaming federation.  These configurations and instructions focus mainly on OpenRoaming Settlement-Free model, but can be used as a basis for developing Settled configuration as well.  

## Support

Issue and bug reports, comments and contributions for the configurations and the guide are welcome and supported by best-effort support from Radiator Software.  Adaptation, customisation and further development of these configurations is supported by the Radiator Software's commercial support and expert services.

## Supported features

Radiator Configuration Guide include configurations, which include among other things the following:

* Both inbound (IdP) and outbound (SP/ANP) OpenRoaming RadSec (RADIUS over TLS) configurations
* Local inbound RadSec (RADIUS over TLS) connectivity configurations (for local network devices, customer RADIUS/RadSec servers)
* Separate RADIUS roaming/proxying instance, separate RADIUS authentication and accounting instances (recommended deployment practice)
* DNS peer discovery (with only Radiator configuration needed)
* 3gppnetwork.org realm translation to pub.3gppnetwork.org for OpenRoaming Settlement-Free SIM authentication (with only Radiator configuration needed)
* Wireless Broadband Alliance (WBA) vendor specific RADIUS attributes dictionary

## Architecture and functionality

The Radiator OpenRoaming Configuration Guide includes five Radiator instance configurations.  An instance is a separate Radiator process configuration with its own specific functionality.  Having multiple separate instances clarifies the configuration, enhances performance and makes scaling easier in the future.  The instances also demonstrate the current Radiator best practices for configuration.

The Radiator systemd instances and their configurations are listed in the table below.

| Radiator instance | Configuration file | Description |
| ----------------- | ------------------ | ----------- |
| radiator@radius_proxy_acct | /etc/radiator/radiator-radius_proxy_acct.conf | RADIUS accounting proxy instance |
| radiator@radius_proxy_auth | /etc/radiator/radiator-radius_proxy_auth.conf | RADIUS authentication proxy instance |
| radiator@radsec_inbound_local_clients | /etc/radiator/radiator-radsec_inbound_local_clients.conf | RadSec instance for local RadSec clients |
| radiator@radsec_inbound_openroaming | /etc/radiator/radiator-radsec_inbound_openroaming.conf | RadSec instance for inbound OpenRoaming requests (IdP) |
| radiator@radsec_outbound_openroaming | /etc/radiator/radiator-radsec_outbound_openroaming.conf | RadSec instance for outbound OpenRoaming requests (SP/ANP) |
| radiator-instances | (/usr/lib/systemd/system/radiator-instances.service) (no need to edit) | Management service for all Radiator instances |

For more information about systemd instances and about managing them, check Radiator Cookbook blog post /Grouping and controlling multiple Radiator instances with systemd/ at: https://blog.radiatorsoftware.com/2019/06/grouping-and-controlling-multiple.html

### RADIUS accounting proxy instance (radiator@radius_proxy_acct)

The RADIUS accounting proxy instance is intended to receive, log and proxy RADIUS accounting.  It is recommended to be a separate instances so that accounting requests do not affect the performance of the authentication even if the backend storing accounting might cause delays.  Having RADIUS accounting on a separate instance also makes it possible to scale the accounting processing capability simply by adjusting FarmSize parameter in the configuration.  The FarmSize setting controls how many parallel child processes the Radiator instance process spawns.  The exact number of processes depends on the amount of accounting requests to be processed simultaneously, but a general guideline is that FarmSize would be at least the amount of (virtual) CPUs available on the (virtual) host.

If these configurations are used to test or provide OpenRoaming Identity Provider (IdP) functionality, this instance configuration needs to be adjusted to store or proxy accounting requests to suitable accounting servers or separate accounting instances.

### RADIUS authentication proxy instance (radiator@radius_proxy_auth)

The RADIUS authentication proxy instance is intended to receive, log and proxy RADIUS authentication.  As with the accounting instance, this is recommended practice to ensure efficient processing of the authentication requests.  This instance configuration also demonstrates how RADIUS proxying can also be scaled with FarmSize, even when proxying EAP requests.  Using HASHBALANCE for proxying RADIUS requests ensures that the requests belonging to same EAP context end up to the same proxy route or authenticating server in a normal situation -- no upstream server has failed in the middle of EAP authentication.

If these configurations are used to test or provide OpenRoaming Identity Provider (IdP) functionality, this instance configuration needs to be adjusted to proxy authentication requests to the suitable authentication servers or separate authentication instances.

The RADIUS authentication proxy instance can also be used for implementing local and global RADIUS roaming logic and determining which requests to proxy to RADIUS using roaming partners and separate roaming federations such as OpenRoaming and eduroam.

### RadSec instance for local RadSec clients

In addition to OpenRoaming connections, RadSec can be also used internally in the organisation to secure traffic from network devices to RADIUS/RadSec servers or from customers running their own RADIUS servers.  If for example an operator is a member of Wireless Broadband Alliance and entitled for WBAID, the operator can provide OpenRoaming as a service to its customers without the need of those customers to join to the Wireless Broadband Alliance directly as members.  The customers can this way connect their RADIUS infrastructure via this RadSec instance to the operator's AAA infrastructure and onwards to the OpenRoaming and other authentication federations.  As this instance is a separate instance with separatae RadSec port, this instance can also use client and server certificates from any PKI such as a private one or operator's own one.  Using this separate instance and a private PKI for local RadSec connections reduces costs as certificates from well-known CAs are not required for RadSec connections.

### RadSec instance for inbound OpenRoaming requests (IdP) (radiator@radsec_inbound_openroaming)

This Radiator instance is for inbound OpenRoaming requests like the ones sent to an OpenRoaming Identity Provider (IdP). The instance is configured to bind to the standard RadSec port (TCP 2083), which must be opened in the firewalls to connections originating from any IP addresses.  The OpenRoaming CA certificates are used to verify the certificates offered by the RadSec clients connecting to this instance.  These OpenRoaming CA certificates as well as the OpenRoaming client-server certificate from the Wireless Broadband Alliances need to be separately installed under /etc/radiator/certificates/radsec_inbound_openroaming/ directory hierarchy.  

The instance configuration proxies the received requests to the RADIUS authentication and accounting proxy instances, which need to be configured to proxy requests to the suitable authenticating RADIUS servers or instances.

### RadSec instance for outbound OpenRoaming requests (SP/ANP) (radiator@radsec_outbound_openroaming)

To implement Service Provider (SP) or Access Network Provider (ANP) functionality, only this and the two RADIUS proxy instances are needed.  This outbound OpenRoaming instance is used to do the DNS discovery (with 3gppnetwork.org realm translation) and dynamic OpenRoaming RadSec connections needed to provide access to the OpenRoaming IdPs.  This same instance can also be used to bypass DNS discovery and Settlement Free OpenRoaming for roaming partner realms, where there are for example separate commercial roaming agreements.  

As with inbound OpenRoaming instance, this instance also requires OpenRoaming CA certificates as well as OpenRoaming client certificate to be separately installed under /etc/radiator/certificates/radsec_inbound_openroaming/ directory hierarchy.

### Management service for all Radiator instances (radiator-instances)

To manage all these separate Radiator systemd instances all at once, radiator-instances systemd service can be used.  This service is part of the Radiator AAA server software package and intended to do mass control of the all enabled systemd Radiator instances.  By starting, stopping or restarting this systemd service, all Radiator instances are started, stopped or restarted without having to manage them separately.  You can still manage instances one by one, but in some cases, a commmon start, stop or restart is more useful.

