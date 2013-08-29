Blah blah blah - now the interesting part:

from the original doc :

The first argument can be an object of hosts, ports mapped to passwords
(i.e., `{'1.2.3.1:6379': 'pass1', '1.2.3.3:6379': 'pass2'}`), or just the password
string.

AND
this config will fail because it goes into this else if block:

 `/index.js` 
  
  `line 52: else if (!util.isArray(nodeList)) .......`

  and sets  the defaults to `127.0.0.1:6379` - congrats on that! Yo!
  
  as a result we get a bunch of errors :
  
```
[19:09:19](haredis#1) warning: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED
[19:09:19](haredis#1) warning: 127.0.0.1:NaN connection dropped, reconnecting...
[19:09:19](haredis#1) warning: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED
[19:09:19](haredis#1) warning: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED
[19:09:19](haredis#1) warning: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED
[19:09:19](haredis#1) warning: Error: 127.0.0.1:NaN connection dropped and reconnection failed!`
```
and not worky worky...  
  
  
  workaround:
  
  to have a config look like that :
``` 
  var nodes = [ {host:'1.2.3.4', port:6379, auth_pass: 'pass1'},
    {host:'host:'1.2.3.5', port:6379,auth_pass:'pass2'},
    {host:'1.2.3.6',  port :6379, auth_pass: 'pass3'} ];
```
  
  and add the 
 `this.auth_pass = spec.auth_pass;`
 
 to the `lib/Node.js lines 11 or 12` and it should be something like:
 
```
 function Node(spec, options) {
  var self = this;
  this.host = spec.host;
  this.port = spec.port;
  this.auth_pass = spec.auth_pass;
  this.clients = [];
  ...
```
