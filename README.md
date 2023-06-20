# QCA7000/7005

Originally designed https://github.com/Millisman/QCA7000, forked to https://github.com/uhi22/QCA7000board

## Changes

### Schematic
- removed JTAG completely
- removed connector for SPI flash, added testpoints instead

- ATP_P and ATP_N are reserved analog test points. Leave unconnected without testpoints.
- ZC_IN connected to ground. Not used.
- PLL_BYPASS has an internal pulldown. Removed the external pulldown.
- Removed the LEDs on the GPIO.
- RSVD06 and 07 are "do not connect". Removed the test points.
- Bugfix: The ground was missing between C13 and C14.

### symbol QCA7000:
 - RSVD01 changed to power_in, because connected to 3.3V
 - RSVD04 changed to power_in, because connected to 1.2V
 - TCK changed from output to input, because this is the test clock driven by debugger. Even if the data sheet says "output".
 
### Footprints in Schematic

- changed the 100nF to 1206 for easy handsoldering
- changed the 10µF to 1210
- changed other 0603 Cs to 0805
- changed L and Rs to 1206



## Todos

- How big are the RBIAS and BIAS_REF in the CCM?
- The use of the LEDs on GPIO0 to 3 is not clear. Are they used in the automotive firmware at all?
- Check boot config on GPIO0 to 2. How they are configured in the CCM?
-- GPIO0: High during boot to boot from the SPI Flash.
-- GPIO1: must be pulled low during reset.
-- GPIO2: 0=legacy SPI, 1=burst SPI. Which is used in the CCM?

- check all pins
- check the QCA footprint
- assign footprint to all components in the schematic
- re-layout the board
- ...