# OpenRoaming CA/ICA certificates directory for inbound RadSec connections

This is a directory for CA/ICA certificates used in verifying inbound
OpenRoaming client certificates for RadSec connections.

OpenRoaming Root CA certificate should be enough for this directory,
but to ensure interoperability also OpenRoaming intermediate CA
files can be added here. This will help even improperly configured
RadSec clients to be able to make inbound connections to this 
server.

After adding certificate, run 'openssl rehash -v .' in this directory.

On some systems the similar command may be 'cacertdir_rehash .' run
in this directory as well.

