Signed datetime
===============

| This is a response for command :doc:`SET_SIGNED_DATETIME <../../commands/security/set-signed-time>`. 
| Returns the status (error code) of the operation performed.
| Notification is unencrypted.

SIGNED_DATETIME notification code is 0x7B

Output params
-------------

Param [0] - status code
    * 0x00 means that datetime was set successfully,
    * 0x01 means that operation failed.
