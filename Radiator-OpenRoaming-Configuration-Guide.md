# Radiator OpenRoaming Configuration Guide

Date: 2023-05-22

Most recent version is always available here: https://github.com/radiator-software/radiator-openroaming/

## Introduction

This is a configuration guide for Radiator AAA (Authentication, Authorisation, Accounting) software and OpenRoaming roaming federation.  The Radiator OpenRoaming Configuration Guide contains both instructions such as this guide and example configurations ready to be adapted and used to join to the Wireless Broadband Alliance coordinated OpenRoaming Wi-Fi roaming federation.  These configurations and instructions focus mainly on OpenRoaming Settlement-Free model, but can be used as a basis for developing Settled configuration as well.  

## Support

Issue and bug reports, comments and contributions for the configurations and the guide are welcome and supported by best-effort support from Radiator Software.  Adaptation, customisation and further development of these configurations is supported by the Radiator Software's commercial support and expert services.

## Supported features

Radiator Configuration Guide include configurations, which include among other things the following:

* Both inbound (IDp) and outbound (SP/ANP) OpenRoaming RadSec (RADIUS over TLS) configurations
* Local inbound RadSec (RADIUS over TLS) connectivity configurations (for local network devices, customer RADIUS/RadSec servers)
* Separate RADIUS roaming/proxying instance, separate RADIUS authentication and accounting instances (recommended deployment practice)
* DNS peer discovery (with only Radiator configuration needed)
* 3gppnetwork.org realm translation to pub.3gppnetwork.org for OpenRoaming Settlement-Free SIM authentication (with only Radiator configuration needed)
* Wireless Broadband Alliance (WBA) vendor specific RADIUS attributes dictionary

## Architecture

The Radiator OpenRoaming Configuration Guide includes five Radiator instance configurations.  An instance is a separate Radiator process configuration with its own specific functionality.  Having multiple separate instances clarifies the configuration, enhances performance and makes scaling easier in the future.  The instances also demonstrate the current Radiator best practices for configuration.

The Radiator systemd instances and their configurations are listed in the table below.

| Radiator instance | Configuration file | Description |
| ----------------- | ------------------ | ----------- |
| radiator@radius_proxy_acct | /etc/radiator/radiator-radius_proxy_acct.conf | RADIUS accounting proxy instance |
| radiator@radius_proxy_auth | /etc/radiator/radiator-radius_proxy_auth.conf | RADIUS authentication proxy instance |
| radiator@radsec_inbound_local_clients | /etc/radiator/radiator-radsec_inbound_local_clients.conf | RadSec instance for local RadSec clients |
| radiator@radsec_inbound_openroaming | /etc/radiator/radiator-radsec_inbound_openroaming.conf | RadSec instance for inbound OpenRoaming requests (IdP) |
| radiator@radsec_outbound_openroaming | /etc/radiator/radiator-radsec_outbound_openroaming.conf | RadSec instance for outbound OpenRoaming requests (SP/ANP) |
| radiator-instances | /usr/lib/systemd/system/radiator-instances.service | An management service for managing all Radiator instances at once |

For more information about systemd instances and about managing them, check Radiator Cookbook blog post /Grouping and controlling multiple Radiator instances with systemd/ at: https://blog.radiatorsoftware.com/2019/06/grouping-and-controlling-multiple.html

