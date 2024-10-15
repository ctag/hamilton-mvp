## The Accursed Hamilton MVP

<p align="center">
  <img width="460" alt="Image of the MVP. It looks so smug." src="https://github.com/ctag/hamilton-mvp/blob/main/resources/mvp.png">
</p>

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

---

### How do I use this?

Glad you asked! There's an example inside the class file, but also pasted here for easy referencing:

```python
import time
import hamilton-mvp.Mvp

def example_usage():
    found_ports = serial.tools.list_ports.comports()
    if len(found_ports) == 0:
        print("No serial devices found!")
        return
    for index, port in enumerate(found_ports):
        print(f"Found port: [{index}], {port.device}: \"{port.manufacturer}: {port.product}\" {port.vid}:{port.pid}")
    port_pick = int(input("Enter the index number (in [ ]) of the port to use: "))
    saline_valve = Mvp(found_ports[port_pick].device)
    # saline_valve = Mvp('/dev/ttyUSB0')  # OR manually. Find serial port with `ls /dev/tty*`
    saline_valve.open_session()
    saline_valve.initialize()
    saline_valve.wait_for_valve()  # Wait for valve motion to stop, or the next command will be ignored.
    existing_valve_type = saline_valve.poll_valve_type()
    print("Current valve type: ", existing_valve_type)
    if existing_valve_type != 6:
        saline_valve.set_valve_type(6)  # Set valve to 2-position, 90-degrees.
    saline_valve.set_valve_position(valve_position=2, counter_clockwise=0)
    saline_valve.wait_for_valve()
    time.sleep(0.5)
    saline_valve.set_valve_position(valve_position=1, counter_clockwise=1)
    saline_valve.wait_for_valve()
    time.sleep(0.5)
    saline_valve.set_valve_angle(valve_angle=90, counter_clockwise=1)
    saline_valve.wait_for_valve()
    time.sleep(0.5)
    saline_valve.set_valve_angle(valve_angle=0, counter_clockwise=0)
    saline_valve.wait_for_valve()
    saline_valve.set_valve_type(existing_valve_type)  # Shouldn't matter, valve settings will reset with power cycle.
    saline_valve.close_session()
    print("done")


if __name__ == "__main__":
    example_usage()
```
