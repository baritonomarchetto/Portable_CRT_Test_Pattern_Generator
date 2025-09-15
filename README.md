# Portable_CRT_Test_Pattern_Generator
A portable CRT Test pattern generator for SCART and arcade monitors based on RP2040.

This project follows Nicholas Murray [RP2040-TestPatternGenerator](https://github.com/nmur/RP2040-TestPatternGenerator) and adopts his firmware as-it-is. So credits are due :)

![](https://github.com/baritonomarchetto/Portable_CRT_Test_Pattern_Generator/blob/main/pics/IMG_20250911_113032_risultato.jpg)

# Features
This project emulates the functions of a once-common tool in CRT repair labs. It produce a series of low resolution images (test patterns) useful to check and fine tune CRT raster's geometry, colors, contrast, convergence etc. etc.

It is built around an RP2040 microcontroller board and a few other components.

Some features of the CRT Test Pattern Generator object of this instructable are:

- compatibility with low resolution arcade monitors (320x240 @15KHz)
- compatibility with EUR- SCART TVs
- small footprint
- portability (battery operated)
- cheap! (is this a feature? For me it is :))

No special components have been used.

As a general rule, I stick with through hole components. They are easier to deal with, especially because they don't ask for special soldering equipment. This project made no ecception and I tryed to use THT only, but in the end it was not possible.

Having SMD components (only two!) makes the project a little bit more difficult to assemble, but anyway doable even if your soldering skills are not top-notch.

# Challenges and Solutions
## Arcade Monitor
Arcade monitors need higher voltages with respect to home TVs on the RGB lines to display a sufficiently bright image. TVs are rated for 0.7Vpp signals, while arcade monitors 3-5Vpp* (TTL level).

This, in addition to the higher impedence arcade monitors have at their input, makes it necessary to re-calculate the series resistor that make up the resistor ladder at microcontroller board's GPIOs.

Luckily, the former library creator Miroslav Nemecek released an excel file with [his calculations](https://github.com/Panda381/PicoVGA/blob/main/vga_resistors.xls) that helped me define the resistor ladder values for arcade application.

A detailed explanation on how the series resistor values for VGA have been calculated is [>>HERE<<](https://github.com/Panda381/PicoVGA/issues/22).

To calculate the resistor values for arcade monitors I took as base an impedence of 1K ohm and a maximum color signal voltage of 3V for the reference RED and GREEN (Pi Pico GPIOs can output a maximum of 3.3V). I couldn't reach low standard deviations such for VGA, and colors gradients are damn poor on the dark side (it's like 2 bits colors instead of 3). I will try to spot better resistor values in the future. Luckily, bright colors are ok and these are the most used while troubleshooting an arcade monitor.

For what concerns the RGB-S connector, being arcade connectors not standard, I have here adopted screw terminals to lock extension cables with the right female connector on the other side. This extends the compatibility to almost all arcade monitors, even if asking for some preparation before use.

## TV SCART Monitor
EUR-SCART monitor are free from the aforementioned impedance and RGB level challenges, being them actually home TV's with the exact same logic of those [PicoVGA](https://github.com/codaris/picovga-cmake) was developed around.

The SCART standard has anyway specific rules that must be followed in order for a signal to be accepted by the receiving monitor.

One is the so called "blanking" signal (pin 16). This has to be in the 1 - 3V range to tell the monitor "hey, you are now receiving an RGB signal!".

Automatic switching to AV channel is also important because it frees from the use of the remote control. Automatic switching to the AV channel is possible by applying a tension of 9.5 to 10V on SCART pin 8.

## Battery Operation
If you have ever worked in your basement, with little to no surfaces where to place your diagnosis tools and flying cables all around, you know how easy it is to screw things up.

I don't have to tell you how important it is to use portable diagnosys tool, then.

The battery of choice for this project is a 9V alkaline battery. I opted for it mainly because of it's dimensions and availability.

A common way to juice the RP2040 Zero is by applying 5V regulated at the power pin. The microcontroller board is not power hungry, nor we have to drive big loads here, so a simple circuit built around a classic 5V voltage regulator is more than enought.

Tensions we have available in a powered pi pico are 3.3V, 5V and the plain B+. What is still missing is the +12V line. To generate such voltage I placed a dedicated, built-in step-up converter circuit.

# Links and Acknowledgments
You can find Nicholas Murray original CRT test pattern generator project at [THIS](https://github.com/nmur/RP2040-TestPatternGenerator) link. Having an already working firmware 100% fitting the task pushed the whole project up a lot, so thanks for sharing!

Nicholas project in turn makes good use of a brilliant library from Miroslav Nemecek. Take a look [HERE](https://github.com/Panda381/PicoVGA).

Interesting informations on how to make best use of most of the patterns included in this tester can be found [HERE](https://junkerhq.net/xrgb/index.php/240p_test_suite).

A special thanks goes to the nice girls and guys at [JLCPCB](https://jlcpcb.com/IAT) for sponsoring PCBs manufacturing and assembly of SMD componentss for this module.

Without their contribution this project would have never seen the light, like many others projects of mine now.

In this run they also sponsored the production of a rev.3 of the [CV mixer](https://www.instructables.com/4-Channels-AudioCV-Mixers-for-Eurorack-Synthesizer/) front panel board, the previous two having minor offset design issue. The new panel board design finally fits perfectly the front board.

This lot included also a revision of the [Sub-oscillator module](https://www.instructables.com/Sub-Oscillator-Module-for-Eurorack-Synthesizers/) main board, where I used a not-so-good footprint for the D flip flop that caused shorts if not soldered with care. Now it's all tested and dumb-proof.

JLCPCB is a high-tech manufacturer specialized in the production of high-reliable and cost-effective PCBs. They offer a flexible PCB assembly service with a huge library of more than 600.000 components in stock at today.

3D printing is part of their portfolio of services so one could create a full finished product, all in one place!

What about nano-coated stencils for your SMD projects? You can take advantage of a coupon and test it for free in these days.

By registering at JLCPCB site via [THIS LINK](https://jlcpcb.com/IAT) (affiliated link) you will receive a series of coupons for your orders. Registering costs nothing, so it could be the right opportunity to give their service a due try ;)

