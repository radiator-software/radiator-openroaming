# OpenRoaming CA/ICA certificates directory for verifying outbound OpenRoaming RadSec connections

This is a directory for CA/ICA certificates used in verifying the
OpenRoaming servers responding to outbound RadSec connections. 

OpenRoaming Root CA certificate should be enough for this directory,
but to ensure interoperability also OpenRoaming intermediate CA
files can be added here. This will help this server to make connections
to even improperly (e.g. without suitable certificate chains) 
configured OpenRoaming IdP servers.

After adding certificate, run 'openssl rehash -v .' in this directory.

On some systems the similar command may be 'cacertdir_rehash .' run
in this directory as well.

