# RP2040
* Data Sheet: https://github.com/Sleepdealr/RP2040-designguide/blob/main/Pico-Resources/rp2040-datasheet.pdf

## Decoupling Capacitor
* tldr; helps ensure equilibrium with input voltages. Too high? => absorb excess
    energy. Too low? => provide more power to keep voltage stable.
* `VREG_IN` will typically need a stronger capacitor (also listed in data sheet)
    than other capacitor-needing components, e.g. I/O pins.
* Capacitors should be as close to the MCU as possible when doing PCB
    schematic/layout.
### VREG_IN schematic
* To get correct, generally just search for `capacitor` in RP2040 data sheet,
    then count the number of pin(s) in the Pinout diagram (search for `pinout`). By
    doing so, you get the following:
    * 6x `IOVDD` (digital IO supply) pins === 6x 100nF caps
    * 1x `USB_VDD` (USB supply) pin => "can use the same power source as IOVDD" => no need for an extra 100nF cap => 0x 100nF caps
    * 1x `ADC_AVDD` (ADC supply) pin => 1x 100nF cap
    * 1x `VREG_IN` (voltage regulator input supply) pin => 1x 1uF cap
* 1x `10uF || 4.7uF` pin => Lastly, most people typically add a stronger capacitor (10uF, or 4.7uF) seated
    the **furthest away** from the MCU to help smooth out low-frequency changes
    in the input voltage:
    https://www.autodesk.com/products/fusion-360/blog/what-are-decoupling-capacitors/.
### VREG_OUT schematic
* Sometimes you'll need to carefully read the pin descriptions to determine if
    they should be connected to `VREG_OUT` or `VREG_IN`.
    * 2x `DVDD` (digital core supply) pins === 2x 100nF caps
    * "`...The connection between the output pin of the on-chip regulator (VREG_VOUT) and the DVDD supply pins is
made off-chip, allowing DVDD to be powered from an off-chip power source if
required...`" (2.9.2 - Digital Core Supply (DVDD))
* TODO: how do you determine where to put power / ground symbols in the capacitor
    circuitry?
