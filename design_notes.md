# RP2040
* Data Sheet: https://github.com/Sleepdealr/RP2040-designguide/blob/main/Pico-Resources/rp2040-datasheet.pdf

## Vias
* Connect traces between different layers of the PCB. Similar terminology as
    holes in case design (through-hole VIAs => VIA that connects bottom and top
    side of PCB).
* Generally better to have 1:1 mapping of vias to component:
    https://electronics.stackexchange.com/a/260106.

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
* 1x `10uF || 4.7uF` pin => Lastly, most people typically add a stronger capacitor (10uF, or 4.7uF) to help smooth out low-frequency changes in the input voltage:
    https://www.autodesk.com/products/fusion-360/blog/what-are-decoupling-capacitors/.
### VREG_OUT schematic
* Sometimes you'll need to carefully read the pin descriptions to determine if
    they should be connected to `VREG_OUT` or `VREG_IN`.
    * 1x `VREG_OUT` pin => 1x 1uF cap
        * "`The regulator must have 1μF capacitors placed close to its input (VREG_VIN) and output (VREG_VOUT) pins.`" (2.10.1 - Application Circuit)
    * 2x `DVDD` (digital core supply) pins === 2x 100nF caps
        * "`...The connection between the output pin of the on-chip regulator (VREG_VOUT) and the DVDD supply pins is
    made off-chip, allowing DVDD to be powered from an off-chip power source if
    required...`" (2.9.2 - Digital Core Supply (DVDD))
* TODO: how do you determine where to put power / ground symbols in the capacitor
    circuitry?

## Debug
* Used for reprogramming the PCB in the event of a failure (don't interface
    through USB).
* ATmega32U4 uses GPIO pins for this (thereby limiting pins for other use like
    RGB), but luckily RP2040 has its own dedicated pins for debug.
* 
