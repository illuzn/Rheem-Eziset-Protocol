#Hardware

Note: The information on this page is incomplete. The author has not disassembled his unit to further investigate what his limited investigations have revealed.

##Use Case

According to information on Rheem's own website, this unit can be used on:
- Rheem 12L (876812)
- Rheem Metro 16L (876T16)
- Rheem 20L (876820)
- Rheem Metro 26L (876T26)
- Rheem 27L (876627, 876027, 866627, 866027)
Rheem claims that the device is only suitable for Rheem 12L to 26L models manufactured since 2018.

*Although the author hasn't investigated the matter further, it is unclear why this unit is restricted to models built after 2018. The device emulates the functionality of common kitchen side and bathroom side controllers that have been available for many years. The author does note that the unit may not fit with the included mount inside older models. Certainly, the authors prior unit that was manufactured in 2009 did not have sufficient clearance to fit this device with the standard mount - that said, with some ingenuity the author is certain that this could be made to work (even if not specifically surpported by the vendor).*

##Powerline Unit

![Powerline Unit](plug.png)

Based upon investigations, this unit appears to be purely passive. It communicates wirelessly with the Control Module and passes that information through using ethernet (10/100T). The method of communication is unknown beside the fact that it is not via wifi.

This information has been surmised because turning off power to the control module results in no response at all from the http server provided by this device.

Rheem Australia uses a specific MAC Address block it has acquired for the purposes of this unit - the following information is extracted from the IEEE MAC Assignments database:

```
50-98-F3   (hex)		Rheem Australia Pty Ltd
5098F3     (base 16)		Rheem Australia Pty Ltd
				1 Alan Street
				Rydalmere  NSW  2116
				AU
```

##Control Module

![Control Module](control.png)

This device is installed inside the water heater unit. 

The white set of 2 cables piggy backs on low voltage the power supply to the water heater control circuitry.

The black set of cables plugs into the water temperature input/output that is used by Rheem's normal temperature setting devices.

This control module also operates the webserver which is used to control the temperature of the water heater.