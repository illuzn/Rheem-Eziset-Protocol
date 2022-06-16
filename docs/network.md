# Network

A nmap scan reveals only port 80 is open on this device.

The device only communicates via LAN and does not have any innate cloud connectivity whatsoever.

Using curl in verbose mode to reveal the http headers reveals the following:

```Text
*   Trying 192.168.XXX.XXX:80...
* Connected to 192.168.XXX.XXX (192.168.XXX.XXX) port 80 (#0)
> GET /getInfo.cgi HTTP/1.1
> Host: 192.168.XXX.XXX
> User-Agent: curl/7.79.1
> Accept: */*
>
* Mark bundle as not supporting multiuse
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
HTTP/1.0 200 OK
< Server: embedded.rheem.com.au
Server: embedded.rheem.com.au
< Content-type: application/json
Content-type: application/json
```
The device does not respond to requests on any other port.