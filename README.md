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
        - [ ] connect the GPIO2 with pullup to 3V3
    - GPIO3: on the CCM, this is connected to µC.43 via R8 (0 ohms).

- [ ] larger footprint for L10
- [ ] check all pins
- [ ] check the QCA footprint
- [ ] assign footprint to all components in the schematic
- [ ] re-layout the board
- [ ] change to 4 layers?
- [ ] add CP logic
- [ ] add microcontroller
- [ ] add power supply
- [ ] add contactor driver outputs
- [ ] add port temperature sensor inputs
- [ ] add port locking driver
- [ ] add PP detection
- [ ] add LEDs for functional check
- ...

## Power supply options

### The OpenInverter VCU supply
From openinverter VCU (https://github.com/jsphuebner/stm32-vcu/blob/master/Hardware/GS450H_VCU_V3%20-%20Schematic.pdf)
- MP2359DJ as step down from (6V to 24V) to 5V.
    - datasheet https://www.monolithicpower.com/en/documentview/productdocument/index/version/2/document_type/Datasheet/lang/en/sku/MP2359/document_id/228
    - cheap (5 parts 6 euros on ebay from China)
    - "not recommended for new designs"
    - 1.2MHz fix frequency
    - output 1.2A
- Inductance 4.7µH
    - https://www.ebay.de/itm/334756668131?hash=item4df10d62e3:g:Hu4AAOSwRCRj86ed&amdata=enc%3AAQAIAAAAwIlevbuviFR%2B0sjLqsPehVa9Mno3sqzzhAv3LPRo3KcUvaykIYunXIY%2F6kRUSaiXK5sxV1t7o6ReDxByHBYLAhPif7xMisKOHJRe1dCUs9QruNQxlB6LcW5UJkvSTVXsbasBOmmE3yyvdi3%2BJhKULt%2FBdUvi7mYam5gGjXVT8NatmV1PEGNg2LraRAexKYMFv26DKrSs18nwkORQiPaLSAPr%2FQbpYCTiZrpDff3C6W%2FKVuP7u9js2OE2MjbdKU8vHQ%3D%3D%7Ctkp%3ABk9SR7q575ylYg

- Diodes for reverse polarity and step down: Schottky SS54B-HF, 40V, 5A.
    - https://www.mouser.de/datasheet/2/80/20190806170503-2505092.pdf
    
- ams1117-3.3V linear from 5V to 3.3V
    - output 1A
    - cheap (10 parts 4 euros on ebay)
    - datasheet https://datasheet.lcsc.com/szlcsc/2001081204_Shikues-AMS1117-1-2_C475600.pdf
Conclusion: Seems ok for feeding the QCA7005 and ESP32 plus other small consumers.

### The zombieverter supply
From https://github.com/jsphuebner/stm32-vcu/blob/master/Hardware/Zombie/ZombieVerter_V1%20-%20Schematic.pdf
- XL1509-5.0 as step down from (6V to 40V) to 5V
    - https://datasheet.lcsc.com/lcsc/1809050422_XLSEMI-XL1509-5-0E1_C61063.pdf
    - 2A constant output
    - 150kHz fix frequency
    - recommended L: 68uH/2A
    - cheap (10 parts 9 euro on ebay from China)
- Inductance 68uH:
    - e.g. SMD 12x12x7mm
    - e.g. https://de.aliexpress.com/item/4000079812764.html?spm=a2g0o.detail.0.0.1db9fSpGfSpGmG&gps-id=pcDetailTopMoreOtherSeller&scm=1007.40050.281175.0&scm_id=1007.40050.281175.0&scm-url=1007.40050.281175.0&pvid=03c46a0a-7aed-47f8-b4c4-6e97bc2d985f&_t=gps-id:pcDetailTopMoreOtherSeller,scm-url:1007.40050.281175.0,pvid:03c46a0a-7aed-47f8-b4c4-6e97bc2d985f,tpp_buckets:668%232846%238114%231999&isseo=y&pdp_npi=3%40dis%21EUR%211.84%211.84%21%21%21%21%21%40211b613116886252208597357e08be%2110000000209047381%21rec%21DE%21
    

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
