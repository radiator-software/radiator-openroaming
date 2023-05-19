# CA/ICA certificates for verifying local RadSec client connections

This is a directory for CA/ICA certificates used in verifying
local client certificates for RadSec connections. These certificates
can be issued and signed for example a local certificate authority.

After adding certificate, run 'openssl rehash -v .' in this directory.

On some systems the similar comamand may be 'cacertdir_rehash .'.

