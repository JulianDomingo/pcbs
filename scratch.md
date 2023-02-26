# RP2040

## Decoupling Capacitor
* tldr; helps ensure equilibrium with input voltages. Too high? => absorb excess
    energy. Too low? => provide more power to keep voltage stable.
* `VREG_IN` will typically need a stronger capacitor (also listed in data sheet)
* Should be as close to the MCU as possible when doing PCB schematic
* To get correct, generally just search for "capacitor" in RP2040 data sheet. By
    doing so, you get the following:
    * 6x `IOVDD` (digital IO supply) pins === 6x 100nF caps
    * 1x `USB_VDD` (USB supply) pin => "can use the same power source as IOVDD" => no need for an extra 100nF cap => 0x 100nF caps
    * 1x `ADC_AVDD` (ADC supply) pin => 1x 100nF cap
    * 1x `VREG_IN` (voltage regulator input supply) pin => 1x 1uF cap
* 1x `10uF || 4.7uF` pin => Lastly, most people typically add a stronger capacitor (10uF, or 4.7uF) seated
    the **furthest away** from the MCU to help smooth out low-frequency changes
    in the input voltage:
    https://www.autodesk.com/products/fusion-360/blog/what-are-decoupling-capacitors/.
