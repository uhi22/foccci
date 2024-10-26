# Pre-Delivery Tests

## Preparation

[ ] 1. Solder bridge JP3 connects 1 to 2, to enable PP wakeup
[ ] 2. visual inpection

## Voltage Tests

### Supply voltage ramp, while button is pressed
[ ] 3. While wakeup input is at supply, increase supply voltage. Controller starts at ~5V supply. Alive LED flashes.
[ ] 4. current decreases while voltage is increased to 18V

### Static voltages
[ ] 5. 5V
[ ] 6. 3.3V
[ ] 7. 1.2V
[ ] 8. 1.6V at all 4 RF pins

## Functional Tests

[ ] 9. Sleep in case of power-up
[ ] 10. wakeup by button, sleep after ~10s
[ ] 11. wakeup by CP, sleep after ~10s
[ ] 12. wakeup by PP 1k5, sleep after ~10s
[ ] 13. wakeup by wakeline=12V, sleep after ~10s
[ ] 14. wakeline stays at 12V while CP PWM is present

### Demo Charging Session

DemoVoltage 220V, DemoControl=Standalone

[ ] 15. LED green blinking
[ ] 16. LED blue blinking
[ ] 17. State C on the EVSE detected
[ ] 18. Connector lock motor controlled
[ ] 19. Contactors activated, one after the other
[ ] 20. Charging loop reached
[ ] 21. Stop button terminates the session
[ ] 22. Connector unlock
[ ] 23. Red LED if the session is aborted by unpowering the EVSE and PP still 1k5
[ ] 24. Red LED flashing if the EVSE applies a 25% PWM. Not red, but blue with newer Clara.

### Web interface

NodeId=22

[ ] 25. Temp1, Temp2, Temp3 react (~33°C with 6k8 resistor)
[ ] 26. Inlet voltage reacts (with Parameter InletVtgSrc=AnalogInput)
[ ] 27. AdcLockFeedback reacts
[ ] 28. ResistanceProxPilot ~1500ohms and infinite
[ ] 29. ControlPilotDuty shows correct values 5% and 25%
[ ] 30. Serial log shows data
