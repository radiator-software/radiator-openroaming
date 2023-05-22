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

