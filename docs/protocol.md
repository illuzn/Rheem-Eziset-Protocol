# Protocol

All requests are made using HTTP GET and are case sensitive. Each subheading below should be passed to your device as follows:
```
curl http://192.168.XXX.XXX/heaterName.cgi`
```

All inputs are also passed via standard HTTP GET arguments as follows:
```
curl http://192.168.XXX.XXX/ctrl.cgi?sid=0&heatingCtrl=1
```

All responses are provided in JSON.

## DDoS protection

Note: This information is based on speculation, the author has not seen the underlying code to confirm whether or not it is correct.

My Rheem water heater has some basic DDoS protection which was causing lots of connectivity issues which I had thought was due to the devices being around 10 metres apart and seperated by a wall and (including a metal door) - which I thought was reducing signal strength.

Essentially, if the device receives more than 1 request per second (the author cannot determine the precise limits of this), the device will enter a state whereby it will refuse all connections. Since updating to the new Home Assistant logic provided here I haven't run into this issue for around 3 weeks (it previously occurred almost daily especially when there would tend to be a lot of radio interference).

Continued requests (again the author cannot state what this limit is) will cause the device to lock itself down and refuse to answer any requests until the water heater is physically switched on and off again.

## /heaterName.cgi

Example output:
```
{"heaterName":"Rheem"}
```

Inputs:
```
None
```

Outputs:

Name | Type | Description
-----|-------|------------
heaterName | string | The friendly name of your heater unit. 


## /version.cgi

Example output:
```
{"FWversion":"1.4"}
```

Inputs:
```
None
```

Outputs:

Name | Type | Description
-----|-------|------------
FWversion | string | The firmware version of your control unit. 


## /getParams.cgi

Example output:
```
{"heaterModel":1,"tempMax":50,"tempMin":37,"bathtempMax":48,"bathtempMin":37,"bathvolMax":500,"bathvolMin":10}
```

Inputs:
```
None
```

Outputs:

Name | Type | Description
-----|-------|------------
heaterModel | integer | Unknown for now. My unit responds with 1. Further research is required to enumerate the model numbers.
tempMax | integer | The maximum temperature that the hot water supply can provide.
tempMin | integer | The minimum temperature that the hot water supply can provide.
bathtempMax | integer | The maximum temperature that the hot water can supply for Bath Fill mode. This mode is designed (as the name implies) to supply a large fixed volume of water at once for filling baths. After the volume is supplied, the water supply will turn itself off.
bathtempMin | integer | The minimum temmperature for Bath Fill mode (see above).
bathvolMax | integer | The maximum volume of water to supply for Bath Fill mode (see above).
bathvolMin | integer | The minimum volume of water to supply for Bath Fill mode (see above).


## /getInfo.cgi

Example output:
```
{"heaterName":"Rheem","mode":10,"temp":37,"flow":0.0,"wtemp":14.1,"intemp":17.9,"state":1,"appErrCode":0,"sid":1154,"sessionTimer":300,"sTimeout":248,"heatingCtrl":1,"bathfillCtrl":0}
```

Inputs:

Name | Required | Type | Description
-----|----------|-------|------------
sid | false | integer | A *session id* from `/ctrl.cgi`. If *session id* is provided then the extra outputs below are revealed.


Outputs:

Name | sid Required | Type | Description
-----|--------------|-------|------------
heaterName | false | string | The friendly name of your heater unit.
mode | false | integer | See below table.
temp | false | integer | The requested hot water temperature
flow | false | float | The current flow of hot water through the unit in L/ min rounded to 1 decimal place.
state | false | integer | 1 = idle, 2 = water heater active, 3 = valve closed after bath fill
appErrCode | false | integer | Apparently, an error code output - codes unknown for now.
sTimeout | false | integer | If a user has control of the water heater, returns the time left until that session expires in seconds. Otherwise, returns 0.
sessionTimer | false | integer | Returns the configured session timeout for a user in seconds.
bathtemp | false | integer | If bath fill mode is activated, returns the requested bath fill temperature.
bathvol | false | integer | If bath fill mode is activated, returns the requested bath fill volume.
fillPercent | false | float | If bath fill mode is activated, returns approximate percentage of bath volume dispensed (this percentage is not accurate for very small bath fill volumes).
wtemp | true | float | The current water temperature as read from the heating block of the water heater in degrees celcius rounded to 1 decimal place.
intemp | true | float | The current intake water temperature from supply in degrees celcius rounded to 1 decimal place.
sid | true | integer | Returns your current session id.
heatingCtrl | true | integer | Returns 1 if you currently have control of the water heater using conventional mode. Otherwise returns 0.
bathfillCtrl | true | integer | Returns 1 if you currently have control of th water heater using bath fill mode. Otherwise returns 0.


Modes:

Value | Description
------|------------
5 | Idle
10 | Heating Control Mode
15 | Water Heater Active (Conventional Mode)
20 | Bath Fill Active (Waiting for Tap Open)
25 | Water Heater Active (Bath Fill Mode)
35 | Bath Fill Complete (Water Flow Disabled)


## /ctrl.cgi Conventional

Example output:
```
{"sid":463,"sTimeout":300,"heatingCtrl":1,"bathfillCtrl":0}
```

Inputs:

Name | Required | Type | Description
-----|----------|-------|------------
sid | false | integer | A *session id* from `/ctrl.cgi`. A *session id* of 0 must be provided to take control of the water heater.
heatingCtrl | false | integer | 1 = Take control, 0 = Relinquish control


Outputs:

Name | sid Required | Type | Description
-----|--------------|-------|------------
sid | false | integer | Returns your sid, if you have control. Otherwise, returns 0.
sTimeout | false | integer | If a user has control of the water heater, returns the time left until that session expires in seconds. Otherwise, returns 0.
heatingCtrl | true | 1 = Your session has control, 0 = Your session does not have control.
bathfillCtrl | true | integer | See Bath Fill Mode below.


## /ctrl.cgi Bath Fill Mode

Example output:
```
{"sid":649,"sTimeout":60,"heatingCtrl":0,"bathfillCtrl":1,"reqbathtemp":37,"reqbathvol":10}
```

Inputs:

Name | Required | Type | Description
-----|----------|-------|------------
sid | true | integer | A *session id* from `/ctrl.cgi`. A *session id* of 0 must be provided to take control of the water heater.
bathfillCtrl | true | integer | 1 = Take control, 0 = Relinquish control
setBathTemp | false | integer | (Required if batthfillCtrl=1) A requested temperature for the bath fill in degrees celcius.
setBathVol | false | integer | (Required if batthfillCtrl=1) A requested volume for the bath fill in litres of water.


Outputs:

Name | sid Required | Type | Description
-----|--------------|-------|------------
sid | false | integer | Returns your sid, if you have control. Otherwise, returns 0.
sTimeout | false | integer | If a user has control of the water heater, returns the time left until that session expires in seconds. Otherwise, returns 0.
heatingCtrl | true | 1 = Your session has control, 0 = Your session does not have control.
bathfillCtrl | true | integer | 1 = Your session has control, 0 = Your session does not have control.
reqbathtemp | true | integer | Returns your requested bath fill temperature in degrees celcius.
reqbathvol | true | integer | Returns your requested bath fill volume in litres.


## /set.cgi

These commands **require** that you have control via `/ctrl.cgi` and a *session id*. At least one of the command inputs must be provided.

Inputs:

Name | Required | Type | Description
-----|----------|-------|------------
sid | true | integer | A *session id* from `/ctrl.cgi`.
setTemp | false | integer | Sets the requested hot water temperature in degrees celcius.
setSessionTimer | false | integer | Sets the session timer in seconds after which your control of the water heater will automatically timeout and allow other users control.


Outputs - setTemp:

Name | Type | Description
-----|-------|------------
temp | integer | Returns the previously set temperature
reqtemp | integer | Returns your requested temperature
sid | integer | Returns your session id
sTimeout | integer | Returns your timeout.


Outputs - setSessionTimer

Name | Type | Description
-----|------|------------
SessionTimer | integer | Returns your requested session timer
