# foccci - The Fully Open CCS Charge Controller Interface

![image](doc/foccci_logo_colored.jpg) Hi, I'm Foccci. Just call me [Foksi] or [Fok-See] or just Foxi :-)
My real name is "Fully Open CCS Charge Controller Interface", but that's just too difficult to remember.

What I am? I'm a Charge Controller that enables CCS charging of homebrew electric vehicles.
I was born in the OpenInverter forum community, https://openinverter.org/forum/viewtopic.php?t=3727, and I'm growing more and more due to the great people there.
Inside, I have a QCA7005 PLC modem and an STM32 microcontroller. I'm a piece of hardware, and because hardware alone is quite boring,
I'm in a team with Clara, https://github.com/uhi22/ccs32clara, which is dealing with the bits and bytes of our mission. And together, we can do a lot for you.

![image](doc/foccci_and_clara_logo_colored.jpg)

# News

## 2024-06-27 Foccci Is Gaining Speed

While some folks are integrating Foccci in their vehicles and providing nice pictures for the galery (https://openinverter.org/forum/viewtopic.php?t=5077), we were proudly presenting the project at the
Augsburg Linux info day (https://openinverter.org/forum/viewtopic.php?t=4800). Sleep and wakeup,
new, fast contactor driver and a new connector pinout provide more flexibility for the car integration. That will
be the version 5, which will hopefully pop-up in the shop in July.

## 2023-12-13 Supercharging works. Halve a dozen boards are spread around the world. 

Johu is charging the Touran on the TESLA Supercharger using the hand-populated V3 board: https://openinverter.org/forum/viewtopic.php?p=64563#p64563 and https://www.youtube.com/watch?v=OKg3VUslol8
The first JLC populated boards have been completed manually, only one bug in the schematic needed a small workaround. Boards are on the way to several enthusiasts for further testing. The software is growing thanks to the great openinverter community.

## 2023-10-25 First Order of Populated Boards

Received four reservations on openinverter forum, and this was the trigger to create version v4.1 with minor layout
improvements, and to order a batch of five
at JLCPCB. Total cost for five: $127.32 including shipping, taxes and duties. The NCV8402 was out of stock,
and the QCA7005 is anyway not in their stock. So the material price is

- 25€ JLCPCB populated PCB
- 12€ QCA7005
- 4€ 2 x NCV8402
- 1€ RF transformator
- <1€ connectors for programming and serial line

## 2023-10-08 Light-Bulb-Demo-Charging on Alpitronic and ABB works

Using the foccci V3 board together with ccs32clara software, the muehlpower voltage sense board, a cheap chinese relay, the light-bulb-demo
worked on the first attempt on alpitronics hypercharger and on an ABB triple charger.

![image](doc/2023-10-08_foccci_on_alpi_with_bulb.jpg)

![image](doc/2023-10-08_foccci_on_alpi_with_bulb_savvyCAN_graph.jpg)

![image](doc/2023-10-08_foccci_on_ABB_with_bulb.jpg)


## 2023-07-28 Charging Loop Reached

The foccci board version 2023-07-12 runs the STM32F103 and the QCA7005, and successfully enters
the charging loop, with simulated evse by pyPLC. It provides the checkpoint number and some other
informations via CAN with 500kBaud. On the other side of the CAN sits an ESP32 Wifi-Kit-32, with
a CAN transceiver added. It shows the data of the CAN on its OLED display.

![image](doc/2023-07-28_first_democharging.jpg)


## 2023-07-09 The board starts talking

After the PCBs and QCAs arrived, the soldering was the next challenge. It went well, and the board started talking to
the ESP32 running ccs32berta https://github.com/uhi22/ccs32berta

https://openinverter.org/forum/viewtopic.php?p=58627#p58627

## 2023-06-21 First PCB with the QCA7005

...is ready in KiCad and is sent to JLCPCB. It is a two-layer, and has so many capacitors, that they do not really fit
to the place where they would make sense. Caused by taking the schematic from the Millisman design, but changing
the size to 1206 for easy hand soldering. Btw: Below five dollars for five PCBs, including shipping, duties and taxes.

## 2023-06-14 The Idea Is Born

...and the discussion is starting on the OpenInverter forum
https://openinverter.org/forum/viewtopic.php?p=57643#p57643

# Board versions

## V1 June 2023

![image](doc/foccci_v1_june2023_foto.jpg)
- Just the QCA7005, exposing the SPI interface.
- No controller, no voltage regulator, no CP logic.
- Test result summary: Light-bulb-demo-charging works on alpitronics, with ccs32berta running on ESP32. (https://github.com/uhi22/ccs32berta)

## V2 2023-07-12

![image](doc/foccci_v2_2023-07-12_foto.jpg)
- STM32 controller
- Power supply
- CAN transceiver
- CP logic
- Two contactor drivers
- Test result summary: Light-bulb-demo-charging works on alpitronics, with ccs32clara https://github.com/uhi22/ccs32clara running on the STM32.  https://openinverter.org/forum/viewtopic.php?p=59821#p59821

## V3 2023-08-17

![image](doc/foccci_v3_2023-08-17_foto.jpg)
- HBridge driver DRV8874 for connector lock
- discrete drivers for RGB LEDs
- Deutsch Connector 2x12pins
- added CP and PP sensing
- changed from 2 layers to 4 layers
- Test result summary: The new output drivers work with https://github.com/uhi22/ccs32clara. Other tests still in progress.
- Issues:
    - Diode to 3V3 is missing in the CP sense path (R58). Need to be patched.
    - There is a via near the pin1-corner of the QCA, which is very near to the metal frame at the corner, and may cause shortcut between 1V2 and ground.

## V4 2023-09-10

![image](doc/foccci_v4_2023-09-10_3DView.jpg)
- Prepared for assembling by JLCPCB
- No 1206 size components anymore. Means: 0805 is the new default size now.
- Test result summary: PCBs not ordered yet. No testing done yet.
- Outdated, never produced.

## V4.1 2023-10-25

- functionally identical to V4
- moved via away from QCA corner to avoid solder bridge
- silk screen improvements: more component names and better readability
- Test result summary: one issue found (shortcut on the RX inputs to ground via inductors). After fixing this, everything works fine.

## V4.2 2024-02-06

![image](doc/foccci_v4.2_2024-02-23_foto.jpg)
- rx path grounding bug resolved
- added RF transformer
- proximity pilot input circuit corrected (330ohm pull-up to 5V instead of 1k to 3.3V)
- hardware variant coding with two resistors on analog port added
- routed the current measurement pin of the lock driver DRV8874 (IPROPI) to the ADC
- more copper cooling area for the contactor drivers
- more spare holes
- added openinverter logo - and lost it later when transferring the schematics update to the PCB - sorry
- added mounting holes
- layout optimizations, footprint and part number corrections
- Issues:
    - R56 (PP pull-up) has 33ohms instead of intended 330ohms. Wrong part number C17634.
    - missing track between D4 and D7
    - missing tracks on C39 (IPROPI)
    - missing track on R63 (IPROPI)
    - missing tracks on R28 (ADC_PP)
    - missing ground on R60 (PP_PULLDOWN)

These are the patches:
![image](doc/foccci_v4.2_patches.jpg)

## V4.3

fixes the issues of the 4.2

## V4.5

- contactor driver is now the same, fast as the lock motor driver
- wakeup logic added

## V4.5b

First version with the changed connector pinout. Intermediate version, never produced.

## V5

Functionally identical with V4.5b. The first official version with the new connector pinout.
- layout fixes (missing connection D20, clearance violations, courtyard violations)
- QCA JTAG preparation
- Added openinverter and Foccci logos

![image](doc/foccci_v5_2024-06-27.jpg)

- Issues
    - enable pin of the step-down gets battery voltage, which leads to current draw
    - wrong resistor values R52 and R64 in the BOM

## V5.1

- added D8 at the enable pin of the step-down, to avoid unintended current flow
- removed spare capacitors on the crystal from schematic
- propagated the 1k values of R52 and R64 to the board
- fixed some rotations

# Background

The QCA7005 schematic and board was originally designed by Millisman https://github.com/Millisman/QCA7000, then forked to https://github.com/uhi22/QCA7000board and then renamed to https://github.com/uhi22/foccci

# Changes to the original Millisman design

## Schematic
- removed JTAG completely
- removed connector for SPI flash, added testpoints instead

- ATP_P and ATP_N are reserved analog test points. Leave unconnected without testpoints.
- ZC_IN connected to ground. Not used.
- PLL_BYPASS has an internal pulldown. Removed the external pulldown.
- Removed the LEDs on the GPIO.
- RSVD06 and 07 are "do not connect". Removed the test points.
- Bugfix: The ground was missing between C13 and C14.

## symbol QCA7000:
 - RSVD01 changed to power_in, because connected to 3.3V
 - RSVD04 changed to power_in, because connected to 1.2V
 - TCK changed from output to input, because this is the test clock driven by debugger. Even if the data sheet says "output".
 
## Footprints in Schematic

- changed the 100nF to 1206 for easy handsoldering (and back to 0805 for some, to safe space)
- changed the 10µF to 1210
- changed other 0603 Cs to 0805
- changed L and Rs to 1206 (and the beads back to 0805)



## Todos for now

[ ] Footprint for L13 leads to complains of JLC, see https://openinverter.org/forum/viewtopic.php?p=72630#p72630


## Todos for Later

- [ ] TVS at the CP    
- [ ] add optional "Molex Mini-Fit Jr. Header, Dual Row" for the case, that the Deutsch is not needed in certain use cases.
- [ ] Docu: add details of step-by-step bring-up, incl current consumption, and typical errors
- [ ] more temperature inputs (6, to support L1, L2, L3, N, DC+ and DC-)
- [ ] The use of the LEDs on GPIO0 to 3 is not clear. Are they used in the automotive firmware at all? GPIO3: on the CCM, this is connected to µC.43 via R8 (0 ohms). Measure the pin.
- [ ] Regarding AMS1117-3.3 for the 3v3 regulators. These are tried and tested, but you may wish to avoid the polarized capacitor by using a more modern regulator. Consider TLV75733PDBV "Stable With a 1-μF Ceramic Output Capacitor"
- [ ] The STM32F103 seems to meet the requirements and it's popular but be aware that it has a basic ARM core with no FPU. And: Do we need a low-power variant (stm32L4 series)?
- [ ] The QCA7005 is labelled QCA7000.
- [ ] missing the JTAG port pull down resistors on the QCA
- [ ] Could do with one more processor decoupler so that a cap can be dropped right against every package power pin.
- [ ] Would be worth adding lines to the QCA7005 Reset (so that is can be reset without a power cycle) and the Int pins (just in case it's needed in the future).
- [ ] Might put a Schottky protection diode on the temperate sense inputs.
- [ ] supply voltage measurement (to be able to adjust the PWM for contactors according to the supply voltage, and just for info)


## Finished Todos
- [x] do not inject >7V into the enable pin of the step-down
- [x] Low-power concept
- [x] Clarify interface (electrically and mechanically)
- [x] more copper cooling area for the NCVs
- [x] analog current feedback from the DRV8874 IPROPI to the controller
- [x] Add a version indication analog circuit. Discussed here: https://openinverter.org/forum/viewtopic.php?p=66745#p66745
- [x] Change the PP pull-up to 330 ohms and 5V, and optionally prepare pull-down 2k7. Use voltage divider to the controller, to be able to measure the full 0 to 5V range. Discussion here: https://openinverter.org/forum/viewtopic.php?p=66208#p66208

- [x] For C69, how to workaround the forced switch from "economy" to "standard" service? Workaround: use C3445244, discussed here: https://openinverter.org/forum/viewtopic.php?p=65016#p65016
- [x] Remove DC-grounding of the RX inputs. Done: C32 and C33 added on the RX inputs.
- [x] Transformer (discussion here: https://openinverter.org/forum/viewtopic.php?p=64465#p64465) see https://jlcpcb.com/partdetail/3170710-CL4532A201/C2904734 Done: Added into new lib transformerCL4532A-201 and to schematic and pcb.
- [x] Adapt the part number for the NCV8402 (discussed here: https://openinverter.org/forum/viewtopic.php?p=62903#p62903). Done: Part number C77755 instead of C236053
- [x] Adapt the footprints / part numbers for the parts which JLC could not populate
    - [x] U1 Flash: Old: Package_SO:SOP-8_5.28x5.23mm_P1.27mm, must be SOIC 150mil (order code SN), which is Package_SO:SOIC-8_3.9x4.9mm_P1.27mm.
    - [x] L11: The https://jlcpcb.com/partdetail/C162588 is normal 1206. Changed from Inductor_SMD:L_1812_4532Metric_Pad1.30x3.40mm_HandSolder to Inductor_SMD:L_1206_3216Metric_Pad1.22x1.90mm_HandSolder
- [x] Add additional mounting holes
- [x] Stepdown layout optimization (discussed here: https://openinverter.org/forum/viewtopic.php?p=61697#p61697), including smaller L, e.g. https://jlcpcb.com/partdetail/LantuMicro-SMS1050100MT/C5127462 instead of C3000379. And, next step, even smaller
https://jlcpcb.com/partdetail/Sunlord-MWSA0503S100MT/C408412 is 5.1 x 5.4mm.
- [x] TVS at the 12V. SMAJ18A
- [x] move as much as possible bottom layer components to upper layer, to allow population by JLBPCB
- [x] adapt layout for changed footprints
- [x] check/remove/correct the JLCPCB part numbers for all parts. Most parts now 0805.
- [x] add protection diode for CP sense
- [x] CP sensing
- [x] 5V on the connector for supply of the muehlpower high voltage sense board
- [x] Clarify expected contactor current and use appropriate drivers
- [x] Footprints for the bigger capacitors too small. -> Use 1210 for C65, C66, C67, 
- [x] Many texts in the silk screen are not readable due to overlap with solder or components
- [x] Some of the many capacitors on the supply lines could be removed
- [x] Use 0805 for all 100nF, instead of mixing 0805 and 1206.
- [x] The EN pin of the CAN transceiver should have the flexibility to solder-bridge to GND and 3V3, to allow different type of transceivers.
- [x] Change footprint of the test points for flash programming, so that squared connector pins fit into.
- [x] highside LED drivers for RGB button
- [x] VDDA of the STM32 needs additional 1µF according to STM application note AN2834. https://www.st.com/resource/en/application_note/an2834-how-to-get-the-best-adc-accuracy-in-stm32-microcontrollers-stmicroelectronics.pdf chapter 4.2.1 -> C14 added.
- [x] add port locking driver
- [x] Complete the schematic and the layout for the DRV8874 h-bridge driver
- [x] add PP detection
- [x] change to 4 layers
- [x] One of your AMS1117 seems to have a 100nF output cap and the other doesn't.
- [x] 470u input cap is an electrolytic? Yes.
- [x] Which connector? DTM13-12PA-12PB-R008 https://openinverter.org/forum/viewtopic.php?p=58697#p58697
- [x] HCT04: Risk of activating the contactors if the controller power fails. Find a better solution.
- [x] Check boot config on GPIO 0 to 2.
    - [x] GPIO0: High during boot to boot from the SPI Flash. (pin60) CCM: R9 is 3k3 to 3V3.
    - [x] GPIO1: must be pulled low during reset. (pin61) CCM: R11 is 3k3 to ground.
    - [x] GPIO2: 0=legacy SPI, 1=burst SPI. Which is used in the CCM? (pin62) CCM: R10 is 3k3 to 3V3, and the not-populated R27 into direction of µC.
        - [x] connect the GPIO2 with pullup to 3V3
- [x] add LEDs for functional check
- [x] add mounting holes
- [x] correct voltage divider in the TPS54302
- [x] 3V3_uC and NRST on connectors for debugger
- [x] removed the wrong part number and datasheet for the 8MHz XTAL
- [x] larger footprint for L10: From 1206 to 1812
- [x] check all pins -> all plausible with the data sheet.
- [x] check the QCA footprint -> fits.
- [x] assign footprint to all components in the schematic
- [x] add CP logic
- [x] add microcontroller
- [x] add power supply
- [x] add contactor driver outputs
- [x] add port temperature sensor inputs
- [x] Takeover from CCM:
   - [x] RBIAS (pin 51, R9). In the CCM this is R19 with 2,5k. Used 3k || 15k, this is exactly 2k5.
   - [x] BIAS_REF (pin 40, R6). In the CCM this is R23 with 6,2k. -> Changed from 2k5 to 6k2 in the schematic. Populated 6k8.
- [x] What's going on with the missing processor Gnd pins, are they mapped within the part? Yes, in the layout they are correct.
- [x] Add LIM-like current input for HVDC voltage measuring, https://openinverter.org/forum/viewtopic.php?p=58839#p58839

# Interface

...is described in the wiki: https://openinverter.org/wiki/Foccci

# Connector and Housing

Ideas: https://openinverter.org/forum/viewtopic.php?p=59972#p59972

Plan:
- use the DTM13-12PA-12PB-R008 - 2 x 12 pins
- data sheet: https://www.te.com/usa-en/product-DTM1312PA12PBR008.html (mentioned here: https://openinverter.org/forum/viewtopic.php?p=60064#p60064)
- Enclosure: https://www.te.com/usa-en/product-EEC-325X4B.html


# Power supply options

## The OpenInverter VCU supply
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

## The zombieverter supply
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
    

## The Catphish supply
From https://openinverter.org/forum/viewtopic.php?p=58555#p58555
- TPS54302 as step down from (6V to 28V) to 5V
    - voltage is flexible by resistor divider
    - output 3A
    - 400kHz fix frequency
    - SOT-23 (6) package
    - no external diode on the output required
    - https://www.ti.com/lit/gpn/tps54302 
- L 10uH, JLC C408412
- TLV75733PDBV linear from 5V to 3V3
    - 1A
    - SOT-23 5pin
    - https://www.ti.com/lit/ds/symlink/tlv757p.pdf?HQS=dis-mous-null-mousermode-dsf-pf-null-wwe&ts=1688651505013&ref_url=https%253A%252F%252Fwww.mouser.co.uk%252F


# CAN Transceiver
- SN65HVD234
    - https://www.ti.com/lit/ds/symlink/sn65hvd230.pdf?HQS=dis-dk-null-digikeymode-dsf-pf-null-wwe&ts=1688711367456&ref_url=https%253A%252F%252Fwww.ti.com%252Fgeneral%252Fdocs%252Fsuppproductinfo.tsp%253FdistId%253D10%2526gotoUrl%253Dhttps%253A%252F%252Fwww.ti.com%252Flit%252Fgpn%252Fsn65hvd230

# Requirements
- [ ] 8V to 30V power supply
- [ ] PLC communication
- [ ] CAN communication
- [ ] CP level handling (and -detection?)
- [ ] PP detection
- [ ] 12V highside LED drivers (for RGB LEDs as in BMW LIM)
- [ ] Contactor driver (with PWM?) for two contactors
- [ ] Charge port temperature sensing (2 channels, either NTC or PT1000)
- [ ] Connector lock motor bridge and connector lock feedback
- Optional:
    - [ ] Analog input for HV voltage sensor at the charging port
    - [ ] fuel flap switch
    - [ ] button to start/stop the charging process
    - [ ] HV contactor feedback
    
# Requirements for the low-power-mode

- The foccci shall fall in low-power-mode, if not needed. The consumption shall be less than 5mA during this mode.
- The foccci shall use the enable pin of the step down converter TPS54302 to shut down the power supply.
- The following ways shall be used to activate the high-power-mode:
    - CP line: A significant voltage (above 4V) for a significant time (at least some milliseconds).
    - Button: A button to ground, internally pulled-up to 12V, if pressed long enough (at least some milliseconds).
    - microcontroller: The STM32 shall be able to keep itself powered by using a toggle pin. In case of stuck to high or low, the foccci shall power-down.
    - Not planned at the moment: Wakeup via CAN communication. This would be a helpful use case, if the user wakes the car via remote key (unlock), and the connector is unlocked by this activity.
    

# Controller

STM32 selection:

Overview: http://stefanfrings.de/stm32/stm32f1.html

https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html
- STM32F103C8T6 ("blue pill") 64kB Flash and 20kB RAM too small. -> FAIL
- STM32F401CC ("black pill") 256kB Flash and 64k RAM. No CAN -> FAIL
    - https://www.st.com/en/microcontrollers-microprocessors/stm32f401cc.html
- STM32F103RC 256k Flash, 48k RAM, CAN 
- STM32F103RD 384k Flash, 64k RAM, CAN (10 pieces 22 euro on Aliexpress)
- STM32F103RE 512k Flash, 64k RAM, CAN (10 pieces 24 euro on Aliexpress)
- STM32F303RE (Nucleo demo board available) (STM32F303RET6) 512K Flash, 80k RAM, LQFP-64, 1 CAN. (6 euro on Aliexpress)

Programming:
- en.st-stm32cubeide_1.12.1_16088_20230420_1057_x86_64.exe

# First run of the June 2023 version

- Starting with power suppy connected to the 3V3, but set to 0V. Slowly increase the voltage to 0.5V. Observe the current. No current is good.
- Measure, whether the DVDD pins of the QCA and 3 pins of the Flash have 0.5V.
- Connect multimeter to 1V2, and turn the power supply slowly up.
- With 2V, the board draws 4.5mA.
- At 2.7V, the current jumps from 6mA to 13mA. This is when the 1.2V regulator starts delivering its intended output voltage.
- At 3.3V, the current is 20mA. This does not look like normal operating current.
- On the L10 input, there is a 0V/3.3V quare wave with 600ns cycle time. This looks normal for the step-down-converter.
- On the R6 (both sides), there is the 25MHz clock, with ~1V amplitude.
- The Reset line is 3.3V (no reset). When pulling it to ground, the current consumption increases from 20.8mA to 23mA.
- The CS of the Flash is permanent high. This means, the QCA does not want to load its programm code from the Flash. Something is wrong.
- The RBIAS and R_BIASREF were not populated. Added them.
- Looks better: At 3V3, the current is 80mA. At the CS of the Flash there is activity for one second after power-on. Seems the QCA is fetching its program code.
- Connect the ESP32 with the ccs32berta via SPI, using 220 ohm protection resistors in all four SPI lines. ESP powered via USB, QCA powered from 3.3V bench power supply. Result: The Berta finds the modem and starts the SLAC.


# Programming the SPI Flash

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

![image](doc/2023-07-28_foto_flash_progr_with_raspberry.jpg)
![image](doc/SPI_FLASH_programming_with_Raspberry.jpg)

# Create Fabrication Output for JLCPCB

Starting with KiCad7, the The xsltproc appears to have been removed. So the procedure to create the BOM needs to be adjusted.
In https://gist.github.com/arturo182/a8c4a4b96907cfccf616a1edb59d0389 we find, that there is a plugin now: "Fabrication Toolkit".
This can be installed with the "Plugin and Content Manager" in the KiCad main window.
Then, in the PCB editor, use the FabricationToolkit from the toolbar. This creates a new folder in <project>/production/, which
contains a zip with Gerber and drill files, and also the BOM, positions and designator files.

The docu of the Fabrication Toolkit is here: https://github.com/bennymeg/JLC-Plugin-for-KiCad

Some of the IC footprints have the wrong orientation in the JLCPCB viewer. The Fabrication toolkit offers the possibility to
rotate (and shift, if necessary) the footprints, by adding in the schematic an attribute 'JLCPCB Rotation Offset'.
Thanks to jrbe for the hint in https://openinverter.org/forum/viewtopic.php?p=61049#p61049




# JLCPCB offers

Offer 2023-08-27: 5 PCBs, 2 populated.
- Option A: All available components: $82, which includes Extended components fee: $45.30
- Option B: Only basic components. $21. Plus shipping and duties, = $38.

Offer 2023-08-28: 5 PCBs, 5 populated.
- Total $107 for 5 populated PCBs, plus shipping ($13) and duties and taxes ($23). Plus QCA, connector, inductor, transformator, housing.
- Includes $42 extended components fee.

# Soldering Lessons

[solder_lessons.md](doc/solder_lessons.md)

# Trouble Shooting / FAQ

## How to trouble shoot Foccci/Clara?

For trouble shooting it is recommended to create a log file of the serial output. You need a serial-to-usb converter and connect it to the UART TX line and ground. For recording, you can use e.g. Putty on windows or any console program. The baud rate is 921600. Not each cheap USB-to-serial is able to catch this high speed. So if you get only garbage, try an other adaptor.

Additionally, use the openinverter web interface to observe spot values. As default, the Node ID of Foccci/Clara is 22.

## No communication with the charger possible

Check the following points (discussed here: https://openinverter.org/forum/viewtopic.php?p=64449#p64449 )
1. Is the 1.2V present and stable?
2. Does the STM detect the modem? Means: Does it show the QCAs software version?
3. Does the QCA go to sleep 30s after power-on? This is when the STM says "zero modems found".
4. Does the QCA stay awake longer than 30s when a central coordinator is present. If not, this means the QCA does not hear the central coordinator, so the RX path is deaf.
5. Are the RX and TX pins (4 in total) on the QCA at around 1.6V statically? If not, then check for short circuits.
6. Is the resistance between the RX pins around 500 ohms (measured when not powered)? If not, check for short circuits or bad soldered pads.
7. Is the resistance between the TX pins around 16kohms (measured when not powered)? If not, check for short circuits or bad soldered pads.
8. If evse (central coordinator) is connected, do you see packets on the RX pins? Using two osci channels, they should be invers on positive and negative RX pins.
9. In the web interface or in the serial log, check whether the spot value ControlPilotDuty shows 5% PWM on the control pilot.
10. In the web interface or in the serial log, check whether the resistance on the ProximityPilot pin is correctly detected. The spot value "ResistanceProxPilot" shall show 10000 ohms (means: infinite) while the charge plug is not connected, and it shall show around 1500 ohms if a CCS2 plug is connected. For CCS1 this should show 150 ohms if the button the plug is not pressed, and 480 ohms if the button on the plug is pressed. (Discussion here: https://openinverter.org/forum/viewtopic.php?p=70435#p70435)


## Clara shows software version "bootloader". Is tis normal?

No. Clara should report something like "[PEVSLAC] received GET_SW.CNF
For MAC 04:65:65:ff:ff:ff software version MAC-QCA7005-1.1.0.730-04-20140815-CS" 

If you see instead something like "[PEVSLAC] received GET_SW.CNF
For MAC 00:b0:52:00:00:01 software version BootLoader" then the QCA7005 was not able to load the firmware from the SPI flash U1. Some possible root causes are
* U1 is not populated
* U1 is not programmed with the software (see above "Programming of the SPI flash")
* U1 is wrong type.
* One of the SPI lines between the QCA and the SPI flash are not soldered correctly.


# Credits

Thanks to Millisman for the initial board design.

Thanks to Lars @evcreate for sponsoring the Ioniq charge control module.

Thanks to celeron55, mikeselectricstuff, Zieg, catphish, projectgus, chrskly, johu, Infant, muehlpower, crasbe, royhen99, asavage, Pete9008, P.S.Mangelsdorf, grgumxlm, royhen99, MeKe, mstegen, jrbe, Bigpie, xvyDFatih, olegiv, ns0708, f0ld, explorer232, dougyip, Gabriel, tom91, quentin42, maciek16c, apple2, CCSknowitall on the openinverter forum for the helpful discussions, proposals, reviews, tests and questions.

Special thanks to projectgus for organizing the chips.

Thanks to cookie_kater1 for the logos.
