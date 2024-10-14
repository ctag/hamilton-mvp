## The Accursed Hamilton MVP

*Want to spend $1000+ on a stepper motor in a box? Hamilton co has you covered!
... Want them to go the extra mile and provide incorrect documentation? Boy howdy, they do that too!*

This repo provides a python class to connect and control a MVP valve using the `DIN Protocol/BDZ+`.

---

The Hamilton MVP is a motorized valve that can be controlled over RS-232 serial.

Product page: https://www.hamiltoncompany.com/oem/oem/36798

Their (wrong) documentation: https://assets-labs.hamiltoncompany.com/File-Uploads/Valve_Serial-MVP_Users-Manual.pdf?v=1537387553

That user manual above provides the wrong values for controlling the MVP over `DIN Protocol/BDZ+`. Sending certain comands has no effect until the 'execute' character is sent. That character is absent from the user manual, leading it to be an excellent case study in the maxim "help is a vector."

A reference for better documentation: 
https://static1.squarespace.com/static/5876205e2e69cfaf7c9fe5a4/t/5a58c78024a69497e6f89da2/1515767685675/Hamilton+Microlab+500+BC+Users+manual.pdf
