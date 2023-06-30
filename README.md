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

- Takeover from CCM:
   - RBIAS (pin 51) CCM R19 is 2,5k.
   - BIAS_REF (pin 40) CCM R23 is 6,2k.
- The use of the LEDs on GPIO0 to 3 is not clear. Are they used in the automotive firmware at all?
- Check boot config on GPIO 0 to 2.
    - GPIO0: High during boot to boot from the SPI Flash. (pin60) CCM: R9 is 3k3 to 3V3.
    - GPIO1: must be pulled low during reset. (pin61) CCM: R11 is 3k3 to ground.
    - GPIO2: 0=legacy SPI, 1=burst SPI. Which is used in the CCM? (pin62) CCM: R10 is 3k3 to 3V3, and the not-populated R27 into direction of µC.
    - GPIO3: on the CCM, this is connected to µC.43 via R8 (0 ohms).

- check all pins
- check the QCA footprint
- assign footprint to all components in the schematic
- re-layout the board
- change to 4 layers?
- add CP logic
- add microcontroller
- add power supply
- ...

## Programming the SPI Flash

An introduction how to use a Raspberry to program the SPI flash is here:
https://www.rototron.info/recover-bricked-bios-using-flashrom-on-a-raspberry-pi/
The connection is quite simple: SPI_FLASH_programming_with_Raspberry.jpg

Make sure that SPI is enabled in the settings of the Raspberry.
Install the flashrom tool: `sudo apt-get install flashrom`
The docu of flashrom is here: https://www.flashrom.org/Flashrom

Check whether the device is detected: `flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -V`

Binary is available here: https://github.com/uhi22/Ioniq28Investigations/blob/main/CCM_ChargeControlModule_PLC_CCS/CCM_FlashDump_SpiFlash_2MB_Ioniq_00_33_79.bin

For viewing the binary, a hex editor can be used, e.g. http://www.funduc.com/fshexedit.htm

To flash the binary file to the SPI flash: `flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -w myfile.bin` In the case that to tool complains that multiple flash chip definitions match the detected chip, choose the correct one with the option `-c "MX25L...something...`
