# Pre-Delivery Tests

## Preparation

[ ] Solder bridge JP3 connects 1 to 2, to enable PP wakeup
[ ] visual inpection

## Voltage Tests

### Supply voltage ramp, while button is pressed
[ ] controller starts at ~5V
[ ] current decreases while voltage is increased to 18V

### Static voltages
[ ] 5V
[ ] 3.3V
[ ] 1.2V
[ ] 1.6V at all 4 RF pins

## Functional Tests

[ ] Sleep in case of power-up
[ ] wakeup by button, sleep after ~10s
[ ] wakeup by CP, sleep after ~10s
[ ] wakeup by PP 1k5, sleep after ~10s
[ ] wakeup by wakeline=12V, sleep after ~10s
[ ] wakeline changes to 12V if CP PWM is present

### Demo Charging Session

DemoVoltage 220V, DemoControl=Standalone

[ ] LED green blinking
[ ] LED blue blinking
[ ] State C on the EVSE detected
[ ] Connector lock motor controlled
[ ] Contactors activated, one after the other
[ ] Charging loop reached
[ ] Stop button terminates the session
[ ] Connector unlock
[ ] Red LED if the session is aborted by unpowering the EVSE and PP still 1k5

### Web interface

NodeId=22

[ ] Temp1, Temp2, Temp3 react (~33°C with 6k8 resistor)
[ ] Inlet voltage reacts (Parameter InletVtgSrc=AnalogInput)
[ ] AdcLockFeedback reacts
[ ] ResistanceProxPilot ~1500ohms and infinite
