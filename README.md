# ZYNQ-FLASH
QSPI flash support for Xilinx's Zynq devices

## Motivation
There are many problem around flash programming of the Zynq FPGAs. The task become harder if there is no DDR memory or the bootmode switch is unavailable, etc...
This repo contains a simple flash util, to read, write, erase the QSPI flash attached to Zynq FPGAs.

## How it work?
You can build a *flash_writer.elf* executable from the given sources, which will handle the QSPI
flash. This program runs on the Zynq's ARM to give an interface for the XSCT/XSDB TCL console to access
the QSPI flash. The *flash_writer.tcl* downloads this executable, and communicates with it to give
handy interface for the user.

## Try prebuilt on ZED board
You can try the prebuilt executables on [ZEDBoard][1] (or any board with 7Z020 device.)

  1. Open an [XSCT console][2].
  2. Go to this repo's folder. `cd zynq_flash`
  3. Source tcl sources: `source zed_bin/ps7_init.tcl` and `source flash_writer.tcl`
  4. Connect and choose your ARM target: `connect` and `targets <num>`
  5. Run all flashing operation (init, erase, write, verify) with one command:
  `flash_image zed_bin/boot.bin` (you can turn on/off erase, blank-check and verify)
  6. Turn off/on the board and check the LEDs. If you have any problem feel free to contact me.

[zed_bin_full_console.txt][3] is a full log of a flash programming.

## Build flash_writer.elf from source
The flash writer executable have to be re-built for different architectures. Here is a small
 description how to do that:

  1. Open Xilinx's XSDK.
  2. Create a new Application project. Use your own hardware-platform / board-support-package.
  Choose empty application. The [zed_bin/system.hdf][4] hardware definition for ZedBoard, so you can
   use that hdf for ZedBoard flashing.
  3. Copy the three sources: `main.c`, `flahs.c` `flash.h` into your application project's sources.
  4. Refresh or rebuild your project and check the output in the Debug directory.

## Hardware requirements
This flash writer does *not* uses DDR-RAM or the Boot-mode switch. So the only requirement is the QSPI flash, which can be enabled for the Zynq in the Vivado's *IP Integrator* (aka. blockdesign) -> Zynq Processing system -> MIO Configuration -> Memory interfaces.

[1]: http://zedboard.org/product/zedboard
[2]: https://forums.xilinx.com/t5/Vivado-Debug-and-Power/Standalone-XSCT-console/m-p/907961/thread-id/12946#M12947
[3]: zed_bin_full_console.txt
[4]: zed_bin/system.hdf
