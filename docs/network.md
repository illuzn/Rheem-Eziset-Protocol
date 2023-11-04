# Network

## Control Module

As far as the author can tell, the control module is the device on which the http web server runs.

Based upon deductive reasoning that:

1. If the water heater is turned off, the http web server becomes completely unresponsive.
2. There appears to be a weak/ intermittent connection between the control module and the powerline unit (possibly zigbee, bluetooth or some other short range communication standard given the control module has intermittent connection from approximately 3 metres away).

## Powerline Unit

As far as the author can tell, the Powerline Unit simply relays what the Control Module sends it onto LAN.

It appears to me that the reason this has been done is because Rheem does not want to deal with the technical hassle of WiFi (and having to change wifi settings if the home network changes). I would also note that having WiFi on a device like this is fraught with danger (see my vulnerability analysis of LG Inverters[^1]).


## http

I used packet sniffing software on the android app to determine how connectivity worked. It is unclear to me how the device determines that there is 

A nmap scan reveals only port 80 is open on this device.

The device only communicates via LAN and does not have any innate cloud connectivity whatsoever (even though the server self reprots as embedded.rheem.com.au).

Using curl in verbose mode to reveal the http headers reveals the following:

``` { .text .no-copy }
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

[^1] https://github.com/illuzn/LGResuAdvisory