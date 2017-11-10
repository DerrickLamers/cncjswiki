# [Raspberry Pi Shutdown Button & LED Script](https://twitter.com/AustinStAubin/status/798058374059335680)

Install & Setup for RaspPi GPIO Pendant: [cncjs-pendant-raspi-gpio](https://github.com/cncjs/cncjs-pendant-raspi-gpio)

Gracefully shutdown the Raspberry Pi using a hardware button.
- http://www.banggood.com/Power-Symbol-Latching-Switch-LED-Light-Push-Button-SPST-p-1064278.html
- https://cad.onshape.com/documents/94796d1c35c5a20444b82aa2/w/23a4445409a14a7f19bbd918/e/306a16744c5d0f265eb26f3c

NOTE: Jumping pins 5 & 6 (grounding GPIO3) will power on the RPi. So I altered the script to use GPIO3, rather than pin 40. Now I can use the same button to turn on and off the Pi!

[![Power Button Image](https://pbs.twimg.com/media/CxNE1T4UsAElq88.jpg)](https://twitter.com/AustinStAubin/status/798058374059335680)

**See RaspPi GPIO Pendant: [cncjs-pendant-raspi-gpio](https://github.com/cncjs/cncjs-pendant-raspi-gpio)**